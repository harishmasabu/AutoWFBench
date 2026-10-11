# Sapiens4 application inventory

**Confirmed architecture direction:** simApp-first for every selected application, even where APIs exist. A simApp supplies the full simulated UI needed by the selected workflow, over authoritative local state. simMCP is a later optional optimization when the application has adequate MCP capabilities; unavailable or limited MCP leaves the UI primary. API/CLI findings below document semantics and feasibility, not permission to substitute an API-first benchmark. See [decisions and roadmap](../decisions-and-roadmap.md).

Research baseline: [Astra customer and workflow report](../customer-workflows/customer-and-workflow-research.md), accessed 11 October 2026. Source SHA-256: `d42fd38b5fb16402216c593a4f740d246b74e95d43ff5f96f43a5bac579a0a47`. Workflow IDs W01–W24 preserve the original report’s numbered catalog. This inventory separates reported stacks from selected implementation examples. A brand named in a benchmark proposal is not evidence that the customer uses it.

## Workflow to application requirements

| ID | Business process | Reported or documented stack | Illustrative or optional applications | Required operations |
|---|---|---|---|---|
| W01 | Enquiry to booking | Acuity, Asana; law-firm owner | Gmail, HubSpot or Sheets | Read enquiry/history; upsert lead; schedule due action; send permitted follow-up; reconcile booking and withdrawal |
| W02 | Quote to signature or expiry | AdobeSign; same owner | Gmail, CRM, Drive | Read quote/signature state; send reminder; record objection; stop on signature; expire only under policy |
| W03 | Onboarding to readiness | Vendor process, no complete observed stack | CRM, Gmail, Drive, Docs, intake form, Calendly, project board | Read won deal; create folder/checklist; copy templates; share with correct people; request missing inputs; confirm readiness |
| W04 | Appointment preparation and recovery | Calendly documentation; consultant uses Calendly | Gmail, Calendar, CRM, Drive | Read event/invitees; collect prework; prepare brief; send reminder; interpret cancellation/no-show; rebook |
| W05 | Open-house follow-up | Unnamed CRM | Forms, Sheets, Gmail, Calendar, licensed property source | Ingest sign-in; deduplicate person; capture preferences; update next action; stop obsolete outreach |
| W06 | Viewing and feedback | Adjacent real-estate evidence | Gmail, Calendar/Calendly, CRM, property data, Sheets | Inspect property availability; coordinate participants; book; update changes; collect feedback |
| W07 | Content approval | Gmail in Make request | Docs, Drive, Canva, Sheets/board, publishing platform | Read current draft; request approval; bind response to version; route revisions; mark ready; optionally publish under authorization |
| W08 | Analytics report | n8n template: Analytics, advertising, CRM, Sheets, Slides, Gmail, Slack | Specific advertising and CRM vendors unresolved | Fetch time-bounded metrics; reconcile units; identify missing inputs; build tables/charts/deck; deliver or flag incomplete report |
| W09 | Candidate intake | Forms, Sheets, WhatsApp/email; separate account: Excel, shared files, Slack, LinkedIn Recruiter, Indeed | ATS | Read form; enumerate CVs; preserve binary files; match candidate and vacancy separately; request missing facts; notify owner |
| W10 | Candidate follow-up | Forms, Sheets, WhatsApp, phone, email | Gmail, ATS, task list | Read stage/history; correlate cross-channel reply; detect withdrawal; cancel due actions; route questions |
| W11 | Interviews and feedback | Manual recruiter pipeline; Calendly vendor example | ATS/Sheets, Gmail, Calendar/Calendly, Docs, Slack | Read approved shortlist; coordinate availability; restrict CV access; book/reschedule; collect feedback; update administrative status |
| W12 | Attachment filing | Gmail, Drive; n8n user | Sheets register | Search messages; enumerate/download attachments; classify client/period; hash duplicates; upload/move; record provenance |
| W13 | Missing documents | Email, client portal, texting; bookkeeper | Gmail, Drive, Sheets, accounting records | Read requirements and received files; validate period/usability; request outstanding items; reconcile partial replies; stop when complete |
| W14 | Invoice follow-up | Payment chasing reported; brands unspecified | Accounting/invoicing, payment ledger, Gmail, CRM/Sheets | Read balance/due date; reconcile payments/credits; send reminder; track promise/dispute; stop only on authoritative settlement |
| W15 | Work to invoice draft | Hourly/monthly billing account; brands unspecified | Time tracker/Sheets, board, Drive/Docs, invoicing, Gmail | Read agreement/rates; approved time/milestones; exclude already billed work; calculate draft; attach provenance; submit for review |
| W16 | Acceptance and handover | Adjacent freelance and Gmail approval evidence | Gmail, Drive, Docs, board, invoice register | Read scope/version; share package; record qualified/partial approval; route revisions; assemble accepted handover |
| W17 | Recurring service scheduling | Cleaning discussion; separate commenters: Sheets/Quo/QuickBooks and Calendar/Slack/QuickBooks Time | Service-management app, Gmail/business messaging | Read recurring jobs/crew; enforce availability; distinguish one event from series; obtain confirmations; update conflicts |
| W18 | Jobs to approved hours | Partial support in same discussion | Schedule/time tracker, Sheets, messaging, payroll draft/import | Join job/time records; detect gaps/overlap; ask correction; retain approval; export approved hours; no wage payment |
| W19 | Order handoff | Vendor ecommerce example | Shopify, fulfillment, CRM, accounting, Sheets | Read paid/canceled orders; match SKU/customer; create downstream records; reconcile edit/cancel/partial shipment |
| W20 | Shipping notice to update | HN: email, Mailparser, SendGrid, 3PL/FedEx emails | Order system, Sheets | Parse explicit notice; match shipment/order; send once; track later corrections/delay; flag ambiguous identity |
| W21 | Return resolution | Shopify merchant documentation | Email, label provider, receipt/inspection register, support | Read policy/order; request missing details; authorize return; track physical receipt; inspect; prepare authorized refund/exchange |
| W22 | Stock shortage resolution | Shopify inventory documentation; proposed extension | Supplier email, purchasing Sheet, customer messaging | Read location stock/commitments; request replenishment ETA; present permitted options; record choice; verify fulfillment |
| W23 | One support ticket per case | Gmail, Jira in n8n request | Knowledge docs, customer directory | Read thread/messages/attachments; deduplicate; create/comment/assign/transition ticket; synchronize case status; reopen appropriately |
| W24 | Hosting/domain renewal | HN: custom register, provider details, email | Sheets/CRM, Gmail, provider account/statements, Drive; GoDaddy example | Read service ownership/expiry/autorenew; request decision; record instruction; perform only authorized action; verify changed expiry |

## Consolidated application registry

Operations below are required by the proposed workflows, not claims of interface support. The capability matrix makes those claims separately.

| Application or family | Workflow IDs | Stack status and minimum capability |
|---|---|---|
| Gmail | W01–W07, W09–W18, W20–W24 | Explicit W07/W12/W23; elsewhere a Gmail implementation of generic email. Threads, messages, drafts/send, attachments, labels/history |
| Google Drive storage | W02–W04, W07, W09, W11–W13, W15–W16, W24 | Explicit W12; otherwise illustrative shared-file implementation. Search, folders, binary upload/download, copy, permissions, provenance |
| Google Docs | W03, W07, W11, W15–W16; W23 optional | Illustrative. Read/create/update structured text, template substitution; storage/sharing through Drive |
| Google Sheets | W01, W05–W10, W12–W15, W17–W20, W22, W24; W11 optional | Explicit recruitment and cleaning discussion; template W08; otherwise illustrative register. Read/write rows, stable IDs, formulas, formatting |
| Google Slides | W08 | Vendor template. Copy deck, fill text/tables, charts/images, export presentation |
| Google Forms | W03, W05, W09 | Explicit W09; W03/W05 optional form implementation. Read responses and question schema; controlled submission events |
| Google Calendar | W04–W06, W11, W17–W18 | Cleaning commenter; otherwise generic scheduling choice. Events/free-busy, invitations, recurring exceptions, time zones |
| WhatsApp Business Platform | W10; optional W01/W04/W05/W06/W09/W13/W17/W18/W22 | Recruitment explicitly uses WhatsApp, not necessarily the API product. Authorized messages, incoming updates, template/status handling; no assumed access to historical personal/group chats |
| HubSpot | W01–W03, W05–W06, W08, W14, W19, W24; optional CRM cases | Evaluated by law-firm owner, not confirmed deployed. Generic CRM choice: contacts/deals/tasks/associations/custom fields |
| Jira | W23; optional W03/W07/W15/W16 | Explicit W23; optional project board elsewhere. Create/read/comment/assign/transition, attachments, thread mapping |
| GitHub | Optional W23/W24 extension only | Illustrative persona tool, not a required operation in the 24 workflows. Issues/repository/release inspection only if later scenario includes software investigation |
| Instagram | Optional W07 publication endpoint | Not an observed stack or mandatory W07 operation. Approval-to-publication extension; publish professional-account assets, read publish status |
| LinkedIn | W09 sourcing context; optional W07 publishing | LinkedIn Recruiter is observed persona context; sourcing automation is outside W09 intake. Approved publishing is an extension; no assumed Recruiter/DM/scraping API |
| Canva | W07; optional W08 design variant | Illustrative. Search/copy/create design, supported template fill, export; precise visual edits depend on tool coverage |
| Calendly | W03–W04, W06, W11 | Consultant observed; scheduling vendor examples. Availability, links, invitees, booking/cancel/no-show, webhooks |
| GoDaddy | W24 | Illustrative provider. Read owned domains/expiry/autorenew; authorized renewal/DNS only if added to scenario scope |
| Telegram | Optional messaging variant W10/W13/W17/W18/W22/W24 | No observed use in this report. Bot messages/update delivery for a deliberate variant; not a required first implementation |
| Acuity Scheduling | W01; optional W04/W06/W11 | Observed law-firm scheduling. Availability, appointments, cancellation/rescheduling, webhooks |
| Adobe Acrobat Sign | W02 | Observed quote signing. Agreements, documents, recipient/signature state, notifications; expiry semantics require validation |
| Asana | W01; optional W03/W07/W15/W16 | Observed manual reminder tool. Tasks/projects/assignments/due dates/status/comments |
| Pipedrive | Optional CRM alternative W01/W02/W03/W05/W06/W14/W19/W24 | Evaluated, not confirmed deployed. Contacts, deals, activities, pipeline stages |
| Slack | W08–W09/W11/W17–W18 | Reported recruiter/cleaning tools and template. Channels/messages/replies, owner alerts, authenticated approval feedback |
| Microsoft Excel | W09; optional register alternative | Observed recruiter. Read/write tables/cells and file identity; distinguish local workbook from Graph-hosted workbook |
| QuickBooks Online and QuickBooks Time | W17–W18; optional W14/W15/W19 | Cleaning commenters; accounting scenarios choose product deliberately. Invoices/payments/customer records or approved time; these are separate APIs/products |
| FreshBooks | Consultant persona; optional W14/W15 | Observed persona, not obligatory workflow brand. Invoice drafts, client/time/payment records |
| Shopify | W19/W21/W22; optional W20 | Vendor-supported, illustrative selected store. Orders, inventory by location, fulfillment, returns/refunds with separate authority |
| SendGrid | W20 | Historical observed email delivery. Send/status; Gmail can implement chosen benchmark delivery alternative |
| Mailparser | W20 | Historical observed intermediary. No need to add this dependency when internal parsing suffices |
| Quo | W17 context; optional W18 communication | Cleaning commenter. Phone/SMS context; no voice-agent requirement established |
| Google Analytics and unspecified advertising platform | W08 | Vendor template. Time-bounded metrics, dimensions, currency/time zone, missing-data indicators. Advertising vendor remains unselected |
| Indeed and LinkedIn Recruiter | W09 persona context | Observed sourcing tools; no requirement to scrape or automatically select candidates. Accept exported/authorized intake data |
| Unnamed ATS, client portal, property-information service, service-management app, time tracker, payroll product, accounting/payment source, warehouse/3PL, label carrier and hosting provider | As applicable above | Preserve unknown vendor identities. Choose contracts only after operations and scenario data are agreed; do not invent integrations |

## Shared simulation capabilities

Reuse domain primitives across brands: message/conversation, contact/application identity, documents and ACLs, tabular registers, appointments and recurring exceptions, work items and approvals, monetary ledgers, orders/inventory/returns, analytics datasets, and subscriptions/renewals. Each branded adapter still needs its own request/response/error/auth semantics. Gmail threads are not WhatsApp conversations; Jira transitions are not arbitrary status writes; cleared payments are not customer promises.

GitHub, Instagram, Telegram, and GoDaddy are included because the Task 3 request names them. Their presence does not justify extending customer research or implementing them before core workflows.

## Repository provenance

Inventory prepared before capability investigation. GitHub clones inspected at AutoWFBench `8e7cc5bb1002b103a2f63d73d3cd1d9de05c0552` and Sapiens4 `71e93cb3de00183c263ec440919b313cf92ed8af`. Existing integrations and proposed integration boundaries are documented in [proposed simulation architecture](proposed-simulation-architecture.md). The previously discovered checkout outside this chat was read only; final recommendations use the user-provided GitHub clones.
