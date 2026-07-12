# SharePoint Build

This document captures the current SharePoint build pattern for the HR Knowledge Desk agent.

## Current Role Of SharePoint

SharePoint currently has three roles:

1. It stores the approved HR source content used to prepare the agent knowledge files.
2. It provides the operational user interface for workflow outputs through SharePoint lists.
3. It displays the validated Microsoft Fabric / Power BI current-state report on the SharePoint site home page so approved documents, workflow list records, and reporting can be shown in one place.

The current site is:

`https://clarityarc.sharepoint.com/sites/HRKnowledgeDeskDemo`

## Current SharePoint Components

| Component | Current purpose |
| --- | --- |
| Approved HR source libraries | Store the governed HR content used to prepare approved knowledge files for the agent. |
| Escalation Log | Operational review list for general HR escalations and confidential workplace concern routing records. |
| Knowledge Gap Backlog | Operational review list for missing or weak source guidance. |
| Feedback Log | Operational review list for feedback about answer quality, source-reference quality, and usefulness. |
| SharePoint home page report embed | Displays the validated `HR Knowledge Desk Current State Report` from Microsoft Fabric / Power BI for demo and HR operations review. |

SharePoint is the current workflow output interface and demo presentation surface. HR reviewers and knowledge owners use the SharePoint list views directly to review records created by the agent workflows. For the demo, the current-state report can be shown from the SharePoint site home page so participants can see approved documents, workflow list records, and reporting in one place.

## Approved Source Content

The approved HR source documents are maintained from the approved SharePoint content set. In the current Copilot Studio build, the SharePoint folder could not be referenced directly from the Knowledge configuration, so approved documents were uploaded individually to the agent knowledge set.

This means the Copilot Studio Knowledge tab is the configured knowledge source for agent answers, while SharePoint remains the governed source-document library and demo surface. Some authored topics also include source links that point to the SharePoint file because topics cannot reference files inside the Knowledge tab as user-openable links. The support team must keep the uploaded Knowledge files and the linked SharePoint files aligned so the agent answer and the source link refer to the same approved content version.

The SharePoint source libraries should contain the same approved source documents that are uploaded to the Copilot Studio Knowledge tab.

Current SharePoint source files:

| SharePoint area | Files |
| --- | --- |
| Approved HR Knowledge | `Approved_HR_Source_Index.docx`; `Benefits_Guide.docx`; `Code_of_Conduct.docx`; `Payroll_FAQ.docx`; `Time_Off_Policy.docx` |
| Form Templates | `Time_Off_Request_Form.pdf`; `Name_Change_Form.pdf` |
| Manager Guidance | `Onboarding_Checklist.docx`; `Manager_Role_Change_Procedure.docx` |

Current uploaded knowledge files include:

- `Onboarding_Checklist.docx`
- `Benefits_Guide.docx`
- `Payroll_FAQ.docx`
- `Approved_HR_Source_Index.docx`
- `Name_Change_Form.pdf`
- `Time_Off_Request_Form.pdf`
- `Manager_Role_Change_Procedure.docx`
- `Time_Off_Policy.docx`
- `Code_of_Conduct.docx`

Each uploaded file showed `Ready` in the Copilot Studio Knowledge section. Web search remains disabled.

The approved source pattern can include:

| Source area | Purpose |
| --- | --- |
| Approved HR Knowledge | General approved HR policy and process content. |
| HR Forms and Links | Approved HR forms, links, and process entry points. |
| Manager Guidance | Approved manager-facing HR guidance, included only when it is acceptable for the full agent audience under the service-account model. |

Future agents should use the same principle: only connect or upload approved source libraries, approved documents, or approved source records that the agent is allowed to use.

Because the agent reads approved content from its configured knowledge set rather than each user's own permissions, include only content that is appropriate for the agent's full audience. Keep restricted or manager-only content that must not be broadly visible in a separate library, separate uploaded source set, or separate agent.

## Operational Lists

| List | Purpose | Current status |
| --- | --- | --- |
| Escalation Log | Records general HR escalations and confidential workplace concern routing records. | Validated |
| Knowledge Gap Backlog | Records missing, unclear, stale, incomplete, or inaccessible source gaps. | Validated |
| Feedback Log | Records feedback about answer quality, source quality, and user usefulness. | Validated |

## List Design Standards

Use these standards for all workflow output lists:

- Include a workflow-generated visible ID such as `ESC-...`, `GAP-...`, or `FB-...`.
- Include a real SharePoint Date and Time field for the workflow timestamp.
- Include `Requester Email` so the business owner can follow up using Outlook or Entra ID.
- Keep summaries brief and avoid unnecessary personal information.
- Store source references as source names or `None` / `Not included`.
- Store workflow confirmation status so reviewers know the record was created by a confirmed workflow.
- Include `Review Status` as the shared HR operations lifecycle field for list review and Microsoft Fabric / Power BI reporting.
- Use consistent category, subcategory, route, risk, and status values so reporting can be built and validated cleanly.

## Review Status Standard

Add `Review Status` to each operational workflow list before building or refreshing Microsoft Fabric / Power BI reporting.

| Setting | Current standard |
| --- | --- |
| Column name | `Review Status` |
| Data type | Choice |
| Required | No for the current build |
| Enforce unique values | No |
| Choices | `New`; `In progress`; `Resolved` |
| Display style | Drop-down menu |
| Allow fill-in choices | No |
| Default value | `New` |
| Add to default view | Yes |
| Flow mapping required | No, if the SharePoint default value is applying correctly |

Use `Review Status` to show the human review lifecycle of each item:

| Value | Meaning |
| --- | --- |
| New | Created by the agent and waiting for first HR or owner review. |
| In progress | HR or the assigned owner is actively reviewing or working the item. |
| Resolved | No further action is currently needed. |

Keep existing workflow-specific status fields. For example, `Workflow Status` confirms the flow successfully created or updated the record, while `Review Status` tells HR whether the item still needs human attention.

The current build validated this pattern by adding `Review Status` to the operational lists with a default value of `New`. A new HR escalation record created by the agent appeared in SharePoint with `Review Status` populated as `New`, confirming that the flow does not need to explicitly map this field as long as the SharePoint default continues to apply.

## Escalation Log Fields

| Field | Data type | Purpose |
| --- | --- | --- |
| Escalation ID | Single line of text | Unique workflow-generated escalation reference. |
| Submitted Date Time | Date and Time | Date/time the escalation was created. |
| Date Submitted | Date and Time or legacy field | Legacy/compatibility field if still present. The current standard is to use `Submitted Date Time` as the primary timestamp. |
| Requester Email | Single line of text | Authenticated requester email used by HR to follow up. |
| Question Summary | Multiple lines of text | Brief summary of the routed HR question or concern. |
| User Type | Single line of text | Employee, manager, or other inferred user type. |
| Category | Single line of text | Broad HR category. |
| Subcategory | Single line of text | More specific HR topic. |
| Expected Route | Single line of text | Intended review path, such as General HR or Workplace Concern / Confidential HR Review. |
| Risk Level | Single line of text | Low, Medium, or High. |
| Sensitivity Flag | Yes/No or text-compatible value | Indicates whether the record is sensitive. |
| Source Status | Single line of text | Missing, Weak, Found, Sensitive, or similar workflow value. |
| Source References | Multiple lines of text | Approved source names or `None` / `Not included`. |
| Workflow Status | Single line of text | Confirmation status from the workflow. |
| Review Status | Choice | Human review lifecycle: New, In progress, or Resolved. Defaults to New. |
| Recommended Next Action | Multiple lines of text | Suggested owner action. |

### Escalation Log Route Standards

| Scenario | Expected route | Source status | Risk level | Sensitivity flag |
| --- | --- | --- | --- | --- |
| General unsupported HR question | General HR | Missing or Weak | Medium unless clearly low or high | False |
| Confidential workplace concern | Workplace Concern / Confidential HR Review | Sensitive | High | True |

For confidential workplace concerns, store only the minimum triage metadata needed for HR follow-up. Do not store detailed allegations collected in chat.

## Knowledge Gap Backlog Fields

| Field | Data type | Purpose |
| --- | --- | --- |
| Gap ID | Single line of text | Unique workflow-generated gap reference. |
| Date Logged | Date and Time | Date/time the knowledge gap was logged. |
| Requester Email | Single line of text | Authenticated requester email for follow-up if needed. |
| Category | Single line of text | Broad HR topic area. |
| Gap Type | Single line of text | Missing source, unclear source, stale source, conflicting source, or similar. |
| Question Summary | Multiple lines of text | Brief summary of the unanswered or unsupported question. |
| Source References | Multiple lines of text | Source names if relevant, otherwise `None`. |
| Risk Level | Single line of text | Low, Medium, or High. |
| Owner | Single line of text | Knowledge owner or review group. |
| Priority | Single line of text | Review priority. |
| Recommended Fix | Multiple lines of text | Suggested source update or content creation action. |
| Status | Single line of text | Open, in review, closed, or similar. |
| Review Status | Choice | Human review lifecycle: New, In progress, or Resolved. Defaults to New. |

### Knowledge Gap Standards

Use the Knowledge Gap Backlog when approved source content is missing, unclear, stale, weak, conflicting, inaccessible, broken, or incomplete.

The `Category` should describe the business topic where possible, not only the source document title. For example, use `Scheduling` for missing compressed workweek guidance rather than only `Time Off Policy`.

## Feedback Log Fields

| Field | Data type | Purpose |
| --- | --- | --- |
| Feedback ID | Single line of text | Unique workflow-generated feedback reference. |
| Date Submitted | Date and Time | Date/time the feedback was captured. |
| Requester Email | Single line of text | Authenticated requester email. |
| Category | Single line of text | HR topic related to the feedback. |
| Feedback Type | Single line of text | Feedback classification, such as helpful, missing source, unclear source, or incorrect answer. |
| Feedback Rating | Number | User-provided or inferred rating. |
| Question Summary | Multiple lines of text | Brief summary of the original user question. |
| Answer Summary | Multiple lines of text | Brief summary of the answer being reviewed. |
| Source Reference | Multiple lines of text | Source used in the answer, or `Not provided`. |
| User Type | Single line of text | Employee, manager, or other inferred user type. |
| Escalation ID | Single line of text | Related escalation reference when applicable. |
| Status | Single line of text | Open, reviewed, closed, or similar. |
| Review Status | Choice | Human review lifecycle: New, In progress, or Resolved. Defaults to New. |
| Gap Created | Yes/No or single line of text | Indicates whether feedback resulted in a knowledge gap. |

### Feedback Standards

Use the Feedback Log when a user says an answer was helpful, not helpful, incorrect, unclear, missing a source, or otherwise needs review.

Feedback can stand alone or be connected later to an escalation or knowledge gap. Do not require the user to understand internal list names or workflow names.

## Date And Time Rule

Use SharePoint `Date and Time` columns for workflow timestamps.

In the flow, initialize the timestamp with:

```text
utcNow()
```

Then map that value directly to the SharePoint Date and Time column. Do not pre-convert the value to Mountain Time inside the flow. SharePoint should handle display according to site and user regional settings.

Current timestamp field standards:

| List | Primary timestamp field |
| --- | --- |
| Escalation Log | Submitted Date Time |
| Knowledge Gap Backlog | Date Logged |
| Feedback Log | Date Submitted |

## Requester Email Rule

Each operational list should include `Requester Email`.

The agent should provide the authenticated user email when available. Do not store only the display name, because HR needs a reliable lookup key for Outlook or Entra ID.

## Current Validation Evidence

| List | Evidence |
| --- | --- |
| Escalation Log | General HR escalation validated with `ESC-20260614-010357`; workplace concern routing validated with `ESC-20260614-004007`. |
| Knowledge Gap Backlog | Knowledge gap logging validated with `GAP-20260613-162254`. |
| Feedback Log | Feedback capture validated with `FB-20260614-005930`. |
| Review Status default | New HR escalation record created by the agent populated `Review Status` as `New`, confirming the SharePoint default works without a flow mapping update. |

## Build Cautions

- If a SharePoint list column is renamed or its data type changes, reselect the list in the Power Automate action and repopulate the mapped fields.
- If a Date and Time field displays unexpectedly, check that the flow sends `utcNow()` as an expression and that the target SharePoint column is a real Date and Time field.
- If requester email is blank, a display name, or has extra punctuation, check the tool input and the Power Automate field mapping.
- Build or revise reporting only after list schemas and field values have stabilized.
