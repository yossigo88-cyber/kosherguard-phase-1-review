# KosherGuard: Phase 1 assessment and decisions

**Prepared November 16, 2026, America/New_York. Phase 1 only.**

This assessment covers the full 20-part assignment. No implementation, repository edits, database changes, enrollment, deployment, purchases or website replacement were performed. Answering the questions below will not authorize building. A separate final build authorization is still required.

## Bottom line

An independently operated MDM and filtering platform is a workable engineering direction, but the proposed family-owned-iPhone service has an unresolved Apple eligibility problem. Software cannot remove that gate. Non-removable ADE enrollment and blocking Settings erasure are supported for eligible supervised devices. Apple Configurator's 30-day provisional release right conflicts with immediate Day One protection for newly added devices. Computer restoration must remain a recovery path, followed by required ADE enrollment only while the device remains correctly assigned and Apple activation/enrollment services are reachable. No promise that a phone can never be erased is appropriate.

The existing repository contains useful real DNS/policy/dashboard work, not a complete iPhone management service. Retain its tested concepts where sound, finish independent review, and build the missing platform around a clearly separated control plane, device-management plane and filtering plane. Do not market the present DNS engine as full-URL filtering or the staging dashboard as operational MDM.

The immediate owner decisions are consolidated in the single numbered list near the end. The earlier sections explain the recommendations so those questions can be answered without technical guesswork.

## 1. What was actually inspected

### Repository and history

Live public clone and remote branch listing of https://github.com/yossigo88-cyber/kosherguard-platform established:

- `main`: `5f75d213a5ef9a19e811cfe4f56c44acc9f06f2a`.
- `stage-9b-deployment-preparation`: `9eaa2213ea357bcd7a7795a23063b8d9e283d771`.
- No `stage-11c-r-hardening` branch was advertised by the remote at inspection.
- Inspected the full tracked-file inventory, recent history, root/component READMEs, charter, MDM preparation, technical investigation, deployment runbook/checklist, Stage 10/10.5/11 plans and review records. Inspected actual schema/authentication/API/publication/DNS/policy/category/generation-loader code, frontend structure, test organization, backup/restore scripts and deployment examples. This is an architecture/security-readiness review, not a line-by-line independent verification of every algorithm or a penetration test.
- No repository program or test suite was executed in this Phase 1 review. Historical test counts below are attributed to committed reports, not new test results.

### Current public surfaces

- https://kosherguard.xyz/ loads a family phone-filtering homepage with setup/filter/change-on-request explanations, contact information and Privacy/Terms links. Its appearance was inspected. It is not the repository's admin application.
- https://kosherguard.xyz/privacy says allowed sites are not stored, blocked names are kept only briefly, and browsing history is not retained. This matters: detailed proxy/DNS logging must not silently contradict that promise.
- https://kosherguard.xyz/terms describes families managing their own phones or their children's phones. That is material to the Apple eligibility review.
- https://129-158-203-53.sslip.io/ loads the KosherGuard Admin sign-in screen and says it is a development environment with no devices connected. The screen was visually inspected. This does not prove the running commit, effective firewall, installed backup jobs or all server configuration.
- https://kosherguard-staging-dashboard.onrender.com/ currently says the service was suspended by its owner. It was not treated as deleted.

### Deployment evidence and limits

The committed Stage 10.5 report describes a separate Oracle staging VM, Caddy TLS termination, persistent SQLite and systemd, originally deployed from `02e8ecb54018cacb683a60107fca8b1e3a8dd0fb`, without Oracle auto-deployment. That is historical deployment evidence, not a current OS inventory. The current README's blanket 'nothing deployed' wording is inconsistent with that report and the reachable staging page. Documentation must distinguish staging from production.

Oracle sign-in accepted the saved login but required an existing security key or the user's phone authenticator. No usable email method or saved authenticator setup was available. No security settings were changed. Consequently current console inventory, billing, region/shape, running SHA, firewall, production source, backups and DNS ownership were not independently verified. Neither live production source nor its database was obtained. No server shell, deployment or configuration change was attempted.

## 2. Existing versus missing

| Area | Evidence in current feature branch | Recommended treatment |
|---|---|---|
| Domain/category policy evaluator | Real allow/block/category decisions and validation; explicit domain allow precedes domain block, then category block, category allow, default | Retain as a reviewed library; deliberately extend precedence for organization locks and delegated customer changes |
| DNS server | UDP/TCP, upstream handling, bounds, JSON decision logs, atomic last-known-good reload | Retain as a supplemental filtering component; benchmark and harden before exposing it |
| Multi-policy resolution | Stage 11C immutable engine sets and IPv4/IPv6 group bindings; credential path reserved and empty | Retain concepts; never equate network/IP binding with authenticated phone identity |
| Publication | Immutable revisions and rollback for existing single-policy publication | Multi-policy dashboard publishing is not built; complete integration before claiming per-device live publication |
| Admin dashboard | Real Python standard-library HTTP API, SQLite, vanilla JS, policies/categories/domains, administrative device registry, audit log | Useful development tool, not the final multi-tenant commercial backend |
| Customer accounts/RBAC | Schema has administrators and global objects; no tenant/customer ownership boundaries | Build tenant-scoped model and authorization before customer access |
| Device registry | Administrative entries and network bindings, not enrolled iPhones; no issued device tokens | Migrate safely into explicit device/group/enrollment identities |
| Apple MDM/ADE/APNs | Preparation documents; no working enrollment/push implementation | Build only after eligibility and credential model are settled; prove on approved spare devices |
| Proxy/PAC/TLS inspection | No production proxy engine or iPhone PAC delivery implementation identified | New isolated filtering-plane work and compatibility testing |
| Native iOS filter/URL filter | Not implemented | New signed iOS app/extensions, entitlement and distribution work |
| Category catalogue | Seed labels/test data, not a complete commercial classification corpus | Licensed ingestion, provenance, review and update pipeline |
| Schedules/apps | No complete scheduling, managed-app deployment or app-policy plane | New backend/device integrations |
| Website | Existing live site is separate; new public/customer website absent from this branch | New website integrated with the new backend; no cutover yet |
| Operations | Backup/restore scripts, Caddy/systemd examples, reports | Extend to off-site encrypted recovery, secrets, monitoring, queue/database restore and rehearsed failover |

Committed Stage 11C material reports 316 engine tests, 276 dashboard tests and 22 Node UI tests in the preceding dashboard stage. These are repository-reported results, not proof of iPhone behavior and not rerun here. Stage 11C says 'ready for independent review', not approved. The DNS engine's source-address grouping cannot isolate two phones sharing a NAT address, and mobile addresses change. The reserved credential map currently does not solve that.

## 3. Apple-supported protection and hard limits

### Eligibility comes before implementation claims

**Current Apple naming:** Apple announced that Apple Business replaced the standalone Apple Business Manager, Business Essentials and Business Connect services from April 14, 2026. This report preserves your ABM/ADE terminology for the enrollment functions, but new setup must follow the current Apple Business interface, APIs and terms. Apple Business includes built-in management; that is not a substitute for your requirement to operate our own MDM server. Its free availability does not establish eligibility for a family-owned-device service. [A1]

Three separate matters must not be combined:

1. **Organization/device eligibility for ABM/ADE.** The current Apple Business Terms of Service, version **LYL252, September 9, 2026**, Attachment 2 section 1 'Authorized Devices', printed page 22, says personally owned devices are not permitted in supervised device management as part of Managed Services 'unless otherwise agreed by Apple in writing'. Owning the filtering company or having parental consent does not by itself establish that exception. [A1]
2. **Authority to offer third-party MDM.** Apple Developer Program License Agreement **LYL262, October 29, 2026**, Attachment 10, section 1.1, printed page 87, permits in-house employee/test use or, 'if You are selected by Apple as a third-party MDM developer', service to MDM customers. Section 3.1 on that page separately excludes 'non-commercial, personal use, unless otherwise expressly permitted in writing by Apple'. Definitions on page 9 exclude consumers/personal users from MDM customers absent Apple's written permission. [A2]
3. **App capabilities/distribution.** Apple's MDM capability review recognizes limited parental-control use. It does not by itself establish ABM device eligibility, grant third-party MDM selection or waive the preceding agreements. Network Extension and URL-filter distribution have their own capabilities and review steps. [A3, A4, A11]

These are express published conditions. Whether Apple will approve KosherGuard's exact business/device model is unresolved; this report is not an Apple eligibility ruling or legal advice. Do not assume a prior 'Not Qualified' state has been resolved, or infer current approval from repository documentation. Do not substitute a third-party certificate or nominal ownership arrangement to evade these requirements.

### Day One non-removable enrollment

Apple documents ADE 'Prevent unenrollment': 'A supervised device can't be unenrolled by the user.' Set mandatory ADE enrollment, supervision and prevention of unenrollment in the enrollment configuration before activation; use AwaitDeviceConfigured to hold Setup Assistant while essential security profiles/app configuration are installed and checked, then release setup deliberately. Validate exact enrollment/profile schemas against the supported OS set. Do not use a manually installed removable MDM profile as a substitute. [A5, A12]

Configurator-added devices are different. Apple states there is a '30-day provisional period to release the device from Apple School Manager or Apple Business, supervision, and the device management service', starting after successful assignment and enrollment. Therefore a newly added Configurator phone cannot honestly be promised the same Day One protection when handed immediately to its user. The supported choices are eligible already-assigned reseller/Apple devices, or a controlled preparation period before protected handover where appropriate and eligible. Do not claim the timer starts at purchase or before successful assignment/enrollment. [A6]

### Settings erasure versus restoration

Apple's supervised restrictions include 'Erase All Content and Settings': 'Users can't erase their device and reset it to factory defaults.' Use the relevant Restrictions key (`allowEraseContentAndSettings = false`) after schema/version validation and test its actual UI effect. Do not promise every Reset submenu disappears, since other reset controls differ. [A7, A16]

Keep these paths separate:

- Finder on Mac and Apple Devices on Windows: supported computer restore paths. Host pairing/accessory restrictions and Find My/Activation Lock can affect the ordinary flow; design them consistently with the requested recovery path. [A8]
- Recovery mode: Apple's documented computer restore recovery path. A restriction controlling software-triggered recovery from an unpaired host is not a promise that physical recovery is impossible. [A7, A8]
- DFU: part of Apple's low-level device boot/recovery architecture. Do not treat it as management persistence inside the erased OS. Prove the post-restore activation/enrollment outcome on the exact supported models. [A9]

Expected design: restore erases local state, Apple activation recognizes the device's continuing assignment, Setup Assistant requires KosherGuard enrollment, baseline restrictions/filter settings are applied, and setup is released. This depends on eligibility, no release from ABM, assignment to the correct MDM, valid enrollment configuration, working credentials/endpoints and network access. Erasure is not a substitute for releasing a device from ABM. Activation Lock is separate and can stop legitimate recovery. Unsupported modified activation, exploits, backup surgery or untested downgrade assumptions are not part of this plan.

A powered-off/offline phone cannot receive immediate management changes. APNs acceptance is not command delivery, and command acknowledgment is not independent proof of continuing filtering coverage.

## 4. Recommended coordinated architecture

**Recommendation:** one owned platform with separate security and failure boundaries, not one large web process responsible for everything. Use established open-source protocol components where their licenses and review allow, while owning deployment, data, policies and product code. 'Our own MDM' need not mean rewriting every Apple protocol parser.

### Control plane

- Separate public website, customer portal and operator admin UI, on owned domain names with separate authentication contexts. Responsive sidebar admin design; every action maps to a real authorized backend capability. Hide or clearly disable unavailable capabilities.
- Production API under a maintained web framework, PostgreSQL, schema migrations and explicit tenant ownership. Retain/reuse validated Python policy logic after review; migrate data through reversible tools. Do not simply expose the current standard-library development HTTP server as the complete commercial service.
- Tenant/customer, user, role, group, device, enrollment, policy, version, exception, schedule, category source, app, command/result and audit records. Scope unique constraints, API queries, background jobs, cache keys and exports to the tenant. Include automated cross-tenant denial tests, not only UI separation.
- Desired state, published state, device-reported state and observed filter behavior are distinct. Store versions and timestamps for each; never label an old 'last seen' value 'online'.
- Transactional outbox and durable worker queue. A PostgreSQL-backed queue is a reasonable starting choice to avoid an unnecessary managed dependency; leasing, crash recovery, deduplication and replay rules still need implementation.

### MDM/ADE plane

- Evaluate self-hosted **NanoMDM + NanoDEP** as MIT-licensed building blocks. Their own descriptions say NanoMDM is one component, not the full service; add SCEP/device identity, TLS termination, enrollment authentication, tenant mapping, policy orchestration, certificate lifecycle and dashboard integration. Pin and audit chosen versions rather than assume their presence delivers the product. [O1]
- Separate ABM/ADE organization/server tokens from APNs push certificates/topics and keys. ADE token: assignment/profile synchronization. APNs MDM push: wake a previously enrolled device. Neither replaces a device identity certificate or app-signing certificate.
- Enrollment: authenticated short-lived, single-use enrollment tickets bound to expected tenant/device assignment; validate identity certificate and approved enrollment record. Distinguish a serial-number inventory lookup from proof of possession. Use supported device attestation where applicable, with a defined fallback instead of inventing universal attestation.
- Check-in service validates TLS/client identity and enrollment lifecycle; persist Authenticate/TokenUpdate/check-out safely. MDM push after valid TokenUpdate. Keep APNs topic/certificate mapping tenant/organization-correct. [A12]
- Command lifecycle: authorized intent -> queued -> push requested/accepted or failed -> device check-in -> command served -> Acknowledged/Error/NotNow -> verification query where applicable. Add CommandUUID, attempt history, deadlines, backoff, retry after unlock/reconnect, dead-letter/operator review and command-specific idempotency. Never blindly retry a destructive command.
- DeviceInformation, profile installation/removal, restrictions, supported app inventory/installation, lock/restart and later declarative state: per-OS/supervision capability matrix. Apps and Books licensing/distribution is a separate integration where required. Unsupported requests must fail explicitly.
- Restrict remote erase, release from ABM, removal of protection and sensitive certificate operations to tightly controlled workflows. The requested recovery requirement is not permission to erase real devices during development.

### Filtering plane

- **Proxy/PAC:** self-hosted authenticated proxy clusters and HTTPS-hosted stable PAC URLs. PAC chooses routing, not the final content verdict. Put most policy decisions on servers; maintain stable installed configuration and versioned policy fetches. PAC refresh/cache behavior is not an instant-update guarantee.
- Do not place long-lived secrets inside downloadable PAC JavaScript or its URL. Authenticate proxy requests/device sessions through a supported tested method; evaluate per-device credentials/certificates and necessary reauthentication. Never rely on source IP alone, including shared cellular/NAT addresses.
- No implicit `DIRECT` fallback for filtered traffic. Treat captive portal/Apple service bypasses as narrow, audited exceptions and distinguish bypass of TLS decryption from bypass of proxy routing/filtering entirely.
- **DNS:** private/authenticated DoH/DoT front end plus the reviewed DNS decision library, capacity limits, safe upstream resolution and no open recursive resolver. Authenticated device or trusted tunnel identity selects policy; network bindings are group hints only.
- **Native iOS app:** plan a managed, non-deletable filter app using supported Network Extension content filtering where approved. `NEFilterDataProvider` evaluates flows under its sandbox; `NEFilterControlProvider` updates rule data. This is distinct from the iOS 26 URL-filter control provider. Coverage and payload coexistence must be validated on real devices. [A10, A11]
- **iOS 26 URL filter:** recommended owned full-URL layer if OS coverage and Apple approval permit. It uses `NEURLFilterManager`, `NEURLFilterControlProvider`, an on-device Bloom prefilter, our PIR database, Privacy Pass issuer and our OHTTP gateway with Apple's OHTTP relay. Apple says relay configuration approval is needed before distribution-signed builds, including TestFlight; development-signed builds are exempt. Native-filter MDM configuration requires supervision. [A11]
- One policy model compiles into the capabilities each plane actually supports. Do not silently translate a URL-path rule into a whole-domain block. Policy version/hash, compile errors, delivery/activation confirmation and rollback are first-class records.

### Operations and secrets

- Separate staging/production and control/traffic-serving privileges. Prefer a small number of maintainable services, not a large microservice fleet for a small launch.
- Envelope-encrypted credential storage with access-controlled signing/decryption services, audited operations, rotation and separately protected encrypted recovery. Where Apple agreements impose customer key-custody constraints, resolve the permitted model before generating or hosting customer keys. Attachment 10 section 4.2 specifically restricts generating/accessing the MDM customer's private key; a universal shared certificate is not an assumed solution. [A2]
- Record certificate/token owners, purpose, topic/organization, expiry and renewal procedure, with early alerts and a tested renewal runbook. Apple says external device-management service tokens expire after one year and require replacement; renew them independently of the MDM push certificate. [A15] Do not collect secrets in chat or put them in GitHub, screenshots, frontend bundles, ordinary logs or support exports.
- PostgreSQL recovery/PITR where justified, encrypted off-site backups, configuration/artifact backups and protected key recovery. Test restores, not just backup creation. Protect against backup deletion by a compromised application account.
- Metrics: check-in/command age, APNs errors, policy convergence, enrollment failures, filter error rates/latency, proxy/DNS saturation, certificate expiry, backup freshness/restore success. Alerts and an on-call owner are required.

## 5. Filtering coverage: promises and gaps

| Layer | Useful coverage | What it does not establish |
|---|---|---|
| DNS | Domain/category blocking, allow/block modes | URL paths, image/page classification, app identity, traffic using other DNS or direct IPs |
| Supervised Global HTTP Proxy | Apple says most device web traffic across Wi-Fi/cellular; PAC or manual proxy | All network traffic. Apple explicitly notes some apps such as FaceTime do not use HTTP and bypass HTTP proxy |
| HTTPS CONNECT without decryption | Destination host-based policy where available | Encrypted URL path, page contents or images |
| TLS interception | More URL/content visibility for compatible consenting traffic | Pinned/custom trust apps, Apple service compatibility, universal QUIC/non-HTTP coverage; creates major privacy/security responsibility |
| Native flow content filter | Supported browser/socket flow verdicts with server-updated rules | Automatic plaintext visibility into arbitrary encrypted app traffic or image rewriting |
| iOS 26 URL filter | Full-URL requests using WebKit/URLSession, without exposing URL histories to our app/server | Automatic coverage of custom stacks; Apple says those apps must voluntarily use the participation API. Not content/image scanning |
| App restrictions | Block/allow supported bundle IDs, install/remove management where supported | Inspecting all content inside an allowed encrypted app |

Apple's Global HTTP Proxy documentation exposes separate switches for direct connection if PAC is unreachable and captive-network bypass. Both are security/product decisions, not harmless defaults. Apple's content-filtering guide also says Apple services stop connections using HTTPS interception and require decryption exemptions for listed Apple hosts. Pinned applications can also fail under inspection. [A10, A13]

**Important DNS detail:** Apple's current `com.apple.dnsSettings.managed` documentation says, 'When installed from an MDM, the setting only applies to managed Wi-Fi networks' and 'When installed manually, this setting also applies to cellular networks.' The current deployment guide repeats the managed-Wi-Fi limitation. This is about that payload/install route, not a claim that DNS filtering on cellular is impossible. Evaluate supported DNS-proxy/app/tunnel alternatives and their OS/supervision/entitlements, and test cellular explicitly. Do not substitute a removable manual profile to hide a coverage gap. [A14]

**GenTech/Livigent evidence:** GenTech's own iOS help describes a PAC file sending traffic to proxy servers, captive-network PAC bypasses, reauthentication when IP changes and GenTech/Livigent CA installation. Its product page says GenTech Pro uses Livigent cloud content filtering. Livigent's support describes proxy/routing modes and content/image filtering. These are vendor descriptions, not independently verified performance claims or proof that all versions/platforms use the same design. The earlier supplied Process Monitor capture does not establish iPhone enforcement. We can use public interface ideas and supported Apple mechanisms, not proprietary databases/code or unproven reverse-engineered conclusions. [V1-V3]

Recommendation: make the iOS app/content-filter and iOS 26 URL-filter path an early approval-and-spare-device feasibility gate. Keep PAC/proxy as a documented compatibility/fallback layer and DNS as supplementary. If the launch must support older iOS and in-page image/content filtering, proxy inspection may be necessary for compatible traffic, with explicit consent and exceptions. It remains insufficient for arbitrary encrypted apps. No 'filters everything' claim is justified.

### Category data

Build a source registry with version, hash, license, attribution, redistribution rights, update cadence, imported categories and local overrides. Import into staging; validate counts/sample quality and apply atomically with rollback. Review false positives and uncategorized domains; do not treat every unknown site as safe by accident.

UT1 is a useful category-source candidate. Its fetched official page describes use with free/commercial tools but links a Creative Commons license without enough verified specificity here to approve every use/redistribution. It also says adult is the actively maintained category and acknowledges inconsistencies. Do not assume all categories have equal maintenance. The direct page retrieval during verification also returned a maintenance page, so availability is a practical risk. [D1]

StevenBlack's repository has an MIT license, but its upstream-source table includes a non-commercial-with-attribution source. Therefore the top-level software license alone is not enough to approve every bundled upstream dataset for a commercial service. It is primarily a host-blocking source, not a complete KosherGuard taxonomy. Audit upstream rights or exclude uncertain inputs. [D2]

No paid classification API or proprietary GenTech/Livigent feed is approved. Human policy/category governance is necessary even with free sources.

## 6. Security-readiness findings

Confirmed in the current inspected source, not claims of exploitation:

- No tenant/customer isolation or role model in the present schema. Existing authenticated endpoints operate on global policy/category/device objects. They cannot be safely repurposed as customer endpoints merely by hiding UI controls.
- Session bearer tokens are stored directly in SQLite (`dashboard/src/auth.py`, `db.py`). Prefer hashing session tokens in the new backend and protect database/backups regardless.
- Login limiting is an in-memory dictionary keyed by username and address. Varying those values creates new keys; it is not a distributed/account-wide abuse control and does not replace MFA. No administrator MFA workflow was identified.
- Password hashing is PBKDF2-HMAC-SHA256 at 200,000 iterations. Reassess against current production guidance/hardware and migrate hashes safely rather than claim this makes the app production-ready.
- DNS decision logging includes queried domains and reasons, including allowed decisions. Excluding client IPs does not make such logs automatically non-personal. Current source comments claiming 'No personal data' are too broad; retention/access/default logging must match the website's privacy promises.
- `render.yaml` still specifies free-plan auto-deploy from the working branch. The current Render service is suspended, but the file is not proof it will remain harmless after a future resume. Keep deployment approval separate from commits and remove/disable dangerous defaults only under build authorization.
- Backup/restore scripts protect file permissions and verify SQLite consistency, but their existence does not establish off-site backup scheduling, encryption, key recovery or current production recoverability.
- Generation manifests use SHA-256 integrity checks, not an independent trust/signature boundary against a party who can rewrite both manifest and content. Production publisher permissions and signing/trust boundaries need explicit design.
- No live attack scans, dependency-exploit validation, complete secret/history scan, server-configuration audit or multi-tenant penetration test were performed. No claim that the repository or production is vulnerability-free is made.

## 7. Costs and external requirements

These are **planning scenarios in USD, checked November 16, 2026**, not vendor quotes or approval to spend. DigitalOcean is an example to make a realistic budget visible, not a selected host. Region, tax, bandwidth, availability, category licensing and support change the total.

Verified example inputs: 2 GiB/1-vCPU basic VM $12/month, 4 GiB/2-vCPU $24/month, daily percentage-based VM backup 30%, Spaces standard storage $5/month, regional HTTP load balancer $12/month. Confirm exact resource compatibility, region and final prices before purchase. [C1-C3]

| Scenario | Arithmetic using those inputs | Limits |
|---|---|---|
| Existing free staging | Potentially $0 incremental if current Oracle resources remain eligible | Current bill not inspected; Oracle says idle Always Free compute can be reclaimed. Not a production reliability guarantee |
| Small controlled pilot, no HA | 2 x $12 VMs + $5 backup/object storage + $7.20 daily VM backups = **$36.20/month** | Shared DB/control resources, no HA; may be too small for TLS inspection/PIR; excludes extra traffic and services |
| More separated pilot | 3 x $24 VMs + $5 storage + $21.60 daily backups = **$98.60/month** | Still not a full redundant production design; benchmark proxy and database separately |
| Small redundant service budget example | 6 x $24 VMs + $12 HTTP LB + $5 storage + $43.20 daily backups = **$204.20/month** | Illustrative two app/MDM, two traffic, two DB nodes; database quorum/fencing, traffic failover, L4 needs and off-region recovery still need design. HTTP LB alone is not a universal proxy/DNS failover solution |

Proxy egress is a major cost driver. As a sizing example, 100 devices x 20 GB/month is 2,000 GB/month before overhead; 1,000 devices x 50 GB is 50,000 GB/month. These are assumptions for discussion, not measured KosherGuard usage. Different provider allowances and traffic products cannot be blindly pooled. TLS inspection and PIR increase CPU/memory requirements. Budget for capacity tests before selecting hardware.

Other expected items:

- Apple Developer Program: official US price **$99 per membership year**, approximately $8.25/month when budgeted annually; regional prices/possible eligible waivers differ. Membership is not MDM authorization. [C4]
- Existing domain renewal: actual registrar rate not inspected. Keep ownership; no domain purchase or DNS change now.
- ABM/ADE account access, organization verification and Apple approvals; vendor CSR capability, appropriate MDM push certificate/topic, ADE server token, device identity/SCEP CA, public HTTPS certificates and protected keys.
- iOS app signing/distribution and Network Extension/URL-filter capabilities; Apps and Books token/licenses if using managed app distribution.
- An approved spare iPhone and test cellular service; Mac/Xcode for native app builds and relevant Configurator work; Mac plus Windows for both computer restore workflows. Existing access/hardware unknown.
- Transactional account/support email, monitoring delivery, encrypted off-site storage, optional classification licensing, security/legal review, staffing and incident response. No provider/account/subscription selected or created.

Open-source self-hosting removes a paid MDM subscription, not Apple service dependence, cloud costs, maintenance or the obligation to support customers. Production should not be budgeted as free by default. [C5]

## 8. One consolidated numbered list of pre-build questions

Answer by number. 'Use your recommendation' is enough where there is a recommendation. Items marked **Technical** do not require you to select an implementation library; they record the decision I can make within approved scope. They are included to distinguish engineering work from choices only you can make. No item authorizes implementation by itself.

### Business model, eligibility and intake

1. **Who is the initial service actually for, and who owns the phones: families using personally owned phones, an organization using its own devices, or both as explicitly separate products?** Why: Apple agreements treat these cases differently. Recommendation: describe the intended family model honestly and seek Apple's written authorization for that exact model; do not relabel household devices as enterprise assets. **Owner: You.**

2. **What Apple decisions or written exceptions do you already have, if any, covering the organization, commercial MDM and personally owned/family devices?** Why: these are separate prerequisites; repository notes do not prove current status. Recommendation: supply only non-secret status/approval evidence first; treat missing approval as unresolved. Credentials come later through secure setup, not chat. **Owner: You; technical review can map each approval to its requirement.**

3. **If Apple declines the family ADE/MDM model, should the project pause at the eligibility gate or launch only an eligible organization-device product while family approval remains unresolved?** Why: an ordinary family filter app cannot meet the unchanged ADE Day One requirement. Recommendation: keep that requirement intact and pause the affected consumer launch; do not quietly downgrade to removable enrollment. No paid third-party MDM fallback is assumed. **Owner: You.**

4. **Will intake be limited to eligible Apple/reseller-assigned devices, or must it also accept existing phones through Configurator? If the latter, can they remain in controlled preparation for the provisional period before protected handover?** Why: immediate Configurator handover conflicts with Day One protection. Recommendation: launch with eligible already-assigned devices; offer Configurator only with a clearly disclosed, tested preparation process and appropriate eligibility. **Owner: You.**

5. **Which countries, customer age groups and languages are in the first launch?** Why: privacy/child consent, distribution, availability, support and data location differ. Recommendation: a defined US English pilot before expanding; do not collect children's names or birth dates unless needed. **Owner: You.**

6. **How many customers/devices are expected at pilot, first production release and 12 months, and where are they located?** Why: proxy/PIR bandwidth, concurrent commands and support costs scale differently. Recommendation: agree explicit device and traffic limits for the pilot, then measure before increasing them. **Owner: You; technical sizing follows.**

### Existing work, migration and access

7. **When you say fresh build, may we retain independently reviewed components of the current `kosherguard-platform` branch, or do you want even that current work replaced?** Why: this branch has real policy/DNS/dashboard code, while the older abandoned project remains excluded. Recommendation: preserve history and reuse only reviewed current-project components that fit; redesign missing platform boundaries. Work remains off `main` in an isolated branch specified in the final authorization. **Owner: You.**

8. **Is first launch for new enrollments only, or must existing customers/devices/accounts be migrated as well?** Why: live source/data are separate, and enrollment migration can involve re-enrollment, device erasure or newer OS migration features. Recommendation: prove a new-device pilot first; plan existing-customer migration separately with backups, consent and device-specific compatibility. No automatic old-project code import. **Owner: You.**

9. **Who can provide read-only access to the current production source/configuration, deployment inventory, DNS and backup evidence, and can the existing Oracle verification be completed when needed?** Why: the public pages/repository do not reveal the live backend; console inspection stopped at existing MFA. Recommendation: verify the current environment before promising reuse or a cutover date. Grant only necessary read access; no passwords, private keys or tokens in chat. **Owner: You for access; technical inventory is mine.**

10. **What customer information and historical records must be preserved during eventual migration, and what may be intentionally left behind?** Why: this sets migration/privacy scope and rollback requirements. Recommendation: only necessary customer/device/policy/account records, with an approved field-level mapping and no browsing-history import by default. **Owner: You.**

### Product policy and filtering

11. **Who may change filtering: only KosherGuard staff, parents/customer administrators, or both, and may customers ever weaken a KosherGuard-locked baseline?** Why: a self-service portal can defeat the intended protection if permissions are too broad. Recommendation: operator-controlled protected baseline; explicitly delegated customer controls and approval requests. **Owner: You.**

12. **What is the launch default policy and who approves category definitions and exceptions?** Why: 'inappropriate' is a product judgment, not something a programmer should invent. Recommendation: named approved policy templates, a designated policy owner and reviewed exceptions; no undocumented universal religious/content standard. **Owner: You.**

13. **For unknown/uncategorized sites, should each policy block pending review or allow unless a known rule blocks it?** Why: this decides safety versus false positives; it is separate from server-outage behavior. Recommendation: offer an explicit strict allowlist mode and a category-block mode, each clearly labeled; choose the launch default yourself. **Owner: You.**

14. **Does launch require only domain/category/URL blocking, or also live page-text, search-result and image/video filtering within otherwise allowed sites?** Why: URL filtering is not content classification; image/video inspection is a much larger product with compatibility and accuracy risks. Recommendation: define URL/domain/app restriction coverage first and list content/image analysis as a separate acceptance-gated capability if required. Do not advertise GenTech-equivalent image filtering without proof. **Owner: You.**

15. **What minimum iOS version and iPhone models must launch support?** Why: iOS 26 URL filtering and older proxy/extension options differ. Recommendation: iOS 26-or-later for the first supported full-URL product if the target fleet allows it; any older-device tier gets a separate honest coverage matrix. **Owner: You; technical support matrix is mine.**

16. **If approval or real-device tests prevent iOS 26 URL filtering at launch, should launch wait, or can a specifically labeled proxy/content-filter product launch if it passes the agreed coverage tests?** Why: capability approval cannot be guaranteed. Recommendation: gate release on required coverage, not a calendar date; no silent DNS-only substitution. **Owner: You.**

17. **Do you authorize designing an optional TLS-inspection mode, and for which users/traffic with what consent?** Why: installing a trusted root gives the service powerful access and can break pinned apps and Apple services. Recommendation: avoid blanket TLS interception; prefer native URL decisions and narrow inspection only where needed, lawful, disclosed and tested. Any future certificate installation still needs approved testing/rollout. **Owner: You.**

18. **Which apps/services must work at launch, and which may be blocked entirely if their encrypted content cannot be inspected?** Why: app allowlisting and content filtering are different; compatibility cannot be promised for every app. Recommendation: provide a must-work list including calls, messages, maps, banking, school/work apps, authentication, App Store and updates; test each with policy-specific exceptions. **Owner: You.**

19. **If filtering services fail, should filtered web traffic stop, and what narrowly defined essential access must remain?** Why: fail-open leaks content, fail-closed can strand a user. Recommendation: redundant services and last-known-good rules, no unrestricted automatic bypass; explicitly approve emergency/Apple activation/APNs/captive-portal recovery exceptions and test them. Do not assume voice/emergency connectivity is unaffected by every network configuration. **Owner: You.**

20. **Who may approve bypasses or temporary exceptions, for how long, and must changes be reviewed by a second person?** Why: a bypass is a security-sensitive policy change, including support work. Recommendation: time-limited scoped exceptions with reason/audit and automatic expiry; stronger approval for global or protection-removal changes. **Owner: You.**

21. **Which schedules are needed: school hours, bedtime, holidays, one-off exceptions, and whose timezone should apply while traveling?** Why: timezone/DST and device clock tampering can change enforcement. Recommendation: policy-owned timezone and server-authoritative versioned schedules with explicit offline behavior; do not infer a holiday calendar or religious schedule. **Owner: You for behavior; technical scheduling is mine.**

22. **How will category/content databases be maintained, and is a licensed commercial source an option if audited free sources do not meet accuracy needs?** Why: free source rights, quality and update frequency vary. Recommendation: audited permitted sources plus local review, with no paid feed unless separately approved. Identify the person/team deciding classification disputes. **Owner: You for budget/governance; source/license validation and import design are mine.**

23. **What policy-update speed is actually required for online devices, and how should stale/offline protection be shown?** Why: APNs, device lock state, PAC caching and Bloom refresh have different delays. Recommendation: measure targets separately, display desired/published/reported versions and last confirmation, and never promise an instant update or claim success from 'push sent'. **Owner: You for required service target; technical measurement/retry design is mine.**

### Device operation, recovery and authority

24. **What is the authorized removal/transfer process when a customer cancels, sells a phone, loses authority over it, or has an ownership dispute?** Why: non-removable management must still have a legitimate offboarding process; ABM release, MDM removal, data deletion and Activation Lock differ. Recommendation: verified owner approval, documented release workflow and audit; no automatic destructive action or permanent lock for nonpayment. **Owner: You.**

25. **Who may issue lock/restart, remove security profiles, erase a device or release it from ABM, and which actions require fresh verification or two-person approval?** Why: these can disrupt or destroy customer data. Recommendation: least privilege; high-impact operations disabled by default for support/customer roles, explicit confirmations and separate authorization for erasure. **Owner: You.**

26. **How should legitimate computer restore and backup recovery work for customers, including personal Apple Accounts, Find My/Activation Lock and eSIMs?** Why: recovery can erase data or strand activation even when ADE is correct. Recommendation: documented supported Finder/Apple Devices/recovery/DFU test matrix, owner-managed personal credentials and no backup modification or shared Apple Accounts. Decide what support promises can be made. **Owner: You for service behavior; technical procedure is mine after testing.**

27. **Which apps should KosherGuard deploy/manage, and will customers use their own App Store accounts, organization Apps and Books licenses, or an approved mixture?** Why: app installation, app ownership and silent management have different requirements. Recommendation: deploy the filter app through an approved managed distribution path; do not assume Enterprise distribution can serve unrelated consumer customers. **Owner: You for app catalogue; technical/license mapping is mine.**

28. **How will customer enrollment identity be verified: staff-assisted setup, invitation plus account authentication, or a self-service flow?** Why: a serial number or public signup alone should not let someone claim another customer's phone. Recommendation: staff-approved pilot enrollment, short-lived invitation, customer authentication and device/ABM assignment checks. **Owner: You for operating model; secure ticket/identity design is mine.**

### Accounts, privacy and commercial operations

29. **What roles are needed at launch, and who are the initial operators and recovery contacts?** Why: global admin access is too broad for customer/support users. Recommendation: owner/security administrator, limited support, customer administrator and read-only roles, with MFA for privileged access and documented break-glass recovery. No need to name unrelated organizations or private contacts. **Owner: You for people/authority; technical permission enforcement is mine.**

30. **What filtering/activity information may parents, customers and staff see, and for how long?** Why: current website promises no allowed-site browsing history; the DNS engine can log domain names. Recommendation: aggregate counters by default, short-lived redacted diagnostics only when justified, no full URLs/query strings/content bodies by default, audited support access and exact retention/deletion rules. **Owner: You, with privacy/legal review.**

31. **What device inventory is actually necessary beyond model, OS, supervision/enrollment, app-management status and policy state?** Why: location, personal app inventory and other identifiers may be sensitive and are not needed for every customer. Recommendation: collect the minimum needed; no location or expanded personal inventory by default. **Owner: You for product need; technical minimization is mine.**

32. **Who will approve the privacy policy, child/guardian consent, TLS-inspection disclosures, terms and data-processing arrangements before launch?** Why: technical consent screens are not a legal/privacy program. Recommendation: qualified review of the actual service/model/jurisdictions and clear identity/authority checks; preserve current commitments until deliberately replaced. **Owner: You.**

33. **Should registration be invitation-only initially, and is billing part of launch or handled outside the platform?** Why: open signup, account recovery, subscription billing/refunds and app-payment rules substantially change scope. Recommendation: invitation-only pilot, no stored payment-card data, and separately approve any payment provider/fees before implementation. **Owner: You.**

34. **Which customer-support channels and account-recovery methods should the new website offer?** Why: sign-in verification, policy requests, disputes and urgent connectivity failures need real handling. Recommendation: verified email/in-product requests, documented staff escalation and safe recovery that cannot be used to remove protection casually. Existing public contact information is already available; no need to ask you to repeat it. **Owner: You for service channels/hours; technical flows are mine.**

35. **What brand/design assets, languages and customer-facing pages must be retained, beyond the already specified new homepage/features/contact/support/enrollment/login?** Why: technical design can be selected without inventing business claims or legal copy. Recommendation: keep the domain and recognizable brand, build accessible responsive pages, and publish only tested capability statements. **Owner: You for assets/claims; interface design and accessibility are mine.**

### Budget, reliability, access and launch gates

36. **What monthly pilot budget, first-production budget and hard spending ceiling are acceptable, including traffic growth?** Why: a self-hosted commercial proxy/MDM service is not realistically guaranteed to run free. Recommendation: use the $36.20/$98.60/$204.20 scenarios only as sizing anchors, then approve an exact host/resource plan after measured traffic/PIR/TLS tests. No purchase is authorized by a budget discussion alone. **Owner: You.**

37. **What uptime/support expectation and acceptable recovery time/data-loss window do you want?** Why: always-on blocking makes outages more consequential, and recovery targets set database/backup/standby costs. Recommendation: define pilot expectations explicitly; for production consider a target of restoring service within an hour with database recovery points no more than 15 minutes apart, subject to funded architecture and restore tests. These are proposed targets, not existing guarantees. **Owner: You for service level/budget; technical design is mine.**

38. **Who owns daily operation, security incidents, Apple certificate/token renewal and urgent customer connectivity problems?** Why: certificates expire and someone must respond outside a development chat. Recommendation: two named authorized organization contacts with escalation and tested recovery, avoiding dependence on one person's account or phone. **Owner: You.**

39. **Do any contractual/customer requirements constrain hosting region, infrastructure provider or customer-controlled key custody?** Why: this changes database, logs, backups and certificate design. Recommendation: a documented US pilot data region unless your obligations require another; keep portable backups and no paid commitment until approved. **Owner: You for obligations; provider/capacity/security evaluation is mine.**

40. **Which spare test iPhones, cellular connections, Mac/Xcode access and Windows restore computer can be made available, and who can authorize their erase/restore tests?** Why: the non-removal/restore/filtering claims cannot be completed with simulated tests. Recommendation: no customer or daily-use phones; dedicated test devices with backed-up disposable test data and explicit permission for each destructive test plan. **Owner: You.**

41. **Is there a target pilot/launch date or contractual promise, and which exact coverage/compatibility tests must pass before accepting a paying customer?** Why: approvals and physical-device behavior cannot be guaranteed on a coding deadline. Recommendation: milestone-based launch gates, not a promise of a finished platform before Apple approval, independent security review and recovery/filter tests. **Owner: You for commitments; technical acceptance matrix is mine.**

42. **May I choose the technical defaults described here once the product decisions are settled: owned self-hosted MDM components, maintained API framework, PostgreSQL, durable queue/outbox, versioned policy compiler, signed native app and separated traffic services?** Why: you should not need to choose parser libraries, queue internals or database indexes. Recommendation: yes within the approved budget/security/license constraints; document exact choices in the final authorization plan and escalate only material scope/cost/coverage changes. **Owner: Technical, with your approval of constraints, not of every library.**

43. **May routine security/engineering choices follow the proposed defaults without more basic questions: tenant-scoped authorization, MFA for privileged access, hashed session tokens, encrypted secrets/backups, no full-URL logging by default, retries with honest status and last-known-good policy?** Why: these are necessary controls, while retention, permitted support visibility and fail-open behavior remain your decisions above. Recommendation: yes; use reviewed standards and tests, not custom cryptography. **Owner: Technical within your privacy/operations decisions.**

44. **What is the final approval sequence for test rollout, production deployment and website cutover after the build is reviewed?** Why: approval to develop must not silently authorize enrollment, spending, DNS changes or replacing the live service. Recommendation: separate gates for code build, approved spare-device tests, paid resources, limited pilot, migration and production/domain cutover; verified backups and rollback before each live change. **Owner: You.**

## 9. Proposed sequence after answers and separate authorization

This is planning, not implementation already started.

- Resolve business/device eligibility and required Apple permissions; define coverage, privacy, costs and supported OS/app matrix.
- Independently review current branch, address confirmed defects, preserve history, establish tenant-safe foundation and production security model.
- Prove the smallest real ADE flow on an approved spare device: mandatory supervision/prevent unenrollment, Settings erase restriction, certificate-authenticated check-in, APNs wake, queried/acknowledged restrictions, and successful post-computer-restore re-enrollment. Do not substitute a mock device to pass this gate.
- In parallel where authorized, prove filtering candidates on Wi-Fi and cellular: PAC/proxy, supported native content filter, iOS 26 URL filter/PIR/OHTTP, DNS and compatibility. Select the tested combination; document deliberate exclusions.
- Build complete customer/operator workflows, policy compilation/publication, schedules, app management and observability around those proven paths.
- Run cross-tenant authorization tests, security review, load/soak testing, network-loss/certificate-expiry tests and encrypted off-site restore rehearsal.
- Controlled customer pilot, then separately authorized migration and website cutover only after rollback and backend readiness are verified.

### Required evidence before 'finished'

Actual setup screenshots and enrollment records; supervision/non-removal/Settings erase checks; Finder, Windows Apple Devices, recovery and DFU outcome matrix; continued ABM assignment after each restoration; command UUID/results and verification queries; Wi-Fi/cellular and network-switch filter tests; custom-stack/pinned-app/Apple-service/captive portal coverage; unknown sites/schedules/offline policies; policy latency measurements; cross-tenant negative tests; session/admin recovery; backup restoration including keys and enrollments; monitored failure scenarios and documented limits. Simulation tests remain clearly separate. No customer devices are authorized for testing by this report.

## 10. Source notes

Official pages were read, not merely inferred from search snippets. Sources checked November 16, 2026. Agreement version/date is stated where visible; account-specific signed terms and Apple decisions still need verification. Static documentation can change.

- **A1** Current Apple Business Terms of Service, LYL252, September 9, 2026, Attachment 2 section 1, printed p.22: https://www.apple.com/legal/platform-services/apple-business/abli.pdf . Official service transition announcement: https://www.apple.com/newsroom/2026/03/introducing-apple-business-a-new-all-in-one-platform-for-businesses-of-all-sizes/ . Older ABM agreement also inspected for continuity, not treated as the current service terms: https://www.apple.com/legal/enterprise/apple-business-manager/abm-us.pdf
- **A2** Apple Developer Program License Agreement, LYL262, October 29, 2026, definitions printed p.9; Attachment 10 pp.87-88, particularly §§1.1, 3.1, 4.1-4.2: https://developer.apple.com/support/downloads/terms/apple-developer-program/Apple-Developer-Program-License-Agreement-English.pdf
- **A3** Apple, Requesting access to an MDM Vendor CSR Signing Certificate: https://developer.apple.com/help/account/certificates/mdm-vendor-csr-signing-certificate/
- **A4** Apple, Submitting a Capability Request for Apps Using MDM, September 6, 2019: https://developer.apple.com/news/?id=09062019a . Historical announcement, used only for the distinction between capability review and other permissions.
- **A5** Apple, Automated Device Enrollment and device management, dated September 17, 2026 in fetched source: https://support.apple.com/guide/deployment/automated-device-enrollment-management-dep73069dd57/web
- **A6** Apple, Add Apple devices to Apple School Manager or Apple Business: https://support.apple.com/guide/deployment/add-apple-devices-school-manager-business-depb5ae24c4a/web
- **A7** Apple, MDM restrictions for supervised Apple devices: https://support.apple.com/guide/deployment/restrictions-for-supervised-devices-dep6b5ae23e9/1/web/1.0
- **A8** Apple, Restore using a computer; recovery-mode restore: https://support.apple.com/en-us/118107 and https://support.apple.com/en-us/118106
- **A9** Apple Platform Security, Boot process for iPad and iPhone devices: https://support.apple.com/guide/security/boot-process-for-ipad-and-iphone-devices-secb3000f149/web
- **A10** Apple, Filter content for Apple devices: https://support.apple.com/guide/deployment/filter-content-dep1129ff8d2/web
- **A11** Apple, WWDC25 Filter and tunnel network traffic with NetworkExtension; URL filters reference: https://developer.apple.com/videos/play/wwdc2025/234/ and https://developer.apple.com/documentation/networkextension/url-filters.md
- **A12** Apple check-in and TokenUpdate protocol: https://developer.apple.com/documentation/devicemanagement/check-in.md and https://developer.apple.com/documentation/devicemanagement/token-update.md . InstallProfile/CertificateList responses: https://developer.apple.com/documentation/devicemanagement/install-profile-command.md and https://developer.apple.com/documentation/devicemanagement/certificate-list-command.md
- **A13** Apple, Global HTTP Proxy payload: https://support.apple.com/guide/deployment/global-http-proxy-payload-settings-dep7ba46fcd/web
- **A14** Apple DNSSettings payload reference and deployment guide: https://developer.apple.com/documentation/devicemanagement/dnssettings.md and https://support.apple.com/guide/deployment/dns-settings-payload-settings-dep86469ba99/web
- **A15** Apple, Link to an external device management service in Apple Business, token renewal: https://support.apple.com/guide/business/link-to-an-external-device-management-service-axm1c1be359d/web
- **A16** Apple device-management schema, exact supervised erase restriction key: https://github.com/apple/device-management/blob/release/mdm/profiles/com.apple.applicationaccess.yaml
- **V1** GenTech iOS help, PAC/proxy/captive networks/certificate statements: https://www.gentechsolution.com/ios-help/
- **V2** GenTech product explanation, Livigent licensing/content-filter claims: https://www.gentechsolution.com/how-it-works/ . Vendor claims, not independent tests.
- **V3** Livigent Q&A, proxy/routing and image-filter discussion: https://www.livigent.com/support/q-a/ . Vendor support material, some examples appear old; not a recommended modern deployment guide.
- **O1** NanoMDM and NanoDEP official repositories, MIT licenses and component scopes: https://github.com/micromdm/nanomdm/ and https://github.com/micromdm/nanodep
- **D1** UT1 official category source: https://dsi.ut-capitole.fr/blacklists/ . Licensing specificity/availability remain unresolved.
- **D2** StevenBlack hosts repository/upstream-source table and license: https://github.com/StevenBlack/hosts and https://github.com/StevenBlack/hosts/blob/master/license.txt
- **C1** DigitalOcean Droplet sizes, transfer allowances and backup percentages: https://www.digitalocean.com/pricing/droplets
- **C2** DigitalOcean Spaces base subscription: https://docs.digitalocean.com/products/spaces/details/pricing/
- **C3** DigitalOcean load balancer prices: https://docs.digitalocean.com/products/networking/load-balancers/details/pricing/
- **C4** Apple Developer Program enrollment/membership price: https://developer.apple.com/programs/enroll/
- **C5** Oracle Always Free idle-resource reclamation conditions: https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm
- **P1** Actual repository/source/history inspection at the SHAs listed above: https://github.com/yossigo88-cyber/kosherguard-platform . Internal paths cited in the report identify the inspected files; no inference that repository deployment examples equal current server state.
- **P2** Actual current website/privacy/terms/staging surfaces: https://kosherguard.xyz/ ; https://kosherguard.xyz/privacy ; https://kosherguard.xyz/terms ; https://129-158-203-53.sslip.io/ ; https://kosherguard-staging-dashboard.onrender.com/ . Reachability confirms those pages, not backend capability.

**Stopped at Phase 1. No build, enrollment, paid service, deployment or website replacement is authorized or claimed.**
