# Ticket Registration Solution

 customer support ticketing system built on the **Microsoft Power Platform**. Customers submit a request through a Microsoft Form, the system logs it as a ticket in Dataverse, emails the customer at each stage, and collects feedback once the ticket is resolved. A Copilot Studio agent sits on top to answer questions using the ticket and feedback data.

This repo contains the exported solution (**v1.0.0.2, unmanaged**) in two forms: a ready-to-import `.zip` and the unpacked source so every flow, table and topic can be read on GitHub.

## What's inside

**Dataverse tables**

| Table | Holds |
|---|---|
| Tickets | Ticket ID, customer name and email, issue type, status, assigned agent, created and resolved dates |
| Tickets Feedback | Ticket ID and customer rating |
| Customer Feedback | Ticket ID, rating and comments |
| Assigned Agents Setup | Agent names and emails |
| Agent Skills | Agent names and their skills |

**Power Automate cloud flows**

| Flow | What it does |
|---|---|
| Ticket Creation (Dataverse) | When a form response is submitted, creates a ticket row, sets values by issue type, and emails the customer a "ticket created" message |
| Assigning agents | On a form response, creates the ticket, looks up the agent skills table and updates the ticket if a match is found |
| Ticket Creation, update customer | When a ticket row is created, emails the customer a confirmation |
| Ticket Creation Workflow | When a ticket's status changes, emails the customer the new status |
| Email feedback form to customer | When a resolved date is set, emails the customer a feedback request |
| Feedback | When a feedback form is submitted, stores the rating in the Tickets Feedback table |

**Copilot Studio agent: Support Assistant**
- Standard conversation topics (Greeting, Escalate, Search, Goodbye and so on)
- Knowledge sources: the Tickets table, the Customer Feedback table and a sample ticket CSV
- An email notification tool

## How the flow works

```
Customer submits form
        |
        v
Ticket row created in Dataverse  -->  "Ticket created" email
        |
        v
Agent lookup and assignment (Agent Skills table)
        |
        v
Status changes  -->  customer gets a status email
        |
        v
Ticket resolved  -->  feedback request email  -->  rating stored in Dataverse
```

## How to import

1. Go to [make.powerapps.com](https://make.powerapps.com) and pick your environment.
2. Open **Solutions**, choose **Import solution**, and select `TicketRegistrationSolution_1_0_0_2.zip`.
3. Set up the connections when asked (Dataverse, Microsoft Forms, Office 365 Outlook).
4. After import, open each Forms-triggered flow and **re-select your own Microsoft Form**. The original form is not included.
5. In **Ticket Creation (Dataverse)**, replace the placeholder recipients (`agent1@example.com` and so on) with your own address.
6. Turn the flows on and publish the Support Assistant agent in Copilot Studio.

## Repo structure

```
ticket-registration-solution/
├── TicketRegistrationSolution_1_0_0_2.zip   Import this into Power Platform
└── src/                                     Unpacked solution source
    ├── Workflows/                           The 6 cloud flows (JSON)
    ├── bots/                                Support Assistant agent settings
    ├── botcomponents/                       Agent topics and knowledge sources
    ├── customizations.xml                   Table and column definitions
    └── solution.xml                         Solution manifest
```

## Notes

- The sample ticket CSV in the agent's knowledge uses made-up customers (`example.com` addresses), not real people.
- Personal email addresses were replaced with placeholders before publishing.
- The solution is unmanaged, so you can edit everything after import.



## Author

Humam Imtiaz, [GitHub](https://github.com/humamimtiaz)
