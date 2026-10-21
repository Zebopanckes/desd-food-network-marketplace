
# Stakeholder and Enterprise Analysis (SCRUM-11)

**Jira:** SCRUM-11, Enterprise analysis: stakeholders, major processes and responsibilities
**Lead:** Bartek (analysis and quality). **Supports:** Ashley (process list), everyone (case study reading)
**Feeds:** the strategic BPMN (SCRUM-13) and the scoping document (SCRUM-15)

## Checklist (from SCRUM-11)

- [X] Stakeholder analysis: first version for producers, customers and network admin
- [ ] Stakeholder analysis: add the two external services and fill the blank customer cells
- [ ] Major business processes and responsibilities table (priority, the BPMN is checked against it)
- [ ] Assumptions listed
- [ ] Case study notes pooled (optional, can stay in personal notes)
- [ ] Rough i* SD notes (optional, not marked until Sprint 2)
- [ ] Reviewed by the modelling lead and confirmed as enough to draw the strategic BPMN from
- [ ] Commit message under the SCRUM-11 key, for example `SCRUM-11: add process table`

## 1. Stakeholder analysis

For each stakeholder: needs or goals, responsibilities and key dependencies. Assumptions go in one list in section 3.

### Summary

| Stakeholder                   | Type             | Main goal                                                        |
| ----------------------------- | ---------------- | ---------------------------------------------------------------- |
| Producers                     | Supplier         | Sell local produce with little admin effort and get paid weekly  |
| Customers (three segments)    | Buyer            | Browse, search and order locally sourced food easily             |
| Network or platform admin     | Operator         | Connect local customers and producers and earn the 5% commission |
| Payment service               | External service | TO DO                                                            |
| Organic certification service | External service | TO DO                                                           |

### Producers

**Needs**

- An intuitive way to list their products with the essential information
- Inventory updates that need no extensive technical knowledge or training
- Visibility of incoming orders, with at least 48 hours' notice
- Weekly payment settlements
- Educational features to share seasonal recipes, storage guidance and farm stories

**Responsibilities**

- Keep inventory and pricing up to date
- Provide essential product information: organic certification status, allergen warnings and best before dates
- Offer last-minute discounts on surplus produce
- Update the educational content

**Key dependencies**

- An intuitive platform that meets their needs and lets them carry out their responsibilities

### Customers

All three segments share one flow at strategic level. Their different needs are listed below and can be developed in the i* models.

**Needs shared by all customers**

- Robust browsing
- Products organised into clearly defined categories
- Extensive item search
- Simple single-vendor and multi-vendor orders
- Clear communication about each supplier's responsibilities, delivery arrangements and collection procedures

**Responsibilities:** TO DO

**Key dependencies:** TO DO 

**Differences between segments**

| Segment                           | Specific needs                                                                                                             | Responsibilities                           | Key dependencies                                              |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| Young professionals and families  | Buy locally sourced produce without visiting farm shops during their limited opening hours                                 | TO DO                                      | Clearly visible produce origin, so they can trust it is local |
| Community groups                  | Reliable and consistent sources of local produce                                                                           | TO DO                                      | TO DO                                                         |
| Independent restaurants and cafes | Order from several small suppliers without maintaining a relationship with each one, in a way that does not take much time | Adapt menus to changes in seasonal produce | TO DO                                                         |

### Network or platform admin

**Needs**

- Connect customers within a 20-mile radius of Bristol city centre with local food producers
- Collect a 5% commission on all orders

**Responsibilities**

- Provide a platform with the functionality stakeholders need

**Key dependencies**

- Producers and farms actively using the platform

### Payment service (external)

**TO DO.** Needs, responsibilities and dependencies. This will be a sandbox payment service, so say what the platform sends it and what it returns.

### Organic certification service (external)

**TO DO.** Needs, responsibilities and dependencies. Say what the platform checks with it and when.

## 2. Major business processes and responsibilities

**TO DO.** One row per major process. The strategic BPMN is checked against this table ("every major process appears on the diagram"). The first five names come from the BPMN walkthrough notes, so **TO CONFIRM** them against the case study and add any that are missing. The README expects six processes.

| Process                                     | Owner or responsible area | Main actors | External services involved |
| ------------------------------------------- | ------------------------- | ----------- | -------------------------- |
| Product listing and inventory management    | TO DO                     | TO DO       | TO DO                      |
| Order and checkout (multi-vendor)           | TO DO                     | TO DO       | TO DO                      |
| Payment, commission and producer settlement | TO DO                     | TO DO       | TO DO                      |
| Quality incident and traceability           | TO DO                     | TO DO       | TO DO                      |
| Community and sustainability features       | TO DO                     | TO DO       | TO DO                      |
| TO DO (sixth process, if any)               | TO DO                     | TO DO       | TO DO                      |

## 3. Assumptions

One line per assumption, saying where it came from.

- Farm staff have basic IT skills (from the producers analysis)
- **TO CONFIRM:** delivery or collection is not shown as its own step, because the case study does not say who delivers. It sits inside checkout and fulfilment (from the BPMN notes)
- **TO DO:** add the rest as you find them

## 4. Case study notes (optional)

Pool notes from reading the case study under these headings. Short bullets are fine, and add the page or section where it helps.

- **Actors:** TO DO
- **Goals:** TO DO
- **Requirements:** TO DO
- **External services:** TO DO

## 5. Rough i* SD notes (optional)

Not marked until Sprint 2, but useful for lining up with the BPMN. For each stakeholder, note main goals, softgoals and who depends on whom.

- TO DO (if any)
