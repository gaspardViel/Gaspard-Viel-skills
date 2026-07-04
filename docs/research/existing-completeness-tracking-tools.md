# Prior art: how existing tools track document completeness and chase missing items

Prior-art research for the Projet-tracking skill bank. Goal: borrow proven vocabulary/mechanics for **Suivi de complétude** (completeness tracking) and **Relance** (reminder/follow-up) rather than reinventing them. Findings below are drawn only from vendor-published documentation/help centers.

---

## 1. démarches-simplifiées.fr (French government dossier platform)

Official docs: `doc.demarches-simplifiees.fr` (documentation site for demarche.numerique.gouv.fr).

- **Dossier status vocabulary** — a single dossier moves through a small, linear state machine:
  - **"En construction"** — the usager (applicant) has started/submitted the dossier and can still edit it; this is the stage where the instructeur checks completeness ("Ce stade permet à l'instructeur de s'assurer de la complétude du dossier") ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
  - **"En instruction"** — instructeur locks the dossier from further edits once it's complete, by clicking "Passer en instruction" ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
  - Terminal states set by the instructeur: **"Accepté"**, **"Refusé"** (requires a written motivation visible to the usager), **"Classé sans suite"** (withdrawn without decision, also requires a motivation) ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
  - **"À corriger"** — a side-state layered on top of "en construction": the instructeur requests a correction from the usager (e.g., fix an attachment); the usager gets an email, the dossier is flagged "à corriger" in their view and "en attente" (pending) correction on the instructeur's dashboard ([search results synthesizing doc.demarches-simplifiees.fr tutoriel-instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
- **Required elements ("pièces justificatives")** are modeled as attachment-type fields on the form, configured per-démarche by the administrateur:
  - A field can be flagged **"Obligatoire"**; if so, the usager cannot submit the dossier until it's filled ([Tutoriel administrateur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-administrateur)).
  - Attachment fields can specialize by document nature (e.g., a "Relevé d'identité bancaire (RIB)" field type auto-extracts IBAN/BIC/holder name for the instructeur) ([Tutoriel administrateur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-administrateur)).
  - Identity-document attachments are auto-watermarked and auto-deleted once the dossier is closed (GDPR minimization) ([Tutoriel usager](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-usager)).
- **Notification/relance mechanics for the *correction* loop are manual, instructor-triggered**:
  - The instructeur uses a per-dossier **"messagerie"** (a threaded message log centralizing all exchanges with that usager) to ask for missing/corrected pieces, or clicks a dedicated **"demander une correction"** action ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
  - Each correction request/message triggers an **email to the usager**; there is no scheduled/automatic re-reminder if the usager doesn't respond — the instructeur must act again manually ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
- **One relance path *is* automatic, but it targets abandonment, not correction**: an unsubmitted draft ("brouillon"/"en construction" dossier the usager never deposited) is auto-deleted after an administrator-configured retention period (max 36 months, for GDPR data-minimization). Before deletion, the platform emails the usager automatically — one warning roughly a month ahead, and again close to the deadline (community reports cite a further warning ~14 days out) — inviting them to submit the draft so it survives ([tutoriel-usager.md, via doc.demarches-simplifiees.fr GitHub mirror](https://raw.githubusercontent.com/betagouv/doc.demarches-simplifiees.fr/main/tutoriels/tutoriel-usager.md); [FAQ](https://www.demarches-simplifiees.fr/faq)). The product team itself considers this cadence insufficient: an open GitHub issue, "Améliorer emails et relances pour prévenir qu'un dossier en construction va expirer / être supprimé," proposes adding more graduated reminders rather than a single warning ([Issue #10488, demarches-simplifiees/demarches-simplifiees.fr](https://github.com/demarches-simplifiees/demarches-simplifiees.fr/issues/10488)).

**Takeaway**: a two-party (usager/instructeur) model with binary "obligatoire" fields, a simple linear status machine, and manual (not scheduled) relances for the correction loop, triggered on a threaded message log — but a genuinely automatic, deadline-based relance does exist for the narrower "you're about to lose this draft" case. Even the vendor's own backlog treats a single time-boxed warning as too thin, which is a useful signal: a real deadline-based Relance wants more than one graduated nudge, not just a T-minus-1 email.

---

## 2. Salesforce.org Grants Management (grant/subvention platform)

Official docs: `help.salesforce.com` Grants Management product documentation and Salesforce's own Trailhead modules for the same product.

- Their unit for "a required element requested from an applicant" is the **"Funding Requirement"** record, tracked in a dedicated **Requirements tab** on a Funding Request: "The Requirements tab contains everything you're requesting the grantseeker provide during the review process" ([Meet Grants Management](https://help.salesforce.com/s/articleView?id=sfdo.gm_overview.htm&language=en_US&type=5)).
- **Completion-status model**: each Requirement carries a **Status** field, with **"Open"** used while work hasn't started; the grantmaker creates requirements, the grantseeker (applicant) responds through a self-service **grantee portal**, and the grantmaker then processes/closes the requirement ([Fund Management: Requirements and Disbursements Guide](https://trailhead.salesforce.com/content/learn/modules/grants-management-tools-and-processes/manage-requirements-and-disburse-funds)).
- Requirements aren't just informational — they can gate money: a Requirement can be **linked to a Disbursement record**, making a payment conditionally blocked until that requirement is satisfied, which the vendor frames as "an important control and audit trail for finance and program staff" ([Fund Management: Requirements and Disbursements Guide](https://trailhead.salesforce.com/content/learn/modules/grants-management-tools-and-processes/manage-requirements-and-disburse-funds)).
- **Reminder mechanics**: the documented mechanism is portal-based visibility + role-based automated emails ("SmartSimple/Salesforce can send automated emails to individuals and groups tailored to their specific role" — see product docs generally); the granular, dated-deadline reminder cadence is not spelled out in the public docs surfaced here, unlike DocuSign's explicit day-offset settings below.

**Takeaway**: the "Requirement record with a Status, tied to a downstream consequence (a blocked payment)" pattern is worth borrowing conceptually — it shows how a Suivi de complétude gains teeth when a missing Élément requis visibly blocks a next step, not just sits as a checkbox.

---

## 3. DocuSign (e-signature / document-collection platform)

Official docs: `developers.docusign.com` (eSignature REST API) and `support.docusign.com`.

- **Status vocabulary** for an envelope (their unit = one send-for-signature request, which can bundle several required documents): valid values are **created, sent, delivered, signed, completed, declined, voided, deleted** ([Envelope status codes](https://developers.docusign.com/docs/esign-rest-api/esign101/concepts/envelopes/status-codes/)).
  - **Declined** = a recipient actively refused/cancelled their part.
  - **Voided** = the sender (document holder) cancelled it, or it hit its expiration ([Envelope Status](https://support.docusign.com/s/document-item?bundleId=oeq1643226594604&topicId=wdm1578456348227.html&_LANG=enus&language=en_US)).
  - These are explicitly **terminal** states — the reason (declined vs. expired) is preserved in envelope history for audit ([How can I differentiate between Expired and Voided envelopes?](https://support.docusign.com/en/articles/How-can-I-differentiate-between-Expired-and-Voided-envelopes)).
- **Reminder mechanics are the most mature of everything surveyed** — fully declarative, threshold/deadline-based, and configurable per-envelope or account-wide:
  - **Automatic reminders**: sender (or an account-wide default) sets "how many days to wait until sending the first reminder" and "how many days to wait between subsequent reminders" (integers 1–999; entering 0 for the interval means no reminders after the first) ([Common API Tasks: Add Reminders and Expiration to an Envelope](https://www.docusign.com/blog/dsdev-common-api-tasks-add-reminders-and-expiration-to-an-envelope)).
  - **Expiration** is a separate, paired setting: envelopes expire after N days from send (default 120), with an optional warning sent to the signer M days before expiry ([Common API Tasks: Add Reminders and Expiration to an Envelope](https://www.docusign.com/blog/dsdev-common-api-tasks-add-reminders-and-expiration-to-an-envelope)).
  - Admins can lock these settings account-wide or leave them editable per-send.

**Takeaway**: DocuSign is the strongest model for a *deadline-driven, automatic* Relance — "N days after request, remind; every M days thereafter; hard-stop/expire after X days" is a clean, reusable cadence spec, independent of the (irrelevant to us) legal-signature workflow it's wrapped in.

---

## 4. Dropbox Sign (formerly HelloSign) — document-collection / e-signature platform

Official docs: `developers.hellosign.com` and `help.dropbox.com`.

- **Status vocabulary** for a "Signature Request" (their per-recipient signing task): status codes include **awaiting_signature**, **signed**, **declined**, **expired**, among others listed on the Constants reference ([Constants](https://developers.hellosign.com/api/reference/constants/)).
- **Reminder mechanics are explicitly manual-trigger, API-gated**, unlike DocuSign's fully declarative schedule:
  - A dedicated **Send Request Reminder** endpoint sends a one-off email nudge to a specific signer ([Send Request Reminder](https://developers.hellosign.com/api/reference/operation/signatureRequestRemind/)).
  - Hard rate-limit: **you cannot send a reminder within 1 hour of the last reminder**, whether that prior reminder was manual or automatic — preventing relance spam ([Send reminders with the Dropbox Sign API](https://help.dropbox.com/integrations/send-signature-request-reminders-api)).
  - Reminders cannot be used on embedded signature requests (a signing flow embedded directly in a third-party app) ([Send reminders with the Dropbox Sign API](https://help.dropbox.com/integrations/send-signature-request-reminders-api)).

**Takeaway**: the "reminder is its own API action, callable manually or by a scheduler you build, but throttled with a minimum cool-down" pattern is a good middle ground between démarches-simplifiées' (purely manual) and DocuSign's (purely declarative schedule) — it fits an agent-driven Relance well: the agent decides *when* to relance, but the mechanic itself enforces a floor so it can't nag hourly.

---

## 5. Notion & Airtable (optional — official docs only)

Included only because both vendors publish first-party documentation of a relevant pattern; neither product is purpose-built for dossier completeness, so treat these as a generic building-block reference rather than a domain match.

- **Airtable — Rollup fields**: a Rollup field aggregates a formula/calculation over records linked from another table ([Rollup field overview](https://support.airtable.com/docs/rollup-field-overview)). The documented pattern for a completion percentage is a rollup that counts linked items meeting a condition, divided by total linked items (e.g. `COUNTA(values)/COUNTALL(values)`), which the UI can then render as a **progress bar** with a numeric percentage overlay ([Airtable Blog: Track your work with progress bars](https://blog.airtable.com/track-your-work-with-progress-bars/) — Airtable's own product blog, not a third party). This is the cleanest off-the-shelf model of "Suivi de complétude as a derived rollup" among everything surveyed: completeness is never stored directly, only computed from the state of each linked Élément requis.
- **Notion — Database automations**: automations are **trigger + condition + action** rules scoped to a database; one of the built-in actions is "Send email," which the vendor's own docs frame explicitly around chasing incomplete work — e.g. sending a reminder email "if you want to send an email to someone to remind them to complete their past-due task" ([Database automations](https://www.notion.com/help/database-automations)). Separately, Notion's **date-property reminders** notify a person at a configured offset from a date value on a page ([Reminders](https://www.notion.com/help/reminders)), and **Relations & rollups** let one database aggregate a computed value (e.g. a count of missing items) from records linked in another database ([Relations & rollups](https://www.notion.com/help/relations-and-rollups)).

**Takeaway**: neither tool models "required element" or "completeness" as a domain concept — they only offer the generic primitives (a linked-record rollup for aggregation, a date-triggered automation for the nudge). This confirms our domain model (Élément requis / Suivi de complétude / Relance as named concepts) is doing real work that a bare database tool leaves to the user to invent by hand each time.

---

## Implications for our design

**Vocabulary to borrow**
- A **per-Élément-requis status field** (démarches-simplifiées' "obligatoire" flag + Salesforce's per-Requirement "Status") maps directly onto our Suivi de complétude: each Élément requis should carry an explicit status, not just an implicit "present/absent" inferred from file existence.
- Distinguish **"missing" from "flagged as needing correction"** — démarches-simplifiées' "à corriger" state (something was submitted but is wrong) is a useful third state beyond simple obtenu/manquant, and maps naturally onto our Pièce justificative subtype (a jury/administration can reject a submitted piece, which our model should represent, not just "obtenu" vs "manquant").
- Track a **reason/motivation string** on terminal negative outcomes (démarches-simplifiées' refus/classement-sans-suite motivation, DocuSign's void reason) — worth carrying on a Relance history so the user can see *why* something was rejected, not just that it was.

**Mechanics to adopt**
- **Gate downstream steps on completeness**, à la Salesforce's Requirement→Disbursement link: a Projet's "ready to submit" or "ready to hand to the jury" state should be computed from, and visibly blocked by, outstanding Éléments requis — completeness should have a consequence, not just be a dashboard percentage.
- **Threshold/deadline-based Relance cadence** (DocuSign's "N days after request, then every M days, hard stop at X days") is the right shape for an *automatic* Relance in an agent context — it's simple to encode as a per-Élément-requis reminder policy attached to a deadline the user cares about (e.g., a submission date for a VAE dossier).
- **Cool-down floor on relances** (Dropbox Sign's 1-hour minimum between reminders) is a cheap, valuable guardrail to prevent an agent from re-sending a relance (email/search) every time it's invoked — encode a "don't relance again before T" rule per Élément requis.
- **Centralize follow-up in one threaded log per Projet** (démarches-simplifiées' messagerie) rather than firing disconnected one-off notifications — makes the Relance history auditable and lets the user (or the agent) see what's already been asked for before asking again.
- **Compute completeness, don't store it** — Airtable's rollup pattern (a percentage derived live from linked-record status, never written down independently) is the right shape for our Suivi de complétude: it should always be a read-time aggregate over each Élément requis's status, so it can never drift out of sync with the elements it's summarizing.
- **Prefer graduated reminders over a single warning** — even démarches-simplifiées' own team flags a lone T-minus-1 email as inadequate (see Issue #10488 above) and is moving toward multiple, spaced warnings. A Relance policy should default to more than one nudge before a deadline, not a single fire-and-forget notification.

**Anti-patterns to avoid**
- Don't copy DocuSign/Salesforce's **multi-actor approval workflow machinery** (submitter → reviewer → approver, role-scoped visibility, disbursement conditionals) — our Projets are single-person/single-household, so a jury or administration is an external Relance target, not a workflow participant we model states for. Keep our status machine as flat as démarches-simplifiées' usager-facing view (obtenu / manquant / à corriger), not as branched as an instructeur's internal tooling.
- Don't build fully automatic, unattended relance scheduling as the default for every kind of gap — the automatic relances we found (DocuSign/Dropbox Sign's signature reminders, démarches-simplifiées' draft-expiry warning) all fire against a concrete deadline the platform already knows (an envelope expiration date, a GDPR retention clock), never against an open-ended "you still haven't found this yet." Our Ressources de cadrage relances (e.g., a web search for guidance) usually have no such fixed deadline and should stay agent/user-triggered rather than silently scheduled, to avoid the tool acting as an unwanted nag; reserve automatic, scheduled relances for Éléments requis that are actually pinned to a real date (a submission deadline, an appointment).
- Avoid conflating **Pièce justificative** (submitted to a third party) and **Ressource de cadrage** (consulted only by the user) under one "document" status model — none of the surveyed tools need this split because they only handle the submission side; we should keep Suivi de complétude reporting the two subtypes distinctly rather than forcing a Ressource de cadrage into a submission-shaped status like "accepté/refusé", which doesn't apply to it.
