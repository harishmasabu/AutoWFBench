# Sapiens4 customer and workflow discovery

Research for AutoWFBench • Tasks 1 and 2 • 11 October 2026

## Executive summary

The research identifies 10 plausible SMB customer archetypes and 24 multistep workflows spanning sales, scheduling, onboarding, recruitment, documents, finance, content, field operations, ecommerce, support, and renewals. These are inputs to customer discovery and benchmark development, not validated market segments or evidence of willingness to pay.

The strongest opportunity for a persistent agent is an unfinished business process: someone has not replied, documents are incomplete, a booking changes, or one application disagrees with another. Sapiens could retain the business goal, inspect new evidence, revise the next action, and stop when the work is complete. Direct customer accounts describe law-firm lead and quote follow-up, fragmented recruiter records, incomplete bookkeeping documents, cleaning-business scheduling, and duplicate support tickets. Each relevant workflow below links to its evidence.

Simple reminders, file copying, and standard reports are already suitable for scheduled or event-driven automation. Persistence by itself is not an exclusive AI advantage: a well-designed conventional workflow engine can also maintain state, wait, retry, and branch. The hypothesis to test is whether Sapiens can interpret a business owner's instructions, handle unstructured exceptions, and maintain the process with less configuration. This report does not claim that it has demonstrated that advantage.

### Evidence and scope

- **Customer post:** a public first-person request or account. Identity, business size, and reported results have not been independently verified.
- **Practitioner discussion:** a firsthand implementation problem or account whose author is not necessarily the purchasing business owner.
- **Vendor documentation or template:** demonstrates an offered use case or described process; it does not establish customer demand or production reliability.
- **Proposed extension:** our workflow design goes beyond the process directly described in the source. The extension is a hypothesis, even when its underlying problem has strong evidence.

**Evidence confidence refers only to support for the stated core problem.** “Direct” means the source explicitly describes it; “indirect” means the source supports an adjacent problem; “vendor only” means no independent customer account was established for that workflow. None of these labels means proven prevalence, commercial value, or technical feasibility.

All natural-language prompts, expected steps, outcomes, and edge cases below are proposed designs. They are not customer quotations or claims that the source already implemented the full workflow. Application stacks are illustrative unless explicitly marked as observed. They do not imply verified APIs, permissions, subscriptions, or existing Sapiens connectors. All sources were accessed on **11 October 2026**. An exact publication date is supplied only where exposed in the retrieved source; otherwise the access date is used.

The geography is broad and English-language evidence predominates. Business sizes below are illustrative archetype boundaries. Tasks 3–5, integration capability analysis, benchmark scenario specifications, and implementation prioritization are outside this installment.

## Task 1 Customer discovery

### 1 Independent consultant

**Business:** A solo consultant or a small advisory practice delivering paid calls and projects for multiple clients.

**Daily challenges:** Delivery competes with booking administration, gathering prework, starting new engagements, and remembering promised follow-up.

**Manual repetitive work:** Send scheduling links; collect intake answers; copy client details into project documents; create folders; request missing inputs; check whether meetings happened.

**Existing tools:** A consultant in [this Hacker News discussion](https://news.ycombinator.com/item?id=16875106) describes Calendly, FreshBooks, email, and written operating procedures. Google Drive, Docs, Sheets, and a lightweight CRM are illustrative additions, not that person's reported stack.

**AI opportunities:** Coordinate onboarding, prepare meeting materials, and recover missed appointments. Relevant workflows: 3 and 4.

**Persistent-agent advantage:** Preserve an engagement's incomplete requirements across replies and rescheduling. A fixed welcome email or reminder needs little agent judgment; interpreting “let's wait until my partner is back” and adjusting the dependent work is the stronger hypothesis.

### 2 Small law firm or professional services practice

**Business:** A partner-led practice converting initial enquiries into paid consultations and signed engagements.

**Daily challenges:** Prospects stop at different points, quotes remain unanswered, and staff must distinguish a lead from an engaged client.

**Manual repetitive work:** Log calls; schedule reminders; email follow-ups; inspect signatures; close expired quotes.

**Existing tools:** [The law-firm owner](https://www.reddit.com/r/smallbusiness/comments/1ger98z/help_me_automate_my_lead_follow_up_process_please/) explicitly describes Acuity Scheduling, AdobeSign, and Asana reminders. HubSpot and Pipedrive were evaluated, not established as deployed.

**AI opportunities:** Reply-aware enquiry follow-up and quote management. Relevant workflows: 1 and 2.

**Persistent-agent advantage:** Remember the prospect's current stage and pause or change the sequence after a booking, objection, signature, or request to stop. The agent would support administration; legal advice and engagement acceptance remain professional decisions.

### 3 Independent real-estate agent or small brokerage

**Business:** An agent or small team handling open houses, buyer enquiries, property viewings, and ongoing relationship management.

**Daily challenges:** Lead context is easy to lose while travelling; purchase timelines change; follow-ups compete with appointments.

**Manual repetitive work:** Enter open-house leads; record preferences; set reminders; arrange viewings; capture feedback and next steps.

**Existing tools:** [The customer discussion](https://www.reddit.com/r/realtors/comments/1h486uz/whats_your_follow_up_system/) describes an unnamed CRM that reminds the agent to contact leads. Gmail, Calendar, Sheets, and property listings are illustrative companion tools.

**AI opportunities:** Preserve lead context and coordinate viewing follow-through. Relevant workflows: 5 and 6.

**Persistent-agent advantage:** Adjust actions to a buyer's changing preferences, timing, and replies. A fixed campaign can send scheduled messages; a useful agent would understand why a previously suitable property or next step no longer fits.

### 4 Marketing and content agency

**Business:** A small agency producing content and managing campaigns for several clients with different approval rules.

**Daily challenges:** Feedback arrives late, approval can refer to an old version, and reporting requires joining data from several places.

**Manual repetitive work:** Send drafts; chase approvals; copy requested changes; update content calendars; compile charts and client reports.

**Existing tools:** Gmail is explicit in a [Make approval request](https://community.make.com/t/approval-process-using-gmail/48343). A [published n8n template](https://n8n.io/workflows/10715-marketing-analytics-reports-with-google-analytics-sheets-slides-and-email-alerts/) describes Analytics, Sheets, Slides, Gmail, Slack, advertising data, and CRM data. Docs, Canva, and publishing tools are illustrative additions.

**AI opportunities:** Maintain an approval trail and investigate incomplete reporting inputs. Relevant workflows: 7 and 8.

**Persistent-agent advantage:** Connect feedback to the correct version, wait for dependencies, and revisit an incomplete report when data arrives. Straightforward accept/reject routing and scheduled exports remain conventional automation candidates.

### 5 Small ecommerce operator

**Business:** An owner-operated online store with a small team and outsourced or internal fulfillment.

**Daily challenges:** Orders, shipping information, stock, customer questions, and returns can disagree across tools.

**Manual repetitive work:** Copy order details; relay tracking information; check fulfillment exceptions; review returns; coordinate stock shortages.

**Existing tools:** Shopify is the illustrative store platform, supported by [its documented returns process](https://help.shopify.com/en/manual/fulfillment/managing-orders/returns/self-serve-returns/management). Email, a fulfillment provider, accounting software, and Sheets are plausible companion systems. One [HN contributor](https://news.ycombinator.com/item?id=16875106) reports forwarding fulfillment emails through Mailparser and SendGrid; that is a separate observed stack.

**AI opportunities:** Resolve order exceptions and maintain customer communication across fulfillment and returns. Relevant workflows: 19–22.

**Persistent-agent advantage:** Retain responsibility for a delayed, split, or returned order until its condition changes. Simple order synchronization and status notifications rarely justify AI alone; interpreting supplier and customer replies may.

### 6 Cleaning company owner or service dispatcher

**Business:** A small cleaning company coordinating recurring appointments and one or more crews.

**Daily challenges:** Customer changes disrupt the schedule; job completion and recorded hours need to agree before invoicing or payroll preparation.

**Manual repetitive work:** Edit schedules; relay changes; collect availability; reconcile missing time entries; prepare approved hours.

**Existing tools:** A [cleaning-business discussion](https://www.reddit.com/r/cleaningbusiness/comments/1wtbqoj/help_automating_scheduling_and_payroll/) requests scheduling automation. A commenter reports Sheets, Quo, and QuickBooks; another describes prior use of Google Calendar, Slack, and QuickBooks Time. These are separate self-reported setups.

**AI opportunities:** Coordinate schedule changes and resolve incomplete job/time records. Relevant workflows: 17 and 18.

**Persistent-agent advantage:** Track confirmation from both customer and crew and reconcile late corrections. A timer can announce tomorrow's jobs; an agent could negotiate feasible alternatives and carry unresolved discrepancies forward.

### 7 Boutique recruitment agency

**Business:** A small agency coordinating candidates and hiring clients across several vacancies.

**Daily challenges:** Candidate details and status are spread across forms, spreadsheets, conversations, and resumes; multiple people may contact the same candidate.

**Manual repetitive work:** Copy application data; save CVs; update stages; chase replies; arrange interviews; gather hiring-manager feedback.

**Existing tools:** [An agency owner](https://www.reddit.com/r/Recruiter_Advice/comments/1trvhif/small_recruitment_agencies_how_do_you_manage/) describes Google Forms, Sheets, WhatsApp, phone, and email. A separate [in-house recruiter](https://www.reddit.com/r/recruiting/comments/1niug4m/recruiting_ats_software_advice/) describes Excel, shared resume files, Slack, LinkedIn Recruiter, and Indeed.

**AI opportunities:** Maintain candidate records and handle communication and interview logistics. Relevant workflows: 9–11.

**Persistent-agent advantage:** Preserve candidate identity, vacancy, stage, prior contact, and pending reply across days. Recruitment decisions remain with the recruiter; the proposed agent organizes facts and coordination rather than making opaque hiring judgments.

### 8 Solo bookkeeper or small accounting practice

**Business:** A practice serving recurring clients whose bookkeeping depends on timely supporting records.

**Daily challenges:** Clients submit incomplete information through different channels; the practitioner must determine what is still missing and chase payment as well as documents.

**Manual repetitive work:** Download attachments; identify client and month; file documents; compare against a checklist; request missing items; inspect unpaid invoices.

**Existing tools:** Email, a client portal, and texting are explicit in [one bookkeeper's account](https://www.reddit.com/r/Accounting/comments/1s715ta/how_do_you_get_clients_to_actually_send_you_what/). Drive, Sheets, and QuickBooks are illustrative tools, with a separate [n8n user request](https://community.n8n.io/t/gmail-attachment-to-google-drive/16218) supporting email-to-Drive filing.

**AI opportunities:** Document intake, targeted collection, and payment follow-up. Relevant workflows: 12–14.

**Persistent-agent advantage:** Update the outstanding list after each partial submission and distinguish a question or payment dispute from silence. Repeatedly sending the same monthly request misses that changing state.

### 9 Freelance designer or creative professional

**Business:** An independent designer, writer, or marketing assistant working on several projects or recurring assignments.

**Daily challenges:** Deliverables, acceptance, billing terms, and payment dates must stay aligned without a dedicated operations person.

**Manual repetitive work:** Track approved work; compile time or milestones; create invoices; gather sign-off; retain final files and acceptance evidence.

**Existing tools:** A [freelance billing discussion](https://www.reddit.com/r/freelance/comments/1b8fea5/how_to_bill_clients_as_a_freelancer/) describes hourly/monthly billing and contract questions. Gmail, Drive, Docs, Sheets, a time tracker, and an invoicing app are illustrative rather than verified as that customer's stack.

**AI opportunities:** Assemble accurate invoices and manage delivery acceptance. Relevant workflows: 15 and 16; workflow 14 also applies.

**Persistent-agent advantage:** Remember which work is approved, which revision is current, and which billing condition is satisfied. A monthly trigger can create a draft, but resolving “approved except for the mobile screens” requires interpretation and continuing coordination.

### 10 Small IT services or website maintenance provider

**Business:** A small provider supporting client websites, applications, hosting, and domain renewals.

**Daily challenges:** Support arrives in email, recurring issues can be duplicated, and renewals depend on client instructions and provider records.

**Manual repetitive work:** Convert requests into tickets; copy follow-ups; track ownership; inspect expiry lists; chase renewal decisions.

**Existing tools:** Gmail and Jira are explicit in a [ticket-creation discussion](https://community.n8n.io/t/trigger-a-process-when-open-new-thread-gmail/23273). An [HN contributor](https://news.ycombinator.com/item?id=16875106) reports a custom client/hosting/domain system with renewal emails. GitHub, Drive, Sheets, and GoDaddy are illustrative additions.

**AI opportunities:** Preserve support-thread continuity and coordinate renewal follow-through. Relevant workflows: 23 and 24.

**Persistent-agent advantage:** Keep a case active through multiple replies and reconcile client decisions with later provider status. Known ticket mappings and fixed expiry reminders are also implementable without AI; ambiguous replies and cross-system exceptions offer a stronger agent test.

## Task 2 Workflow discovery

### Catalog overview

| Workflow | Main function | Primary persona | Evidence for core problem |
|---|---|---|---|
| 1 Enquiry follow-up through booking | Sales and CRM | Law firm | Direct customer post |
| 2 Quote follow-up through signature or expiry | Sales operations | Law firm | Direct customer post |
| 3 New-client onboarding through readiness | Client operations | Consultant | Vendor example; proposed extension |
| 4 Appointment preparation and no-show recovery | Scheduling | Consultant | Vendor documentation |
| 5 Open-house lead follow-up | Sales and CRM | Real-estate agent | Direct customer post |
| 6 Viewing coordination and feedback | Scheduling | Real-estate agent | Indirect; proposed extension |
| 7 Content approval through publication readiness | Content | Marketing agency | Direct practitioner request |
| 8 Client analytics report with missing-data handling | Reporting | Marketing agency | Vendor-hosted community template |
| 9 Candidate intake and record reconciliation | Recruitment | Recruitment agency | Direct customer posts |
| 10 Candidate follow-up with stage awareness | Recruitment | Recruitment agency | Direct customer post |
| 11 Interview coordination and feedback collection | Recruitment and scheduling | Recruitment agency | Indirect plus vendor documentation |
| 12 Email attachment intake and filing | Documents | Bookkeeper | Direct practitioner request |
| 13 Missing-document collection through completeness | Documents | Bookkeeper | Direct customer post |
| 14 Invoice follow-up with payment reconciliation | Finance | Bookkeeper | Direct customer posts |
| 15 Approved work to invoice draft | Finance | Freelancer | Direct practitioner account |
| 16 Deliverable acceptance and project handover | Project operations | Freelancer | Indirect; proposed extension |
| 17 Recurring service scheduling and change handling | Field operations | Cleaning company | Direct customer-side request |
| 18 Completed jobs to approved time records | Field operations | Cleaning company | Practitioner discussion; extension |
| 19 Order synchronization and fulfillment handoff | Ecommerce | Online store | Vendor use case |
| 20 Supplier shipping notice to customer update | Fulfillment | Online store | Direct practitioner account |
| 21 Return request through resolution | Customer support | Online store | Vendor documentation |
| 22 Stock shortage through customer resolution | Inventory operations | Online store | Vendor documentation; extension |
| 23 Email support thread to one maintained ticket | Customer support | IT provider | Direct practitioner request |
| 24 Hosting and domain renewal coordination | Recurring operations | IT provider | Direct practitioner account |

### 1 Enquiry follow-up through booking

**Persona and problem:** A law firm loses track of prospects who enquire but do not book a paid appointment.

**Evidence — direct customer post:** The owner describes manual Asana reminders and asks for follow-ups after two days, one week, and two weeks. [Reddit, 29 October 2024](https://www.reddit.com/r/smallbusiness/comments/1ger98z/help_me_automate_my_lead_follow_up_process_please/). The problem and cadence are sourced; reply interpretation and CRM reconciliation below are proposed.

**Applications:** Observed Acuity and Asana; illustrative Gmail and HubSpot or a lead Sheet.

**Owner request:** “Keep track of people who call but don't book. Follow up after two days, a week, and two weeks, and stop once they book or say they're not interested.”

**Expected steps:**

1. Capture the enquiry, contact details, call date, and permitted follow-up channel.
2. Match an existing lead before creating a record; record whether an appointment already exists.
3. At each due point, recheck bookings, replies, and opt-outs.
4. Send the appropriate authorized follow-up or route a substantive question to staff.
5. Move booked or declined leads to the correct state and cancel remaining reminders.

**Outcome:** Eligible enquiries receive follow-through, with current lead status and no obsolete chase messages.

**Errors and edge cases:** Duplicate enquiry; shared email; booking under another address; late reply; bounced message; appointment canceled; staff already contacted the lead.

**Benchmark value:** Temporal reasoning, identity matching, stopping conditions, and reacting to events after a wait. Persistent value comes from changing context; fixed reminders alone are a conventional baseline.

### 2 Quote follow-up through signature or expiry

**Persona and problem:** The same practice must chase quotes and close those that expire.

**Evidence — direct customer post:** The owner explicitly requests AdobeSign tracking, repeated quote reminders, and closure after roughly 30 days. [Reddit, 29 October 2024](https://www.reddit.com/r/smallbusiness/comments/1ger98z/help_me_automate_my_lead_follow_up_process_please/). This shares an evidence source with workflow 1; it is not a second independent demand signal.

**Applications:** Observed AdobeSign; illustrative Gmail, CRM, and Drive.

**Owner request:** “Keep an eye on sent quotes, remind people while they're open, and close them after 30 days unless they've signed or we're discussing a change.”

**Expected steps:**

1. Register the sent quote, version, client, expiry date, and signature status.
2. Recheck signature and conversation state before each reminder.
3. Pause when a revision or clarification is requested and assign the question to the owner.
4. On signing, record acceptance and create the engagement handoff once.
5. On expiry, apply the owner's closure rule, record the disposition, and stop the sequence.

**Outcome:** Each quote has an accurate accepted, pending, revised, or expired state.

**Errors and edge cases:** Signature and expiry arrive together; a quote is superseded; multiple signers; signature service unavailable; reopened negotiation; wrong document version.

**Benchmark value:** Long-lived state, race handling, version control, and avoiding duplicate downstream engagements.

### 3 New-client onboarding through readiness

**Persona and problem:** A consultant needs the agreement, intake information, shared materials, and kickoff to agree before work starts.

**Evidence — vendor example, with proposed extension:** Zapier discusses immediate welcome communication and client visibility into project progress, including an agency using Trello. It does not document the full checklist below. [Zapier, 11 March 2019](https://zapier.com/blog/client-onboarding/).

**Applications:** Illustrative CRM, Gmail, Drive, Docs, intake form, Calendly, and project board.

**Owner request:** “When a new client signs, set them up, collect the information we need, and arrange the kickoff once they're ready.”

**Expected steps:**

1. Confirm the signed agreement and any owner-defined prerequisites, such as a received deposit.
2. Match the client and create a project, folder, and checklist from the agreed service template.
3. Send the welcome message and a client-specific request for inputs.
4. Record arriving information and ask only for outstanding or unclear items.
5. Arrange kickoff when readiness conditions are met; hand the owner a concise brief.

**Outcome:** A ready-to-start client project with linked records and an explicit list of any blockers.

**Errors and edge cases:** Duplicate sign event; amended scope; wrong folder access; incomplete intake; client changes contact; payment not cleared.

**Benchmark value:** Plan creation from a business goal, dependency tracking, partial completion, and resumption. A fixed setup checklist is a useful conventional baseline.

### 4 Appointment preparation and no-show recovery

**Persona and problem:** A consultant must prepare attendees and recover missed meetings without continuing obsolete reminders.

**Evidence — vendor documentation:** Calendly documents reminder, reconfirmation, resource-sharing, and no-show rebooking automations. [Calendly, updated 5 October 2026](https://calendly.com/learn/calendly-workflows). This establishes existing product use cases, not independent demand for an AI agent.

**Applications:** Calendly, Gmail, Calendar; illustrative CRM and Drive preparation materials.

**Owner request:** “Make sure consultation clients have the right prework. If they miss the call, help them rebook and keep their record up to date.”

**Expected steps:**

1. Link the booking to the client and retrieve the appropriate preparation checklist.
2. Send relevant prework and record outstanding items.
3. Reconcile confirmations, cancellations, and rescheduling before further messages.
4. Use an explicit attendance signal; if marked a no-show, offer rebooking.
5. Link the replacement booking and close the old follow-up chain.

**Outcome:** Correct meeting preparation and one current appointment or a clearly unresolved case.

**Errors and edge cases:** Time zones; calendar changes outside the booking tool; group-event attendance; no reliable attendance signal; replacement booked before a reminder fires.

**Benchmark value:** Event supersession and cross-app consistency. Native reminders are already automated; Sapiens' proposed value is handling conversational exceptions and incomplete prework.

### 5 Open-house lead follow-up

**Persona and problem:** A real-estate agent fails to follow through consistently with open-house leads.

**Evidence — direct customer post:** An experienced agent reports poor follow-up and says their CRM only reminds them when to reach out. [Reddit, 1 December 2024](https://www.reddit.com/r/realtors/comments/1h486uz/whats_your_follow_up_system/).

**Applications:** CRM; illustrative Forms or Sheets, Gmail, Calendar, and an authorized property-information source.

**Owner request:** “Follow up with the people from Saturday's open house. Use their notes, find out their timing, and tell me who wants a viewing.”

**Expected steps:**

1. Import attendee details and deduplicate against existing contacts.
2. Preserve property, preferences, relationship context, and communication permission.
3. Send an appropriate follow-up using the agent's approved information.
4. Interpret replies into updated needs and a next action; flag uncertainty.
5. Arrange an agent handoff or viewing and stop irrelevant messages.

**Outcome:** Updated lead records and actionable next steps for the agent.

**Errors and edge cases:** Illegible sign-in details; one household has multiple contacts; existing representation; changed budget; stale property status; explicit opt-out.

**Benchmark value:** Extracting and revising preferences without inventing facts, retaining conversation history, and determining when a sequence should end.

### 6 Viewing coordination and feedback

**Persona and problem:** A real-estate agent needs to carry an interested buyer through a confirmed viewing and a recorded next step.

**Evidence — indirect, proposed extension:** The [open-house discussion, 1 December 2024](https://www.reddit.com/r/realtors/comments/1h486uz/whats_your_follow_up_system/) establishes weak lead follow-up, not this detailed viewing process. Meeting-state handling is an adjacent documented use case in [Calendly's guide, updated 5 October 2026](https://calendly.com/learn/calendly-workflows).

**Applications:** Illustrative Gmail, Calendar or Calendly, CRM, property details, and Sheets.

**Owner request:** “Arrange a viewing for this buyer, confirm the property is available, and collect their feedback afterward so I know what to do next.”

**Expected steps:**

1. Retrieve buyer preferences, listing status, viewing constraints, and required participants.
2. Propose feasible times and resolve conflicting replies.
3. Confirm the agreed slot and distribute current location and access instructions.
4. Reconcile any cancellation or change before reminders.
5. Request feedback after confirmed completion, update preferences, and assign the next action.

**Outcome:** A coordinated appointment and usable feedback tied to the correct buyer and property.

**Errors and edge cases:** Property withdrawn; access unavailable; two bookings collide; different time zones; ambiguous “next Friday”; viewing never occurred.

**Benchmark value:** Multilateral coordination, constraint changes, and state continuity. Customer interviews are needed to validate this extension's exact boundaries.

### 7 Content approval through publication readiness

**Persona and problem:** An agency needs client acceptance or rejection to update its content workflow.

**Evidence — direct practitioner request:** A Make user asks how to send sample content through Gmail and receive the approval decision back into the workflow. [Make Community, 29 July 2024](https://community.make.com/t/approval-process-using-gmail/48343). Version tracking and revision handling below extend that request.

**Applications:** Gmail; illustrative Docs, Drive, Canva, a content Sheet or board, and the intended publishing platform.

**Owner request:** “Send these drafts for approval, keep track of requested changes, and make sure only the version the client approves is ready to publish.”

**Expected steps:**

1. Identify the asset, version, approver, and proposed publication date.
2. Send the review request with an unambiguous link to that version.
3. Interpret approval, rejection, or requested edits; request clarification for mixed responses.
4. Route revisions, invalidate superseded approval, and request review again as needed.
5. Record final approval and move the correct asset into the authorized publishing queue.

**Outcome:** A traceable approval state and the correct publication-ready asset.

**Errors and edge cases:** “Looks good except…”; approval of an older file; conflicting approvers; forwarded approval; duplicate callback; missed deadline.

**Benchmark value:** Human-in-the-loop waiting, semantic interpretation, version-specific authorization, and correct resumption. Public publication is a separate owner-defined action, not implied by a review request.

### 8 Client analytics report with missing-data handling

**Persona and problem:** A marketing agency assembles recurring client reports from several systems.

**Evidence — vendor-hosted community template:** The n8n template describes gathering Analytics, ad, and CRM data, logging KPIs in Sheets, generating Slides, emailing reports, and alerting on failures. Exact date unavailable; [accessed 11 October 2026](https://n8n.io/workflows/10715-marketing-analytics-reports-with-google-analytics-sheets-slides-and-email-alerts/). Template descriptions are not execution validation.

**Applications:** Google Analytics, advertising platform, CRM, Sheets, Slides, Gmail, and Slack.

**Owner request:** “Prepare each client's monthly report. If a source is missing or a number looks wrong, resolve it or flag it clearly before the report goes out.”

**Expected steps:**

1. Load client-specific metrics, period, recipients, and approved definitions.
2. Retrieve and reconcile data for the same date range, currency, and account.
3. Calculate agreed metrics; distinguish missing values from zero.
4. Draft charts and a concise explanation grounded in the data.
5. Resolve or label missing inputs, then distribute the approved report and retain its source period.

**Outcome:** A coherent report with explicit unresolved data limitations.

**Errors and edge cases:** Late attribution; zero denominator; account mismatch; revised source data; duplicate monthly rows; inaccessible slides.

**Benchmark value:** Numerical grounding, joining heterogeneous data, and recovering from partial failure. Data investigation is the proposed agent extension; routine report generation already fits scheduling.

### 9 Candidate intake and record reconciliation

**Persona and problem:** A recruitment agency copies application details manually and loses the relationship between candidate records, CVs, and jobs.

**Evidence — direct customer posts:** An agency owner describes Forms-to-Sheets copying and fragmented conversations: [Reddit, accessed 11 October 2026](https://www.reddit.com/r/Recruiter_Advice/comments/1trvhif/small_recruitment_agencies_how_do_you_manage/). Separately, an in-house recruiter describes Excel entry, resume storage, and Slack handoffs: [Reddit, 16 September 2025](https://www.reddit.com/r/recruiting/comments/1niug4m/recruiting_ats_software_advice/).

**Applications:** Observed Forms, Sheets or Excel, file sharing, Slack, email; an ATS is an optional destination.

**Owner request:** “Put each new application in the right place, attach the CV, and show me missing details without creating duplicate candidates.”

**Expected steps:**

1. Capture submission and attachment identifiers.
2. Extract explicit candidate details and preserve the original CV.
3. Match identity cautiously, retaining separate applications for different vacancies.
4. Create or update the candidate and application records with source links.
5. Request missing administrative information and notify the assigned recruiter.

**Outcome:** A usable application record with provenance and fewer duplicate entries.

**Errors and edge cases:** Two people share a name; revised CV; multiple vacancies; unreadable attachment; missing consent; existing candidate changes email.

**Benchmark value:** Entity resolution, document extraction, provenance, and avoiding destructive merges. Candidate selection is outside the proposed automation.

### 10 Candidate follow-up with stage awareness

**Persona and problem:** A recruiter cannot easily tell who was contacted, who replied, and who needs a follow-up.

**Evidence — direct customer post:** These exact tracking problems are listed by a small agency owner using Forms, Sheets, WhatsApp, and email. [Reddit, accessed 11 October 2026](https://www.reddit.com/r/Recruiter_Advice/comments/1trvhif/small_recruitment_agencies_how_do_you_manage/).

**Applications:** Sheets or ATS, Gmail, authorized business messaging, recruiter task list.

**Owner request:** “Keep the candidate follow-ups moving. Stop chasing anyone who's replied or withdrawn, and flag people waiting on us.”

**Expected steps:**

1. Load application stage, assigned recruiter, prior contact, and next due action.
2. Reconcile new conversations against the correct candidate and vacancy.
3. Distinguish silence, a question, withdrawal, and a promise to respond later.
4. Send authorized follow-ups only when appropriate; assign substantive questions to the recruiter.
5. Record the new next action and report stalled cases awaiting internal action.

**Outcome:** Current candidate communication status and fewer unresolved handoffs.

**Errors and edge cases:** Reply on another channel; two recruiters act concurrently; role closes; candidate wants a later date; imported history is incomplete.

**Benchmark value:** Durable conversational state, cancellation of obsolete actions, and recognizing that the business—not the candidate—owes the next response.

### 11 Interview coordination and feedback collection

**Persona and problem:** A recruiter must align candidate and interviewer availability, then collect feedback to move the process forward.

**Evidence — indirect plus vendor documentation:** The [recruiter account, 16 September 2025](https://www.reddit.com/r/recruiting/comments/1niug4m/recruiting_ats_software_advice/) describes a manually tracked interview-to-offer process. [Calendly, updated 5 October 2026](https://calendly.com/learn/calendly-workflows), documents interview reminders and no-show rebooking. The complete coordination loop is proposed.

**Applications:** Illustrative ATS or Sheets, Gmail, Calendar or Calendly, interview brief in Docs, and Slack.

**Owner request:** “Schedule the shortlisted candidates with the hiring manager, keep changes in sync, and chase interview feedback so nobody gets stuck.”

**Expected steps:**

1. Read the recruiter-approved shortlist and required interview participants.
2. Gather available slots and coordinate a mutually acceptable time.
3. Confirm the interview and provide the correct CV and brief to authorized participants.
4. Reconcile rescheduling and attendance signals.
5. Request missing feedback and update the administrative stage after the recruiter's decision.

**Outcome:** Confirmed interviews and a clear feedback queue.

**Errors and edge cases:** Time-zone ambiguity; interviewer replacement; candidate withdraws; simultaneous booking; no-show mislabeled; feedback arrives after the role closes.

**Benchmark value:** Dependent scheduling, sensitive record routing, and handling the transition from external waiting to internal waiting.

### 12 Email attachment intake and filing

**Persona and problem:** A bookkeeper or administrator needs PDFs from email stored in the correct client folders.

**Evidence — direct practitioner request:** An n8n user describes subject-based Gmail searches and PDF uploads to particular Drive folders; follow-up discussion raises multiple attachments. [n8n Community, 30 July 2022; attachment discussion 25 August 2022](https://community.n8n.io/t/gmail-attachment-to-google-drive/16218). Client/month classification is our extension.

**Applications:** Gmail, Drive; illustrative Sheets intake register.

**Owner request:** “File incoming client documents in the right client and month folder, and show me anything you can't identify confidently.”

**Expected steps:**

1. Detect eligible messages and enumerate all relevant attachments.
2. Identify client, document category, and covered period from approved reference data and document content.
3. Check for duplicates or revisions before writing.
4. Save to the correct folder and record the original message and file links.
5. Isolate ambiguous documents for review and mark processing complete only after successful storage.

**Outcome:** Traceable document storage and an exception queue.

**Errors and edge cases:** Multiple attachments; same filename with different content; image-only PDF; password-protected file; missing folder; revoked access; wrong month inferred from arrival date.

**Benchmark value:** Binary-file handling, classification uncertainty, many-to-one and one-to-many relationships, and retry safety. Straightforward attachment copying remains a conventional baseline.

### 13 Missing-document collection through completeness

**Persona and problem:** A bookkeeper repeatedly requests documents and receives partial submissions or clarification questions.

**Evidence — direct customer post:** A solo bookkeeper with approximately 22 recurring clients describes this monthly cycle, ignored reminders, and an unused portal. [Reddit, 29 March 2026](https://www.reddit.com/r/Accounting/comments/1s715ta/how_do_you_get_clients_to_actually_send_you_what/). The reported experience is one person's account, not a market estimate.

**Applications:** Gmail or approved client channel, Drive or portal, Sheets checklist, accounting records as needed.

**Owner request:** “Get the remaining month-end documents from each client. Only ask for what's missing, explain unclear requests, and tell me which clients are blocking completion.”

**Expected steps:**

1. Load each client's explicit requirements and reporting period.
2. Compare received, usable documents with the requirements.
3. Send a specific outstanding-items request using the owner's communication rules.
4. Reconcile each partial submission; answer straightforward administrative questions using approved examples.
5. Stop on completeness, or escalate named missing items at the agreed deadline.

**Outcome:** A current completeness record and fewer unnecessary repeat requests.

**Errors and edge cases:** Wrong-period statement; unreadable upload; duplicate file; client says an account closed; requirement waived by accountant; document arrives just before a reminder.

**Benchmark value:** Maintaining a changing set of obligations, semantic completeness, and successful resumption after several partial replies.

### 14 Invoice follow-up with payment reconciliation

**Persona and problem:** A bookkeeper or freelancer spends time chasing unpaid invoices while trying to preserve client relationships.

**Evidence — direct customer posts:** A practice owner reports payment chasing and clients declining autopay: [Reddit, 20 January 2024](https://www.reddit.com/r/Bookkeeping/comments/19bfffc/handling_clients/). A marketing freelancer separately describes completed work and missing weekly payment: [Reddit, accessed 11 October 2026](https://www.reddit.com/r/freelance/comments/1pd0m6u/need_advise/). Automated reconciliation is proposed.

**Applications:** Illustrative accounting or invoicing system, payment-status source, Gmail, CRM or Sheets.

**Owner request:** “Follow up on overdue invoices politely, stop when they're paid, and bring disputes or promised payment dates to me.”

**Expected steps:**

1. Identify invoices past their agreed due dates and inspect outstanding balances.
2. Reconcile payments, credits, existing disputes, and recent correspondence.
3. Send the approved reminder with the correct invoice and remaining balance.
4. Record a payment promise, route a dispute, or schedule the next permitted follow-up.
5. Close the chase after authoritative payment confirmation; escalate according to the owner's policy.

**Outcome:** Accurate collection status and fewer reminders sent to clients who already paid.

**Errors and edge cases:** Partial payment; bank transfer pending; credit note; incorrect billing contact; payment allocated to another invoice; contested work; foreign currency.

**Benchmark value:** Financial consistency and divergent reply paths. A statement that payment was sent is not equivalent to cleared payment. The workflow does not authorize new charges or refunds.

### 15 Approved work to invoice draft

**Persona and problem:** A freelancer must turn completed work into a correctly calculated invoice under the agreed billing model.

**Evidence — direct practitioner account:** A UI designer asks about billing and contracts; a respondent describes hourly billing and monthly invoices for work to date. [Reddit, accessed 11 October 2026](https://www.reddit.com/r/freelance/comments/1b8fea5/how_to_bill_clients_as_a_freelancer/). Cross-app invoice assembly is our design.

**Applications:** Illustrative time tracker or Sheets, project board, agreement in Drive or Docs, invoicing app, Gmail.

**Owner request:** “Prepare this month's invoices from approved work using each client's agreed rates. Flag anything disputed or already billed before I send them.”

**Expected steps:**

1. Read the agreement's billing basis, rates, period, and invoice recipient.
2. Collect eligible approved time or accepted milestones.
3. Exclude previous invoices, rejected entries, and work outside the billing period.
4. Calculate line items using supplied tax and currency settings; create a draft with evidence links.
5. Present exceptions and the draft for review, then record any authorized issue action once.

**Outcome:** Reviewable invoices tied to actual eligible work.

**Errors and edge cases:** Mixed rates; retainer allowance; missing time approval; milestone partly accepted; rounding; duplicate export; retrospective correction.

**Benchmark value:** Reconciling documents with operational records and doing grounded arithmetic. The agent must not invent rates, tax treatment, or evidence that work was accepted.

### 16 Deliverable acceptance and project handover

**Persona and problem:** A freelancer needs to distinguish completed work, requested revisions, and client acceptance before closing a project.

**Evidence — indirect, proposed extension:** The [freelance billing discussion, accessed 11 October 2026](https://www.reddit.com/r/freelance/comments/1b8fea5/how_to_bill_clients_as_a_freelancer/) discusses deliverables and payment milestones. A separate [Make request, 29 July 2024](https://community.make.com/t/approval-process-using-gmail/48343), directly supports email-based content acceptance. Neither source establishes this complete handover process.

**Applications:** Illustrative Gmail, Drive, Docs, project board, and invoice register.

**Owner request:** “Send the final deliverables, collect sign-off, and prepare the handover when the client is happy. Keep revision requests separate from accepted work.”

**Expected steps:**

1. Match deliverables to agreed acceptance requirements and current versions.
2. Check sharing access and send a clear review request.
3. Record acceptance or route requested changes to the freelancer.
4. Reissue revised items while retaining the decision history.
5. On complete acceptance, assemble the handover package and trigger the agreed billing or closure handoff.

**Outcome:** An explicit acceptance record and accessible final package.

**Errors and edge cases:** Partial approval; broken links; new work disguised as a revision; different approver; accepted file changed afterward; silence mistaken for acceptance.

**Benchmark value:** Interpreting qualified approval and keeping acceptance tied to a specific scope and version. Discovery interviews should establish actual customer acceptance practices.

### 17 Recurring service scheduling and change handling

**Persona and problem:** A cleaning-business owner needs help coordinating recurring jobs and subsequent changes.

**Evidence — direct customer-side request:** A business owner's family member asks for scheduling automation and cheaper alternatives; commenters describe their own scheduling stacks. [Reddit, accessed 11 October 2026](https://www.reddit.com/r/cleaningbusiness/comments/1wtbqoj/help_automating_scheduling_and_payroll/). The detailed dispatch logic below is proposed.

**Applications:** Illustrative Sheets or service-management system, Calendar, Gmail or business messaging, crew availability records.

**Owner request:** “Organize next week's cleaning jobs. If a customer changes a time or a cleaner is unavailable, find an option that works and keep everyone informed.”

**Expected steps:**

1. Load recurring commitments, durations, locations, crew skills, and availability.
2. Propose assignments that satisfy the owner's constraints, including travel buffers where supplied.
3. Obtain any required customer and crew confirmations and update the schedule authority.
4. On a change, inspect affected jobs and coordinate feasible alternatives.
5. Distribute the current schedule and preserve unresolved conflicts for the dispatcher.

**Outcome:** One current schedule and a clear list of unfilled or unconfirmed jobs.

**Errors and edge cases:** Recurring-series change versus one visit; crew illness; access restrictions; job runs late; simultaneous edits; route assumptions unsupported by data.

**Benchmark value:** Constraint reasoning, cascading changes, and continued coordination across multiple people. The proposed system should avoid presenting unconfirmed assignments as final.

### 18 Completed jobs to approved time records

**Persona and problem:** A service owner must reconcile jobs and recorded hours before payroll preparation.

**Evidence — practitioner discussion, partial support:** The cleaning thread is titled scheduling and payroll; a commenter describes using Calendar, Slack, and QuickBooks Time. Much of the detailed payroll advice in the thread is recommendation or promotion, not verified customer experience. [Reddit, accessed 11 October 2026](https://www.reddit.com/r/cleaningbusiness/comments/1wtbqoj/help_automating_scheduling_and_payroll/).

**Applications:** Illustrative schedule system, time tracker, Sheets, messaging, payroll application's draft/import area.

**Owner request:** “Get this week's hours ready for approval. Ask about missing entries and flag anything that doesn't match the jobs before payroll is prepared.”

**Expected steps:**

1. Join assigned jobs, completion records, and time entries by worker and date.
2. Detect missing punches, overlaps, duplicate entries, and unexplained differences.
3. Ask the appropriate worker or supervisor for corrections and retain the explanation.
4. Recalculate using the owner's supplied rules and collect approval.
5. Export or prepare only approved records and record the handoff status.

**Outcome:** A reconciled set of hours for human payroll approval.

**Errors and edge cases:** Overnight job; shared job; travel time; correction after approval; changing worker identifier; disputed hours; unsupported pay rule.

**Benchmark value:** Cross-record reconciliation and a review loop with material consequences. Paying wages and choosing employment-law rules are outside this proposed workflow.

### 19 Order synchronization and fulfillment handoff

**Persona and problem:** An ecommerce operator needs order information to reach fulfillment, customer records, and finance consistently.

**Evidence — vendor use case:** Zapier describes linking storefront order data with shipping, inventory, CRM, and accounting tools. Exact publication date unavailable; [accessed 11 October 2026](https://zapier.com/automation/e-commerce-automation). This is a vendor process description, not independent evidence of adoption.

**Applications:** Illustrative Shopify, fulfillment system, CRM, accounting app, and operations Sheet.

**Owner request:** “Keep new orders consistent across our store, fulfillment list, and customer records. Show me anything that can't be handed off.”

**Expected steps:**

1. Read the order and its current payment and fulfillment conditions.
2. Match customer and product identifiers and validate required delivery details.
3. Create or update the appropriate downstream records using the order's stable identity.
4. Confirm the handoff or record the exact failed step for retry.
5. Reconcile later edits, cancellations, and partial fulfillment without duplicating completed work.

**Outcome:** Consistent order records and an actionable exception list.

**Errors and edge cases:** Duplicate order event; canceled order arrives late; address edit after export; partial shipment; unmapped SKU; downstream timeout after successful write.

**Benchmark value:** Multi-application consistency, retries, and detecting partial success. Standard synchronization can be conventional automation; resolving ambiguous exceptions is the agent hypothesis.

### 20 Supplier shipping notice to customer update

**Persona and problem:** A store using a fulfillment provider needs to translate supplier shipping emails into usable customer communication.

**Evidence — direct practitioner account:** An HN contributor describes extracting information from FedEx-generated 3PL emails, reformatting it, and sending it through SendGrid. [Hacker News, 19 April 2018](https://news.ycombinator.com/item?id=16875106). Order reconciliation and delivery-exception handling are proposed extensions.

**Applications:** Observed email, Mailparser, SendGrid, and 3PL messages; illustrative order system and Sheets. Those named intermediaries are evidence of the historical setup, not recommended dependencies.

**Owner request:** “Use the warehouse's shipping emails to keep customers informed, and flag anything that doesn't match an order or looks delayed.”

**Expected steps:**

1. Identify a relevant supplier message and extract explicit shipment details.
2. Match order, shipment, and customer; isolate ambiguous matches.
3. Update shipment status and send the authorized customer message once.
4. Track later notices for split shipments, corrections, or delays.
5. Escalate unresolved exceptions with the relevant order history.

**Outcome:** Accurate customer shipment updates and visible fulfillment exceptions.

**Errors and edge cases:** Supplier layout changes; reused reference; several tracking numbers; duplicate notice; notice contains no actual dispatch; changed customer email.

**Benchmark value:** Unstructured document parsing linked to transactional identity, followed by continued monitoring.

### 21 Return request through resolution

**Persona and problem:** An online store must evaluate a return, coordinate shipping, and resolve it after receipt.

**Evidence — vendor documentation:** Shopify documents reviewing return requests, approval or decline, return instructions, and refunds or restocking after inspection. Exact publication date unavailable; [accessed 11 October 2026](https://help.shopify.com/en/manual/fulfillment/managing-orders/returns/self-serve-returns/management).

**Applications:** Shopify, customer email, return-label provider where applicable, warehouse receipt record, support register.

**Owner request:** “Handle return requests under our policy, gather missing information, and keep them moving until the item is inspected and the case is resolved.”

**Expected steps:**

1. Match the request to the order and items and inspect the store's actual policy.
2. Ask for required missing facts or route policy exceptions to the owner.
3. Apply the authorized decision and provide supported return instructions.
4. Track receipt and inspection; do not infer either from a customer message alone.
5. Prepare or perform the separately authorized refund/exchange action and record closure.

**Outcome:** A resolved return with consistent customer, order, and stock status.

**Errors and edge cases:** Partial return; exchange out of stock; late request; wrong item received; duplicate refund; return label unavailable in the relevant geography.

**Benchmark value:** Policy application, asynchronous physical events, and financial-action boundaries. The proposed agent advantage is exception resolution, beyond Shopify's documented return process.

### 22 Stock shortage through customer resolution

**Persona and problem:** An online store must resolve orders that cannot be fulfilled as expected.

**Evidence — vendor documentation, proposed extension:** Shopify explains overselling and how inventory at locations that do not fulfill online orders can leave a product unavailable online. [Shopify, accessed 11 October 2026](https://help.shopify.com/en/manual/inventory-and-locations/setup/selling-when-out-of-stock). Customer negotiation and supplier follow-up below are proposed, not described customer demand.

**Applications:** Illustrative Shopify inventory and orders, supplier email, purchasing Sheet, and customer messaging.

**Owner request:** “Watch orders we can't fulfill. Check replenishment or alternatives, tell me the options, and keep affected customers updated.”

**Expected steps:**

1. Identify an affected order and verify relevant location-level stock and commitments.
2. Check open replenishment orders or ask the supplier for an explicit expected date.
3. Present approved options such as waiting, substitution, partial shipment, or cancellation.
4. Record the customer's choice and coordinate the authorized operational action.
5. Recheck fulfillment and close the exception only when resolved.

**Outcome:** A defined resolution for each affected order and fewer unsupported delivery promises.

**Errors and edge cases:** Stock exists at an ineligible location; supplier gives a tentative date; inventory changes during negotiation; alternative costs more; several customers compete for one unit.

**Benchmark value:** Constraint interpretation, uncertainty communication, and multi-party waiting. This extension requires direct merchant validation.

### 23 Email support thread to one maintained ticket

**Persona and problem:** An IT provider creates duplicate tickets when customers reply within an existing email conversation.

**Evidence — direct practitioner request:** An n8n user asks for Gmail-thread-to-Jira creation and reports that each reply creates another Jira ticket. [n8n Community, 21 February 2023; duplicate-ticket follow-up 8 March 2023](https://community.n8n.io/t/trigger-a-process-when-open-new-thread-gmail/23273).

**Applications:** Gmail, Jira, and a persistent thread-to-ticket mapping; illustrative knowledge documents and customer directory.

**Owner request:** “Turn new support conversations into tickets, keep replies on the same case, and make sure unresolved customer questions have an owner.”

**Expected steps:**

1. Determine whether the message belongs to an existing support case.
2. Create a ticket only for a new case; otherwise append relevant new information.
3. Summarize the request, preserve message links, and assign the configured owner.
4. Track questions and updates between the customer and assignee without creating message loops.
5. Reconcile resolution or reopening and keep the customer-facing status current.

**Outcome:** One coherent case history with accountable ownership.

**Errors and edge cases:** Forwarded thread; new issue raised in an old thread; same issue sent to two addresses; attachment missing; duplicate delivery; reply after closure.

**Benchmark value:** Durable identity, deduplication, ambiguous case boundaries, and cross-app continuity. Thread-ID deduplication alone is a deterministic baseline, not evidence that an LLM is required.

### 24 Hosting and domain renewal coordination

**Persona and problem:** A website-maintenance provider tracks different clients, hosting plans, domain providers, and expiry dates.

**Evidence — direct practitioner account:** An HN contributor reports a custom system retaining those details and sending renewal emails. [Hacker News, 19 April 2018](https://news.ycombinator.com/item?id=16875106). Client-decision interpretation and confirmation of renewal are proposed extensions.

**Applications:** Illustrative client register in Sheets or CRM, Gmail, provider account or statements, Drive; GoDaddy is an example provider, not the source's stated choice.

**Owner request:** “Keep client hosting and domain renewals on track. Get their decision in time, follow up on unanswered requests, and confirm what actually renewed.”

**Expected steps:**

1. Reconcile each service's provider, owner, expiry, and current renewal status.
2. Check whether existing instructions already authorize renewal or discontinuation.
3. Request an outstanding decision and track reminders against the real deadline.
4. Route the decision to the authorized renewal action or responsible person.
5. Verify the updated expiry or documented cancellation and close the case.

**Outcome:** A current renewal register and visible unresolved services.

**Errors and edge cases:** Auto-renew already enabled; billing fails; service transferred; owner changed; duplicate provider email; ambiguous “don't renew that”; renewal approved but not executed.

**Benchmark value:** Long-horizon follow-through and distinguishing intention, attempted action, and confirmed outcome. Purchasing or canceling a service requires the owner's applicable authorization.

## Interpretation and discovery follow-up

**Demand remains to be validated.** Public posts demonstrate that specific people describe these problems; they do not establish how frequently a target segment experiences them, how much they would pay, or whether existing tools already solve enough of the problem. Vendor-only ecommerce and onboarding coverage is particularly suitable for direct follow-up interviews.

**The archetypes deliberately span different operating environments.** An owner working in Sheets and messaging has a different starting point from an established ATS or service-management user. Interviews should establish the real system of record, who has authority to act, and whether the owner wants full execution, drafts, or exception handling.

**Persistent operation needs a fair baseline.** Existing automation can store state and wait for events. Customer value would have to come from easier setup, better interpretation of changing business context, lower maintenance, or more complete outcomes. This report identifies those hypotheses without assigning a performance score.

**The evidence is uneven in age and proximity to the buyer.** Historical HN and automation-community accounts are useful for process shape and failure modes; they are not current product-capability evidence. Several discussions include vendor promotion. Their promotional replies are not treated as independent proof of customer success. No useful Product Hunt evidence was needed to fill the catalog, and no claim of comprehensive platform coverage is made.

The next discovery step is to interview practitioners from these archetypes and walk through a recently completed and a recently failed instance of the relevant workflow. Ask for the actual handoffs, time spent, exception frequency, current tools, acceptable stopping conditions, and what the owner would authorize the agent to do. Those interviews can validate or revise the proposed extensions before application analysis and benchmark scenario design begin.
