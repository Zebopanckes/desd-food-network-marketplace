# Bartek: Customers and Producers pools

**Jira:** SCRUM-13 (strategic BPMN). **Due:** Fri 23 Oct. **Merged by:** Alec, Sun 25 Oct.

Keep this open next to Camunda Modeler. It tells you what to read, what to draw, and what to check before you commit.

## 1. Your part in one paragraph

You draw the two actor pools: **Customers** and **Producers**. Each is a short left-to-right process showing what that actor does and where it sends or receives messages from the Marketplace. You are not drawing the Marketplace itself. Draw it as an empty black-box pool so your message flows have somewhere to connect.

## 2. Read these first (inputs)

| What | Where | Why |
|---|---|---|
| Case study | Bristol Regional Food Network case study (course materials) | Source of truth for what each actor does |
| Walkthrough notes | [rough-meeting-notes.md](../../rough-meeting-notes.md) | Step-by-step wording for both pools (sections Customers and Producers) |
| Strategic BPMN README | [README.md](../../README.md) | Conventions and the structure table |
| Your stakeholder analysis | [stakeholder-analysis.md](../../../docs/analysis/stakeholder-analysis.md) | Check the steps match the needs and responsibilities you wrote |
| Existing rough draft | [strategic-bpmn-draft.bpmn](../../strategic-bpmn-draft.bpmn) | Has a rough Customers flow. Use it for wording only, then redraw in Camunda Modeler |
| Example of a finished part | [Alec's folder](../alec-qual-incident-network-admin/) | Shows the target look: black-box pools, collapsed sub-processes, labelled message flows |

## 3. What to draw

### Customers pool (expanded)

One pool for all three customer segments (families, community groups, restaurants and cafes). They follow the same flow at this level.

| Order | Element | Type | Name |
|---|---|---|---|
| 1 | Start | Start event | Needs local food |
| 2 | Task | Task | Browse and search products |
| 3 | Task | Task | Place multi-vendor order and pay |
| 4 | Task | Task | Receive confirmation, delivery or collection |
| 5 | Task | Task | Report food quality concern or receive safety notice |
| 6 | End | End event | Needs met |

Join them with sequence flows in that order.

### Producers pool (expanded)

| Order | Element | Type | Name |
|---|---|---|---|
| 1 | Start | Start event | Has produce to sell |
| 2 | Task | Task | List products and update seasonal stock |
| 3 | Task | Task | Receive and prepare orders (48h notice) |
| 4 | Task | Task | Receive weekly settlement |
| 5 | Task | Task | Respond to quality incident |
| 6 | End | End event | Produce sold |

### Marketplace pool (black box)

An empty pool named **Marketplace**, placed between Customers (top) and Producers (bottom). Leave it empty.

### Message flows you draw

Use exactly these names. Customers messages connect to the Marketplace pool edge. Alec replaces the black box with the real Marketplace when merging.

| Message name | From | To |
|---|---|---|
| Customer order placed | Customers: Place multi-vendor order and pay | Marketplace |
| Confirmation to customer | Marketplace | Customers: Receive confirmation, delivery or collection |
| Concern reported | Customers: Report food quality concern or receive safety notice | Marketplace |
| Safety notice | Marketplace | Customers: Report food quality concern or receive safety notice |
| Listing submitted | Producers: List products and update seasonal stock | Marketplace |
| Producer order | Marketplace | Producers: Receive and prepare orders (48h notice) |
| Weekly settlement | Marketplace | Producers: Receive weekly settlement |
| Incident response request | Marketplace | Producers: Respond to quality incident |

The report concern task has two message flows, one out and one in. That is intended.

## 4. Step by step in Camunda Modeler

1. File, then New BPMN diagram. Save it in this folder as `part-bartek.bpmn`.
2. Create the **Customers** pool with the pool tool. Name it.
3. Add the start event, four tasks and the end event inside it, left to right, and join them with sequence flows.
4. Create the **Producers** pool below it and repeat for its six elements.
5. Create an empty **Marketplace** pool between them. Name it. Do not add anything inside.
6. Draw each message flow in the table above: select the sending element, drag from the message flow icon in its context pad to the receiving element or pool, then double-click the flow to type its name.
7. Keep the layout tidy: tasks in a straight line, pools the same width, message flows as straight vertical lines where possible.
8. Export a PNG (File, then Export as, then PNG) and save it here as `part-bartek.png`.

## 5. Shared rules (everyone follows these so the parts merge cleanly)

- **Level:** strategic only. Use process boxes. No detailed gateways, forms or exception paths (that is Sprint 2).
- **Naming:** use the exact pool, lane and message names given in this guide. Activities are verb plus object, for example 'List products and update seasonal stock'.
- **Flows:** sequence flows inside a pool, message flows between pools.
- **Pools you do not own:** draw them as empty black boxes. Alec swaps in the real ones when merging.
- **IDs:** leave the IDs Camunda Modeler generates. Do not rename them.
- **Major processes** (not in your part) are drawn as collapsed sub-processes with a placeholder inside. See Alec's file for the pattern.

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

- [ ] Customers pool has 1 start event, 4 tasks, 1 end event, all joined in order
- [ ] Producers pool has 1 start event, 4 tasks, 1 end event, all joined in order
- [ ] Marketplace pool is empty and named exactly
- [ ] All 8 message flows are drawn and labelled with the exact names
- [ ] No gateways, forms or detailed exception paths
- [ ] Steps match the case study and your stakeholder analysis
- [ ] File opens in Camunda Modeler with no errors or warnings

## 8. Commit

Commit `part-bartek.bpmn` and `part-bartek.png` from your own GitHub account. Message example: `SCRUM-13: Bartek part - Customers and Producers pools`.

## 9. Assumptions and open questions

- Delivery or collection is not its own step, because the case study does not say who delivers. It sits inside checkout and fulfilment.
- Only the customer can report a quality concern for now. Should producers and admin also be able to? Note your view in the commit message or message Alec.
- If anything in the case study contradicts the notes, follow the case study and tell Alec.

## 10. If you get stuck

Message Alec in the group chat. Do not wait until Friday. A rough but committed version is better than nothing.
