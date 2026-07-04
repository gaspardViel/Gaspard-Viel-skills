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
- **Notification/relance mechanics**: no automated deadline-based reminders exist in the product. Follow-up is a **manual, instructor-triggered** action:
  - The instructeur uses a per-dossier **"messagerie"** (a threaded message log centralizing all exchanges with that usager) to ask for missing/corrected pieces, or clicks a dedicated **"demander une correction"** action ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).
  - Each correction request/message triggers an **email to the usager**; there is no scheduled/automatic re-reminder if the usager doesn't respond — the instructeur must act again manually ([Tutoriel instructeur](https://doc.demarches-simplifiees.fr/tutoriels/tutoriel-instructeur)).

**Takeaway**: a two-party (usager/instructeur) model with binary "obligatoire" fields, a simple linear status machine, and manual (not scheduled) relances triggered on a threaded message log. No automatic nagging — completeness checking is a deliberate, one-at-a-time human action.

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

**Anti-patterns to avoid**
- Don't copy DocuSign/Salesforce's **multi-actor approval workflow machinery** (submitter → reviewer → approver, role-scoped visibility, disbursement conditionals) — our Projets are single-person/single-household, so a jury or administration is an external Relance target, not a workflow participant we model states for. Keep our status machine as flat as démarches-simplifiées' usager-facing view (obtenu / manquant / à corriger), not as branched as an instructeur's internal tooling.
- Don't build fully automatic, unattended relance scheduling as the default — every reviewed platform that automates reminders (DocuSign, Fluxx per grant-management marketing copy) does so in a *formal, third-party-facing* submission context with legal/financial stakes; our Ressources de cadrage relances (a web search for guidance) have no such stakes and should stay agent/user-triggered, not silently scheduled, to avoid the tool acting as an unwanted nag.
- Avoid conflating **Pièce justificative** (submitted to a third party) and **Ressource de cadrage** (consulted only by the user) under one "document" status model — none of the surveyed tools need this split because they only handle the submission side; we should keep Suivi de complétude reporting the two subtypes distinctly rather than forcing a Ressource de cadrage into a submission-shaped status like "accepté/refusé", which doesn't apply to it.
