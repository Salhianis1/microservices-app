# Event-Driven Scalable Microservices Ticket System

---

### Architecture Diagrams:

![Ticket System Design](image/ticket-system-design.png)  
![Microservices Order Flow](image/microservices-order.png)  
![RabbitMQ Ticket Messaging](image/rabbitmq-ticket.png)

---

## About the App

This app is a ticketing system built on event-driven microservices architecture. When a user creates an order (buys a ticket), the ticket is reserved for 2 minutes (simulated; can be extended up to 15 minutes). If the payment is not completed within this time, the ticket becomes available again. Upon successful purchase, the ticket is removed from the UI.

---

## Features

- **Authentication Microservice**
- **Ticket Microservice**
- **Order Microservice**
- **Expiration Microservice**
  - Uses Redis and BullJS to expire orders after the time limit
- **Payments Microservice**
  - Integrated with Stripe for payment processing
- **Client Microservice**
  - Built with React

---

## Technologies Used

- Kubernetes, Docker, Ingress
- RabbitMQ Cluster Operator
- Node.js, TypeScript, Express
- MongoDB
- Redis, BullJS (for job queue and expiration handling)
- Stripe (payment gateway)
- Amqplib (RabbitMQ client)
- Common NPM package: `@eftickets/common`
- JSON Web Token (JWT), Cookies (for authentication)
- React with Material UI (MUI), Context API, and Custom Hooks
- Design Patterns, CRUD operations, Validation
- Authentication & Authorization
- Skaffold (for Kubernetes workflow automation)

---

## Configuration Notes

To change the order expiration time limit, update this constant in the expiration microservice:

```typescript
// Default (simulation)
const EXPIRATION_WINDOW_SECONDS = 2 * 60;

// Production recommendation
const EXPIRATION_WINDOW_SECONDS = 15 * 60;
```
