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

  - Purpose: 
  - Trigger:
  - Actors:
  - Main Process:
  - Exceptions:
  - Proposed BPMN Elements:
    - User Tasks/Forms:
    - Service Tasks (Automated Processing):
    - Gateways (Business Decisions):
    - External Service Integration:
    - Exception Handling:
  - Strategic BPMN Traceability:


## Functionality 2 - Payment Processing & Settlement

  - Purpose:
  - Trigger:
  - Actors:
  - Main Process:
  - Exceptions:
  - Proposed BPMN Elements:
    - User Tasks/Forms:
    - Service Tasks (Automated Processing):
    - Gateways (Business Decisions):
    - External Service Integration:
    - Exception Handling:
  - Strategic BPMN Traceability:




## Camunda Feasibility Assessment


### Functionality 1 - Feasibility


### Functionality 2 - Feasibility


### Sprint 2 Implementation Plan


### Sprint 3 Implementation Plan

