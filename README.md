# Event-Driven Order Processing System

This repository contains a production-simulated order processing system designed to demonstrate advanced backend engineering practices using Go. 

The system's architecture decouples the synchronous ingestion of orders from their asynchronous processing via message brokers. This approach ensures high availability, scalability, and immediate client response times, which are critical requirements for modern distributed systems.

## System Architecture

The application is divided into two main components:
1.  **REST API (Synchronous):** Ingests incoming order requests, persists the initial state to the database, publishes a task to the message broker, and immediately returns an HTTP 202 Accepted response to the client.
2.  **Background Worker (Asynchronous):** Subscribes to the message broker, consumes incoming tasks, processes the business logic (e.g., payment simulation, inventory check), and updates the database state accordingly.

## Technical Stack

*   **Language:** Go (focusing on the standard library `net/http` and concurrency primitives)
*   **Database:** PostgreSQL
*   **Database Interactions:** `sqlc` for generating type-safe Go code from raw SQL queries (avoiding heavy ORMs)
*   **Message Broker:** Redis (using `asynq`) / RabbitMQ
*   **Containerization:** Docker and Docker Compose for seamless local development and deployment

## Key Engineering Practices Implemented

This project is built with a focus on maintainability and robustness, highlighting the following practices:

*   **Clean Architecture & Dependency Injection:** The codebase is structured using interfaces and the repository pattern to decouple the business logic from infrastructure details, making the system highly testable.
*   **Context Management:** Strict use of `context.Context` across all layers to handle timeouts, deadline cancellations, and prevent resource leaks.
*   **Graceful Shutdown:** Interception of OS signals (SIGINT/SIGTERM) to ensure the HTTP server stops accepting new requests while allowing current in-flight requests and background worker tasks to complete safely before termination.
*   **Idiomatic Error Handling:** Explicit error checking and wrapping to maintain clear stack traces and domain-specific error responses.
*   **Testing:** Comprehensive test coverage using Go's standard `testing` package and Table-Driven test patterns.

## Getting Started

### Prerequisites
*   Go 1.22 or higher
*   Docker and Docker Compose
*   Make (optional, for running Makefile commands)

### Running the Application

1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/go-event-driven-orders.git
   cd go-event-driven-orders
   ```

2. Start the infrastructure (Database and Message Broker) and the application via Docker Compose:
   ```bash
   docker-compose up --build
   ```
   
3. The API will be available at http://localhost:8080.

