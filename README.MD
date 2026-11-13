# Data Access Decision Report

**EduConnect Ghana | DevOps Engineering | Monday morning ticket review**

## Summary of Decisions

| Req. | Requester | Request | Decision | Lifecycle Stage |
|---|---|---|---|---|
| 1 | Sarah Owusu, Marketing Manager | Full student database for referral campaign | Deny (offer conditional alternative) | Share |
| 2 | David Mensah, Head of Product | AWS database access for DataInsights Inc. (US) | Deny as submitted; conditional path after controls | Share (cross-border) |
| 3 | Comfort Asante, Customer Support Lead | Full deletion for James Boateng | Approve, with narrow retention | Destroy (Archive for retained items) |

## Request 1: Marketing Campaign

| Field | Detail |
|---|---|
| **Decision** | **DENY** the request as submitted. Offer a conditional alternative (see Action Steps). |
| **Lifecycle Stage** | **Share**. Data would leave the system of record and move into a marketing context. |
| **Data Classified** | Email addresses (CONFIDENTIAL); phone numbers (CONFIDENTIAL, treated as PII); names and enrollments (INTERNAL per request, but tied to identifiable students). |
| **Current Access** | INTERNAL only. The request needs CONFIDENTIAL access, which Sarah does not hold. |

### Justification

- **Classification policy:** Email and phone are CONFIDENTIAL. Access to them requires a documented approval above the requester's current level. An INTERNAL role does not qualify.
- **Least privilege:** A campaign needs contact only for students who opted in to marketing. It does not need the full database, course enrollments, or phone numbers. Granting the full export gives access well beyond the purpose.
- **Purpose limitation and consent (Ghana DPA):** Students gave data for platform services, not for referral marketing. Using it for marketing requires a lawful basis, typically consent, and must be disclosed.
- **Urgency is not a control.** The Friday deadline does not change the access rules.
- **Classification inconsistency:** The request labels names as INTERNAL while the data is identifiable. Classify the export as CONFIDENTIAL in the ticket.

### Action Steps

1. Reply to Sarah: request denied as submitted. Explain the reason in one short paragraph.
2. Offer a conditional path: marketing gets a campaign list containing only students who have opted in to marketing messages, with email and phone limited to that list.
3. Route the request through the data owner (Data Protection Officer or privacy lead) for written approval. Create a new access grant for this campaign only, with an expiry date after the campaign ends.
4. Prefer a platform-side campaign tool, where the marketing team sends messages through the system without downloading contact data.
5. Pseudonymize or minimize the file: remove enrollments and any field the campaign does not use.
6. Log the approval, the export, and the access grant. Set deletion of the campaign export after the campaign closes.

**Consult before final decision:** Data Protection Officer or privacy lead (lawful basis and consent); Legal counsel (Ghana DPA compliance); data owner for student records; Security lead (export controls); Sarah's manager (business case).

## Request 2: Analytics Partnership

| Field | Detail |
|---|---|
| **Decision** | **DENY** the request as submitted. Do **not** share login credentials or direct production database access with DataInsights Inc. A conditional path exists after the controls below are in place. |
| **Lifecycle Stage** | **Share** (cross-border transfer to an external processor). Access to the AWS database would also involve **Use** at the third party. |
| **Data Classified** | Activity logs (INTERNAL); student profiles (CONFIDENTIAL). Combined data about identifiable students is CONFIDENTIAL. |
| **Compliance Note** | Company is subject to the Ghana Data Protection Act (Act 843). Transfers outside Ghana are regulated. |

### Justification

- **Credential sharing is a control failure.** Sending login credentials by email, and giving an external party direct access to production AWS, removes accountability, logging, and the ability to revoke access quickly.
- **Least privilege:** Analytics does not need live production access or identifiable profiles. It needs a de-identified or aggregated extract.
- **Cross-border transfer:** Ghana DPA restricts transfers of personal data outside Ghana unless adequate protection is ensured or another legal basis applies. The company must confirm the transfer mechanism before any data leaves the country.
- **Contract gap:** A signed partnership is not enough. A data processing agreement must define purpose, scope, security obligations, sub-processors, breach notice, retention, and deletion at the end of the contract.
- **Purpose and consent:** Students must be informed that their learning activity may be analyzed by a third party. Confirm whether consent or notice covers this use.
- **Security concern:** Credentials attached to an email are already exposed. Treat them as compromised.

### Action Steps

1. Do not use the attached credentials. Revoke or rotate them immediately, and check the access logs for any use since they were shared.
2. Report the credential exposure to the security lead under the incident process.
3. Reply to David: access denied as submitted. Offer the conditional path below.
4. Conditional path (all required before any data is shared): (a) signed DPA with DataInsights; (b) legal confirmation of the cross-border transfer basis; (c) privacy impact assessment; (d) confirmed student notice or consent covering the analysis; (e) security review of the vendor.
5. Provide data by export, not access. Build a de-identified or pseudonymized extract of activity logs, with no names, emails, or profile fields. Deliver it through an encrypted transfer with an expiry date.
6. Use a separate AWS account or read-only replica for the extract. No vendor IAM users on production.
7. Log every extract, transfer, and deletion. Require the vendor to certify deletion when the contract ends.

**Documentation required before approval:** signed DPA; transfer assessment; privacy impact assessment; student notice text; data extract specification; security review result; approval record from the data owner.

## Request 3: Archive and Deletion

| Field | Detail |
|---|---|
| **Decision** | **APPROVE** the deletion, with a narrow retention exception for records the law requires. Complete within the statutory response window. |
| **Lifecycle Stage** | **Destroy** for personal and profile data. **Archive** for any legally required financial or tax records, held under restricted access. |
| **Data Classified** | All student data for this account (CONFIDENTIAL). Payment records and any academic records are CONFIDENTIAL and subject to specific retention rules. |
| **Current Status** | Account inactive for six months; course completed; no pending payments. No open obligation prevents deletion. |

### Justification

- **Can the request be fulfilled?** Yes. Under the Ghana DPA, a data subject may request erasure of personal data. The request is valid, and the account has no open obligation that blocks deletion.
- **Retention exception:** Financial and payment records may be kept where a tax or accounting obligation requires it. Keep only the minimum needed (transaction ID, date, amount, and the legal basis), not the full profile.
- **Academic records:** Confirm with the academic or accreditation owner whether completed-course records have a legal or institutional retention requirement. Do not assume they must be kept. Keep only what is required, and only in the minimum form.
- **Deletion scope:** Deletion must reach production, backups on their rotation schedule, analytics stores, support tickets, logs, and any third-party processor that holds the data.
- **Least privilege during the process:** Only the people who need to execute the deletion should have access to the record while it is in progress.
- **Timing and verification:** Verify the requester's identity before deletion. Respond within the timeline set by the DPA and the company policy, and confirm the outcome in writing.

### What Can Be Retained and Why

| Data Item | Retain? | Basis and Limit |
|---|---|---|
| Full profile (name, email, phone, address) | No | No legal requirement. Delete. |
| Activity logs and learning data | No | No retention requirement identified. Delete or fully anonymize. |
| Payment and transaction records | Only if required | Tax or accounting obligation. Minimum fields, restricted access, fixed retention end date. |
| Support tickets mentioning the student | No | Redact personal details, or delete. |
| Backups | Expire on schedule | Do not restore. Mark the record for deletion so it is not reloaded. |

### What Customer Support Should Do

1. Log the request with the date received, the requester, and the deadline.
2. Verify James Boateng's identity using the account's existing verification method. Do not collect extra information unless needed.
3. Send the request to the data owner and the privacy lead for approval of the retention list.
4. Pass the approved deletion request to DevOps. Include the list of systems and processors.
5. Confirm deletion in every system, including backups scheduled for expiry, and notify processors that hold the data.
6. Send James a written confirmation that states what was deleted and what, if anything, was retained and why.
7. Close the ticket with the evidence: deletion logs, retention decision, and confirmation sent.

## Notes

This report reflects general data-protection practice and the classification policy as described in the ticket. Confirm the specific Ghana DPA provisions, retention periods, and cross-border transfer rules with legal counsel before acting.

## Approval and Signature

| Field | Detail |
|---|---|
| **Prepared and signed by** | **Igizeneza Serge Benit** |
| **Signature** | */Igizeneza Serge Benit/* |
| **Date** | 13 November 2026 |
| **Location** | Kigali, Rwanda |
