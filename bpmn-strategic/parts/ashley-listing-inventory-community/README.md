# Ashley: Listing and inventory path, and community and sustainability path

**Jira:** SCRUM-13 (strategic BPMN). **Due:** Fri 23 Oct. **Merged by:** Alec, Sun 25 Oct.

Keep this open next to Camunda Modeler. It tells you what to read, what to draw, and what to check before you commit.

## 1. Your part in one paragraph

You draw two small processes inside the **Marketplace** pool: path A (producer product listing and inventory, which talks to the organic certification service) and path D (monthly community and sustainability reporting). Everything else around them is a black box. These are the smallest parts of the diagram, so you also have time to help Bartek with the process table in SCRUM-11 if you can.

## 2. Read these first (inputs)

| What | Where | Why |
|---|---|---|
| Case study | Bristol Regional Food Network case study (course materials) | Product listing, certification, surplus, food miles, recipes and farm stories |
| Walkthrough notes | [rough-meeting-notes.md](../../rough-meeting-notes.md) | Sections Marketplace path A, path D and External pools |
| Strategic BPMN README | [README.md](../../README.md) | Conventions and structure table |
| Process list | [stakeholder-analysis.md](../../../docs/analysis/stakeholder-analysis.md) | Your SCRUM-12 process list lives in the processes table |
| Example of a finished part | [Alec's folder](../alec-qual-incident-network-admin/) | Shows a Marketplace pool with lanes, a message start event and collapsed sub-processes |

## 3. What to draw

### Marketplace pool with two lanes

Create one pool named **Marketplace** with two lanes, using these exact names, top to bottom:

1. Product and Inventory Management
2. Network Administration and Sustainability

Alec's incident path uses the left side of the admin lane. Start your community path to the right of it (about 900 px from the left of the pool) and make the pool wide enough.

### Path A: listing and inventory (lane: Product and Inventory Management)

| Order | Element | Type | Name |
|---|---|---|---|
| 1 | Start | Message start event | Listing submitted |
| 2 | Process | Collapsed sub-process | List and manage products |
| 3 | End | End event | Products listed and in stock |

Inside the sub-process put a placeholder: Start, then a task named 'Placeholder: detail in Sprint 2', then End.

### Path D: community and sustainability (lane: Network Administration and Sustainability)

| Order | Element | Type | Name |
|---|---|---|---|
| 1 | Start | Timer start event | Monthly |
| 2 | Process | Collapsed sub-process | Community and sustainability reporting |
| 3 | End | End event | Reports and offers published |

This path covers food miles reporting, surplus and last-minute discounts, recipes and farm stories. Put the same placeholder inside. It is standalone for now, with no message flows.

### Black-box pools (empty, collapsed)

| Pool | Why |
|---|---|
| Producers | Sends the listing message |
| Organic certification service | External service you talk to |

Leave both empty.

### Message flows you draw

| Message name | From | To |
|---|---|---|
| Listing submitted | Producers pool | Message start event 'Listing submitted' |
| Certification check request | Sub-process 'List and manage products' | Organic certification service pool |
| Certification result | Organic certification service pool | Sub-process 'List and manage products' |

The certification service connects both ways: the organic claim goes out, the result comes back.

## 4. Step by step in Camunda Modeler

1. File, then New BPMN diagram. Save it in this folder as `part-ashley.bpmn`.
2. Create the Marketplace pool, add a second lane, and name both lanes exactly.
3. Draw path A in the first lane, left to right. Make the sub-process collapsed: drag a sub-process in, then click the small minus marker to collapse it.
4. Click the plus marker on the collapsed box to open it, add Start, the placeholder task and End, then use the breadcrumb at the top to go back. If your Modeler has no plus marker, tell Alec.
5. Draw path D in the second lane, to the right. For the timer, add a start event and set its type to Timer using the wrench menu. Collapse the sub-process and add the placeholder the same way.
6. Create empty Producers and Organic certification service pools above and below the Marketplace pool.
7. Draw and name the three message flows. Select the sending element, drag from the message flow icon in its context pad to the receiver, and double-click to name it.
8. Export a PNG and save it here as `part-ashley.png`.

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
- [ ] Path A: message start, collapsed sub-process with placeholder, end event
- [ ] Path D: timer start, collapsed sub-process with placeholder, end event
- [ ] Producers and Organic certification service pools are empty black boxes
- [ ] 3 message flows drawn and labelled with the exact names
- [ ] Both processes appear in the SCRUM-11 process table, and the names match
- [ ] No gateways, forms or detailed exception paths
- [ ] File opens in Camunda Modeler with no errors or warnings

## 8. Commit

Commit `part-ashley.bpmn` and `part-ashley.png` from your own GitHub account. Message example: `SCRUM-13: Ashley part - listing and inventory, community and sustainability`.

## 9. Assumptions and open questions

- The notes say listing flows on to the order path (products must be listed and in stock before orders). End your path with the end event above. Alec will decide at the merge whether to link the two.
- Are the community and sustainability features major enough to keep as their own process? Draw it for now and give your view to Alec.
- Check both processes against the case study and the SCRUM-11 table.

## 10. If you get stuck

Message Alec in the group chat. Do not wait until Friday. A rough but committed version is better than nothing.
