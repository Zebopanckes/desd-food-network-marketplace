# Strategic BPMN (Sprint 1)

Jira: SCRUM-13 (strategic BPMN), using the analysis from SCRUM-11 and the scoping from SCRUM-15.

## What the strategic BPMN needs to be

A high-level model of the whole enterprise in the Bristol Regional Food Network case study, not just our chosen use case. It should show:

- the major business processes
- organisational functions and responsibilities
- key internal and external actors
- external organisations and services
- how different areas of the enterprise interact
- how the principal processes relate to each other

It should also line up with the stakeholder goals and dependencies in our analysis and i* models. Detailed gateways, forms and exception paths belong in the operational BPMN (Sprint 2), not here.

ADD A FILES SECTION HERE EXPLAINING WHAT EACH ONE IS 

## How the model should be structured

| Element          | What it shows                                                                                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Customers pool   | Browsing, ordering and paying, receiving orders, quality concerns                                                                                                                    |
| Producers pool   | Listing products, preparing orders, weekly settlement, incident response                                                                                                             |
| Marketplace pool | Six major processes, split into five lanes (product and inventory, order and checkout, payments and settlement, quality and traceability, network administration and sustainability) |
| External pools   | Payment service (sandbox) and organic certification service                                                                                                                          |
| Data store       | Records linking customers, products and producers (for traceability)                                                                                                                 |
| Notes            | Customer, producer and network goals                                                                                                                                                 |

in the collapsed subprocesses like product listing and inventory management or similar, each one should contain a placeholder for now - sprint 2 we will go into that more and complete

## Chosen use case and candidate functionalities

Use case 2: customer multi-vendor order, checkout and payment. Both functionalities are still to be agreed with the module team. - the colour just means in camunda the box is coloured

- Candidate functionality 1 (colour blue): multi-vendor order and checkout
- Candidate functionality 2 (colour green): payment, 5% commission and producer settlement

## Conventions

- Strategic level only: process boxes, no detailed gateways or forms
- Pools for actors, lanes for organisational functions
- External services shown as collapsed pools with message flows
- Message flows between pools; sequence flows within a pool
- The Customers process is marked executable only so Camunda validation passes. The model is not meant to be run

## Checklist before review

- [ ] Every major process in the SCRUM-11 analysis table appears on the diagram
- [ ] Group review held against the case study, and changes made
- [ ] `.bpmn` and `.png` committed with the Jira key in the message (for example `SCRUM-13: add strategic BPMN`)
- [ ] Everyone in the group can walk through the diagram
