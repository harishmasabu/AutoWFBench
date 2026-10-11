# AutoWFBench simulation requirements

**Confirmed architecture direction:** simApp-first for every selected application, even where APIs exist. A simApp supplies the full simulated UI needed by the selected workflow, over authoritative local state. simMCP is a later optional optimization when the application has adequate MCP capabilities; unavailable or limited MCP leaves the UI primary. API/CLI findings below document semantics and feasibility, not permission to substitute an API-first benchmark. See [decisions and roadmap](../decisions-and-roadmap.md).

Design date: 11 October 2026. Requirements derive from the 24 workflows in Astra’s report and the [capability matrix](application-capability-matrix.md). They describe proposed work; nothing here is implemented by this task.

## Isolation and contract policy

Reuse official semantics, not live customer accounts. Every benchmark run receives local application state, synthetic identities, message recipients, credentials, documents and events. Customer replies, provider notifications, cleared payments, physical receipts and signatures are controlled inputs. An official SDK or CLI may be reused offline only after proving that all API, discovery, upload, auth and telemetry destinations can be contained. Otherwise represent its necessary contract with a small internal adapter, clearly identifying deviations.

Classify operations, not entire brands. **A** means a documented interface can be faithfully represented; **B** means an unbranded simulated API/CLI is needed; **C** means a selected operation must be performed through simulated UI; **D** combines API/CLI/UI with asynchronous or external state; **E** means eligibility or semantics remain unresolved. These historical classifications describe available contract evidence. They no longer choose the agent interface: simApp UI is the default for all 24 workflows. Optional simMCP can be evaluated later without removing UI coverage.

The current simApp response envelope, `POST /apps/{app}/{operation}`, is a valid internal contract. It is not Gmail REST, HubSpot REST, Shopify GraphQL or the Telegram Bot API. Publish separate interface-fidelity labels: domain behavior, selected vendor API contract, selected MCP contract, CLI syntax, and UI interaction. A passing generic CRM task cannot be counted as passing a faithful HubSpot integration task.

## Operation-level requirements by workflow

| Workflow | A documented subset | B internal or vendor-unselected subset | Primary simApp UI scope | D asynchronous/hybrid needs | E unresolved details |
|---|---|---|---|---|---|
| W01 | Acuity appointment read; Asana task; chosen CRM/email | Lead identity and next-action register | Inbox/lead record and booking state; reproduce selected workflow in simApp | Booking, reply, opt-out, follow-up deadlines | Exact chosen CRM and outreach authority |
| W02 | Acrobat Sign agreement/status/document read; email | Quote business terms and reminder policy | Quote/agreement review and follow-up; reproduce selected workflow in simApp | Signer decision, delayed processing, expiry, objections | Automatic expiry/reminder contract and account tier |
| W03 | Drive folders/ACL/copy; Docs text; chosen CRM/tasks | Readiness checklist and unspecified intake portal | Drive folder and onboarding checklist; reproduce selected workflow in simApp | Partial submissions, human confirmation, permissions | Form/board vendor and initial folder access |
| W04 | Calendly events/invitees/cancel/book; Calendar; email | Preparation checklist | Calendar and appointment preparation; reproduce selected workflow in simApp | Attendance/no-show input, reschedule/cancel races | Source of authoritative attendance and booking tier |
| W05 | Selected CRM, Forms response read/Sheets | Licensed listing fixture, lead source match | Lead register and sign-in results; reproduce selected workflow in simApp | Cross-channel reply and changed preferences | Property data license/vendor; no invented sourcing API |
| W06 | Calendar or Calendly availability/booking | Viewing constraints/property authority | Viewing calendar and feedback; reproduce selected workflow in simApp | Seller/buyer confirmation, feedback, slot conflict | Actual listing/viewing authorization |
| W07 | Gmail, Docs/Drive, verified Canva template/export | Version-specific approval ledger and content calendar | Content review and approval; reproduce selected workflow in simApp | Delayed/partial/revoked approval, asynchronous export | Canva precise edits; platform/account/media access |
| W08 | GA4 report, Sheets/Slides/Drive, email/Slack | Unselected advertising dataset, report input manifest | Reporting inputs and report view; reproduce selected workflow in simApp | Late metrics and corrected source exports | Advertising vendor, attribution and tool coverage |
| W09 | Forms response read, Sheets/Excel selected surface, Drive/Slack | Candidate/application identity; ATS if unselected | Candidate intake and reconciliation; reproduce selected workflow in simApp | Revised CV, missing consent, recruiter handoff | ATS API; sourcing permissions are separate |
| W10 | Sheets/email; scoped WhatsApp business subset after verification | Candidate next-action register | Candidate stage and follow-up; reproduce selected workflow in simApp | Withdrawal, cross-channel reply, recruiter collision | Migration/history/coexistence and current Meta restrictions |
| W11 | Calendar/Calendly; Docs/Drive; email/Slack | Approved shortlist/ATS feedback register | Interview scheduling and feedback; reproduce selected workflow in simApp | Rebooking, withdrawal, replaced interviewer, late feedback | ATS permissions and true attendance source |
| W12 | Gmail MIME attachment read; Drive blob/metadata/ACL | Classification exception register and duplicate matching | Attachment intake and Drive filing; reproduce selected workflow in simApp | Incoming attachments, revoked access, revision | OCR handling and password policy; no automatic decryption |
| W13 | Gmail/Drive/Sheets | Requirement checklist; unselected client portal | Document checklist and collection; reproduce selected workflow in simApp | Partial/wrong-period submissions, waiver, reminder race | Selected portal interface; who may waive requirements |
| W14 | Verified invoice/payment reader and email | Monetary ledger until finance vendor chosen | Invoice/payment reconciliation; reproduce selected workflow in simApp | Partial/cleared payment, dispute, credit, pending transfer | Bank clearance source; product-specific access |
| W15 | Chosen invoice API; Drive/Docs; Sheets/tasks | Approved time/milestone and billing calculation | Billing review and invoice draft; reproduce selected workflow in simApp | Corrections/approval, draft issuance only when authorized | Rates/tax settings provided as data; exact draft/send lifecycle |
| W16 | Gmail/Drive/Docs/task record | Scope/version/acceptance ledger | Deliverable review and handover; reproduce selected workflow in simApp | Partial acceptance and revisions | Whether signature is required; contractual acceptance policy |
| W17 | Calendar recurrence and selected messaging | Job/crew/skill/location schedule constraints | Dispatch board and recurring schedule; reproduce selected workflow in simApp | Crew/customer response, illness, recurring exceptions | Service-management product and travel-time source |
| W18 | Selected schedule/Sheets read | Time-entry correction/approval/export model | Time-entry review and export; reproduce selected workflow in simApp | Late correction, supervisor approval | Time/payroll API, region, import semantics; wage payment excluded |
| W19 | Shopify selected orders/fulfillment; selected CRM | Unselected warehouse/accounting mapping | Order/fulfillment state; reproduce selected workflow in simApp | Duplicate/out-of-order order events, partial handoff | Warehouse vendor, protected customer data scopes |
| W20 | Gmail or SendGrid message contract | Supplier format parsing and unselected 3PL/shipment register | Supplier notice and customer update; reproduce selected workflow in simApp | Corrections, multiple shipments, delivery/bounce | Carrier live tracking and warehouse acknowledgment contract |
| W21 | Shopify return/refund subset, separate permissions | Warehouse inspection/receipt and policy record | Return request and resolution; reproduce selected workflow in simApp | Receipt, inspection, refund outcome | Label provider/geography; exchanges and policy exceptions |
| W22 | Shopify location stock/order read | Purchase-order/supplier commitment register | Stock shortage and substitution review; reproduce selected workflow in simApp | Replenishment, competing stock claims, customer choice | Reservation/substitution semantics and supplier API |
| W23 | Gmail threads; Jira issue/comment/transition/attachment | Persistent message/case mapping and ambiguity queue | Support thread and maintained ticket; reproduce selected workflow in simApp | Reply/reopen/new issue, duplicate deliveries, assignee updates | Rules for multiple issues in same thread |
| W24 | Verified domain detail read; email/register | Generic service ownership/renewal record | Renewal register and authorization; reproduce selected workflow in simApp | Decision, billing failure, renewal confirmation/transfer | GoDaddy exact renewal write/eligibility; hosting provider |

## Minimum shared state

| Domain | Records to simulate | Critical invariants and inspection |
|---|---|---|
| Messaging | Account, conversation, message, attachment, draft, recipient, outbox, status event | Stable message/conversation IDs; thread attribution; sender authority; no messages to unlisted recipients; accepted and delivered separately |
| Identity and workflow records | Contact, candidate, application, vacancy, CRM deal, task, case mapping | Candidate distinct from application; duplicate email/name does not authorize destructive merge; ownership and current stage inspectable |
| Documents | Binary bytes, checksum, metadata, parent, MIME type, revision, principal grants | Correct client/period; protected original bytes; approved revision cannot silently change; ACL denial matches chosen facade |
| Tables and presentations | Workbook/sheet/range, supported formula, document tree, slide elements, exports | Exact arithmetic/units; data provenance; preserve unrelated cells/elements; unsupported formulas explicit |
| Scheduling | Event, recurrence rule, instance exception, attendee/RSVP, availability, crew/job constraints | UTC plus original time zone; recurring-series vs occurrence; check-and-book conflicts; confirmation distinct from proposal |
| Approvals and agreements | Approver identity, scope, artifact revision, decision, expiry, signature state | Only configured actor can approve; stale/partial/revoked approval handled; no agent-generated fake signer event |
| Finance | Invoice/line/credit, cleared allocation, pending payment, promise/dispute, approved hours | Decimal/minor-unit arithmetic; correct currency; paid only after ledger confirmation; no duplicate invoice/refund/export |
| Commerce and service lifecycle | Order/line/SKU/location stock, shipment, return, receipt/inspection, subscription/domain expiry | Ordered state transitions; partial quantities; stock source validity; intent/attempt/confirmation distinct |
| Analytics | Metric/dimension rows, reporting interval, source freshness, quota state | Missing is not zero; compatible currency/timezone/attribution; deterministic report content |

Brand-specific adapters map these records to real external representations. Avoid a single generic “status” field that permits transitions forbidden by the real application. Internal data can share implementation without erasing provider distinctions.

## Events, permissions and failure fidelity

Each case specifies a fixture version, data seed, logical start time, ordered event sequence, permission profile and fault schedule. Public messages/documents remain ordinary business inputs; private trigger logic stays evaluator-only. Events may be time-driven or depend on publicly observable actions, but should not secretly reward one exact action sequence when other valid business plans exist.

Required controllable faults: invalid/expired token; insufficient scope; object ACL denial; subscription gate; 429 with Retry-After or provider-specific equivalent; temporary 5xx; malformed or incomplete document; stale revision/conflict; duplicate/out-of-order webhook; read-after-write delay; transaction committed with response lost; permanent object deletion; missed webhook requiring reconciliation. Place faults at named operations/attempts with deterministic counters. A transport timeout differs from an explicit retryable business error.

Authentication is synthetic yet behaviorally relevant: principal, tenant, granted scopes, expiry, and resource ACL. A valid token for another client must fail. The agent can encounter a permission error and explain the block; it cannot mint an admin token or inspect fixture authorship. No real OAuth login, browser cookies, phone numbers, API keys or live recipients are needed.

Idempotency behavior belongs to the provider contract. Internal command handlers may maintain receipts; expose a provider idempotency header only where documented. Otherwise an ambiguous send/invoice creation must require business-key reconciliation and bounded retries. Do not make every API magically exactly-once.

## Reset and reproducibility

A reset starts a fresh application state store, object files, credentials, event cursor, fault counters, rate buckets and observer artifacts. A resume retains those elements plus agent state. Reset and resume are different operations and must be tested separately. Use stable seeded IDs for fixture entities; a run UUID is audit identity, not a source of randomized business facts.

Sapiens state must also be fresh per independent case: `corpora.sqlite3`, agent histories/Notes, workspace files, browser profile, model session and configured tools. Within a persistence case, restart or pause/resume those artifacts deliberately without resetting business state. Do not leak previous held-out cases through Notes or session history.

Real-time model latency and generation are nondeterministic. Guarantee fixture/event/failure/outcome-check reproducibility, not identical reasoning transcripts. Pin repository commits, model identifier, prompts, interface versions, runtime settings, case package hashes and supported data semantics. Report variability over repeated trials separately from application determinism.

## Verification and evaluator privacy

Verify authoritative state and recorded simulator events, not candidate logs. Evidence should include object readback, recipient/outbox records, binary file hashes, ACLs, version-specific approvals, ledger allocations and event ordering where business rules require it. An agent's final answer can be assessed for communication, but it is not proof that a booking, signature, refund or upload exists.

Keep private oracle predicates, future event rules, held-out fixtures, admin credential and scoring thresholds in a separate evaluator process and filesystem. Agent-facing API discovery contains only operation contracts and genuine application validation rules. Ordinary customer policy can be visible; private evaluation criteria cannot. Restrict snapshot endpoints to privileged runner access and publish a filtered public trace.

Current AutoWFBench `/admin/finalize` authentication is useful but insufficient if the agent can read the benchmark checkout or inherited process credentials. Sapiens’s unrestricted Codex launch makes process/filesystem/network isolation a required deployment boundary.

## Scope decisions before implementation

Start with the Google Drive folder-creation simApp and its full selected-workflow UI. Extend Drive to Docs and Sheets as files, then choose subsequent workflow simApps. Add simMCP only later where supported as an optimization, never as the prerequisite for UI coverage. Do not force Instagram, GitHub or Telegram into the researched workflows just because they were requested in the capability survey. Investigate Meta access, actual payroll/ATS/portal vendors, Canva editing coverage and domain renewal writes before treating those as faithful provider simulations. Record deviations and unsupported operations explicitly.
