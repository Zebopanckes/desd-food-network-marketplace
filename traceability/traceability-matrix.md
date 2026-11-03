# Traceability matrix

- i* IDs (G-, T-, R-, Q-, D-) are the `ID` custom properties in the i* model.
- Strategic BPMN names are the element names in the strategic BPMN. Pools, lanes and subprocesses belong to the Marketplace pool unless stated.
- Message flows are written as "Name" (source to target).

## 1. Actors

| Stakeholder goal / dependency (i*) | Strategic BPMN element                                                                                                              | Operational BPMN element | Camunda implementation |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ---------------------- |
| Customer (role)                    | Pool "Customers"                                                                                                                    |                          |                        |
| Producer (role)                    | Pool "Producers"                                                                                                                    |                          |                        |
| Marketplace platform (agent)       | Pool "Marketplace" (lanes: Product and Inventory Management, Order and Checkout, Payments and Settlement, Quality and Traceability) |                          |                        |
| Platform admin (role)              | Lane "Network Administration and Sustainability" in pool "Marketplace"                                                              |                          |                        |
| Payment service (agent)            | Pool "Payment service"                                                                                                              |                          |                        |

## 2. Dependencies (SD)

| Stakeholder goal / dependency (i*)                                                   | Strategic BPMN element                                                                                                                                                               | Operational BPMN element | Camunda implementation |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------ | ---------------------- |
| D-01 Producer to Platform: simple listing and inventory tool (quality)               | Task "List products and update seasonal stock" (Producers); subprocess "List and manage products"; message flow "Listing submitted"                                                  |                          |                        |
| D-03 Producer to Platform: producer order with 48h notice (resource)                 | Message flow "Producer order" (subprocess "Order and checkout" to task "Receive and prepare orders (48h notice)")                                                                    |                          |                        |
| D-04 Producer to Platform: weekly settlement with audit trail (goal)                 | Message flow "Weekly settlement" (subprocess "Payment and settlement" to task "Receive weekly settlement")                                                                           |                          |                        |
| D-05 Producer to Customer: orders (goal)                                             | Task "Place multi-vendor order and pay"; message flow "Customer order placed"; start event "Customer order placed"                                                                   |                          |                        |
| D-06/07 Customer to Producer: safe, accurate local produce and fulfilment (resource) | Task "Receive and prepare orders (48h notice)" (Producers); task "Receive confirmation, delivery or collection" (Customers)                                                          |                          |                        |
| D-08 Customer to Platform: product info, origin and availability (resource)          | Task "Browse and search products"; subprocess "List and manage products"; end event "Products listed and in stock"                                                                   |                          |                        |
| D-09 Customer to Platform: multi-vendor order coordination (task)                    | Subprocess "Order and checkout"; start event "Customer order placed"                                                                                                                 |                          |                        |
| D-11 Customer to Platform: order confirmed (goal)                                    | Message flow "Confirmation to customer" (subprocess "Payment and settlement" to task "Receive confirmation, delivery or collection"); end event "Order confirmed and producers paid" |                          |                        |
| D-12 Customer to Platform: payment secure (quality)                                  | Subprocess "Payment and settlement"; message flows "Payment request" and "Payment result" (pool "Payment service")                                                                   |                          |                        |
| D-14 Platform to Customer: valid payment details (resource)                          | Task "Place multi-vendor order and pay" (Customers) into subprocess "Payment and settlement"                                                                                         |                          |                        |
| D-15 Platform to Producer: accurate stock and allergen info (resource)               | Task "List products and update seasonal stock"; message flow "Listing submitted"                                                                                                     |                          |                        |
| D-18/19 Platform to Payment service: process payment and return result (task)        | Message flows "Payment request" and "Payment result" (subprocess "Payment and settlement" and pool "Payment service")                                                                |                          |                        |
| D-23 Admin to Platform: 5% commission calculated (task)                              | Subprocess "Payment and settlement" (lane "Payments and Settlement")                                                                                                                 |                          |                        |

## 3. Goals and tasks (SR)

| Stakeholder goal / dependency (i*)                   | Strategic BPMN element                                                                                                                              | Operational BPMN element | Camunda implementation |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ---------------------- |
| G-C1 Buy local produce conveniently                  | Customers process: start event "Needs local food" to end event "Needs met"                                                                          |                          |                        |
| G-C2 Receive correct order                           | Task "Receive confirmation, delivery or collection"; end event "Needs met"                                                                          |                          |                        |
| T-C1 Browse products and check origin                | Task "Browse and search products"                                                                                                                   |                          |                        |
| T-C2 Add products and coordinate basket              | Task "Place multi-vendor order and pay"                                                                                                             |                          |                        |
| T-C3 Pay securely and retry if declined              | Task "Place multi-vendor order and pay"; subprocess "Payment and settlement"                                                                        |                          |                        |
| T-C4 Receive delivery or collection                  | Task "Receive confirmation, delivery or collection"                                                                                                 |                          |                        |
| G-M1 Orders coordinated and processed                | Subprocess "Order and checkout" (lane "Order and Checkout")                                                                                         |                          |                        |
| G-M2 Payments and settlements handled                | Subprocess "Payment and settlement" (lane "Payments and Settlement"); end event "Order confirmed and producers paid"                                |                          |                        |
| T-M1 Provide listing and inventory tool              | Subprocess "List and manage products"                                                                                                               |                          |                        |
| T-M2 Publish product info and origin                 | Subprocess "List and manage products"; end event "Products listed and in stock"                                                                     |                          |                        |
| T-M3 Coordinate multi-vendor order                   | Subprocess "Order and checkout"                                                                                                                     |                          |                        |
| T-M4 Request payment and update status               | Subprocess "Payment and settlement"; message flows "Payment request", "Payment result", "Confirmation to customer"                                  |                          |                        |
| T-M5 Run weekly settlement and commission            | Subprocess "Payment and settlement"; message flow "Weekly settlement"                                                                               |                          |                        |
| G-P1 Reach customers beyond farm gate                | Producers process: start event "Has produce to sell" to end event "Produce sold"                                                                    |                          |                        |
| G-P2 Get paid weekly                                 | Task "Receive weekly settlement"; end event "Produce sold"                                                                                          |                          |                        |
| T-P1 List products and update stock/allergens        | Task "List products and update seasonal stock"; message flow "Listing submitted"                                                                    |                          |                        |
| T-P2 Prepare orders with 48h notice                  | Task "Receive and prepare orders (48h notice)"                                                                                                      |                          |                        |
| T-P3 Accept 5% commission on sales                   | Task "Receive weekly settlement" (commission deducted at settlement, per the stakeholder analysis assumption)                                       |                          |                        |
| G-A2 Earn 5% commission                              | Subprocess "Payment and settlement" (lane "Payments and Settlement"); no admin task in the strategic BPMN                                           |                          |                        |
| T-PS1 Process payment and return result              | Pool "Payment service"; message flows "Payment request" and "Payment result"                                                                        |                          |                        |
| Qualities (Q-C1, Q-C2, Q-M1, Q-M2, Q-P1, Q-P2, Q-P3) | No direct strategic BPMN element. Non-functional, addressed in the operational BPMN and Camunda. Q-M1 (Secure) relates to the payment message flows |                          |                        |

## 4. enterprise context strategic BPMN parts

These are enterprise context (use cases 1 and 3 and community features) that are not implemented and are not modelled in the trimmed i* models.

- Pool "Organic certification service" and message flows "Certification check request" and "Certification result".
- Subprocess "Quality incident handling", start event "Quality concern reported", data store "Customer, product and producer records".
- Subprocess "Review and close incident".
- Subprocess "Community and sustainability reporting".
- Customers tasks "Report food quality concern" and "Act on safety notice".
- Producers task "Respond to quality incident".
