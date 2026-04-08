# Lead Capture & Auto-Assignment App
### Salesforce Declarative Automation Portfolio Project

---

## Business Problem

Sales teams often manually review and route incoming leads — a process that can take 24+ hours per assignment, introduces human error, and creates no audit trail. Without automation, reps miss timely follow-ups and managers have no visibility into lead distribution across territories.

This project automates the full lead lifecycle: from data capture and validation, through territory-based routing, to instant rep notification — all without writing a single line of code.

---

## What I Built

| Component | Type | Purpose |
|---|---|---|
| Custom Fields (`Territory__c`, `Product_Interest__c`, `Lead_Score__c`) | Data Model | Capture territory and product context on every lead |
| Validation Rule (`Require_Territory_on_Lead`) | Data Quality | Blocks saving a Lead without a Territory value |
| Lead Assignment Rule (`Territory Based Assignment`) | Automation | Auto-routes leads to the correct rep by territory |
| Record-Triggered Flow | Automation | Sends an instant email alert to the assigned rep on lead creation |
| Custom Lightning Record Page | UI/UX | Polished, grouped layout surfacing key fields for reps |

---

## Data Flow

```
Lead Created (Web Form / Manual Entry)
        │
        ▼
Validation Rule checks Territory__c
        │  ← Error shown if blank
        ▼
Assignment Rule routes Lead by Territory
        │  North → North Rep
        │  South → South Rep
        │  East  → East Rep
        │  West  → West Rep
        ▼
Record-Triggered Flow fires on creation
        │
        ▼
Email alert sent to Owner: "New Lead Assigned: [Name]"

```

---

## Components Detail

### Custom Fields — Lead Object

Three fields were added to the standard Lead object via Setup > Object Manager > Lead > Fields & Relationships:

- **Territory__c** (Picklist — Required): Values: North, South, East, West. Drives assignment rule logic.
- **Product_Interest__c** (Picklist — Optional): Values: Product A, Product B, Product C. Helps reps prioritise outreach.
- **Lead_Score__c** (Number 3,0 — Optional): Range 1–100. Supports future scoring automation and reporting.


### Lead Assignment Rule

- **Rule Name:** `Territory Based Assignment`
- **Logic:** Four rule entries match on `Territory__c` equals a specific value and assign the Lead to the corresponding rep (or queue in a multi-user org).

> **Note:** This Developer Edition org uses a single user for demonstration. In a real deployment, each rule entry would point to a different user or territory queue.
> 
> <img width="1920" height="1080" alt="Screenshot 2026-04-08 185002" src="https://github.com/user-attachments/assets/410a1bda-b317-43ac-80e8-8c450d82c7ca" />


### Record-Triggered Flow

- **Trigger:** Record created (Lead object)
- **Action:** Send Email
  - **To:** `{!$Record.Owner:User.Email}` — dynamically targets whoever the assignment rule just assigned
  - **Subject:** `New Lead Assigned: {!$Record.Name}`
  - **Body:** Includes Lead name, company, territory, and lead source
<img width="1920" height="1080" alt="Screenshot 2026-03-30 111920" src="https://github.com/user-attachments/assets/9b4913b8-3b6b-4093-ad53-f35438622c87" />


This ensures reps are notified the moment a lead lands in their queue, without any manual intervention.

### Lightning Record Page Layout

The custom page groups fields logically to reduce cognitive load for reps:

| Section | Fields |
|---|---|
| Highlights Panel | Name, Company, Status, Owner |
| Lead Details (left) | First Name, Last Name, Title, Phone, Email, Lead Source |
| Territory Info (left) | Territory__c, Product_Interest__c, Lead_Score__c |
| Activity (right) | Activity Timeline |
| Chatter (right) | Chatter Feed |

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b3cea60e-5712-4e80-b187-3359b5aba542" />

---

## Testing Checklist

| # | Test Case | Expected Result |
|---|---|---|
| 1 | Create a Lead without selecting Territory | Validation error appears on Territory field |
| 2 | Create a Lead with Territory = North | Lead assigned to North rep |
| 3 | Check email / Setup > Email Log Files | Notification email received for new lead |
| 4 | Open the Lead record | Custom Lightning page layout renders correctly |
| 5 | Setup > Flows > View Details and Run History | Flow ran successfully, no errors shown |

---


---

## Project Extensions (Planned)

- **Lead Conversion Process** — auto-populate Contact and Opportunity with Territory data on conversion
- **Reports & Dashboard** — Leads by territory, average lead score per rep, conversion rate by product interest
- **Web-to-Lead Form** — public Experience Cloud page feeding directly into this automation pipeline
- **Scoring Flow** — a scheduled flow that updates `Lead_Score__c` based on activity and engagement signals

---

## Skills Demonstrated

- Salesforce data modelling (standard object customisation, field types, picklist management)
- Declarative automation (Flow, Assignment Rules, Validation Rules)
- Lightning App Builder and UI/UX layout design
- End-to-end testing and documentation
- Understanding of when to use Flow vs Apex (declarative-first approach)

---

## Tech Stack

| Tool | Usage |
|---|---|
| Salesforce Flow (Record-Triggered) | Email notification automation |
| Lead Assignment Rules | Territory-based routing |
| Object Manager | Custom field creation and validation |
| Lightning App Builder | Custom record page layout |
| Salesforce Developer Edition | Development and testing environment |

---

*Built as part of a Salesforce Administrator/Developer portfolio. All automation is 100% declarative.*
