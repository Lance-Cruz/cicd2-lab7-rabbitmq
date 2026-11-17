# CICD-2 RabbitMQ Lab 7

This repository contains a RabbitMQ lab for the CICD-2 module.

---

## Lab 7 - Using Message Queues with RabbitMQ

This lab uses RabbitMQ, to publish an order from FastAPI, queues it, and process it in a seperate Python worker.

---
## Repository Structure

```text
cicd2-lab7-rabbitmq/
│
├── app/
│   ├── __init__.py
│   ├── main.py
│   └── worker.py
│
├── .gitignore
├── README.md
├── compose.yaml
└── requirements.txt
```
---
## README Questions
### 1. What is the difference between a producer, a queue, and a consumer?

A producer sends a message to RabbitMQ, A queue stores a message, and a consumer receives a message from the queue and processes it.

### 2. When the worker has stopped, why did the Ready increase?

It increases because the producer sent an order, and because there is no worker available it will store it in the queue until one becomes available to process it.

### 3. What changed in RabbitMQ Manangement while the worker was processing a message?

Consumer count increased to 1. When processing, a message would move from Ready to Unacknowledged. When message is acknowledged, the unacknowledged goes to 0. 

### 4. What does HTTP 202 Accepted tell the client, and what does it not guarantee?

202 means an order has been accepted for processing. it does not guarantee that the order has been processed successfully.

### 5. Why is await useful when a FastAPI endpoint communicates wtih RabbitMQ?

await allows FastAPI to wait for RabbitMQ communcation while also allowing the application to handle other requests in the meantime. 

### 6. What changed when you set prefetch_count=2? Did the worker actually process two orders at the same time?

No, it allows RabbitMQ to send up to two unacknowledged message to the worker, but the worker can only process one order at a time.

### 7. With two consumers using prefetch_count=1, what did the Consumers and Unacknowledged counts show?
 
Consumers count increased to 2 because two worker were connected to the same queue. Unacknowledged count could reach 2 because each worker could receive one message each.

### 8. What happened when one worker stopped before acknowledging an order? Why can the same message be delivered twice?

When one worker stopped before acknowledging an order, RabbitMQ could return the unacknowledged message to the queue and deliver it to another worker. The same message can be delivered twice because RabbitMQ did not get a acknowledge from the first worker.

### 9. In your own words, what does an event loop do while an async function is awaiting a network operation?

Event loop lets the program work on other tasks while waiting for something like a network response. Once the response comes back, it goes back to the task that was waiting and continues running it from there.