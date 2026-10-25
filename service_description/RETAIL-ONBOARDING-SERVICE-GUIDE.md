# Retail Onboarding Service — Briefing Guide

This guide describes the implementation in `us-onboarding-service`, based on its Java source, Spring configuration, interaction definitions, Maven dependencies, migrations, and tests. It is intended as a concise walkthrough for explaining the service and answering common architecture questions.

## Read this first: quick customer flow

### One-line version

**Enter ID + mobile → service checks the customer → app submits location result → SMS OTP verifies mobile ownership → select the terms/journey route → conditional Nafath identity check → enter required profile details → set password → create/provision customer and account → show success.**

This is the main idea, not one guaranteed route for every person. The service branches for existing customers, new customers, migrated customers, minors, ineligible applicants, waitlisting, and missing information.

### Essential steps to remember

1. **Start / identify:** customer enters national/client ID and mobile number. `CheckIsClientExistsHandler` validates the ID and checks existing-customer/eligibility information.
2. **Choose route:** based on the checks, the user may be stopped, waitlisted, treated as existing, sent to migration, or allowed to continue. The service may prefill information it already knows.
3. **Location:** the mobile app submits its location-verification result. `VerifyLocationHandler` records the submitted validity flag in the case and returns it; Flow routes to OTP when valid or to the location-not-Saudi outcome when invalid. So the Java handler records/routes the result; location detection itself is reported by the client in this request.
4. **Verify the mobile — SMS OTP:** customer requests a code, receives it by SMS, and enters that code. This proves they can access the phone number; it does **not** prove their national identity.
5. **Terms and journey selection:** after OTP, Flow selects lite, full, or migration path based on customer state and configuration. The exact placement of terms and Nafath differs between these routes.
6. **Verify identity — Nafath, when required:** the service starts a Nafath request and gives the client a chosen number/session key. The customer confirms in Nafath; the app checks the session status. In the full journey, Nafath checks can be included in the terms/full-onboarding step; in the lite journey, the dashboard can route to Nafath afterwards. This is identity verification, separate from the SMS OTP.
7. **Collect required details:** customer fills whichever sections apply, such as address, account purpose, employment, income/financial information, declarations, PEP questions, and additional questions. Existing data can be prefilled.
8. **Create credentials and banking records:** customer submits a password. The service creates or updates customer data, enrolls the user, creates/fetches the account, and performs related integrations. New, existing, and migrated customers use different action chains.
9. **Finish:** on success, the service returns account details and a device-registration token, and sends a welcome SMS. The welcome SMS is not an OTP.

**Order correction:** in this configured journey, the initial customer check comes first, then location and SMS OTP, then terms. Nafath is a later, conditional identity step; do not explain the flow as “Nafath first, then onboarding” for every customer.

### What “step by step” means for stored information

- Flow creates/uses an onboarding **case** to represent this customer's application.
- Each screen submits a request to its named interaction handler.
- The handler reads the current case, changes the fields for that step, then writes the case data back through `CaseDataManipulator` / `JourneyMapper`.
- The next request uses the same case, so the service can continue where the customer left off. The app does not need to send every previously entered answer again.
- Flow/Camunda persists the main case and journey/process state in its configured database. This service also has separate tables for specific technical data, including OTP and Nafath session cache. Liquibase creates/updates these tables.
- **Do not say every field is stored in one table.** The applicant journey belongs to Flow case data; the OTP code/attempt state and Nafath session cache have their own persistence; identity/account data is also written to external banking/platform services during finalization.

### OTP confusion: how many codes are there?

For the normal first-time onboarding path shown in `onboarding.json`, there is **one customer-entered SMS OTP verification stage**:

- `request-otp` generates/stores the code and sends it to the mobile.
- `verify-otp` checks the code the customer enters.
- These are two API/handler calls for **one OTP**, not two different OTPs.
- `DEOneTimePassword` checks expiry, request limits, resend delay, and wrong-code attempts. `OtpCodeService` / `OtpRepository` persist the code and attempt/lock state in `client_otp_code`.
- `RealSendOtpService` sends it through the configured SMS SOAP service. Mock/direct connector mode can change the implementation.
- Configured defaults in `application.yml`: 4 digits, 300-second expiry, up to 3 verification attempts, 30-second resend wait, 3 requests per 24-hour reset window, and 900-second account lock after the configured failure limit.

**Nafath is not another SMS OTP.** It has its own start request, a Nafath-selected number/session key, and later status polling/verification. The user confirms the request in Nafath; the onboarding app asks the service for the result. A guardian Nafath check may additionally apply to a minor. That is another identity workflow, not another code sent by this service's SMS OTP handler.

There can also be ordinary informational SMS messages (for example, a welcome message after account creation). Those are notifications, not verification codes. If someone says “there are 2–3 OTPs,” ask which part they mean: this code request/verification pair is one OTP; Nafath/guardian is a separate identity check.

### Three customer routes at the end

- **New customer:** create the SNB customer and additional details, enroll user, create account, then do applicable loyalty/segment/arrangement work.
- **Existing customer:** update eligible customer/FATCA information, enroll user, create account if needed, then do applicable related work.
- **Migrated customer:** enroll the user and fetch migrated account details; it does not follow the brand-new customer creation path.

The actual classes are `NewClientActionsChain`, `ExistingClientActionsChain`, and `MigrateClientActionsChain`.

## Internal service working: from startup to one screen request

### A. What the service loads at startup

- **Spring Boot starts** at `UsOnboardingFlow`.
- **Configuration is read** from `application.yml` plus the active profile (`local`, `local-mysql`, `eureka`, etc.). Values choose mock/direct integrations, database settings, limits, service names, case/interaction definitions, and public collection names.
- **Liquibase applies database changes** from `db/changelog/db.changelog-custom-persistence.xml` and its included changesets. The Flow platform also owns its persistence schema.
- **Flow loads the workflow definition** from `src/main/resources/interactions/onboarding.json`. This is the workflow map: step name, handler bean name, optional hooks, and routing conditions.
- **Flow loads the case definition** from `src/main/resources/case-definitions/onboarding.json`. It defines the structure/schema of the onboarding data stored against a case.
- **Spring registers Java components** such as `@Component("check-is-client-exists")`. The name must match the handler name in the interaction JSON. Constructor injection supplies collaborators such as repositories, service interfaces, and mappers.
- **Collection providers register** under names such as `education-levels`, `occupations`, and `employment-statuses`; only the names listed in `backbase.flow.collections.public-collections` are configured as public collections.
- **Integration clients are wired** by configuration, for example `InternalServiceCommunicationConfiguration` makes generated REST clients using the platform inter-service `RestTemplate`; `ExternalServicesAutoConfiguration` supplies mock or direct service adapters.
- After startup, Flow has the workflow and handler registry ready to process interaction requests. The service does not build the onboarding screen sequence from a Java `main` method; Flow reads the JSON definition.

### B. What happens every time the mobile app submits a screen

1. **Request arrives** at the Backbase/Flow interaction API. The request identifies the interaction/case and the operation/handler, and includes that screen's input. Exact external URL and gateway authentication are environment/platform configuration; this source does not define every deployed route.
2. **Security and context are established** by the platform filters/IAM. Flow determines the user/session permissions and interaction context. `InteractionContext` gives handlers the current case key, interaction ID, and related context. Do not confuse a development local IAM user with production authentication.
3. **Flow finds the handler by name.** For example, JSON handler name `submit-employment-status` is implemented by Spring bean `SubmitEmploymentStatusHandler`, annotated `@Component("submit-employment-status")`.
4. **Input is deserialized** into that handler's request DTO, for example `SubmitEmploymentStatusRequest`. The response type is another DTO. This keeps mobile JSON shape separate from persistence and external-system objects.
5. **Validation runs.** Base handler classes and handler-specific validators check fields, required values, formats, allowed dictionary codes, age/employment rules, or business rules. Invalid input returns an `ActionResult` containing a Flow error; that screen's success transition should not run.
6. **Handler loads current data.** It gets the case key from `InteractionContext`, then uses `CaseDataManipulator`, `AdditionalInformationCaseDataManipulator`, or `OtpCaseDataManipulator` to read the correct part of the case/journey data.
7. **Business logic runs.** The handler can use pure Java rules, call a service interface (which may call a REST/SOAP service), fetch a collection, or combine these. Mapping classes convert between API DTOs, case-data models, and external-system models.
8. **Changed data is written back.** The manipulator calls a Flow `JourneyMapper`/writer and the Flow `CaseDataService`. The case key ties the new values to the same application. This write is different from saving an OTP JPA row or updating an external customer system.
9. **Handler returns `ActionResult`.** It contains a response body (or no body for a command) and, if needed, structured errors. The body can include decision flags such as `employed`, `isNewClientNeoAndSnb`, `fatcaCrsAvailable`, or Nafath verification outcome.
10. **Flow evaluates the JSON transition.** It checks `success()` and response fields, then keeps the same case and moves the interaction to the next configured step or outcome. The exact client API operation shape is provided by the Backbase Flow interaction API; this module's JSON describes the handlers and routing rules rather than implementing a separate REST endpoint for each screen.
11. **Events/hooks and logs run** where configured. Event hooks publish case tracking status; `LoggingAspect` adds case/interaction/user correlation fields to the log context.

### C. Concrete screen examples: load data versus submit data

- **Employment screen — load options**
  - Flow's `employment-status-adult` or `employment-status-not-adult` step calls `fetch-employment-status`.
  - `FetchEmploymentStatusHandler` reads the applicant from the existing case.
  - It chooses adult/minor allowed statuses from configuration and asks `EmploymentStatusProvider` for localized labels.
  - `EmploymentStatusProvider` reads `/collections/employment-status/employmentStatus.json`.
  - Handler returns a response list to the app. This is a **read**; it does not save the user's selection.
- **Employment screen — submit selection**
  - App sends its selected status to `submit-employment-status`.
  - `SubmitEmploymentStatusHandler` checks the string is a valid `EmploymentStatus` enum and checks minor/age rules.
  - It updates `AdditionalInformationData`, writes that data against the case, then returns flags.
  - Flow checks those flags: an employed existing customer may go to `additional-information-employed`; a new customer may instead go to `job-information-new-employed-client`; not-employed cases take their matching branch.
- **Job screen — load/reference data**
  - `FetchJobInformationHandler` reads existing job data and returns it, including descriptions for codes.
  - Education and occupation providers use `CcrCollectionProvider`/`CcrService` to fetch localized reference data from the CCR/reference-data service. `OccupationProvider` also filters occupations using employment status/sub-status.
  - The response can contain both code and human-readable label so the UI can show a selection while the server preserves a stable code.
- **Job/additional-information screen — submit**
  - `SubmitJobInformationHandler` fetches existing additional info, validates mandatory fields, date/email/text formats, and whether the chosen occupation is allowed for the sub-entity.
  - Mappers update job fields in the case. It may call `PepService` using occupation code; then it stores the PEP check result and returns a routing flag.
- **Purpose screen — submit**
  - `SubmitPurposeOfAccountHandler` stores the purpose in case data and calculates whether mandatory job/financial data is already complete, whether the applicant is under age, and whether FATCA/CRS data is available.
  - Flow routes directly to credentials when required information is already present; otherwise it routes to the relevant employment step.
- **Financial information — load and submit**
  - `FetchFinancialDetailsHandler` reads saved values and resolves code descriptions through income/payment/amount/proof providers.
  - Those dictionary providers call CCR reference data rather than being a general collection of static JSON in this example.
  - `SubmitFinancialDetailsHandler` validates allowed codes, required fields, and numeric amounts, maps values into case data, then writes the case.
- **Customer declaration — submit**
  - `SubmitCustomerDeclarationHandler` validates the request against existing additional-info data, maps it to a `CustomerDeclaration`, marks its source as `USER`, and writes it back.
- **Additional questions — submit**
  - `SubmitAdditionalQuestionsHandler` reads the case, calls the risk-ranking service, stores the answers and risk result, writes them to the case, and returns a high-risk flag. The JSON then routes high-risk applicants to an ineligible outcome; other successful applicants can continue to credentials.

### D. Collection data: JSON file or another service?

- **Collections are dropdown/reference options**, not the applicant's submitted answers.
- Some collection providers read packaged files. Example: `EmploymentStatusProvider` extends `JsonFileCollectionProvider` and loads `src/main/resources/collections/employment-status/employmentStatus.json`. The resource includes codes and display descriptions.
- Other providers call CCR/reference data at request time. Example: `EducationLevelsProvider` uses `CcrCollectionProvider` and asks `CcrService` for `ACADEMIC_QUALIFICATIONS`; `OccupationProvider` requests `OCCUPATION` and filters it based on employment information.
- `application.yml` lists which provider names are public, while provider Java classes define the source, mapping, filtering, localization, and validation behavior.
- A JSON file existing in `resources/collections` does **not** prove the matching provider reads that file. Check the provider's superclass and `query` method. For example, although education-level JSON files are present, the inspected `EducationLevelsProvider` queries CCR.
- When the app submits a selected value, a handler may validate that code against the allowed dictionary and then save the code (plus related structured fields) into case data. The collection itself is not copied wholesale into each case.

### E. What gets stored, and where?

- **Main application answers/state:** case data (`Onboarding`, applicant, purpose, declarations, job/financial details) through `JourneyMapper` → Flow `CaseDataService` → configured Flow database.
- **Workflow position/status:** Flow interaction/process/case persistence, so it knows the current step and can resume the journey.
- **OTP code and counters:** `DEOneTimePassword` → `OtpCodeService` → `OtpRepository` → `client_otp_code` table. On success the code is removed; expiry and lock rules are checked.
- **Nafath session lookup:** Nafath cache service/repository → `nafath_session_cache` table when that cache path is used. The main verification request/response also carries the session key.
- **New customer/user/account records:** written to external SNB/core/Keycloak/User Lifecycle/arrangement/loyalty services by adapters/actions as the completion sequence progresses; these are not just case fields.
- **Schema version:** Liquibase records applied changesets in its tracking table. It creates/updates the service's custom OTP/Nafath tables and Flow persistence schema includes.
- **Important distinction:** a handler may update case data, but an external-system write happens only if that handler/action calls the relevant service. Saving the answer “purpose of account” is not the same operation as creating the bank account.

### F. Simplified internal call diagram

```text
Mobile app
  → Backbase/Flow interaction API + IAM context
  → interaction JSON chooses named handler
  → request DTO + validation
  → read current case via CaseDataManipulator / JourneyMapper
  → business rules + mapper
      ├─ collection provider → resource JSON OR CCR reference-data API
      ├─ service interface → direct REST/SOAP adapter OR mock adapter
      └─ JPA repository for custom OTP/cache data
  → write changed case data / external side effect
  → ActionResult (response DTO + errors)
  → interaction JSON evaluates transition
  → next handler / next step / exit outcome
```

## 1. What the service does

`us-onboarding-service` is a Spring Boot / Backbase Flow application that runs the retail customer onboarding journey. The onboarding journey is a stateful workflow: it checks and identifies an applicant, collects and verifies information, then creates or updates customer, user, and account records in connected banking services.

It is not a conventional REST application where every endpoint is implemented by a controller. Most journey operations are Flow **interaction handlers**: `us-onboarding-service/src/main/resources/interactions/onboarding.json` declares steps, names each handler, and defines conditional transitions; Java handler beans perform the work. The Flow platform supplies the interaction API, case/process lifecycle, persistence, IAM integration, and much of the runtime.

The executable entry point is `com.backbase.flow.onboarding.us.UsOnboardingFlow`. It enables Spring Boot component discovery, JPA entities and repositories, caching, scheduling, retry support, and configuration-property scanning.

## 2. Main request and onboarding flow

The client exchanges JSON request/response DTOs with Flow's interaction API. The definition identifies the current step and handler; Spring resolves the handler by its bean name. A handler validates/maps the request, reads or updates the current case data, calls services, returns an `ActionResult` containing a response DTO (and possibly an error), and Flow applies the configured transition.

The normal route is conditional rather than one fixed sequence. In broad terms:

1. **Check and prefill:** `existing-client-check` uses `CheckIsClientExistsHandler` and prefill handlers to check applicant eligibility, existing SNB/NEO status, whitelist and migration conditions, and possible redirection outcomes.
2. **Location and phone verification:** location analysis can branch to an out-of-country result; OTP handlers request and verify a code. OTP settings include code length, expiration, verification/request limits, and lockout durations.
3. **Consent and identity:** terms/referral and customer declaration steps lead into identity verification. Nafath request/status checks, including guardian verification for applicable minors, are represented by dedicated handlers and transition events.
4. **Collect onboarding data:** handlers gather address, purpose of account, PEP/customer declarations, employment, job and financial details, additional information, and secondary contact information as applicable. Validators and mappers normalize and validate these values.
5. **Create credentials and customer/account records:** `SubmitCredentialsHandler` checks the password and chooses the new-client, migrated-client, or existing-client action chain. That chain integrates with customer/user/account and related services, then returns account details and a device-registration token on success.

The detailed branch conditions and handler/step names are in `us-onboarding-service/src/main/resources/interactions/onboarding.json`; the case data model is declared in `us-onboarding-service/src/main/resources/case-definitions/onboarding.json`. These JSON definitions are the best source when explaining a particular customer journey variant.

## 3. Package and class map

There are 569 Java source files under `us-onboarding-service/src/main/java` (and 260 under `us-onboarding-service/src/test/java`). The production classes are organized by responsibility; DTO, event, enum, and exception classes are included in the relevant package groups below rather than described one at a time.

| Package (under `com.backbase.flow.onboarding.us`) | Responsibility and representative classes |
|---|---|
| root | `UsOnboardingFlow`: Spring Boot application bootstrap. |
| `interaction` | Journey handlers, request/response DTOs, validation handlers, hooks, resume rules, and `handler/actions` action chains. Examples: `SubmitCredentialsHandler`, `CheckIsClientExistsHandler`, `VerifyNafathHandler`, `NewClientActionsChain`, `ProcessingContext`. |
| `additionalinfo` | Read/write and completeness checks for additional applicant information; `mappers`, `validators`, and domain `exceptions` are split into subpackages. |
| `casedata` | Case-data read/write/manipulation helpers (`CaseDataManipulator`). |
| `collections` | Public lookup/reference data providers, dictionaries, occupation/income/education data, and provider DTOs. |
| `configuration` | Spring beans, property binding, connector selection, Keycloak, OTP, and internal REST client configuration. |
| `service` | Service contracts and implementations for bank and external capabilities. `de` contains many direct/mock adapters; other subpackages cover account, address, CCR/reference data, SNB clients, migration, loyalty, PEP, remote config, risk, segment, and whitelist. |
| `service/clients/snb` | SNB customer-service interface/implementation, request creators, MapStruct mappers, and client-specific DTOs. |
| `mapper` | Case/journey and integration mapping; subpackages include address, loyalty, onboarding, and OTP. Several use MapStruct. |
| `otp` | OTP generation, case mapping/validation, persistence-facing service, and OTP domain types; `entity`, `repository`, and `exception` are nested packages. |
| `nafath` | Nafath verification/case-data logic, session cache and guardian consent, with `cache` and `guardian` subpackages. |
| `credentials` | Password rules, legal-entity selection, FATCA/tax mapping, device-registration token creation, and credential-related errors. |
| `clientcheck`, `id`, `phone`, `declarations`, `fatca`, `pep` | Focused applicant/client, identifier, phone, declaration, FATCA and PEP validation or mapping logic. |
| `db` | Custom case repository, sequence access, and status-count projection. |
| `controller` | The custom `ExtendedCaseStatsController`; most journey endpoints are supplied by Flow. |
| `event`, `epic`, `publisher`, `interaction/hook/epic` | Case tracking/status event types, constants, fraud publication, and the `OnboardingEventPublisher` that publishes Flow case events. |
| `error` | Flow exception handling, action error conversion, error types, and exception logging aspect. |
| `logging` | `LoggingAspect`, which adds request/case/interaction identity fields to the logging MDC. |
| `cache`, `time`, `util`, `validation`, `exclude` | Scheduled cache eviction, clock abstraction, shared utilities/validators, and feature-disable configuration. |

When discussing a concrete class, describe its role in this chain: **interaction handler → domain/service interface → real or mock adapter → external client/repository**, with DTOs and mappers at the boundaries. Many small source files are simple DTOs, enums, events, exceptions, or one-purpose providers rather than standalone business layers.

## 4. External services and communication

`InternalServiceCommunicationConfiguration` creates generated service API clients and gives them the platform's inter-service `RestTemplate`. The configured service IDs include user lifecycle, user manager, access control, access-group integration, arrangement manager, Nafath authentication, loyalty integration, identity integration, remote config, and CCR/reference data. In local development, discovery may be replaced by simple configured service URLs.

The adapters also integrate with legacy Cordys/SOAP services through generated SOAP types and Backbase `SoapClient`. The service code includes customer, customer-v12, accounts, credit verification, SMS notification, utilities, and loyalty SOAP capabilities. Other configured connector integrations include AML/ComplyAdvantage, SendGrid email, SmartyStreets address validation, and Jumio identity verification.

`ExternalServicesAutoConfiguration` chooses mock or direct implementations from connector mode. This supports local/test journeys without live banking services and direct integrations in the relevant environments. Most handler-to-service calls are synchronous: the handler waits for the client response before producing its interaction response.

The application also publishes Flow case tracking/status events (for example, OTP verified, account created, rejection/failure) via `CaseEngineEventPublisher`. This is event publication for the Flow case/epic model; it does **not** by itself establish that the service uses Kafka or asynchronous messaging.

## 5. Database, repositories, and transactions

Flow's case, process, and interaction state use the persistence provided by the Backbase Flow/Camunda platform. This service adds JPA entities and Spring Data repositories for OTP codes, Nafath session data, guardian consent, and whitelist/teaser-customer data. `CaseCustomRepository` extends `JpaRepository` for case statistics; custom repositories use `CrudRepository` where basic CRUD is enough.

Liquibase is enabled through `flow-liquibase-starter` and `liquibase-core`. `db.changelog-custom-persistence.xml` includes the Flow persistence schema and this service's custom changelogs. Custom changes include a risk-ranking sequence and tables/changes for OTP storage, Nafath session caching, OTP sending limits, and later schema adjustments. Hibernate `ddl-auto` is `none`, so schema changes should be made through Liquibase rather than automatic Hibernate DDL.

Profiles/configuration support H2 for standalone defaults documented in the service README, Oracle in the local profile, and MySQL through the `mysql-driver` / `local-mysql` setup. A project-specific connection-pool size/tuning is not evident in the service config; actual pool behavior comes from the Spring/Backbase runtime and selected datasource.

Explicit service-level transaction evidence is limited: `CaseCustomRepository.countCasesPerStatus` is a read-only transaction. Do not claim that the full external customer/account creation sequence is one database transaction; remote SOAP/REST side effects cannot be rolled back by an ordinary local DB transaction.

## 6. OTP, authentication, and cryptography

- OTP behavior is split across interaction handlers, OTP service/generator classes, persistence, and SMS/OTP service adapters. `OtpCodeService` reads, writes, removes, and updates attempt/lock fields; `OtpRepository` is Spring Data. Direct mode uses a random OTP generator; mock mode can use a mock generator. Limits and expirations are configuration properties, not hard-coded in the journey handler.
- Request security is provided mainly by Backbase Flow IAM/access control and Spring Security OAuth2 resource-server dependencies. Case/process authorization is enabled in the Camunda configuration. The local profile supplies local IAM permissions for development; production identity/resource-server settings are profile/deployment configuration.
- Keycloak Admin Client use is for creating/managing onboarding users (`KeycloakConfiguration` and `KeycloakUserServiceImpl`); it is distinct from authenticating every journey call.
- `SubmitCredentialsHandler` accepts an encrypted-password field and decrypts it using the Backbase `CryptoUtils` integration when present, while retaining a plain-password compatibility path. `CheckIsClientExistsHandler` also decrypts a client identifier when the supplied value is not in expected plain format. The local encryption integration is disabled by default; production keys/configuration must be provided securely.
- `DeviceRegistrationTokenCreator` creates a one-hour HS256-signed JWT with onboarding/device-registration claims. JWT signing is **not encryption**. Do not use a source default or local sample secret in production; supply a managed secret.

## 7. Logging, caching, scheduling, resilience, and concurrency

`LoggingAspect` puts the anonymous user ID, case ID, and interaction ID into SLF4J MDC and removes them at request completion. The logging pattern can therefore correlate messages across one interaction. `@Slf4j` is used by service/action classes; `CordysFlowExceptionHandler` and the error utilities translate/report application errors. Avoid logging credentials, OTPs, tokens, or sensitive applicant values.

Be aware that some current handler log statements include client ID and phone-number values (for example, `CheckIsClientExistsHandler`, `SnbRequestOtpHandler`, and `SnbVerifyOtpHandler`). When explaining logging, distinguish MDC correlation identifiers from applicant PII, and note that PII logging should be restricted/redacted according to the bank's policy.

Night-mode checks use Spring caching in `RealNightModeService` and `MockNightModeService`; false results are not cached. `NightModeCacheEvictTask` clears the `nightModeCache` on its configured cron schedule and zone. Nafath session/guardian data is also cached in database-backed repositories; configuration provides an expiry period.

The source uses `@EnableScheduling`, but no application-level `@Async`, custom thread pool/executor, or `CompletableFuture` implementation was found. The scheduled eviction task is scheduled work, not an asynchronous onboarding workflow. HTTP calls use configured Spring/Backbase clients; a separate custom connection-pool/thread-pool policy was not found.

`spring-retry` is a Maven dependency, but no `@Retryable`/`@Recover` use was found in this service's Java sources. No Resilience4j/circuit-breaker implementation was found. Do not present retries, circuit breaking, or a particular fault-tolerance guarantee as service-level behavior unless an inherited platform component or deployment configuration is confirmed separately.

## 8. Design and Java concepts you can explain

- **Interfaces and polymorphism:** service contracts (such as `NafathService`, `AccountService`, and `SnbClientService`) have direct and mock implementations. Spring connector configuration selects the implementation.
- **Strategy/branching:** `SubmitCredentialsHandler` selects the new-client, migration, or existing-client `ActionsChain`; connector modes select mock versus real integrations.
- **Action chain and compensation:** actions implement `Action`; `ActionChain` runs them in order. `RevertableAction` provides `undoIt`, and the chain invokes supported compensations after an exception. `UpdateFatcaDeclarationAction`, `CreateAccountAction`, and `ArrangementIngestionAction` are examples. This is a **saga-like local compensation pattern**, not evidence of a durable distributed Saga orchestrator or atomic rollback of all remote systems.
- **Decorator:** `DoNotStopDecorator` wraps selected non-critical actions so their failure does not stop the rest of the chain.
- **Dependency inversion / composition:** handlers and services depend on injected interfaces/clients rather than constructing integrations themselves; constructor injection and Lombok annotations reduce boilerplate.
- **Mapping and DTO boundaries:** request/response DTOs keep the interaction contract separate from case and external API models. MapStruct `@Mapper` implementations handle repeated structural conversions; hand-written mapper/creator classes handle business-specific mapping.
- **Enums:** enums encode constrained values such as legal entity, employment sub-status, ID type, Nafath status, and date/verification strategy values, reducing free-form string comparisons.
- **Sealed classes:** no Java `sealed class` or `sealed interface` declaration was found in the service source.
- **SOLID in practice:** the packages and interfaces show single-purpose components, open extension via service implementations, and dependency inversion through injected abstractions. Explain these as design tendencies/examples, not as a formal guarantee that every class perfectly follows every SOLID rule.

## 9. API schema, dependencies, and tests

The Maven module inherits dependency versions from the Backbase onboarding parent/BOM. Quick POM revision—**dependency keyword → why it is here**:

- `flow-iam-starter` → Flow case/task authorization and IAM integration.
- `flow-journey-starter`, `flow-collections-starter` → define/run customer journeys and serve reference-data collections.
- `flow-liquibase-starter`, `liquibase-core` → apply database schema changes at startup.
- `flow-dbs-access-control-starter`, `flow-dbs-common-starter`, `auth-security-dbs-accesscontrol` → Backbase access-control/security and shared DBS integration.
- `flow-dbs-audit-starter` → audit integration capability; whether it is active depends on configuration (`backbase.audit.enabled` is false in the shown local config).
- `snb-clients` and generated API specs → typed REST clients/models for SNB and other Backbase internal services.
- `spring-boot-starter-security`, `spring-boot-starter-oauth2-resource-server`, `spring-security-oauth2-jose` → request-security framework and OAuth/JWT support.
- `keycloak-admin-client` → create/manage onboarding users and credentials in Keycloak; used by `RealUserEnrollmentService`.
- `crypto-utils` → decrypt compatible encrypted request fields; used in handlers such as `SubmitCredentialsHandler`.
- `jjwt` → create the signed device-registration JWT in `DeviceRegistrationTokenCreator`.
- `customer-segment` → customer segment integration used by the segment service/action.
- `service-item-anti-money-laundering`, `service-item-identity-verification`, `service-item-product-selection`, `integration-core-product-selection`, `service-item-resume` → Flow connectors/features for AML, identity verification, product selection, and resuming journeys.
- `email-messaging-spring-boot-starter` → SendGrid email integration capability, such as configured onboarding messages.
- `flow-direct-insights-engine-starter`, `flow-direct-insights-restapi-starter` → Flow Direct Insights calculation and REST endpoints.
- `spring-retry` → retry library is available, but no service-level `@Retryable` usage was found in the Java source.
- `mapstruct-processor`, `lombok` → compile-time mapper generation and reduction of repetitive Java boilerplate.
- `commons-validator`, `jakarta.validation-api`, `swagger-annotations`, `jackson-databind-nullable` → value validation, request constraints, API metadata, and OpenAPI nullable-field compatibility.
- `spring-boot-starter-webflux` → reactive HTTP client/runtime support included with Spring; the inter-service client configuration examined here uses the platform `RestTemplate`.
- `h2` → embedded/local runtime database option; `mysql-connector-java` → MySQL profile; `ojdbc8` → Oracle JDBC connectivity.
- Test dependencies: `spring-boot-starter-test` / `spring-security-test` → JUnit, Spring test utilities, and security tests; `wiremock-standalone` → mock HTTP/SOAP dependencies; `testcontainers`, `junit-jupiter`, `oracle-xe` → database/container integration tests; `awaitility` → wait for eventual test outcomes; `flow-test-shared` and `soap-client-test-support` → Flow/SOAP test helpers.

Use `us-onboarding-service/pom.xml` to verify versions/scopes and the current dependency list. A dependency being present means the code can use it; it does not prove that a feature is enabled or used by this service.

Swagger annotations and OpenAPI-related client/code-generation dependencies appear in the POM. Generated API clients are used for service-to-service calls. A Springdoc Swagger UI/runtime API-documentation setup was not evident in this module, so distinguish **OpenAPI-generated client models/specs** from a guaranteed live Swagger UI endpoint.

Tests include unit tests for validators/mappers/credentials, MockMvc-based Spring integration tests, journey-flow scenarios, WireMock REST/SOAP stubs, and Testcontainers dependencies (including Oracle XE). Awaitility is available for eventual-condition tests. The README notes that Testcontainers integration tests require Docker and that the Failsafe configuration may need enabling locally; confirm the current Maven profile/CI setup before promising those tests run in every pipeline.

## 10. Common counter questions — short answers

**Is the flow synchronous or asynchronous?**  
The client interaction and most outbound service calls are synchronous request/response. The service emits Flow case events and runs a scheduled cache-eviction task, but no custom async worker/thread pool was found.

**Is this a Saga?**  
It has ordered action chains with selected compensating `undoIt` operations, which is saga-like. It is not a full distributed Saga engine, and some external side effects may not be reversible.

**Where are API endpoints implemented?**  
Most onboarding endpoints come from the Flow interaction engine, configured in `us-onboarding-service/src/main/resources/interactions/onboarding.json`. The custom controller currently visible in this module is for case statistics.

**Where is data stored?**  
Flow/Camunda owns case and process persistence. Service-specific entities/repositories store OTP, Nafath/guardian, and whitelist-related data. Liquibase manages schema changes.

**Where are retries and circuit breakers?**  
Retry is present as a dependency, but no service-level retry annotation/use or Resilience4j circuit breaker was found. Do not claim either without checking platform/deployment behavior.

**Which service makes the final account?**  
The credential submission handler chooses an action chain. `CreateAccountAction` delegates to the account service adapter, which in direct mode can call the legacy SOAP accounts service; later actions handle loyalty, segmentation, registration events, and arrangement ingestion.

**How are mock services used?**  
Connector-mode conditions in `ExternalServicesAutoConfiguration` select mock or direct adapters. The local profile commonly uses mock integrations to make the journey runnable without all remote systems.

## 11. Useful files to open while presenting

- `us-onboarding-service/src/main/resources/interactions/onboarding.json` — journey steps, handlers, conditions, and transitions.
- `us-onboarding-service/src/main/resources/case-definitions/onboarding.json` — case-data structure.
- `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/interaction/handler/SubmitCredentialsHandler.java` — final credential submission and action-chain selection.
- `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/interaction/handler/actions/` — orchestration actions and compensation.
- `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/configuration/ExternalServicesAutoConfiguration.java` — mock/direct connector wiring.
- `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/configuration/InternalServiceCommunicationConfiguration.java` — generated internal REST clients.
- `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/otp/` and `us-onboarding-service/src/main/java/com/backbase/flow/onboarding/us/nafath/` — OTP and Nafath persistence/logic.
- `us-onboarding-service/src/main/resources/db/changelog/` — Liquibase changes.
- `us-onboarding-service/src/test/java/com/backbase/flow/onboarding/us/` — end-to-end journey tests; `us-onboarding-service/src/test/java/.../wiremock/` — external-service stubs.

## 12. Interview walkthrough: explain one request end to end

Use this example when asked, “What happens when a customer starts onboarding and creates an account?”

1. **Flow resolves the journey.** The Flow interaction API receives the interaction request and identifies the active case and step. The `onboarding` interaction definition (version 151) maps that step's handler name to a Spring bean. The case uses definition version 65. Those versions are visible at the top of `us-onboarding-service/src/main/resources/interactions/onboarding.json`.
2. **The handler validates and reads the case.** For the first check, the bean is `CheckIsClientExistsHandler`, registered as `"check-is-client-exists"`. Its `BaseValidationActionHandler` parent handles the framework validation boundary. It decrypts an encoded client ID if necessary, checks the ID format, looks up identity/NEO/SNB data, checks phone linkage and eligibility, and updates the applicant in the case.
3. **Case data persists between interactions.** `CaseDataManipulator.fetchCaseData` gets or creates the case key, then reads the `Onboarding` model through `JourneyMapper`. `writeCaseData` writes it back. This is how several separate client requests share journey state without passing the entire application state back and forth in each request.
4. **Flow chooses the next step.** The handler returns an `ActionResult` with a typed response and optional error. Conditions in the JSON inspect response fields and select the next step, such as location verification, waitlisting, mobile-not-linked, existing-customer, or an ineligible outcome. This is configuration-driven workflow routing; Java does not contain one giant method controlling every transition.
5. **OTP is a two-interaction exchange.** `SnbRequestOtpHandler` reads `OneTimePasswordDefinition`, chooses a communication language, and delegates to `SnbOneTimePassword.generateCode`. `SnbVerifyOtpHandler` delegates code validation, marks the phone verified in case data, updates the lite-onboarding flag, and publishes the OTP-verified event. Expiry, request limits, and verification/lock settings come from configuration and OTP implementation, not from the Flow transition expression alone.
6. **Nafath identity verification is also a multi-request exchange.** `NafathRequestVerificationCodeHandler` calls the `NafathService` to start verification and returns a chosen code, session key, and polling-delay settings. A later `VerifyNafathHandler` retrieves the result using that session key. `RealNafathService` calls the generated Nafath API; the result is mapped and evaluated, relevant case data is updated/cached, and Flow tracking events are published. A waiting response is not the same as success: the client can poll again.
7. **The customer submits information in separate steps.** Address, declarations, purpose, employment, job, financial, and additional-information handlers each process their own request/response DTO and case-data fields. Their validators and mappers keep format and integration conversion logic out of the overall route definition.
8. **Credential submission selects a business path.** `SubmitCredentialsHandler` decrypts a supplied encrypted password when used, validates password rules, reads the case, and selects `NewClientActionsChain`, `MigrateClientActionsChain`, or `ExistingClientActionsChain`. The selected chain shares a `ProcessingContext`, so each action can pass the customer key, provisioned user, account details, and other results to later actions.
9. **Actions invoke side-effecting systems.** Typical actions update FATCA/client data, create user enrollment, create/fetch an account, create loyalty records, calculate segment, and ingest the arrangement. The direct account adapter (`RealAccountService`) builds a SOAP request and maps its response into `AccountDetails`. User enrollment uses Keycloak and User Lifecycle. The handler saves final case data, sends an SMS, and returns a success response containing account details and a signed device-registration JWT.
10. **Failures become interaction errors and/or compensation.** Known integration/business exceptions in `SubmitCredentialsHandler` map to stable action errors. When an action throws, `ActionChain` attempts `undoIt` in reverse order, starting with the action that threw and then previously completed actions, for each `RevertableAction`. Some individual actions also clean up as part of their own failure handling. This reduces orphaned records, but does not make multiple remote systems one atomic transaction.

The exact route includes branches and optional handlers; follow the transitions in `us-onboarding-service/src/main/resources/interactions/onboarding.json` for a specific applicant scenario rather than presenting the ten steps as mandatory for every customer.

## 13. SOLID and OOP — explain with the actual code

| Concept | Concrete example | How to explain the design and its limit |
|---|---|---|
| Encapsulation | `ProcessingContext`, `OtpCode`, case-data classes | Related state is grouped behind types rather than passed as unrelated primitive arguments. Lombok generates boilerplate accessors/builders in several classes. |
| Abstraction | `Action`, `ActionsChain`, `AccountService`, `UserEnrollmentService`, `NafathService` | Callers depend on a small contract; the implementation details (SOAP, REST, Keycloak, mock) are behind it. |
| Polymorphism | `RealAccountService` and `MockAccountService` implement `AccountService`; real/mock Nafath, phone, loyalty, segment and other adapters implement their service interfaces | The action/handler uses the interface; Spring selects the implementation for the configured connector mode. The call site can use either implementation without branching on adapter type. |
| Composition | `SubmitCredentialsHandler` receives three chains and many collaborators through constructor injection; each chain receives action beans | Behavior is assembled from collaborators, rather than building a single inheritance-heavy workflow class. |
| Single Responsibility (SRP) | `PasswordValidator`, `ApplicantMapper`, `CaseDataManipulator`, `NightModeCacheEvictTask` | Each has a focused reason to change: password policy, field conversion, case read/write, or scheduled cache cleanup. Some handlers are larger orchestration classes, so do not claim every class is perfectly single-purpose. |
| Open/Closed (OCP) | Service interfaces plus real/mock implementations and connector-mode selection in `ExternalServicesAutoConfiguration` | A mock or direct adapter can be selected/added without changing every caller. Adding a new branch or changing shared behavior can still require edits to configuration or a handler. |
| Liskov Substitution (LSP) | A mock and direct implementation of a service contract should honor the same behavior expected by the handler | This is the design intent. Mocks are for local/test behavior and need not emulate every remote-system detail; avoid claiming they are behaviorally identical in every edge case. |
| Interface Segregation (ISP) | `Action` exposes only `doIt(ProcessingContext)`. `RevertableAction extends Action` adds `undoIt` only for actions with compensation. `ActionChain` tests for the optional capability. | This is the clearest ISP example: simple actions do not have to implement an irrelevant undo operation. `CreateUserEnrollmentAction` is a plain `Action`; `UpdateFatcaDeclarationAction`, `CreateAccountAction`, and `ArrangementIngestionAction` are `RevertableAction`s. |
| Dependency Inversion (DIP) | `CreateAccountAction` depends on `AccountService` and `UserEnrollmentService`, not directly on SOAP or Keycloak classes | High-level orchestration depends on abstractions; adapter implementations contain infrastructure details. Spring injects the chosen concrete bean. |

**Be precise about the action pattern:** `ActionChain` is an ordered command/action runner. On failure, it attempts compensation for the failed action (if revertable) and earlier completed revertable actions, in reverse order. `DoNotStopDecorator` wraps an action, catches/logs its exception, and lets the chain proceed for best-effort work such as loyalty. This decorator intentionally changes failure behavior, so it is used selectively. Several `undoIt` implementations are TODO/no-op, and account creation performs specific user cleanup when account creation throws. Call it **saga-like compensation**, not a fully durable Saga framework.

## 14. Class-by-class source map for the main path

Open these in order during a code walkthrough. This list focuses on classes involved in the running path rather than listing every generated DTO, enum, event, and exception file in the 569-file source tree.

| Order / use case | Class(es) to inspect | What to point out |
|---|---|---|
| Application starts | `UsOnboardingFlow` | Spring Boot main method and enabled JPA repositories, entities, cache, retry infrastructure, scheduling, and property scanning. |
| Journey routes | `us-onboarding-service/src/main/resources/interactions/onboarding.json` | Step names, handler bean names, guards/conditions, and transition destinations. Current interaction version is 151. |
| Case schema | `us-onboarding-service/src/main/resources/case-definitions/onboarding.json`; `CaseDataManipulator`; `OnboardingCaseDataUtils` | Versioned onboarding case data and centralized read/write/applicant access. |
| Initial customer decision | `CheckIsClientExistsHandler`; `CheckIsClientExistsResponseFactory`; `ClientCheckPredicates` | ID normalization, external lookups, eligibility/migration checks, and conversion of conditions to response flags/errors. |
| OTP request/verification | `SnbRequestOtpHandler`; `SnbVerifyOtpHandler`; `SnbOneTimePassword`; `OtpCodeService`; `OtpRepository`; `OtpCodeEntity` | Separate request and verify interactions; domain contract, implementation/state handling, Spring Data storage. |
| Nafath initiation/result | `NafathRequestVerificationCodeHandler`; `VerifyNafathHandler`; `NafathService`; `RealNafathService`; `VerifyNafathService`; `NafathClientMapper` | Start/poll interaction, generated API client boundary, status-to-domain mapping, decision and cache handling. |
| Applicant conversion | `ApplicantMapper`; `SnbClientMapper`; `FatcaDetailsMapper`; `CustomerAddressMapper`; other `mapper` classes | External/domain/case fields are mapped at boundaries. MapStruct is used where declared; other mappings are explicit Java. |
| Request validation | `BaseValidator` and the specific `additionalinfo/validators/*Validator`; `validation/*Validator` | Shared field checks plus section-specific business validation; errors are returned through Flow action errors. |
| Credential rules | `SubmitCredentialsHandler`; `PasswordValidator`; `CryptoUtils` usage | Password extraction/decryption compatibility, policy validation, exception-to-action-error translation, and outcome response. |
| Existing customer creation | `ExistingClientActionsChain`; `ActionChain`; actions listed in section 15 | Existing-customer branch starts with FATCA/client updates and enrollment/account, then best-effort related work and arrangement ingestion. |
| New customer creation | `NewClientActionsChain`; `CreateSnbClientAction`; `CreateCustomerAdditionalDetailsAction` | New-customer-specific records are created before enrollment/account actions. |
| Migration path | `MigrateClientActionsChain`; `FetchMigratedAccountDetailsAction`; `NeoCustomerMigrationService` | Uses migrated identity/account details rather than treating it as a brand-new customer. |
| User creation | `UserEnrollmentService`; `RealUserEnrollmentService`; `UserEnrollmentRollbackOrchestrator`; `KeycloakConfiguration` | Keycloak identity creation and password reset, User Lifecycle provisioning, and cleanup/deprovisioning path. |
| Account creation | `CreateAccountAction`; `AccountService`; `RealAccountService`; `MockAccountService` | Optional existing-account lookup, SOAP account request/response mapping, and compensating user deletion on account-creation failure. |
| Case tracking | `OnboardingEventPublisher`; `OnboardingFlowRoutingEventPublisher`; `event/*` | Typed case tracking events are published for audit/status/analytics and journey routing; distinguish from a Kafka message bus. |
| Error/log context | `CordysFlowExceptionHandler`; `ActionErrorUtil`; `LoggingAspect`; `LogActionErrors` | Stable client-facing errors and correlation IDs in MDC. |
| Local statistics API | `ExtendedCaseStatsController`; `CaseCustomRepository`; `StatusCount` | Example of a service-owned REST controller and a read-only case-status aggregation query. |

## 15. Three credential action chains, step by step

All chains operate on the same `ProcessingContext`; each action can read fields written by earlier actions. The action list order is significant.

| New client (`NewClientActionsChain`) | Existing client (`ExistingClientActionsChain`) | Migration (`MigrateClientActionsChain`) |
|---|---|---|
| Clear any prior customer key; update FATCA declaration; create customer additional details; create SNB client; update SNB address; enroll user; create account; best-effort loyalty account/details, segment, registration/account hooks, and Nafath cache cleanup; ingest arrangement. | Update FATCA declaration and SNB client; enroll user; create account; best-effort loyalty account/details; calculate segment; run account-creation hook and cache cleanup; ingest arrangement. | Enroll user; fetch migrated account details; best-effort loyalty account/details; ingest arrangement. |

The source of truth is each chain's `processActions` method. The action classes are under `interaction/handler/actions`; each action delegates actual remote work to service interfaces under `service`. Each chain invokes `ActionChain` separately for its main action list and for arrangement ingestion, so compensation is scoped to each invocation; an arrangement failure does not automatically undo the earlier chain's work. Some behavior is explicitly best-effort via `DoNotStopDecorator`, while core enrollment/account steps are allowed to fail the request.

## 16. Authentication and authorization: what to say accurately

There are two related but distinct concerns:

- **Protecting service/case operations:** the POM includes `flow-iam-starter`, DBS access-control support, Spring Security, and OAuth2 resource-server dependencies. Flow IAM permissions cover case, task, journey, and related operations. `application.yml` enables Camunda authorization and defines the case definition. The local profile sets IAM implementation to `local` with a development `admin` user and permissions; it is not a production identity policy.
- **Anonymous onboarding identity:** `application.yml` configures the anonymous-user cookie name (`anonymousUserId`). `LoggingAspect` reads the `FlowSecurityContext` user ID and adds it to logging MDC. An anonymous applicant session is not the same as an authenticated banking user session.
- **Provisioning a new customer identity:** `KeycloakConfiguration` builds a Keycloak Admin Client from `KeycloakProperties`; `RealUserEnrollmentService` creates a Keycloak user, sets the password and attributes, and provisions a user through User Lifecycle. `UserEnrollmentRollbackOrchestrator` removes associated permissions/user/legal-entity resources in applicable cleanup paths.
- **Exceptions/public surfaces:** local config exposes selected actuator metrics paths. `ExtendedCaseStatsController` is explicitly annotated `@RunWithoutAuthorization`; mention this endpoint as a deliberate exception when explaining authorization and verify its deployment exposure policy.

The code shows the frameworks and local IAM configuration, but the complete production token issuer, JWT validation keys, gateway policy, secret provisioning, and role/permission deployment are outside this source snapshot. Say those are platform/environment responsibilities unless you have deployment configuration to demonstrate. Likewise, Keycloak Admin Client usage provisions users; it does not prove that every incoming service request authenticates directly against Keycloak.

## 17. Enums, sealed classes, transaction, and other “advanced” components

- **Enums are used where values form a controlled vocabulary.** Examples: `LegalEntityCode`, `EmploymentSubStatus`, `IdNumberType`, `SoapNafathVerificationStatus`, `WaitingListStatus`, `PreferredLanguage`, and `DateType`. For example, Nafath statuses are converted to an enum before the handler branches on completed/waiting/expired/rejected; employment-sub-status codes map to a parent employment status. This gives compiler-visible named values and avoids repeating raw strings, though external API boundaries may still use string codes.
- **Sealed classes/interfaces:** there are no Java `sealed` declarations in the production sources. Do not claim sealed-type usage; the code uses ordinary interfaces, classes, and enums.
- **Transactions:** Spring Data/JPA and Flow persistence are present, and `CaseCustomRepository.countCasesPerStatus` explicitly uses `@Transactional(readOnly = true)`. There is no evidence of one `@Transactional` wrapping the whole customer + Keycloak + account + arrangement sequence. Remote calls have independent side effects and are handled through explicit cleanup/compensation.
- **Caching:** `@Cacheable` on night-mode services avoids repeated checks under configured conditions; scheduled `@CacheEvict` clears those cached values every configured interval. Nafath session and guardian caches have their own service/repository and configured expiry. OTP's database persistence is not itself a cache.
- **Scheduling:** `@EnableScheduling` activates the `NightModeCacheEvictTask` schedule. This does not mean the customer onboarding flow runs in a background scheduler.
- **Async/thread pools:** no custom `@Async`, executor, `CompletableFuture`, or application-owned thread pool was found. Explain the request flow as synchronous unless a platform-level implementation is demonstrated.
- **Connection pools:** datasource connectivity comes from Spring/Backbase runtime and driver configuration. No service-specific pool sizing/tuning was identified in the checked YAML. Avoid quoting a pool size without resolved environment properties.
- **Discovery and internal communication:** generated service clients use the inter-service `RestTemplate` and service IDs in `InternalServiceCommunicationConfiguration`. Eureka is configured in an optional profile; local YAML can instead supply simple discovery instances or disable discovery. This is service discovery, not a circuit breaker.
- **Resilience:** action compensation and mock/direct service adapters are implemented. Spring Retry is a declared dependency but no service-level `@Retryable`/`@Recover` use was found; no Resilience4j circuit breaker was found. A dependency alone does not establish a behavior.
- **OpenAPI/Swagger:** generated OpenAPI clients and Swagger annotations are present in the build. That describes API contracts/client generation; this module does not visibly declare a Springdoc UI endpoint. Do not promise a live `/swagger-ui` route based on those dependencies alone.
- **Liquibase:** custom changesets are included by `db.changelog-custom-persistence.xml`; Hibernate DDL generation is disabled. On startup, enabled Liquibase applies pending changes to its changelog tracking table before application features rely on those tables.
- **Crypto vs hashing/signing:** `CryptoUtils.processDecrypt` decrypts compatible request values. The device-registration JWT is HMAC-signed, which proves integrity/authenticity but does not hide claims. Passwords are handed to Keycloak for credential management; do not describe the JWT signature as password encryption.

## 18. Two-minute answer you can practice

> “This is a Spring Boot service built on Backbase Flow. The customer journey is declared in `us-onboarding-service/src/main/resources/interactions/onboarding.json`, and Flow persists each case using the versioned onboarding case model. Each step resolves a named Spring handler, which validates the request, reads or writes case data through `CaseDataManipulator`, calls service interfaces, and returns an `ActionResult`; the JSON conditions then route the case to the next step.
>
> “The main journey checks customer identity and eligibility, verifies location and phone with OTP, performs Nafath identity checks when required, collects declarations and financial/employment details, and finally creates credentials and an account. The credential handler selects one of three action chains for a new, existing, or migrated customer. Actions call adapter interfaces; direct implementations communicate with SNB APIs, SOAP core services, Keycloak, User Lifecycle, and other integrations, while mock implementations support local/test modes.
>
> “The action design is a useful SOLID example: `Action` requires only `doIt`, while `RevertableAction` adds `undoIt` only where compensation is supported—an Interface Segregation example. `ActionChain` orders these actions, and `DoNotStopDecorator` makes selected integrations best-effort. This is saga-like compensation, not a distributed transaction. Flow owns journey/case persistence; Liquibase manages the schema, and service repositories handle OTP and other custom data. Spring IAM/access-control protects case operations, while Keycloak is used to provision the new user's identity. We use scheduled cache eviction and conditional caching for night mode, and publish Flow case events. I would not claim a custom async thread pool, sealed classes, or a Resilience4j circuit breaker because those are not implemented in this service source.”

If asked to prove a detail, name/open the class in section 14 rather than describing a framework capability as if the application itself implemented it.
