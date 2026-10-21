# Okan: Order, checkout and payment path

**Jira:** SCRUM-13 (strategic BPMN). **Due:** Fri 23 Oct. **Merged by:** Alec, Sun 25 Oct.

Keep this open next to Camunda Modeler. It tells you what to read, what to draw, and what to check before you commit.

## 1. Your part in one paragraph

You draw the central path of the diagram: a customer order arrives at the **Marketplace**, goes through order and checkout, then payment and settlement, and ends with the order confirmed and producers paid. These are your two candidate functionalities from SCRUM-15, so they carry colour in the diagram: **blue** for order and checkout, **green** for payment and settlement.

## 2. Read these first (inputs)

| What | Where | Why |
|---|---|---|
| Case study | Bristol Regional Food Network case study (course materials) | Multi-vendor order, 5% commission, producer settlement, payment |
| Your scoping document | [SCRUM-15-scoping.md](../../../docs/scoping/SCRUM-15-scoping.md) | The two functionalities and their names. Strategic labels must match them |
| Walkthrough notes | [rough-meeting-notes.md](../../rough-meeting-notes.md) | Section Marketplace path B and External pools |
| Strategic BPMN README | [README.md](../../README.md) | Conventions, colours and structure table |
| Example of a finished part | [Alec's folder](../alec-qual-incident-network-admin/) | Shows a Marketplace pool with lanes, a message start event and collapsed sub-processes |

## 3. What to draw

### Marketplace pool with two lanes

One pool named **Marketplace** with two lanes, using these exact names, top to bottom:

1. Order and Checkout
2. Payments and Settlement

### The path

| Order | Element | Type | Lane | Name | Colour |
|---|---|---|---|---|---|
| 1 | Start | Message start event | Order and Checkout | Customer order placed | None |
| 2 | Process | Collapsed sub-process | Order and Checkout | Order and checkout | Blue (functionality 1) |
| 3 | Process | Collapsed sub-process | Payments and Settlement | Payment and settlement | Green (functionality 2) |
| 4 | End | End event | Payments and Settlement | Order confirmed and producers paid | None |

Join them with sequence flows in that order. The flow from step 2 to step 3 crosses lanes, which is fine.

Put a placeholder inside each sub-process: Start, then a task named 'Placeholder: detail in Sprint 2', then End.

For reference, the content you will detail in Sprint 2:
- Order and checkout: stock check, split the basket by producer, set delivery or collection.
- Payment and settlement: payment, 5% commission, producer shares and settlement.

Do not draw any of that yet. It belongs in the operational BPMN.

### Black-box pools (empty, collapsed)

| Pool | Why |
|---|---|
| Customers | Sends the order and receives the confirmation |
| Producers | Receives producer orders and settlement |
| Payment service | External service you talk to (sandbox only, no live payments) |

### Message flows you draw

| Message name | From | To |
|---|---|---|
| Customer order placed | Customers pool | Message start event 'Customer order placed' |
| Producer order | Sub-process 'Order and checkout' | Producers pool |
| Payment request | Sub-process 'Payment and settlement' | Payment service pool |
| Payment result | Payment service pool | Sub-process 'Payment and settlement' |
| Confirmation to customer | Sub-process 'Payment and settlement' | Customers pool |
| Weekly settlement | Sub-process 'Payment and settlement' | Producers pool |

The payment service connects both ways: request out, result back.

## 4. Step by step in Camunda Modeler

1. File, then New BPMN diagram. Save it in this folder as `part-okan.bpmn`.
2. Create the Marketplace pool, add a second lane, and name both lanes exactly.
3. Add the message start event in the first lane. Name it and set its type to Message using the wrench menu.
4. Add the two sub-processes, each in its own lane. Collapse each with the small minus marker, then click the plus marker to open it, add Start, the placeholder task and End, and use the breadcrumb at the top to go back. If your Modeler has no plus marker, tell Alec.
5. Add the end event in the second lane and join everything with sequence flows.
6. Colour the sub-processes: select one, open the colour picker in the context pad, and choose blue for 'Order and checkout' and green for 'Payment and settlement'.
7. Create empty Customers (top), Producers and Payment service pools (bottom) and name them.
8. Draw and name the six message flows. Select the sending element, drag from the message flow icon in its context pad to the receiver, and double-click to name it.
9. Export a PNG and save it here as `part-okan.png`.

## 5. Shared rules (everyone follows these so the parts merge cleanly)

- **Level:** strategic only. Use process boxes. No detailed gateways, forms or exception paths (that is Sprint 2).
- **Naming:** use the exact pool, lane and message names given in this guide. Activities are verb plus object.
- **Flows:** sequence flows inside a pool, message flows between pools.
- **Pools you do not own:** draw them as empty black boxes. Alec swaps in the real ones when merging.
- **IDs:** leave the IDs Camunda Modeler generates. Do not rename them.
- **Major processes** are collapsed sub-processes with a placeholder inside.

Pool names: Customers, Marketplace, Producers, Payment service, Organic certification service.

Marketplace lanes, top to bottom: Product and Inventory Management; Order and Checkout; Payments and Settlement; Quality and Traceability; Network Administration and Sustainability.

## 6. All hand-off messages (for reference)

| Message | From | To |
|---|---|---|
| Listing submitted | Producers | Marketplace (listing path start) |
| Certification check request | Marketplace (List and manage products) | Organic certification service |
| Certification result | Organic certification service | Marketplace (List and manage products) |
| Customer order placed | Customers | Marketplace (order path start) |
| Producer order | Marketplace (Order and checkout) | Producers |
| Payment request | Marketplace (Payment and settlement) | Payment service |
| Payment result | Payment service | Marketplace (Payment and settlement) |
| Confirmation to customer | Marketplace (Payment and settlement) | Customers |
| Weekly settlement | Marketplace (Payment and settlement) | Producers |
| Concern reported | Customers | Marketplace (quality path start) |
| Incident response request | Marketplace (Quality incident handling) | Producers |
| Safety notice | Marketplace (Quality incident handling) | Customers |

## 7. Before you commit

- [ ] Marketplace pool has two lanes with the exact names
- [ ] Message start event, two collapsed sub-processes with placeholders, and an end event, joined in order
- [ ] 'Order and checkout' is blue and 'Payment and settlement' is green
- [ ] Customers, Producers and Payment service pools are empty black boxes
- [ ] 6 message flows drawn and labelled with the exact names
- [ ] Sub-process names match the functionalities in your scoping document, or the traceability row explains the difference
- [ ] No gateways, forms or detailed exception paths
- [ ] File opens in Camunda Modeler with no errors or warnings

## 8. Commit

Commit `part-okan.bpmn` and `part-okan.png` from your own GitHub account. Message example: `SCRUM-13: Okan part - order, checkout and payment path`.

## 9. Assumptions and open questions

- Delivery or collection is not its own step, because the case study does not say who delivers. It sits inside checkout and fulfilment.
- The two functionalities still need agreement from the module team. Note the status in your scoping document.
- The shared records data store (customer, product and producer records) sits in Alec's part. Do not draw a second one. If you want to show orders being recorded, tell Alec and he will link it when merging.

## 10. If you get stuck

Message Alec in the group chat. Do not wait until Friday. A rough but committed version is better than nothing.
