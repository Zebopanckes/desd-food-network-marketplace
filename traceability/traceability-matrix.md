# Traceability matrix

Traces i* (SD and SR) to the strategic BPMN, the two operational BPMN models and the Camunda implementation.

- The first column uses element names from the i* models. Dependencies are written as depender to dependee, with the dependum (name from the SD) and its type.
- Strategic BPMN names are taken from `bpmn-strategic/bristol-regional-food-network-strategic.bpmn`.
- Operational BPMN: F1 = `bpmn-operational/order-checkout/functionality-1-order-and-checkout.bpmn.bpmn`, F2 = `bpmn-operational/payment-processing/functionality-2-payment-and-settlement.bpmn.bpmn`.
- Camunda implementation = what is executable in the same files (job types, forms, DMN, message correlation, FEEL).
- "Not modelled" means the element belongs to a use case outside the two selected functionalities (Use Case 2 only).

## Camunda artefacts

| Artefact | File | Used by |
| --- | --- | --- |
| Executable process F1 (`Marketplace Order and Checkout (Functionality 1)`) | `bpmn-operational/order-checkout/functionality-1-order-and-checkout.bpmn.bpmn` | Checkout |
| DMN `Check Stock Availability` (id `check-stock-availability`, hit policy First, simulated stock) | `bpmn-operational/order-checkout/decision-check-stock-availability.dmn` | F1 business rule task `Check stock availability` |
| Forms `form-review-and-confirm-order`, `form-inform-and-amend-basket`, `form-remove-or-replace-product` | `bpmn-operational/order-checkout/` | F1 user tasks |
| Executable process F2 (`Marketplace Payment and Settlement (Functionality 2)`) | `bpmn-operational/payment-processing/functionality-2-payment-and-settlement.bpmn.bpmn` | Payment and settlement |
| Forms `form-retry-or-cancel-payment`, `form-review-failed-settlement` | `bpmn-operational/payment-processing/` | F2 user tasks |

## 1. Actors

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer (role) | Pool `Customers` | F1 and F2 pool `Customers` | Customer interaction through Camunda Forms (user tasks) |
| Producer (role) | Pool `Producers` | F2 pool `Producers` (message flows `Producer order`, `Retried producer order`, `Weekly settlement`) | Producers only receive messages; no user task |
| Marketplace (agent) | Pool `Marketplace` (lanes: Product and Inventory Management, Order and Checkout, Payments and Settlement, Quality and Traceability, Network Administration and Sustainability) | F1 process `Marketplace Order and Checkout (Functionality 1)`; F2 process `Marketplace Payment and Settlement (Functionality 2)` | Two executable processes (`isExecutable=true`) |
| Platform Admin (role) | Lane `Network Administration and Sustainability` in pool `Marketplace` | F2 lane `Network Administration and Sustainability`: user task `Review failed settlement`, gateway `Retry settlement? (failures < 3)`, task `Flag for manual follow-up` | User task with `form-review-failed-settlement` |
| Payment Service (agent) | Pool `Payment service` | F2 pool `Payment Service`; F1 pool `Payment and Settlement (Functionality 2)` (target of the `Payment requested` handoff) | Messages `Payment result` and `Settlement result` (published to the engine by the simulated service) |
| External Stock Service (not an i* actor) | Not in the strategic BPMN | F1 and F2 pool `External Stock Service (Inventory API)` (message flows `Reserve stock`, `Release stock`, `Deduct stock`) | Stock availability is simulated by the DMN |

## 2. Dependencies (SD, 14)

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer `Add products to basket` to Marketplace `Coordinate multi-vendor order`: Multi-vendor order coordination (task) | Task `Place order and pay`; message flow `Customer order placed`; subprocess `Order and checkout` | F1 message start event `Customer order placed` (message flow `Customer order placed`); F1 tasks `Identify producers in the order`, `Split into producer-specific order elements` | F1 message start event; job types `identify-producers`, `split-producer-order-elements` |
| Customer `Pay and retry if declined` to Marketplace `Request payment and update status`: Order confirmed (goal) | Message flow `Confirmation to customer` (subprocess `Payment and settlement` to task `Receive confirmation, delivery or collection`); end event `Order confirmed and producers paid` | F2 gateway `Payment successful?` (Yes), tasks `Confirm order`, `Send order confirmation` (message flow `Confirmation to customer`); end event `Payment completed` | Condition `= paymentSuccessful = true`; job types `confirm-order`, `send-order-confirmation` |
| Customer `Pay and retry if declined` to Marketplace `Secure`: Payment secure (quality) | Subprocess `Payment and settlement`; message flows `Payment request` and `Payment result` | F2 `Payment result received`, `Payment response timeout`, gateway `Payment successful?`, flows to `Notify customer of payment decline` and `Record payment service unavailable` | Message `Payment result` with correlation key `=paymentRequestId`; timer PT2M; default flow = failure |
| Marketplace `Request payment and update status` to Customer `Pay and retry if declined`: Valid payment details (resource) | Task `Place order and pay` into subprocess `Payment and settlement` | F1 user task `Review and confirm order`; F1 end event `Payment requested` (payment amount); F2 start event `Payment requested` | Form `form-review-and-confirm-order`; Message Send connector with `orderId` correlation; payment details are simulated (no card data) |
| Marketplace `Request payment and update status` to Payment Service `Process payment and return result`: Process payment and return result (task) | Message flows `Payment request` and `Payment result` (subprocess `Payment and settlement` and pool `Payment service`) | F2 service task `Send payment request`; message flows `Payment request`, `Payment result`; event `Payment result received` | Job type `send-payment-request`; `paymentRequestId` is built as `PAY-<orderId>-<attempt>` |
| Customer `Browse products and check origin` to Marketplace `Publish product info & origin`: Product info, origin and availability (resource) | Task `Browse and search products`; subprocess `List and manage products`; end event `Products listed and in stock` | Not modelled (Use Case 1). Availability is checked in F1 business rule task `Check stock availability` | DMN `Check Stock Availability` (`productId`, `requestedQuantity` to `sufficientStock`) |
| Customer `Receive delivery or collection` to Producer `Prepare orders with 48h notice`: Safe, accurate local produce and fulfilment (resource) | Task `Receive confirmation, delivery or collection` (Customers); task `Receive and prepare orders (48h notice)` (Producers) | F1 task `Determine delivery or collection arrangements`; F2 `Send order confirmation` and `Notify producers` | Job types `determine-delivery-arrangements`, `send-order-confirmation`, `notify-producers` |
| Producer `Prepare orders with 48h notice` to Marketplace `Coordinate multi-vendor order`: Producer order with 48h notice (resource) | Message flow `Producer order` (subprocess `Order and checkout` to task `Receive and prepare orders (48h notice)`) | F2 task `Notify producers` (message flow `Producer order`); boundary event `Producer notification failed`; `Record notification failure`; `Retry producer notification` (message flow `Retried producer order`); `Flag notification for manual follow-up` | Job types `notify-producers`, `record-notification-failure`, `retry-producer-notification`, `flag-producer-notification-followup`; error `ERR_PRODUCER_NOTIFY` |
| Producer `List products & update stock/allergens` to Customer `Add products to basket`: Orders (goal) | Task `Place order and pay`; message flow `Customer order placed` | F1 message start event `Customer order placed` | F1 message start event |
| Producer `Get paid weekly` to Marketplace `Weekly settlement & commission`: Weekly settlement with audit trail (goal) | Message flow `Weekly settlement` (subprocess `Payment and settlement` to task `Receive weekly settlement`) | F2 start event `Weekly settlement due`; tasks `Retrieve pending settlements`, `Initiate weekly producer settlements`, `Record successful settlement`, `Notify producers of settlement` (message flow `Weekly settlement`); audit trail via `Record pending settlements` | Job types `record-pending-settlements`, `retrieve-pending-settlements`, `initiate-weekly-settlements`, `record-successful-settlement`, `notify-producers-settlement` |
| Producer `List products & update stock/allergens` to Marketplace `Provide listing and inventory tool`: Simple listing and inventory tool (quality) | Task `List products and update seasonal stock`; subprocess `List and manage products`; message flow `Listing submitted` | Not modelled (Use Case 1). Stock is held in the DMN | DMN rows act as the mock stock table |
| Marketplace `Publish product info & origin` to Producer `List products & update stock/allergens`: Accurate stock and allergen info (resource) | Task `List products and update seasonal stock`; message flow `Listing submitted`; subprocess `List and manage products` | Stock interactions only: F1 `Reserve stock` (message flow `Reserve stock`), F1 `Release stock reservation`, F2 `Update inventory` (message flow `Deduct stock`) | Job types `reserve-stock`, `release-stock-reservation`, `update-inventory` |
| Platform Admin `Earn 5% commission` to Marketplace `Weekly settlement & commission`: Calculate 5% commission (task) | Subprocess `Payment and settlement` (lane `Payments and Settlement`); message flow `Weekly settlement` | F2 script tasks `Calculate 5% commission`, `Calculate producer allocation` | FEEL results `commissionAmount`, `producerAllocations` |
| Marketplace `Weekly settlement & commission` to Payment Service `Process producer payouts and return result`: Process producer payouts and return result (task) | No payout message flow in the strategic BPMN (gap, see open points) | F2 message flows `Settlement request`, `Settlement result`, `Settlement retry request`; tasks `Initiate weekly producer settlements`, `Wait for settlement result`, `Retry failed settlements`; gateway `Settlement successful?` | Job types `initiate-weekly-settlements`, `retry-failed-settlements`; message `Settlement result` with correlation key `=settlementRequestId`; timer PT2M |

## 3. Goals, tasks, resources and qualities (SR)

| Stakeholder goal / dependency (i*) | Strategic BPMN element | Operational BPMN element | Camunda implementation |
| --- | --- | --- | --- |
| Customer: Buy local produce conveniently | Customers process: start event `Needs local food` to end event `Needs met` | F1 `Customer order placed` to `Payment requested`, then F2 to `Payment completed` | F1 hands over to F2 with the Message Send connector |
| Customer: Receive order | Task `Receive confirmation, delivery or collection`; end event `Needs met` | F2 `Send order confirmation` | Job type `send-order-confirmation` |
| Customer: Browse products and check origin | Task `Browse and search products` | Not modelled (browsing happens before checkout) | None |
| Customer: Add products to basket | Task `Place order and pay` | F1 message start event `Customer order placed` | Variables `basketId`, `customerId`, `productId`, `requestedQuantity` |
| Customer: Pay and retry if declined | Task `Place order and pay`; subprocess `Payment and settlement` | F1 user task `Review and confirm order`; F2 `Notify customer of payment decline`, user task `Review payment issue and retry or cancel`, gateway `Retry payment? (failures < 3)` | Forms `form-review-and-confirm-order`, `form-retry-or-cancel-payment`; `paymentRetryCount` |
| Customer: Receive delivery or collection | Task `Receive confirmation, delivery or collection` | F1 `Determine delivery or collection arrangements` | Job type `determine-delivery-arrangements` |
| Customer: Product information (resource) | Input to task `Browse and search products`; output of subprocess `List and manage products` | F1 `Check stock availability` reads `productId` | DMN input `productId` |
| Customer: Payment details (resource) | Input to task `Place order and pay` and subprocess `Payment and settlement` | F1 `Review and confirm order`; F2 `Send payment request` | `paymentAmount`; simulated, no card data |
| Marketplace: Orders coordinated and processed | Subprocess `Order and checkout` (lane `Order and Checkout`) | F1 process; subprocesses `Handle stock shortage` and `Order creation and retry` | F1 executable process |
| Marketplace: Coordinate multi-vendor order | Subprocess `Order and checkout`; start event `Customer order placed` | F1 `Identify producers in the order`, `Create marketplace order`, `Split into producer-specific order elements` | Multi-instance over `orderProducers`; job types per task |
| Marketplace: Products listed | Subprocess `List and manage products` (lane `Product and Inventory Management`); end event `Products listed and in stock` | Not modelled (Use Case 1) | None |
| Marketplace: Publish product info & origin | Subprocess `List and manage products` | Not modelled; availability check only | DMN |
| Marketplace: Provide listing and inventory tool | Subprocess `List and manage products`; start event `Listing submitted` | Not modelled (Use Case 1) | DMN stands in for stock data |
| Marketplace: Payments and settlements handled | Subprocess `Payment and settlement` (lane `Payments and Settlement`); end event `Order confirmed and producers paid` | F2 process (start events `Payment requested` and `Weekly settlement due`) | F2 executable process |
| Marketplace: Request payment and update status | Subprocess `Payment and settlement`; message flows `Payment request`, `Payment result`, `Confirmation to customer` | F2 `Send payment request`, `Confirm order`, `Update inventory`, `Send order confirmation` | Job types as listed in section 2 |
| Marketplace: Weekly settlement & commission | Subprocess `Payment and settlement`; message flow `Weekly settlement` | F2 weekly settlement path from `Weekly settlement due` to `Weekly settlements completed` | Timer start, `pendingSettlementsFound`, `settlementSuccessful` |
| Producer: Reach customers beyond farm gate | Producers process: start event `Has produce to sell` to end event `Produce sold` | F2 `Notify producers` | Job type `notify-producers` |
| Producer: List products & update stock/allergens | Task `List products and update seasonal stock`; message flow `Listing submitted` | Not modelled (Use Case 1) | None |
| Producer: Prepare orders with 48h notice | Task `Receive and prepare orders (48h notice)` | F2 `Notify producers` (message flow `Producer order`) | Job type `notify-producers` |
| Producer: Get paid weekly | Task `Receive weekly settlement`; end event `Produce sold` | F2 `Record successful settlement`, `Notify producers of settlement` | Job types `record-successful-settlement`, `notify-producers-settlement` |
| Producer: Accept 5% commission on sales | Task `Receive weekly settlement` (commission deducted at settlement) | F2 `Calculate 5% commission` (calculated at payment time) | FEEL `commissionAmount` |
| Platform Admin: Earn 5% commission | Subprocess `Payment and settlement` (no admin task in the strategic BPMN) | F2 `Calculate 5% commission`, `Calculate producer allocation` | FEEL script tasks |
| Payment Service: Process payment and return result | Pool `Payment service`; message flows `Payment request` and `Payment result` | F2 pool `Payment Service`; `Payment result received` | Message `Payment result` |
| Payment Service: Process producer payouts and return result | Pool `Payment service` (no payout message flow in the strategic BPMN) | F2 pool `Payment Service`; `Wait for settlement result` | Message `Settlement result` |
| Qualities: Convenient, Trustworthy, Accurate info, Low admin effort, Fair income | No direct strategic BPMN element (non-functional) | Addressed by forms, retry and exception paths | Forms, retries, timers |
| Quality: Secure (Marketplace) | Message flows `Payment request` and `Payment result` with pool `Payment service` | F2 `Payment result received` validation, timers, retry limit (3) | Correlation key `=paymentRequestId`; assertions on `paymentSuccessful` |
| Quality: Compliant (Marketplace) | Subprocess `Payment and settlement` (weekly settlement and audit trail) | F2 `Record pending settlements`, `Record successful settlement`, `Record settlement failure` | Job types per task |

## 4. Enterprise context (not implemented)

These are in the strategic BPMN but outside the two selected functionalities, and are not in the i* models.

- Pool `Organic certification service` and message flows `Certification check request` and `Certification result`.
- Subprocess `Quality incident handling`, start event `Quality concern reported`, data store `Customer, product and producer records`.
- Subprocess `Review and close incident`.
- Subprocess `Community and sustainability reporting`.
- Customers tasks `Report food quality concern` and `Act on safety notice`.
- Producers task `Respond to quality incident`.

## 5. Open points and known gaps

- Strategic BPMN task is `Place order and pay` (the previous matrix said `Place multi-vendor order and pay`).
- The strategic BPMN has no payout message flows to the `Payment service` pool. F2 has `Settlement request`, `Settlement result` and `Settlement retry request`.
- The strategic BPMN sends `Producer order` from subprocess `Order and checkout`. F2 sends it after payment succeeds (`Notify producers`), as in Use Case 2.
- The strategic end event `Order confirmed and producers paid` implies per-order payout. F2 pays producers through the weekly settlement path.
- `Payment requested` (F1 to F2 handoff) is an internal message. The strategic BPMN only shows `Payment request` and `Payment result` with the `Payment service` pool.
- `External Stock Service (Inventory API)` appears in F1 and F2 but not in the strategic BPMN or the i* models.
- Naming differences: pool `Payment service` (strategic) against `Payment Service` (F2 and i*); F1 pool `Payment and Settlement (Functionality 2)`.
- Commission is calculated per payment in F2 (`Calculate 5% commission`), while the i* and strategic BPMN describe it at weekly settlement.
- `bpmn-operational/payment-processing/functionality-2-payment-and-settlement.bpmn.png` was last updated before the latest F2 change and should be regenerated. F1 has no exported image.
- All service tasks except the F1 business rule task, the F2 script tasks and the Message Send connector still need job workers or Play/Test completion. Stock data and payment results are simulated for Sprint 2.
