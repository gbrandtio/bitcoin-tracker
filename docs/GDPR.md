# GDPR: Data Protection Reference and Guidelines

## 1. Purpose, status and scope

This document is the binding data protection reference for every project built on the stacks documented in this repository. It turns the obligations of data protection law into engineering rules that design, code, configuration, operations and documentation must satisfy.

**Status.** Binding. Every rule marked MUST or MUST NOT is mandatory. No waiver, exception or temporary bypass is permitted. When a rule conflicts with a feature requirement, the conflict is raised with the project owner and resolved before implementation continues. The resolution is either a change to the feature or a change to this document; code never silently deviates from it.

**Legal position.** This document sets engineering rules. It does not replace legal advice. Where the controller (section 4) obtains legal advice that requires a stricter measure than a rule here, the stricter measure applies and this document is updated in the same change.

**Scope.** The document covers three tiers and everything between them, including third-party services called by any tier:

| Tier | Technology | Targets |
|---|---|---|
| Frontend | Flutter | iOS, Android, Web, Desktop (macOS, Windows, Linux) |
| Backend | ASP.NET Core API | All deployed environments |
| Database | PostgreSQL | All deployed environments, including replicas and backups |

**Legal regimes covered.**

| Regime | Instruments | Referred to as |
|---|---|---|
| European Union | Regulation (EU) 2016/679 (General Data Protection Regulation) | "GDPR", articles as "Art." |
| European Union | Directive 2002/58/EC as amended by Directive 2009/136/EC (ePrivacy Directive), Article 5(3), as transposed in each member state | "ePrivacy" |
| United Kingdom | UK GDPR, Data Protection Act 2018, Privacy and Electronic Communications Regulations 2003 | "UK GDPR", "DPA 2018", "PECR" |
| European Union | Regulation (EU) 2024/1689 (Artificial Intelligence Act), where it touches personal data and transparency | "AI Act" |

Article numbers of the GDPR are used throughout. The UK GDPR uses the same article numbers for the provisions cited here. Where the EU and UK regimes differ, the rule states both, and a project that serves both regions applies both.

**Relationship to other documents.**

- `docs/SECURITY.md` defines the security controls. Article 32 obligations (section 13) are met through it. When both documents apply, the stricter requirement wins.
- `docs/ASPNET_API_ARCHITECTURE.md` and `docs/FLUTTER_ARCHITECTURE.md` define structure and coding standards. Where they describe a mechanism that handles personal data, this document defines the minimum it must satisfy.
- `docs/features/` holds one document per feature. Each feature document holds the data inventory (section 6), the lawful basis records (section 7), the processor reviews (section 15) and, where required, the DPIA (section 17) for that feature.

## 2. Normative language and rule identifiers

The keywords MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT and MAY are interpreted as described in RFC 2119 and RFC 8174 when they appear in capitals.

Because no waivers exist (section 1), SHOULD is used only where a stronger alternative is also acceptable. Choosing not to follow a SHOULD requires the stronger alternative to be in place and to be recorded in the feature document under `docs/features/`.

Every rule has a stable identifier of the form `GDPR-<AREA>-<NN>`, for example `GDPR-RET-03`. Identifiers are cited in code reviews, doc comments, test names, feature documents and pull request descriptions. A removed rule's identifier is never reused.

| Area code | Section | Topic |
|---|---|---|
| `CORE` | 3 | Binding rules |
| `ROLE` | 4 | Roles and accountability |
| `CLS` | 5 | Identifying and classifying personal data |
| `ROPA` | 6 | Data inventory and records of processing |
| `LAW` | 7 | Lawful basis and purpose |
| `CON` | 8 | Consent and device storage |
| `TRN` | 9 | Transparency and privacy notice |
| `PBD` | 10 | Privacy by design and by default |
| `DSR` | 11 | Data subject rights |
| `RET` | 12 | Retention and deletion |
| `SEC` | 13 | Security of processing |
| `LOG` | 14 | Logs, monitoring, analytics and crash reports |
| `PRC` | 15 | Processors, third-party services and SDKs |
| `TRF` | 16 | International transfers |
| `DPIA` | 17 | Data protection impact assessment |
| `AI` | 18 | LLM layer, ingested third-party text and the AI Act |
| `BRE` | 19 | Personal data breaches |

## 3. Binding rules

This section holds the core rules that every change MUST follow. They are the engineering form of the principles in Art. 5 and the accountability duty in Art. 5(2) and Art. 24. Sections 4 to 19 detail how each rule is met and are equally binding. A change that follows sections 4 to 19 but breaks a rule in this section is not compliant.

### 3.1 Principles

- **GDPR-CORE-01. Lawfulness.** Personal data MUST NOT be collected, stored, derived, logged, cached, sent or displayed unless a lawful basis for that specific purpose is recorded before the processing starts (section 7).
- **GDPR-CORE-02. Fairness and transparency.** Every processing activity MUST be described in the privacy notice in plain language before it starts (section 9). Hidden collection, dark patterns and processing a person would not reasonably expect from the notice are forbidden.
- **GDPR-CORE-03. Purpose limitation.** Personal data MUST be used only for the purposes recorded for it in the data inventory (section 6). Using it for a new purpose requires a new record and the compatibility test of GDPR-LAW-05.
- **GDPR-CORE-04. Data minimization.** Only the personal data that a recorded purpose requires MUST be collected, and only at the precision it requires. "It may be useful" is not a purpose.
- **GDPR-CORE-05. Accuracy.** Users MUST be able to correct their personal data (GDPR-DSR-05), and derived data MUST be recomputed or deleted when its source is corrected.
- **GDPR-CORE-06. Storage limitation.** Every item of personal data MUST have a retention period no longer than the maximums in section 12, and MUST be deleted or anonymized automatically when the period ends.
- **GDPR-CORE-07. Integrity and confidentiality.** Personal data MUST be protected by the controls of `docs/SECURITY.md` for its data class (section 13).
- **GDPR-CORE-08. Accountability.** Every decision this document requires (lawful basis, legitimate interest assessment, processor review, transfer tool, DPIA, retention period) MUST be recorded in writing in the feature document before implementation, so compliance can be demonstrated.

### 3.2 Engineering rules

- **GDPR-CORE-09.** No field, file, log event, cache entry, analytics event or outbound payload that contains personal data MAY exist without a row in the owning feature's data inventory (section 6).
- **GDPR-CORE-10.** Privacy protective settings MUST be the default. Optional processing is off until the user turns it on (section 10).
- **GDPR-CORE-11.** No third-party service, SDK or processor MAY receive personal data before its review is recorded and, where it acts as a processor, a data processing agreement is in force (section 15).
- **GDPR-CORE-12.** No personal data MAY leave the United Kingdom or the European Economic Area without a recorded transfer tool (section 16).
- **GDPR-CORE-13.** No personal data MAY be sent to a large language model except as section 18 allows.
- **GDPR-CORE-14.** Production personal data MUST NOT be copied into development, test, staging or demo environments, fixtures, screenshots or documentation (SEC-INF-08). Non-production environments use synthetic data only.
- **GDPR-CORE-15.** Real data and simulated data MUST NOT be mixed in one code path (SEC-SDLC-09, FL-SIM-01). Synthetic personal data is never sent to a production system, and real personal data is never processed by a simulation path.
- **GDPR-CORE-16.** Every feature that stores personal data MUST support export and erasure of that data for one data subject (section 11).
- **GDPR-CORE-17.** Every change that collects, stores, derives, logs, caches, sends or displays personal data, adds a third-party service or SDK, or touches ingested third-party text or any LLM layer MUST pass the checklist in section 20 and MUST be approved by the privacy reviewer (GDPR-ROLE-03).
- **GDPR-CORE-18.** When it is unclear whether a data item is personal data, it MUST be treated as personal data until the privacy reviewer records otherwise (GDPR-CLS-03).

## 4. Roles and accountability

- **GDPR-ROLE-01.** The project owner is the controller (Art. 4(7)). The controller decides purposes and means of processing, answers data subject requests, signs data processing agreements, decides on breach notification and is the contact named in the privacy notice.
- **GDPR-ROLE-02.** No Data Protection Officer is appointed. The controller MUST reassess this at every DPIA (section 17) and at every change that adds a processing activity. A DPO MUST be appointed before processing starts when any Art. 37(1) condition applies: the core activities consist of regular and systematic monitoring of data subjects on a large scale, or of large-scale processing of special category or criminal offence data. The assessment is recorded in the consolidated record (GDPR-ROPA-04).
- **GDPR-ROLE-03.** A privacy reviewer MUST be designated by the controller. The privacy reviewer approves every change covered by GDPR-CORE-17, every data inventory change, every processor review and every DPIA. The reviewer MUST NOT approve their own change; a second person with privacy review responsibility does so (the same separation as SEC-SDLC-05).
- **GDPR-ROLE-04.** When the controller is not established in the EU and offers goods or services to, or monitors, people in the EU, an EU representative MUST be designated in writing (Art. 27). When the controller is not established in the UK and does the same for people in the UK, a UK representative MUST be designated. Both are named in the privacy notice. The Art. 27(2) exemption applies only when the controller records why the processing is occasional, excludes large-scale special category data and is unlikely to result in a risk.
- **GDPR-ROLE-05.** Every other party that processes personal data for the project MUST be recorded with its role: processor (Art. 28), sub-processor, joint controller (Art. 26) or independent controller. A joint controller relationship MUST have a written arrangement that sets out each party's duties, and its essence MUST be available to data subjects.
- **GDPR-ROLE-06.** When the ICO data protection fee applies under the Data Protection (Charges and Information) Regulations 2018, the controller MUST pay it before processing personal data of people in the UK.
- **GDPR-ROLE-07.** Every person with access to production personal data MUST be bound by confidentiality and MUST have completed data protection training within the previous 12 months.

## 5. Identifying and classifying personal data

### 5.1 What counts as personal data

Personal data is any information relating to an identified or identifiable natural person (Art. 4(1)). A person is identifiable when they can be singled out directly or indirectly, by the project or by anyone with reasonable means, alone or combined with other data.

- **GDPR-CLS-01.** The following MUST be treated as personal data wherever they appear, including in logs, metrics, caches and third-party payloads:
  - names, email addresses, phone numbers, postal addresses, user names and display names;
  - account identifiers, user identifiers and any other internal identifier linked to a person;
  - IP addresses, device identifiers, installation identifiers, advertising identifiers, push notification tokens and public keys registered to an installation (SEC-SIG-03);
  - location data at any precision finer than country;
  - user-generated content, free text entered by users and uploaded files;
  - behavioral data: interaction events, usage history, preferences and settings tied to an account or device;
  - financial data: holdings, transactions, balances, payment details and any identifier that can be linked to them;
  - any inference or profile derived from the items above;
  - text ingested from third parties that names or describes a natural person (section 18).
- **GDPR-CLS-02.** Pseudonymized data (Art. 4(5)) is personal data. Hashing, encrypting or tokenizing an identifier does not make it anonymous. Only data that meets the anonymization test of GDPR-RET-10 is outside this document.
- **GDPR-CLS-03.** A data item whose status is unclear MUST be treated as personal data (GDPR-CORE-18). Reclassifying it as non-personal requires a written reason approved by the privacy reviewer.

### 5.2 Special categories and criminal data

- **GDPR-CLS-04.** Special category data (Art. 9(1): racial or ethnic origin, political opinions, religious or philosophical beliefs, trade union membership, genetic data, biometric data used to identify a person, health data, data about sex life or sexual orientation) and criminal offence data (Art. 10) MUST NOT be processed unless the feature document records the Art. 6 basis, the Art. 9(2) condition (and, for the UK, the DPA 2018 Schedule 1 condition and appropriate policy document), and a completed DPIA.
- **GDPR-CLS-05.** Features MUST be designed so that special category data cannot be inferred where the purpose does not need it. Free text fields, uploaded files and LLM outputs are treated as able to contain special category data (GDPR-PBD-05).
- **GDPR-CLS-06.** On-device biometric unlock (SEC-FE-12) uses the operating system's biometric service; the biometric template never reaches the application. The application MUST NOT capture, store or transmit biometric data itself.

### 5.3 Mapping to security data classes

Every item of personal data is assigned a class from `docs/SECURITY.md` section 5, which decides its security controls.

| Personal data kind | Minimum security class |
|---|---|
| Any personal data not listed below | Confidential |
| Special category data, criminal offence data | Restricted |
| Financial records, payment details | Restricted |
| Credentials, tokens, recovery codes, third-party credentials held for users | Restricted |
| Government identifiers (passport, national insurance, tax numbers) | Restricted |
| Pseudonymized identifiers used only in logs (GDPR-LOG-03) | Confidential |
| Data that meets the anonymization test (GDPR-RET-10) | Internal |

- **GDPR-CLS-07.** Personal data MUST NOT be classified below Confidential. SEC-CLS-01 to SEC-CLS-04 apply.

## 6. Data inventory and records of processing

### 6.1 Per-feature data inventory

- **GDPR-ROPA-01.** Every feature document under `docs/features/` MUST contain a data inventory table with one row per personal data item the feature collects, derives, stores, caches, logs or sends. An item with no row MUST NOT exist in code (GDPR-CORE-09).
- **GDPR-ROPA-02.** Each row MUST record:

| Column | Content |
|---|---|
| Item | Field, file, event or payload name as it appears in code or schema |
| Data subjects | Whose data it is (users, contacts of users, third parties named in ingested text) |
| Security class | Per section 5.3 |
| Purpose | The specific purpose (GDPR-LAW-01) |
| Lawful basis | Per section 7, with a link to the LIA when legitimate interests apply |
| Source | User, device, system derived, third party (named) |
| Storage locations | Tables, client caches, logs, backups, processors |
| Recipients | Processors and other recipients (section 15) |
| Transfers | Destination country and transfer tool (section 16), or "none" |
| Retention | Period and trigger, within the maximums of section 12 |
| Export and erasure | The exporter and eraser that handle it (section 10.3) |

- **GDPR-ROPA-03.** The data inventory MUST be updated in the same change that adds, removes or alters an item, its purpose, its recipients or its retention.

### 6.2 Consolidated record of processing activities

- **GDPR-ROPA-04.** The controller MUST keep a consolidated record of processing activities (Art. 30(1)) generated from the feature inventories. It lists: the controller and any representative; each purpose; categories of data subjects and personal data; categories of recipients; transfers and their tools; retention periods; and a general description of the security measures, referring to `docs/SECURITY.md`. It also holds the DPO assessment (GDPR-ROLE-02). The Art. 30(5) exemption for small organizations MUST NOT be relied on.
- **GDPR-ROPA-05.** The record MUST be available to the supervisory authority on request and MUST be reviewed by the privacy reviewer at every release.

### 6.3 Single source for public declarations

- **GDPR-ROPA-06.** The privacy notice (section 9), the Apple privacy manifest and App Store privacy details, and the Google Play Data safety form MUST be derived from the data inventory (FL-STORE-09). A difference between the inventory, the declarations and the actual behavior of the app is a defect, and it is fixed before release.

## 7. Lawful basis and purpose

- **GDPR-LAW-01.** Each purpose MUST be specific and explicit, for example "send a password reset email", not "improve the service". One data item may serve several purposes; each purpose has its own row and basis.
- **GDPR-LAW-02.** Each purpose MUST have exactly one lawful basis from Art. 6(1), recorded before processing starts. The basis MUST NOT be switched after processing starts to keep processing that the original basis no longer covers.
- **GDPR-LAW-03.** Contract (Art. 6(1)(b)) MUST be used only for processing that is objectively necessary to deliver the service the user asked for. Analytics, product improvement, marketing, advertising and model training are not necessary for the contract.
- **GDPR-LAW-04.** Legitimate interests (Art. 6(1)(f)) MUST be supported by a legitimate interest assessment recorded in the feature document: the interest, why the processing is necessary for it, and the balancing test against the interests, rights and reasonable expectations of the people concerned. Users MUST be able to object (GDPR-DSR-09).
- **GDPR-LAW-05.** Using personal data for a purpose other than the one it was collected for MUST pass the compatibility test of Art. 6(4) (link between purposes, context, nature of the data, consequences, safeguards), recorded in the feature document, unless the new purpose is based on consent or law. An incompatible purpose requires new collection with its own basis.
- **GDPR-LAW-06.** Legal obligation (Art. 6(1)(c)) MUST name the specific law that requires the processing.
- **GDPR-LAW-07.** Consent (Art. 6(1)(a)) MUST meet section 8 and MUST NOT be used where refusal would stop the user from using the core service.

## 8. Consent and device storage

### 8.1 Valid consent

- **GDPR-CON-01.** Consent MUST be freely given, specific, informed and unambiguous (Art. 4(11), Art. 7). This means:
  - one separate choice per purpose; bundled consent and consent hidden in terms of service are forbidden;
  - no pre-ticked boxes, pre-enabled toggles, or consent inferred from scrolling, continued use or closing a dialog;
  - declining MUST be as easy as accepting: the same number of steps, a button of equal visual weight on the same layer;
  - access to functionality that does not depend on the purpose MUST NOT be conditioned on consent;
  - the request names the purpose, the data, any processor or third party receiving it, and how to withdraw.
- **GDPR-CON-02.** Withdrawing consent MUST be as easy as giving it and MUST be available at any time from a privacy settings screen reachable in the app. Processing based on the withdrawn consent stops immediately, and data collected only on that basis is deleted within the period of GDPR-RET-01 unless another recorded basis applies.
- **GDPR-CON-03.** The backend MUST store proof of every consent decision: pseudonymous subject identifier, installation identifier, purpose, choice, timestamp, the version identifier and a SHA-256 hash of the text shown, the language, and the channel (app platform or web). Consent records are append-only; a change of choice adds a record.
- **GDPR-CON-04.** When the text or purpose of a consent changes materially, the previous consent MUST NOT be relied on for the changed purpose, and the user MUST be asked again.
- **GDPR-CON-05.** Consent MUST be asked again at most every 13 months. After a refusal, the request MUST NOT be shown again for at least 6 months unless the user opens the privacy settings themselves.

### 8.2 Device storage and access (ePrivacy and PECR)

Storing information on, or reading information from, a user's device requires consent unless it is strictly necessary for a service the user explicitly asked for (ePrivacy Art. 5(3), PECR regulation 6). This applies on every platform, not only to browser cookies: it covers local storage, IndexedDB, files, shared preferences, keychain entries, SDK identifiers and device fingerprinting.

- **GDPR-CON-06.** Reading from or writing to the device MUST NOT happen before consent, except for storage that is strictly necessary. Strictly necessary storage is limited to: authentication tokens and installation keys (SEC-FE-01, SEC-SIG-01); security and fraud prevention state (RASP, attestation); the user's own consent choices; user interface preferences the user set (language, theme); and encrypted offline data of a feature the user uses (SEC-FE-02). Analytics, crash reporting beyond the scrubbed minimum of GDPR-LOG-07, advertising, A/B testing, personalization not requested by the user and third-party SDK storage are not strictly necessary.
- **GDPR-CON-07.** Each device storage item MUST be listed in the data inventory with its name, purpose, lifetime and whether it is strictly necessary.
- **GDPR-CON-08.** A third-party SDK that stores or reads data on the device, or sends data off the device, for a purpose that needs consent MUST NOT be initialized, loaded or given any identifier before consent for that purpose is recorded. Consent state is checked at start-up before any such SDK is created. When consent is withdrawn, the SDK is stopped, and its stored data and identifiers are deleted from the device.
- **GDPR-CON-09.** Device fingerprinting (combining device attributes to recognize a device without stored identifiers) MUST NOT be used for any purpose other than security (SEC-RASP-06, SEC-RASP-07), and security use MUST be recorded in the data inventory.
- **GDPR-CON-10.** On iOS, App Tracking Transparency permission (FL-STORE-10) is required in addition to consent under this section. Either one alone is not sufficient.
- **GDPR-CON-11.** Direct marketing by email, SMS or push notification MUST have prior consent, except where PECR regulation 22(3) or the equivalent member state rule allows the soft opt-in for existing customers; in that case an opt-out is offered at collection and in every message.

### 8.3 Children

- **GDPR-CON-12.** Each project MUST record in its data inventory whether its services are offered to children. When they are not, sign-up MUST include an age declaration, the terms MUST state the minimum age, and an account found to belong to a child below the minimum age MUST be deleted.
- **GDPR-CON-13.** When a project relies on consent for a child who is offered an information society service directly, the minimum age for the child's own consent is 13 in the UK and the age set by each EU member state (between 13 and 16) in the EU (Art. 8). Below that age, consent MUST be given or authorized by the holder of parental responsibility, and the project MUST make reasonable efforts to verify it. A project in this situation MUST complete a DPIA (section 17) and, for UK users, conform to the ICO Age Appropriate Design Code.

## 9. Transparency and privacy notice

- **GDPR-TRN-01.** A privacy notice MUST exist for every project. It MUST be written in clear and plain language, localized into every language the app supports (FL-L10N-01), and reachable in the app without signing in and from both store listings (FL-STORE-08), and on the project's website.
- **GDPR-TRN-02.** The privacy notice MUST contain every item in Art. 13 and Art. 14:
  - the identity and contact details of the controller and of any EU or UK representative (GDPR-ROLE-04);
  - each purpose and its lawful basis, and for legitimate interests, the interest pursued;
  - the categories of personal data, and for data not obtained from the user, its source;
  - recipients or categories of recipients, including processors;
  - transfers outside the UK or EEA and the transfer tool, and how to obtain a copy of the safeguards;
  - the retention period of each category, or the criteria that set it;
  - the rights of access, rectification, erasure, restriction, portability and objection, and how to exercise them in the app;
  - the right to withdraw consent at any time, where consent is the basis;
  - the right to lodge a complaint with a supervisory authority, naming the ICO for UK users;
  - whether providing the data is a statutory or contractual requirement and the consequences of not providing it;
  - the existence of automated decision-making, including profiling, with meaningful information about the logic and consequences (Art. 22);
  - the use of AI systems and AI-generated content (section 18).
- **GDPR-TRN-03.** The privacy notice MUST be versioned. Its content is derived from the data inventory (GDPR-ROPA-06) and updated in the same change that changes what it describes. Users are informed in the app of material changes before they take effect.
- **GDPR-TRN-04.** Every screen that collects personal data MUST show a short just-in-time notice at the point of collection stating what is collected and why, with a link to the relevant part of the privacy notice. Runtime permission explanations (FL-STORE-07) include this information.
- **GDPR-TRN-05.** When personal data is obtained from a source other than the data subject, the information of Art. 14 MUST be provided within one month of obtaining it, or at the first communication with the person, or before disclosure to another recipient, whichever is first. The Art. 14(5) exemptions MAY be relied on only when the reason is recorded in the feature document and approved by the privacy reviewer.

## 10. Privacy by design and by default

### 10.1 Design rules

- **GDPR-PBD-01.** Every feature design MUST record which data protection measures it applies (Art. 25(1)), with rule identifiers from this document, before implementation starts.
- **GDPR-PBD-02.** By default, only the personal data necessary for each purpose MUST be processed, stored for the shortest period and made accessible to the fewest people (Art. 25(2)). Optional features that process personal data are off until the user enables them. User content and profiles are private until the user chooses otherwise.
- **GDPR-PBD-03.** API request and response DTOs MUST carry only the fields the operation needs. Returning whole entities, and returning personal data the caller does not display or use, is forbidden (SEC-AUTHZ-05).
- **GDPR-PBD-04.** Where a purpose does not need to know who a person is (statistics, security correlation, rate limiting), the data MUST be pseudonymized at the point of collection through `IPseudonymizer` (section 10.3).
- **GDPR-PBD-05.** Free text fields and file uploads MUST be added only where the purpose needs them, with the need recorded in the feature document. Structured choices are preferred. Free text is assumed to contain personal and special category data and is classified accordingly.
- **GDPR-PBD-06.** Precision MUST match the purpose: dates of birth are not collected when an age check suffices; precise location is not collected when a region suffices; exact timestamps are coarsened in analytics.
- **GDPR-PBD-07.** Personal data MUST NOT appear in URLs, route paths, query strings, push notification bodies shown on the lock screen, deep links or file names (SEC-JWE-02, FL-NAV-03).
- **GDPR-PBD-08.** Test, demo, store screenshot and documentation data MUST be synthetic (GDPR-CORE-14). Synthetic personal data MUST be recognizably fictional and MUST NOT copy a real person.

### 10.2 Data subject rights built in

- **GDPR-PBD-09.** Every feature that stores personal data MUST implement a `IPersonalDataExporter` and a `IPersonalDataEraser` for the data it owns, registered through dependency injection (section 11). A feature without both MUST NOT be released.
- **GDPR-PBD-10.** Every stored personal data item MUST be reachable from the subject identifier, directly or through documented relations, so that access, export and erasure are complete. Personal data stored without a link to its subject (for example, in free text of another user) MUST be identified in the data inventory with the method used to find it.

### 10.3 Privacy components and SOLID

Privacy components are small, replaceable and testable, in the same way as the security components in `docs/SECURITY.md` section 23.

| Component (port) | Responsibility | Principle and reason |
|---|---|---|
| `IConsentStore` | Record and read consent decisions (GDPR-CON-03) | **SRP**: the only place that knows consent records and versions. **DIP**: features and SDK wrappers depend on this port, not on its storage. |
| `IConsentGate` (Flutter) | Decide whether a consent-dependent SDK or processing path may start | **SRP**: start-up and runtime gating in one place (GDPR-CON-08). **OCP**: a new purpose is a new registered entry, not an edit to the gate. |
| `IPersonalDataExporter` | Export one feature's data for one subject | **ISP**: each feature implements only export for its own data. **OCP**: the export orchestrator gains a new feature by registration, without changing existing exporters. |
| `IPersonalDataEraser` | Erase or anonymize one feature's data for one subject | **ISP** and **OCP**: as for the exporter. **SRP**: the eraser owns only its feature's deletion rules. |
| `IDataSubjectRequestService` | Orchestrate access, export, erasure, restriction and objection requests | **SRP**: request lifecycle, deadlines and the request log (GDPR-DSR-12). **DIP**: depends only on the exporter and eraser ports. |
| `IRetentionPolicy` | Retention period and trigger per data item | **SRP**: retention rules change in one place. **OCP**: a new data item adds a policy without changing the deletion job. |
| `IRetentionJob` | Scheduled deletion and anonymization (GDPR-RET-06) | **SRP**: executes policies; it does not define them. |
| `IPseudonymizer` | Keyed pseudonymization of identifiers (GDPR-LOG-03) | **SRP**: one key and algorithm for pseudonyms. **LSP**: key versions and implementations are substitutable without callers changing. |
| `IPersonalDataRedactor` | Remove or mask personal data from text before logging or LLM calls (GDPR-AI-04) | **SRP**: redaction rules in one place. **OCP**: new detectors are added as implementations. |
| `IProcessorRegistry` | Allowlist of processors and the data categories each may receive (GDPR-PRC-06) | **SRP**: one source of truth for outbound personal data flows. **DIP**: outbound adapters depend on this port. |

- **GDPR-PBD-11.** Privacy components MUST be accessed only through their ports and registered through dependency injection. Each component's doc comment MUST state the business rule it enforces, the rule identifiers it implements and the SOLID principle its design applies.

## 11. Data subject rights

### 11.1 Common rules

- **GDPR-DSR-01.** The rights of access, rectification, erasure, restriction, portability and objection, and the Art. 22 safeguards, MUST be available to every data subject. Where the user has an account, every right MUST be exercisable in the app from the account or privacy settings without contacting support. Every right MUST also be exercisable through a contact channel named in the privacy notice, including for people without an account.
- **GDPR-DSR-02.** Identity MUST be verified in proportion to the request. In the app, re-authentication within the previous 5 minutes (SEC-AUTH-21) is sufficient. Requests through other channels are verified without collecting more personal data than the project already holds. Identity documents are not requested unless the controller records why no other method is possible.
- **GDPR-DSR-03.** Requests MUST be answered without undue delay and at the latest within one month of receipt (Art. 12(3)). The period MAY be extended by two further months only where the request is complex or requests are numerous; the reason is recorded, and the requester is told within the first month. Automated in-app requests complete immediately or within 24 hours.
- **GDPR-DSR-04.** Requests are free of charge. A request MAY be refused or charged for only when it is manifestly unfounded or excessive (Art. 12(5)), with the reason recorded and the requester told of the right to complain.

### 11.2 Individual rights

- **GDPR-DSR-05. Rectification (Art. 16).** Users MUST be able to correct inaccurate data and complete incomplete data. Corrections propagate to derived data (GDPR-CORE-05) and to processors that received it (GDPR-DSR-11).
- **GDPR-DSR-06. Access (Art. 15).** The response MUST include a copy of all personal data held about the subject from every feature, including logs linked by pseudonym where the project can re-link them, and the Art. 15(1) information (purposes, categories, recipients, retention, source, rights, automated decisions, transfer safeguards). The copy MUST NOT include personal data of other people unless they have consented or the controller records why disclosure is justified.
- **GDPR-DSR-07. Portability (Art. 20).** Data provided by the user and processed by consent or contract MUST be exportable in a structured, commonly used, machine-readable format: JSON with a documented schema, plus CSV for tabular data. The export is produced by the exporters of every feature (GDPR-PBD-09) and delivered encrypted, through an authenticated download that expires within 7 days.
- **GDPR-DSR-08. Erasure (Art. 17).** On a valid request, account deletion (FL-STORE-11) or the end of the retention period, the erasers of every feature MUST delete or anonymize the subject's data from live systems within the period of GDPR-RET-01, and from backups by GDPR-RET-07. Data that must be retained under a legal obligation is restricted (GDPR-DSR-10) and deleted when the obligation ends; the retained categories and the law are recorded in the data inventory. Erasure MUST also be requested from every processor holding the data (GDPR-DSR-11).
- **GDPR-DSR-09. Objection (Art. 21).** Where the basis is legitimate interests, users MUST be able to object, and processing stops unless the controller records compelling legitimate grounds that override the user's interests. Objection to direct marketing is absolute and takes effect immediately.
- **GDPR-DSR-10. Restriction (Art. 18).** The data model MUST support marking a subject's data as restricted. Restricted data is kept but not used for any purpose other than storage, legal claims or the protection of others' rights, and the user is told before the restriction is lifted.
- **GDPR-DSR-11. Notification of recipients (Art. 19).** Rectification, erasure and restriction MUST be communicated to every recipient in the data inventory that received the data, through the processor's documented deletion or update interface.

### 11.3 Records and tests

- **GDPR-DSR-12.** Every request MUST be logged with: request type, pseudonymous subject identifier, received time, verification method, completion time, outcome and any extension or refusal reason. The request log MUST NOT contain the exported data itself.
- **GDPR-DSR-13.** Automated tests MUST prove, for every feature, that the exporter returns all inventory items for a subject and nothing of another subject, and that after the eraser runs no inventory item of the subject remains in the database, caches or search indexes.

## 12. Retention and deletion

### 12.1 Maximum periods

The periods below are binding upper limits. A feature MAY set a shorter period in its data inventory. A longer period is not permitted unless a law requires it; the law is then named in the data inventory (GDPR-LAW-06).

| Data | Maximum period |
|---|---|
| Account data after an erasure request or account deletion | Erased from live systems within 30 days |
| Backups containing erased data | Crypto-shredded, or aged out within 35 days of the erasure |
| Inactive account (no sign-in) | Owner notified at 23 months; account deleted at 24 months |
| Application and diagnostic logs | 30 days |
| Raw IP addresses in any log or store | Truncated or deleted within 7 days |
| Security event logs, pseudonymized (SEC-OPS-01) | 12 months |
| Crash reports | 90 days |
| Analytics identifiers stored on the device | 13 months |
| Collected analytics data | 25 months |
| Consent proof records (GDPR-CON-03) | Lifetime of the consent plus 3 years |
| Data subject request log (GDPR-DSR-12) | 3 years after the request is closed |
| Breach register (GDPR-BRE-07) | 5 years after the breach is closed |
| Data export files (GDPR-DSR-07) | 7 days |
| Password reset and verification tokens | Their expiry (SEC-AUTH-17) |
| Client caches of personal data (SEC-FE-02) | Deleted on logout and at most 30 days after last use |

### 12.2 Deletion mechanisms

- **GDPR-RET-01.** Erasure from live systems MUST complete within 30 days of the trigger. In-app account deletion removes access immediately and completes erasure within that period.
- **GDPR-RET-02.** Every retention period MUST have a defined trigger (creation, last sign-in, account deletion, end of contract) recorded in the data inventory and implemented in `IRetentionPolicy`.
- **GDPR-RET-03.** Soft deletion (a deleted flag) MUST NOT be treated as erasure. Soft-deleted data counts as held and is subject to the same period as a hard delete.
- **GDPR-RET-04.** Erasure MUST cover every storage location in the data inventory: tables, search indexes, caches, object storage, queues, processor copies and client storage on devices where the user is signed in.
- **GDPR-RET-05.** Before an inactive account is deleted, the owner MUST be notified at least 30 days in advance through the channel on file, with a way to keep the account by signing in.
- **GDPR-RET-06.** Deletion and anonymization MUST run as scheduled automated jobs (`IRetentionJob`) at least daily. Every run records the number of items deleted per data item, without personal data. A failed run raises an alert.
- **GDPR-RET-07.** Backups MUST support erasure. Personal data in backups is encrypted under per-subject or per-record DEKs (SEC-KEY-05), and erasure destroys the DEK (crypto-shredding, SEC-KEY-11). Where a data item is not under a per-subject DEK, the backup that contains it MUST age out within 35 days, and a restore process MUST re-apply every erasure recorded since the backup was taken before the restored system is used.
- **GDPR-RET-08.** Logs MUST have retention enforced by the log platform configuration, not only by policy.
- **GDPR-RET-09.** Processors MUST be contractually required to delete or return personal data at the end of the service and within the periods of this section (GDPR-PRC-03).
- **GDPR-RET-10.** Data MAY be kept beyond its period only when it is anonymized so that no person can be identified by any means reasonably likely to be used, alone or combined with other data. The method (aggregation, generalization, suppression of small groups below 10) is recorded and approved by the privacy reviewer. Removing names or hashing identifiers is not anonymization (GDPR-CLS-02).

## 13. Security of processing

Art. 32 requires security appropriate to the risk. The controls are defined in `docs/SECURITY.md`; this section states how they meet Art. 32 and adds the rules specific to personal data.

| Art. 32 requirement | Controls in `docs/SECURITY.md` |
|---|---|
| Pseudonymization and encryption | ALE for Confidential and Restricted fields (section 8), envelope encryption and HSM keys (section 7), TLS 1.3 and JWE in transit (sections 9 and 11) |
| Confidentiality | Authorization in three layers and object-level checks (section 14), row-level security (section 15), client storage and screen protection (section 18) |
| Integrity | Request and response signing (section 10), transactional writes (SEC-DB-15), tamper-evident logs (SEC-OPS-02) |
| Availability and resilience | DDoS mitigation (SEC-INF-05), rate limits (SEC-APP-07), encrypted immutable backups (SEC-DB-19) |
| Timely restoration | Restore tested every 90 days (SEC-DB-20) |
| Regular testing and evaluation | Pipeline gates, DAST, penetration testing (SEC-SDLC-01 to SEC-SDLC-08) |

- **GDPR-SEC-01.** Every personal data item MUST receive at least the controls of its security class (section 5.3).
- **GDPR-SEC-02.** Access to personal data by staff MUST be limited to named roles that need it, granted just in time (SEC-AUTHZ-07, SEC-DB-09), and recorded. Support tooling MUST show only the fields a support task needs.
- **GDPR-SEC-03.** Reads of Restricted personal data and writes of Confidential personal data MUST be audited (`docs/SECURITY.md` section 5, SEC-OPS-01).
- **GDPR-SEC-04.** A change that weakens a security control on personal data is a change to this document's compliance and MUST pass the privacy review in addition to the security review.

## 14. Logs, monitoring, analytics and crash reports

- **GDPR-LOG-01.** Logs, traces and metrics MUST NOT contain personal data in clear form (SEC-OPS-03, FL-CODE-17). This includes request and response bodies, headers carrying identifiers, email addresses, names, free text and full IP addresses.
- **GDPR-LOG-02.** Every log statement that includes an identifier of a person MUST use the pseudonym from GDPR-LOG-03, never the raw identifier.
- **GDPR-LOG-03.** Pseudonyms MUST be produced by `IPseudonymizer` as HMAC-SHA-256 of the identifier under a dedicated pseudonymization key held in the key service, separate from every encryption and blind index key (SEC-KEY-03). The key is versioned and rotated at least every 12 months. Re-linking a pseudonym to a person is permitted only for security investigations and data subject access requests, by the roles named in the incident plan, and is audited.
- **GDPR-LOG-04.** IP addresses MUST be truncated (last octet for IPv4, last 80 bits for IPv6) before they are written to any store other than the short-lived rate limiting and security stores, and the full address is deleted within 7 days (section 12.1).
- **GDPR-LOG-05.** Log fields MUST be structured and allowlisted per event type. Logging an arbitrary object, exception message that may contain input, or a whole model is forbidden.
- **GDPR-LOG-06.** Product analytics MUST be based on consent (GDPR-CON-06), MUST NOT record personal data, screen content or free text (FL-UI-17), MUST use a random analytics identifier that is not linked to the account and is reset when consent is withdrawn, and MUST use an analytics provider reviewed under section 15. Server-side aggregate metrics that contain no personal data do not need consent.
- **GDPR-LOG-07.** Crash reports MUST be scrubbed of personal data and secrets on the device before they are sent (SEC-FE-10, FL-DATA-16). The strictly necessary minimum sent without consent is: app version, platform, operating system version, device model class, stack trace and a random crash identifier. Anything more requires consent.
- **GDPR-LOG-08.** Monitoring and observability vendors are processors and MUST pass section 15. Session replay and screen recording tools MUST NOT be used.

## 15. Processors, third-party services and SDKs

### 15.1 Review before adoption

- **GDPR-PRC-01.** Before any third-party service, API, SDK or package that receives, stores or can access personal data is adopted, a processor review MUST be recorded in the feature document that introduces it (FL-CODE-31) and approved by the privacy reviewer. The review records:
  - the provider's legal name, role (processor, joint controller or independent controller) and location;
  - the exact personal data it receives, including data the SDK collects on its own (device identifiers, IP address, usage events);
  - the purpose and lawful basis;
  - where the data is stored and processed, and the transfer tool (section 16);
  - its sub-processors;
  - its retention and deletion behavior;
  - whether it uses the data for its own purposes, including advertising or model training;
  - its security certifications and breach notification commitment;
  - the consent gating required (GDPR-CON-08).
- **GDPR-PRC-02.** A provider that uses personal data for its own purposes, including training models, advertising or selling data, MUST NOT be used as a processor. Where such a provider is unavoidable, it is an independent controller, a separate lawful basis and notice are required, and the privacy reviewer approves it after a DPIA.

### 15.2 Contracts

- **GDPR-PRC-03.** Personal data MUST NOT flow to a processor before a data processing agreement meeting Art. 28(3) is in force. It MUST cover: processing only on documented instructions; confidentiality of personnel; Art. 32 security; sub-processor approval and flow-down; assistance with data subject rights, DPIAs and breaches; deletion or return at the end of the service within section 12 periods; audit rights; and breach notification to the controller without undue delay and at most within 24 hours of the processor becoming aware.
- **GDPR-PRC-04.** Sub-processor changes MUST be notified to the controller in advance with a right to object. The controller reviews each notice under GDPR-PRC-01 before the change takes effect.

### 15.3 Register and enforcement

- **GDPR-PRC-05.** The controller MUST keep a processor register listing every processor, its DPA, its transfer tool, the data categories it receives and the features that use it. It is part of the consolidated record (GDPR-ROPA-04).
- **GDPR-PRC-06.** Backend outbound calls that carry personal data MUST go through the outbound client allowlist (SEC-APP-04), and each destination MUST be listed in `IProcessorRegistry` with the data categories it may receive. A payload category that is not allowed for a destination is blocked.
- **GDPR-PRC-07.** The Web build MUST NOT load third-party scripts at runtime (SEC-RASP-13). Native SDKs are bundled at build time and are covered by GDPR-PRC-01 and GDPR-CON-08.
- **GDPR-PRC-08.** Processor reviews MUST be repeated at least every 12 months and whenever the provider changes its terms, location or sub-processors.

## 16. International transfers

Personal data may leave the UK or EEA only under Chapter V. Access from outside the region, including remote support access and a provider's staff access, is a transfer.

- **GDPR-TRF-01.** Hosting of production data, backups and logs containing personal data MUST be in the EEA or the UK, or in a country with an adequacy decision (Art. 45; for the UK, UK adequacy regulations) recognized by both regimes when both apply.
- **GDPR-TRF-02.** Any transfer to a country without adequacy MUST use one of these tools, recorded in the transfer register:
  - EU: the European Commission Standard Contractual Clauses (Commission Implementing Decision (EU) 2021/914), in the correct module;
  - UK: the ICO International Data Transfer Agreement, or the International Data Transfer Addendum to the EU SCCs;
  - for the United States: certification of the recipient under the EU-US Data Privacy Framework and, for UK data, the UK Extension, verified on the official list before the transfer and at every processor review.
- **GDPR-TRF-03.** A transfer based on SCCs, the IDTA or the Addendum MUST have a transfer impact assessment (for UK data, a transfer risk assessment) recorded before the transfer, assessing the destination's laws and the supplementary measures applied. ALE and JWE, where the provider cannot access keys, are the preferred supplementary measures.
- **GDPR-TRF-04.** Art. 49 derogations (explicit consent, contract necessity) MUST NOT be used for regular or repeated transfers.
- **GDPR-TRF-05.** The transfer register records, for each transfer: the recipient, destination country, data categories, tool, date of the assessment and date of the next review. It is part of the consolidated record (GDPR-ROPA-04).

## 17. Data protection impact assessment

- **GDPR-DPIA-01.** A DPIA (Art. 35) MUST be completed and approved before implementation of any feature that meets any of these conditions:
  - systematic and extensive evaluation of people, including profiling, that produces legal or similarly significant effects;
  - large-scale processing of special category, criminal offence or financial data;
  - systematic monitoring of people, including tracking of location or behavior;
  - use of new technology, including any LLM or AI system that processes personal data (section 18);
  - processing of children's data, or of other vulnerable people;
  - matching or combining datasets from different sources;
  - processing that prevents people from using a service or entering a contract;
  - any item on the published DPIA lists of the ICO or of the relevant EU supervisory authority;
  - two or more of the criteria of the EDPB guidelines on DPIA (WP248 rev.01).
- **GDPR-DPIA-02.** When a feature does not require a DPIA, the feature document MUST record a screening that states why none of the conditions apply.
- **GDPR-DPIA-03.** A DPIA MUST contain: a systematic description of the processing and its purposes; an assessment of necessity and proportionality; an assessment of risks to people's rights and freedoms; and the measures that address those risks, with rule identifiers from this document and `docs/SECURITY.md`. It is stored in the feature document next to the STRIDE threat model (SEC-GOV-08).
- **GDPR-DPIA-04.** When the residual risk remains high after measures, the feature MUST NOT be implemented until the controller has consulted the supervisory authority (Art. 36) and followed its advice.
- **GDPR-DPIA-05.** A DPIA MUST be reviewed whenever the processing it covers changes, and at least every 12 months.

## 18. LLM layer, ingested third-party text and the AI Act

This section applies to every call to a large language model or other AI system, whether hosted by a provider or run by the project, and to every component that ingests text from third parties. It is applied together with SEC-LLM-01 to SEC-LLM-08.

### 18.1 Personal data sent to a model

- **GDPR-AI-01.** Restricted data MUST NOT be sent to a model (SEC-LLM-04).
- **GDPR-AI-02.** Confidential personal data MAY be sent to a model only when all of the following are recorded in the feature document and approved:
  - a specific purpose and lawful basis for this processing (section 7);
  - a DPIA (GDPR-DPIA-01);
  - the provider is a processor with a DPA in force (section 15) that contractually excludes retention beyond the request and use for training (SEC-LLM-05);
  - processing and any retention stay in the UK, EEA or an adequate country, or are covered by a transfer tool (section 16);
  - the privacy notice describes the processing (GDPR-TRN-02).
- **GDPR-AI-03.** Prompts MUST contain only the personal data the purpose requires. Retrieval for prompts applies the caller's authorization (SEC-LLM-07), so a prompt never contains another user's personal data.
- **GDPR-AI-04.** Before any text is sent to a model, `IPersonalDataRedactor` MUST remove or replace personal data that the purpose does not require, including identifiers, contact details and special category indications. Where re-identification of the output is needed, placeholders are mapped back only inside the backend after the response is validated.
- **GDPR-AI-05.** Model outputs that contain personal data are personal data. They MUST be listed in the data inventory, classified, retained under section 12, and covered by the exporter and eraser of the feature.
- **GDPR-AI-06.** Personal data MUST NOT be used to train, fine-tune or evaluate a model unless that specific use has its own purpose, lawful basis, DPIA and notice. Consent-based training requires a separate consent (GDPR-CON-01).

### 18.2 Ingested third-party text

- **GDPR-AI-07.** Text ingested from third parties (feeds, web pages, documents, API responses) may contain personal data of people who are not users. The feature MUST record in its data inventory whether ingested text can contain personal data and, if so, the purpose, lawful basis and Art. 14 handling (GDPR-TRN-05).
- **GDPR-AI-08.** Ingested text MUST be kept only as long as its purpose needs, within section 12. Personal data in ingested text MUST NOT be extracted, indexed by person, profiled or combined with other data about that person unless that is the recorded purpose and a DPIA covers it.
- **GDPR-AI-09.** Ingested text MUST be treated as untrusted (SEC-LLM-01). Instructions inside it MUST NOT be able to cause personal data of users to be sent anywhere.

### 18.3 Automated decisions

- **GDPR-AI-10.** Decisions based solely on automated processing, including profiling, that produce legal effects or similarly significant effects on a person (Art. 22) MUST NOT be made unless the feature records one of the Art. 22(2) exceptions, a DPIA, and safeguards: meaningful information about the logic, the right to obtain human review by a person with authority to change the decision, the right to express a view and to contest the decision. For the UK, the equivalent safeguards of the DPA 2018 as amended apply.
- **GDPR-AI-11.** Automated decisions MUST NOT be based on special category data unless Art. 22(4) is met.

### 18.4 AI Act transparency touchpoints

- **GDPR-AI-12.** When users interact directly with an AI system (for example a chat assistant), they MUST be told clearly, at the latest at their first interaction, that they are interacting with an AI system (AI Act Art. 50(1)), unless this is obvious from the context.
- **GDPR-AI-13.** AI-generated or AI-manipulated text, images, audio or video presented to users MUST be marked as AI generated in the interface. Where the project provides the generating system, outputs MUST also carry a machine-readable marking where the AI Act Art. 50(2) requires it.
- **GDPR-AI-14.** The project MUST NOT use AI systems for any practice prohibited by AI Act Art. 5, including manipulative or deceptive techniques that distort behavior, exploitation of vulnerabilities, social scoring and emotion recognition. When a use case could fall under a high-risk category of AI Act Annex III (for example creditworthiness assessment), the feature MUST NOT be implemented until the controller records a classification assessment.
- **GDPR-AI-15.** Prompts, tool calls and outputs logged for security review (SEC-LLM-08) MUST pass through `IPersonalDataRedactor` and follow section 14.

## 19. Personal data breaches

A personal data breach is a breach of security leading to the accidental or unlawful destruction, loss, alteration, unauthorized disclosure of, or access to, personal data (Art. 4(12)). It includes loss of availability, and breaches at processors.

- **GDPR-BRE-01.** Every security event and incident (SEC-OPS-01, SEC-OPS-04) MUST be assessed for whether personal data was affected. The assessment records the data categories, the number of people and records, and the likely consequences.
- **GDPR-BRE-02.** The incident response plan (SEC-OPS-06) MUST include the breach assessment, notification decision and communication steps of this section, with named roles and contact details for the supervisory authorities.
- **GDPR-BRE-03.** A breach MUST be notified to the competent supervisory authority without undue delay and, where feasible, within 72 hours of becoming aware of it (Art. 33), unless it is unlikely to result in a risk to people's rights and freedoms. For people in the UK this is the ICO. For people in the EU this is the lead supervisory authority, or each authority concerned where there is no main establishment in the EU. When both regions are affected, both are notified.
- **GDPR-BRE-04.** The notification MUST contain the Art. 33(3) information: the nature of the breach, categories and approximate numbers of people and records, contact details of the controller, the likely consequences and the measures taken or proposed. Information not yet available is provided in phases without further undue delay, and a late notification states the reasons for the delay.
- **GDPR-BRE-05.** When a breach is likely to result in a high risk to people, the affected people MUST be informed without undue delay in clear language (Art. 34), with the nature of the breach, the likely consequences, the measures taken and what they can do to protect themselves. Informing them is not required only where Art. 34(3) applies (for example, the data was encrypted under ALE and the keys were not compromised), and that reason is recorded.
- **GDPR-BRE-06.** Processors MUST notify the controller of a breach within 24 hours of becoming aware of it (GDPR-PRC-03). The controller's 72-hour period starts when the controller is informed.
- **GDPR-BRE-07.** Every breach, including breaches that were not notified, MUST be recorded in a breach register with the facts, effects, decision on notification and its reason, and remedial actions (Art. 33(5)). The register is kept for 5 years after the breach is closed.
- **GDPR-BRE-08.** The breach runbooks MUST be exercised as part of the incident exercise (SEC-OPS-07) at least every 12 months, and the findings turned into completed changes.

## 20. Compliance checklist

A change that collects, stores, derives, logs, caches, sends or displays personal data, adds a third-party service or SDK, or touches ingested third-party text or any LLM layer is complete only when every applicable line below is true and the affected rule identifiers are listed in the pull request.

- [ ] The binding rules of section 3 are met (GDPR-CORE-01 to GDPR-CORE-18).
- [ ] The change has been approved by the privacy reviewer, who is not its author; roles, representatives and joint arrangements are recorded (GDPR-ROLE-01 to GDPR-ROLE-07).
- [ ] Every personal data item is identified and classified, special category data is absent or covered by an Art. 9 condition and DPIA (GDPR-CLS-01 to GDPR-CLS-07).
- [ ] The data inventory is updated in the same change, and the privacy notice, Apple privacy manifest and Play Data safety form match it (GDPR-ROPA-01 to GDPR-ROPA-06).
- [ ] Each purpose is specific and has one recorded lawful basis, with an LIA or compatibility test where required (GDPR-LAW-01 to GDPR-LAW-07).
- [ ] Consent is granular, unbundled, as easy to refuse and withdraw as to give, recorded as proof, and no non-essential device storage or SDK starts before it; children's rules are applied (GDPR-CON-01 to GDPR-CON-13).
- [ ] The privacy notice and just-in-time notices describe the change (GDPR-TRN-01 to GDPR-TRN-05).
- [ ] Defaults are privacy protective, DTOs are minimal, pseudonymization is applied where identity is not needed, test data is synthetic, and privacy components are used through their ports with doc comments stating rule identifiers and SOLID principles (GDPR-PBD-01 to GDPR-PBD-11).
- [ ] The feature's exporter and eraser cover every new item, and the export and erasure tests pass (GDPR-DSR-01 to GDPR-DSR-13).
- [ ] Every item has a retention period and trigger within the maximums, enforced by an automated job, and backups are covered by crypto-shredding or age-out (GDPR-RET-01 to GDPR-RET-10).
- [ ] Security controls match the data class (GDPR-SEC-01 to GDPR-SEC-04).
- [ ] Logs, traces, metrics, analytics and crash reports contain no clear personal data; identifiers are pseudonymized; IP addresses are truncated (GDPR-LOG-01 to GDPR-LOG-08).
- [ ] Every processor or SDK is reviewed, has a DPA in force, is in the processor register and in the outbound allowlist (GDPR-PRC-01 to GDPR-PRC-08).
- [ ] Every transfer outside the UK or EEA has a recorded tool and assessment (GDPR-TRF-01 to GDPR-TRF-05).
- [ ] A DPIA is completed, or a screening records why none is required (GDPR-DPIA-01 to GDPR-DPIA-05).
- [ ] LLM and AI use sends no Restricted data, redacts what the purpose does not need, uses a non-retaining and non-training processor, handles ingested third-party text, respects Art. 22, and meets the AI Act transparency rules (GDPR-AI-01 to GDPR-AI-15).
- [ ] Breach assessment and notification steps cover the change's data (GDPR-BRE-01 to GDPR-BRE-08).
- [ ] The `docs/SECURITY.md` section 24 checklist passes.
- [ ] Test coverage is at least 90 percent, including the tests required by GDPR-DSR-13.
- [ ] The feature document under `docs/features/` is created or updated.
