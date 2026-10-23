# SCRUM 15 - Use Case Selection and Functionality Scoping

#### Selected Use Case: Use Case 2 - Customer Multi-Vendor Order, Checkout and Payment
#### Lead: Okan Kaynak (Automation)
#### Supporting Members: Bartek(Analysis), Alec(Tutor Contact)
#### Status: In Progress

---

## Introduction

This document evaluates the three use cases provided by the University and justifies the selection of the Customer Multi-Vendor Order, Checkout and Payment Use Case (Use Case 2). It also proposes two substantial business functionalities for further development: **Checkout & Order Processing** and **Payment Processing & Settlement**.

Each functionality will be scoped by identifying its trigger, actors, main process steps, business decisions, automated activities, external service interactions and exception flows. The document will also consider how these functionalities can relate to the strategic BPMN model and assess their feasibility for implementation using Camunda during Sprints 2 and 3.



## Use Case Comparison

| Use Case | Advantages | Challenges |
|---|---|---|
| **1. Producer Listing and Inventory** | Automated product validation, stock updates, external certification checks, clear decision points | Managing seasonal availability, invalid product information and unavailable certification services. |
| **2. Multi-Vendor Checkout and Payment** | Automated stock checks, multi-producer order processing, external payment integration and clear exception handling. | Managing stock shortages, declined payments, unavailable payment services and accurate producer payment allocations. |
| **3. Food Quality Incident and Traceability** | Product traceability, automated customer notifications, incident escalation and human decision-making. | Identifying affected customers, managing product suspensions, failed notifications and incident follow-ups. |



## Selected Use Case Justification

As a group, we have selected Use Case 2 - Customer Multi-Vendor Order, Checkout and Payment as it covers two major business functionalities: **Checkout & Order Processing** and **Payment Processing & Settlement**. 

This use case was the most suitable choice because it involves customer interactions, automated tasks such as stock checking and payment processing, business decisions using gateways, and integration with an external payment service. In addition, it includes exception handling for situations such as insufficient stock or declined payments. These features make Use Case 2 suitable for BPMN modelling and Camunda implementation, as they allow us to develop two connected business processes. 

Compared to the other use cases, we decided that this use case is the best choice because it offers a good balance of functionality and complexity while remaining achievable within the project timeframe.

## Functionality 1 - Checkout & Order Processing

  - **Purpose**: To allow customers to purchase products from multiple producers in a single order while ensuring that products are available and the order information is correct.
  - **Trigger**: The customer proceeds to checkout after adding products to their basket.
  - **Actors**:
      - **Customer**: reviews their basket and confirms their order.
      - **Marketplace System**: checks stock availability, identifies producers and creates the order.
      - **Producers**: supply the products included in the order.
  - **Main Process**:
      1. Customer adds products from one or more producers to their basket.
      2. Customer proceeds to checkout.
      3. System checks whether the selected products are available in the requested quantities.
      4. If sufficient stock is available, the system identifies the producers involved.
      5. System determines the relevant delivery or collection arrangements.
      6. Customer reviews the order details and confirms the purchase.
      7. System creates the overall marketplace order.
      8. System separates the order into producer-specific order elements.
      9. The order proceeds to payment processing.
  - **Exceptions**:
      - Insufficient Stock: The customer is informed and asked to amend their basket.
      - Unavailable Product: The customer must remove or change the unavailable product before continuing.
      - Order Creation Failure: The system records the failure and follows a recovery process.
  - **Proposed BPMN Elements**:
    - **User Tasks/Forms**: Customer reviews and confirms their order.
    - **Service Tasks (Automated Processing)**: Check stock availability, identify producers and create the order.
    - **Gateways (Business Decisions)**: Determine whether sufficient stock is available to proceed.
    - **External Service Integration**: Proposed inventory API or simulated stock-checking service.
    - **Exception Handling**: Return the customer to their basket if stock is insufficient, allowing them to amend the order.
  - **Strategic BPMN Traceability**: This functionality relates to the customer checkout and order management activities in the strategic BPMN model. The specific activities will be identified once the group's strategic BPMN diagram is confirmed.


## Functionality 2 - Payment Processing & Settlement

  - **Purpose**: To process customer payments, calculate the marketplace's commission and manage the payments allocated to each producer.
  - **Trigger**: The customer has confirmed their order and proceeds to payment.
  - **Actors**:
      - **Customer**: provides payment details and completes the payment.
      - **Marketplace System**: processes payment requests, calculates commission and records producer payment allocations.
      - **Payment Provider**: handles the payment transaction and returns the payment result.
      - **Producers**: receive their allocated payments through the settlement process.
  - **Main Process**:
      1. The system receives the confirmed order from the checkout process.
      2. Customer selects their payment method and submits payment details.
      3. System sends a payment request to the external payment provider.
      4. Payment provider processes the transaction and returns the result.
      5. If payment is successful, the system confirms the payment and updates the order status.
      6. System calculates the marketplace's 5% commission.
      7. System calculates and records the amount allocated to each producer.
      8. System sends order notifications to the relevant producers.
      9. Producer payments are processed accordingly to the weekly settlement schedule.
  - **Exceptions**:
      - Payment Declined: Customer is informed and given the option to retry or use a different payment method.
      - Payment Service Unavailable: The system records the issue and follows a retry or recovery process.
      - Notification Failure: The system records the failed notification and attempts to retry.
      - Settlement Failure: The failed settlement is recorded for retry or manual review.
  - **Proposed BPMN Elements**:
    - **User Tasks/Forms**: Customer selects a payment method and submits payment details.
    - **Service Tasks (Automated Processing)**: Submit payment request, calculate commission, record producer allocations and send notifications.
    - **Gateways (Business Decisions)**: Determine whether payment and settlement was successful.
    - **External Service Integration**: External payment provider or simulated payment gateway.
    - **Exception Handling**: Allow payment retries, handle unavailable payment services and record failed settlements.
  - **Strategic BPMN Traceability**: This functionality relates to payment processing, producer payment allocation and settlement activities in the strategic BPMN model.




## Camunda Feasibility Assessment

### Functionality 1 - Feasibility
Checkout & Order Processing is considered feasible to implement using Camunda, as it supports the user tasks, automated processing and gateways required for this functionality. The main challenge would be integrating the stock-checking service and handling unavailable products.

### Functionality 2 - Feasibility
Payment Processing & Settlement is considered feasible using Camunda, although it is more complex due to external payment integration and producer settlements. A simulated payment gateway could be used to simplify implementation and test different payment outcomes.


### Sprint 2 & 3 Implementation Plan
- Sprint 2: Develop the BPMN models, configure the main tasks and gateways, and begin implementing the processes using simulated external services.
- Sprint 3: Complete the implementation, connect both functionalities, test exception handling and prepare the processes for the final demonstration.
