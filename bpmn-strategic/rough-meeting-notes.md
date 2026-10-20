# Strategic BPMN (midway version): walkthrough notes

Rough notes, not final. Reading order: customers, producers, marketplace paths, external services.

## Customers (pool)
- Start: wants local food -> Browse and search products -> Place multi-vendor order and pay -> Receive confirmation, delivery or collection -> Report food quality concern or receive safety notice -> End: needs met
- "Place order and pay" sends a message to the marketplace (customer order placed), which starts the order path
- "Receive confirmation" is fed by the marketplace payment and settlement step
- "Report concern or receive safety notice" has two links: it sends the concern in, and the marketplace quality process sends the notice back
- One pool for all three customer segments (families, community groups, restaurants and cafes). They follow the same flow at this level, and their different needs can come out in the i* models

## Producers (pool)
- Start: has produce to sell -> List products and update seasonal stock -> Receive and prepare orders (48h notice) -> Receive weekly settlement -> Respond to quality incident -> End: produce sold
- "List products" sends a message that starts the marketplace listing path
- "Receive and prepare orders" is fed by the order and checkout step (producer-specific order elements)
- "Receive weekly settlement" is fed by the payment and settlement step
- "Respond to quality incident" is fed by the quality incident step
- Delivery or collection is not shown as its own step because the case study doesn't say who delivers, so it sits inside checkout and fulfilment. Flag as an assumption

## Marketplace path A: listing and inventory
- Message start: listing submitted -> List and manage products
- The external certification service connects both ways (organic claim sent out, result back)
- Hands on to the order path. Products have to be listed and in stock before orders can happen (simplified here)
- Lane: Product and Inventory Management

## Marketplace path B: order, checkout and payment
- Wait for: customer order placed -> Order and checkout -> Payment and settlement -> End: order confirmed and producers paid
- Order and checkout: stock check, split the basket by producer, set delivery or collection. Candidate functionality 1
- Order and checkout also sends producer-specific orders to the producers
- Payment and settlement: payment, 5% commission, producer shares and settlement. Candidate functionality 2
- The external payment service connects both ways (request out, result back)
- Payment and settlement sends confirmation to the customer and settlement to the producer
- Lanes: Order and Checkout, then Payments and Settlement

## Marketplace path C: quality incident
- Message start: quality concern reported -> Quality incident handling -> Review and close incident -> End: incident closed
- Triggered by the customer's report. The use case says a customer, producer or admin could report it, so only the customer route is shown for now
- Quality incident handling needs to find affected orders, producers and customers, so it relies on records linking them
- Sends messages to the producer (respond) and to the customer (safety notice)
- Review and close sits in the Network Administration lane
- Lanes: Quality and Traceability, then Network Administration and Sustainability

## Marketplace path D: community and sustainability
- Timer start: monthly -> Community and sustainability reporting -> End: reports and offers published
- Covers food miles reporting, surplus and last-minute discounts, recipes and farm stories
- Standalone for now, with no links to other paths
- Lane: Network Administration and Sustainability

## External pools
- Payment service: linked from payment and settlement (sandbox only, no live payments)
- Certification service: linked from list and manage products

## Where candidate functionalities sit
- Use case 2 (multi-vendor order, checkout and payment) is confirmed
- Functionality 1 = Order and checkout. Functionality 2 = Payment and settlement
- Both still need module team agreement

## Open questions / to refine
- Does the listing path really flow into the order path, or should they be separate?
- Add producer and admin as reporters of quality concerns?
- Show the customer, product and producer records as a data store? (the final version does)
- Are the community and sustainability features major enough to keep as a process?
- Add goal notes per stakeholder for the i* link?
- Check every process against the SCRUM-11 analysis table
