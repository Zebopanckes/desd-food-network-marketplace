# Operational BPMN

Detailed end-to-end BPMN models for the two agreed major functionalities (Sprint 2).

## Running in Camunda (Play / Test mode)

Upload both folders' BPMNs, forms and DMN into one Web Modeler project. For the checkout-to-payment handoff, deploy both processes to the same Camunda 8.9 cluster and default tenant, with the built-in Message Send connector enabled. The forms and the DMN are linked by ID (`formId`, `decisionId`) and must also be deployed.

Example data supplies suggested variables when you manually start instances, complete jobs or publish messages in Play/Test. It does not implement the custom service-task workers or automatically publish payment responses. Outside Play/Test, those job types still need their external implementations; no runner or workers are included here.

### Order and checkout

- Starting with empty variables `{}` (or with the start event's example data) defaults to `apple-box`, quantity `5` (`BASKET-1001`, `CUST-001`), which passes the stock check. Do not pass `sufficientStock` at start—the DMN computes and overwrites `sufficientStock`.
- To test the shortage path, start with `requestedQuantity` above the demo stock (`apple-box` 20, `bread-loaf` 10), or use any other `productId` to test an unavailable item.
- The decision `check-stock-availability` uses hit policy FIRST. Choosing to remove an unavailable item sets `productId` to `remaining-basket-items` and `requestedQuantity` to `0`, which passes so remaining multi-vendor items can proceed; negative quantities fail.
- "Create marketplace order" must return `orderId` and a positive `paymentAmount`; its example output provides both.
- The "Payment requested" end event uses Camunda's [Message Send connector](https://docs.camunda.io/docs/8.9/components/connectors/out-of-the-box-connectors/message-send/) in correlate mode. It sends the order IDs, amount and producer sub-orders to F2's "Payment requested" message start and stores the receipt in `paymentHandoff`. A missing receiver raises an incident instead of discarding the message.
- A message arrow does not link Play deployment dependencies. Do not assume playing F1 alone deploys F2 into that isolated environment. For the real handoff both must be deployed in the same environment; mocking/completing the connector job without executing it does not start F2. F2 can also be played independently with its start example data.

### Payment and settlement

- "Send payment request" generates `paymentRequestId` (`PAY-<orderId>-<attempt>`) before sending, and exposes it after completion. The payment service must echo it as the correlation key for the "Payment result" message. A missing order ID raises an incident.
- After completing the send task in Play/Test, use the publish-message action on "Payment result received", not the gateway, with `{"paymentSuccessful": true, "paymentReference": "TXN-5001"}` and the current `paymentRequestId`.
- To test a decline, publish "Payment result" with that `paymentRequestId` as the correlation key and variables like `{"paymentSuccessful": false, "paymentFailureReason": "Card declined"}`.
- Response flags accept booleans or the strings `"true"`/`"false"` and are normalized to booleans. Missing/invalid flags raise an explicit incident rather than silently taking "No". Retrying clears the previous result and failure reason and uses a new correlation key.
- If no matching message arrives within 2 minutes, the timeout route runs. A real payment service should buffer early replies with a positive message TTL so they are not lost before the catch event is active.
- The weekly settlement path has its own timer start event; select it in Test mode when supported. Supply `pendingSettlementsFound` and `settlementBatchId` when completing "Retrieve pending settlements" (example data is included). "Initiate weekly producer settlements" prepares `settlementRequestId` (`SET-<settlementBatchId>-<attempt>`) before sending. Publish "Settlement result" with that key and `{"settlementSuccessful": true}` or `{"settlementSuccessful": false, "settlementFailureReason": "..."}`.
