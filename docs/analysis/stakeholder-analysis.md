# Stakeholder and Enterprise Analysis (SCRUM-11)

**Jira:** SCRUM-11, Enterprise analysis: stakeholders, major processes and responsibilities
**Lead:** Bartek (analysis and quality). **Supports:** Ashley (process list), everyone (case study reading)
**Feeds:** the strategic BPMN (SCRUM-13) and the scoping document (SCRUM-15)

Use Case 2 is our chosen use case and the only one we will implement. Use Cases 1 and 3 are referenced here as enterprise context for the strategic model.

## 1. Stakeholder analysis

For each stakeholder: needs or goals, responsibilities and key dependencies. Assumptions go in one list in section 3.

### Summary

| Stakeholder                   | Type             | Main goal                                                        |
| ----------------------------- | ---------------- | ---------------------------------------------------------------- |
| Producers                     | Supplier         | Sell local produce with little admin effort and get paid weekly  |
| Customers (three segments)    | Buyer            | Browse, search and order locally sourced food easily             |
| Network or platform admin     | Operator         | Connect local customers and producers and earn the 5% commission |
| Payment service               | External service | Process payment requests and return the result                   |
| Organic certification service | External service | Verify organic certification claims                              |

### Producers

**Needs**

- An intuitive way to list their products with name, description, pricing and availability timeframes (case study)
- Inventory updates that need no extensive technical knowledge or training - skill vary across the network, and some producers work only by email and phone (case study)
- Visibility of incoming orders, with at least 48 hours' notice (case study)
- Weekly payment settlements with transparent audit trails for tax reporting and accounting (case study)
- Features to share seasonal recipes, storage guidance and farm stories (case study)

**Responsibilities**

- Keep inventory and pricing, and seasonal availability up to date (case study, use case 1)
- Provide traceability information: organic certification status, allergen warnings and best before dates (case study)
- Offer last-minute discounts on surplus produce and update the educational content (case study, producer ownership **TO CONFIRM**)

**Key dependencies**

- Producers depend on the platform for a simple listing and inventory tool (softgoal), order notice of at least 48 hours (resource) and weekly settlements (goal)
- Producers depend on customers for orders (goal)

### Customers

All three segments share one flow at strategic level.

**Needs shared by all customers**

- Robust browsing by clearly defined categories (vegetable, dairy, bakery goods, preserves and seasonal specialties) and efficient search (case study)
- Display of harvest dates, farm origin and seasonal availability (Case study)
- Single-vendor and multi-vendor checkout with clear communication about supplier responsibilities, delivery arrangements and collection procedures (Case study)
- Being told promptly if a product they bought is subject to a food quality or safety concern (Use Case 3)

**Responsibilities**

* Review and confirm the order, and amend it if stock is insufficient (use case 2)
* Provide payment, and retry if declined (use case 2)
* Report food quality or safety concerns (use case 3)

**Key dependencies**

* Customers depend on producers for safe, accuratley described local produce (resource)
* Customers depend on the platform for stock checks and multi-vendor order coordination (task) and for safety notifications (goal)

**Differences between segments**

| Segment                           | Specific needs                                                                             | Responsibilities or dependencies                              |
| --------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |
| Young professionals and families  | Buy locally sourced produce without visiting farm shops during their limited opening hours | Clearly visible produce origin, so they can trust it is local |
| Community groups                  | Reliable and consistent sources of local produce                                           |                                                               |
| Independent restaurants and cafes | Source locally without administrative overhead of managing many small suppliers            | Adapt menus to seasonal produce (**TO CONFIRM**)              |

### Network or platform admin

The case study describes platform duties, and the admin role only appears in Use Case 3. Which duties belong to the system and which to a human administrator is **TO CONFIRM**.

**Needs**

- Connect customers within a 20-mile radius of Bristol city centre with local producers
- Calculate the networks's 5% commission automatically

**Responsibilities**

* Provide the platform: product management, multi-vendor order processing and secure payment (Case study)

- Distribute payments to producers on the agreed timeframes, weekly (Case study)
- Keep financial transactions compliant with data protection and financial services regulations (Case study)
- Hold records linking customers, products and producers, and use them to communicate quickly about quality or food safety concerns (Case study, Use Case 3)
- Review and close quality incidents (Use Case 3)

**Key dependencies**

- Platform depends on producers actively using it and giving accurate stock and allergen information.
- Platform depends on the payment service for payment results
- Platform depends on the certification service for verification results

### Payment service (external, Sandbox)

* **Needs:** a payment request with the order reference and amount. some expected fields may be: amount, currency(maybe not needed), idempotency key(unique to prevent duplicate requests), order ID (proposed, **TO CONFIRM**)
* **Responsibilities:** Process the request and return success or declined. platform also handles service being unavailable
* **Dependency:** The payment service depends on the platform for a correct, complete request

### Organic certification service (external or simulated)

**Needs:** A verification request when a producer claims organic certification

**Responsibilities:** Return a verification result. The platform handles failed validation and service being unavailable

**Dependency:** The service depends on the platform for the certification details to check

## 2. Major business processes and responsibilities

Each process maps to a lane in the marketplace pool of the strategic BPMN. Processes 2 and 3 are our two functionalities

| Process                                        | Responsible area (marketplace lane)                                                | Main actors                                                             | External services     | Link                                                            |
| ---------------------------------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------- | --------------------------------------------------------------- |
| 1. Product listing and inventory management    | Product and inventory management                                                   | Producers, platform                                                     | Organic certification | Use case 1                                                      |
| 2. Order and checkout (multi-vendor)           | Order and checkout                                                                 | Customers, platform, producers (receive their orders)                   | None                  | Functionality 1: Checkout and Order processing                  |
| 3. Payment, commission and producer settlement | Payments and settlements                                                           | Customers, platform, producers (weekly settlement), admin (commission) | Payment service       | Functionality 2: Payment processing and settlement              |
| 4. Quality incident and traceability           | Quality and traceability, with review in Network administration and sustainability | Customers, producers, admin                                             | None                  | Use case 3                                                      |
| 5. Community and sustainability features       | Network administration and sustainability                                          | Producers, admin (**TO CONFIRM**), customers                            | None                  | Case study:food miles, surplus discounts, educational discounts |

#### How the processes connect - simplified at strategic level

* Listing makes product and stock available
* Order and  checkout uses that stock and creates producer specific orders
* Payment and settlement pays for the order, takes 5% commission and settles with producers
* Quality incident handling uses the records linking customers, products and producers

## 3. Assumptions

- All three customer segments follow one flow at strategic level - modelling
- Browsing and searching are the entry steps of order and checkout, not a separate process - modelling
- Delivery and collection sit inside checkout and fulfilment, with each producer responsible for its own arrangements  - case study: "individual supplier responsibilities, delivery arrangements and collection procedures" - **TO CONFIRM**
- The 5% commission is deducted from producer payments at weekly settlement - case study: Commission calculated automatically, payments distributed on agreed timeframes - **TO CONFIRM**
- The strategic BPMN shows only customers reporting quality concerns for readability, although use case 3 allows producers and admins to report.
- Producers have a device and internet access to use the platform - not stated in the case study - **TO CONFIRM**
