# Traceability matrix

- The first column uses the element names in the i* model. Dependencies are written as depender to dependee, with the dependum and its type.
- Strategic BPMN names are the element names in the strategic BPMN. Pools, lanes and subprocesses belong to the Marketplace pool unless stated.
- Message flows are written as "Name" (source to target).

## 1. Actors

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer (role) | Pool "Customers" | | |
| Producer (role) | Pool "Producers" | | |
| Marketplace (agent) | Pool "Marketplace" (lanes: Product and Inventory Management, Order and Checkout, Payments and Settlement, Quality and Traceability) | | |
| Platform Admin (role) | Lane "Network Administration and Sustainability" in pool "Marketplace" | | |
| Payment Service (agent) | Pool "Payment service" | | |

## 2. Dependencies (SD)

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer "Add products to basket" to Marketplace "Coordinate multi-vendor order": multi-vendor order coordination (task) | Task "Place multi-vendor order and pay"; message flow "Customer order placed"; subprocess "Order and checkout" | | |
| Customer "Pay and retry if declined" to Marketplace "Request payment and update status": order confirmed (goal) | Message flow "Confirmation to customer" (subprocess "Payment and settlement" to task "Receive confirmation, delivery or collection"); end event "Order confirmed and producers paid" | | |
| Customer "Pay and retry if declined" to Marketplace "Secure": payment secure (quality) | Subprocess "Payment and settlement"; message flows "Payment request" and "Payment result" | | |
| Marketplace "Request payment and update status" to Customer "Pay and retry if declined": valid payment details (resource) | Task "Place multi-vendor order and pay" into subprocess "Payment and settlement" | | |
| Marketplace "Request payment and update status" to Payment Service "Process payment & return result": process payment and return result (task) | Message flows "Payment request" and "Payment result" (subprocess "Payment and settlement" and pool "Payment service") | | |
| Customer "Browse products and check origin" to Marketplace "Publish product info & origin": product info, origin and availability (resource) | Task "Browse and search products"; subprocess "List and manage products"; end event "Products listed and in stock" | | |
| Customer "Receive delivery or collection" to Producer "Prepare orders with 48h notice": safe, accurate local produce and fulfilment (resource) | Task "Receive confirmation, delivery or collection" (Customers); task "Receive and prepare orders (48h notice)" (Producers) | | |
| Producer "Prepare orders with 48h notice" to Marketplace "Coordinate multi-vendor order": producer order with 48h notice (resource) | Message flow "Producer order" (subprocess "Order and checkout" to task "Receive and prepare orders (48h notice)") | | |
| Producer "Get paid weekly" to Marketplace "Weekly settlement & commission": weekly settlement with audit trail (goal) | Message flow "Weekly settlement" (subprocess "Payment and settlement" to task "Receive weekly settlement") | | |
| Producer "List products & update stock/allergens" to Customer "Add products to basket": orders (goal) | Task "Place multi-vendor order and pay"; message flow "Customer order placed" | | |
| Producer "List products & update stock/allergens" to Marketplace "Provide listing and inventory tool": simple listing and inventory tool (quality) | Task "List products and update seasonal stock"; subprocess "List and manage products"; message flow "Listing submitted" | | |
| Marketplace "Publish product info & origin" to Producer "List products & update stock/allergens": accurate stock and allergen info (resource) | Task "List products and update seasonal stock"; message flow "Listing submitted"; subprocess "List and manage products" | | |
| Platform Admin "Earn 5% commission" to Marketplace "Weekly settlement & commission": calculate 5% commission (task) | Subprocess "Payment and settlement" (lane "Payments and Settlement"); message flow "Weekly settlement" | | |

## 3. Goals, tasks, resources and qualities (SR)

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer: Buy local produce conveniently | Customers process: start event "Needs local food" to end event "Needs met" | | |
| Customer: Receive order | Task "Receive confirmation, delivery or collection"; end event "Needs met" | | |
| Customer: Browse products and check origin | Task "Browse and search products" | | |
| Customer: Add products to basket | Task "Place multi-vendor order and pay" | | |
| Customer: Pay and retry if declined | Task "Place multi-vendor order and pay"; subprocess "Payment and settlement" | | |
| Customer: Receive delivery or collection | Task "Receive confirmation, delivery or collection" | | |
| Customer: Product information (resource) | Input to task "Browse and search products"; output of subprocess "List and manage products" | | |
| Customer: Payment details (resource) | Input to task "Place multi-vendor order and pay" and subprocess "Payment and settlement" | | |
| Marketplace: Orders coordinated and processed | Subprocess "Order and checkout" (lane "Order and Checkout") | | |
| Marketplace: Coordinate multi-vendor order | Subprocess "Order and checkout"; start event "Customer order placed" | | |
| Marketplace: Products listed | Subprocess "List and manage products" (lane "Product and Inventory Management"); end event "Products listed and in stock" | | |
| Marketplace: Publish product info & origin | Subprocess "List and manage products" | | |
| Marketplace: Provide listing and inventory tool | Subprocess "List and manage products"; start event "Listing submitted" | | |
| Marketplace: Payments and settlements handled | Subprocess "Payment and settlement" (lane "Payments and Settlement"); end event "Order confirmed and producers paid" | | |
| Marketplace: Request payment and update status | Subprocess "Payment and settlement"; message flows "Payment request", "Payment result", "Confirmation to customer" | | |
| Marketplace: Weekly settlement & commission | Subprocess "Payment and settlement"; message flow "Weekly settlement" | | |
| Producer: Reach customers beyond farm gate | Producers process: start event "Has produce to sell" to end event "Produce sold" | | |
| Producer: List products & update stock/allergens | Task "List products and update seasonal stock"; message flow "Listing submitted" | | |
| Producer: Prepare orders with 48h notice | Task "Receive and prepare orders (48h notice)" | | |
| Producer: Get paid weekly | Task "Receive weekly settlement"; end event "Produce sold" | | |
| Producer: Accept 5% commission on sales | Task "Receive weekly settlement" (commission is deducted at settlement, per the stakeholder analysis assumption) | | |
| Platform Admin: Earn 5% commission | Subprocess "Payment and settlement" (lane "Payments and Settlement"); there is no admin task in the strategic BPMN | | |
| Payment Service: Process payment & return result | Pool "Payment service"; message flows "Payment request" and "Payment result" | | |
| Qualities: Convenient, Trustworthy, Accurate info, Low admin effort, Fair income | No direct strategic BPMN element. Non-functional, addressed in the operational BPMN and Camunda | | |
| Quality: Secure (Marketplace) | Message flows "Payment request" and "Payment result" with pool "Payment service" | | |
| Quality: Compliant (Marketplace) | Subprocess "Payment and settlement" (weekly settlement and audit trail) | | |

## 4. enterprise context strategic BPMN parts

These are enterprise context (use cases 1 and 3 and community features) that are not implemented and are not modelled in the i* models.

- Pool "Organic certification service" and message flows "Certification check request" and "Certification result".
- Subprocess "Quality incident handling", start event "Quality concern reported", data store "Customer, product and producer records".
- Subprocess "Review and close incident".
- Subprocess "Community and sustainability reporting".
- Customers tasks "Report food quality concern" and "Act on safety notice".
- Producers task "Respond to quality incident".

## 5. Open points

- The strategic BPMN sends the "Producer order" message from subprocess "Order and checkout", while Use Case 2 and the scoping notify producers after payment succeeds. The i* dependency follows the BPMN for now. This must be settled so the i*, strategic BPMN and operational BPMN agree.
- The end event "Order confirmed and producers paid" suggests per-order payment, while the i* and case study use weekly settlement.
