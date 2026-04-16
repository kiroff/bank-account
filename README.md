# Bank Account Management System (CQRS & Event Sourcing)

This project is a bank account management system implemented using the **CQRS (Command Query Responsibility Segregation)** and **Event Sourcing** architectural patterns. It is built with Spring Boot and leverages Kafka for event distribution, MongoDB as an event store, and PostgreSQL for the read model.

## Architecture

The system is split into two main parts:
- **Command Side**: Handles state-changing operations (Open Account, Deposit, Withdraw, Close Account). It stores events in MongoDB and publishes them to Kafka.
- **Query Side**: Consumes events from Kafka to maintain a read-optimized view of the account data in a PostgreSQL database.

### Key Concepts
- **Event Sourcing**: The state of an account is not stored directly. Instead, all changes are stored as a sequence of events. The current state is reconstructed by replaying these events.
- **CQRS**: Separates read and write operations into different models, allowing them to scale independently.

## Project Structure

The project is organized into several Maven modules:

- `bank-account-command`: The command-side microservice.
- `bank-account-query`: The query-side microservice.
- `bank-account-common`: Shared DTOs, events, and constants.
- `cqrs-core`: Core infrastructure for CQRS and Event Sourcing (located in a sibling directory `../cqrs-core`).

## Technologies Used

- **Java 25**
- **Spring Boot 4.x**
- **Apache Kafka**: Event bus for distributing events.
- **MongoDB**: Event store for the command side.
- **PostgreSQL**: Read database for the query side.
- **Docker & Docker Compose**: For infrastructure setup.

## Prerequisites

- JDK 25 or higher
- Maven 3.9+
- Docker and Docker Compose

## Setup and Running

### 1. Infrastructure

Start the required infrastructure (Kafka, MongoDB, PostgreSQL, Adminer) using Docker Compose:

```bash
cd docker
docker-compose up -d
```

### 2. Building the Project

From the root directory:

```bash
mvn clean install
```

### 3. Running the Services

You can run the command and query services separately:

**Command Service (Port 5000):**
```bash
cd bank-account-command
mvn spring-boot:run
```

**Query Service (Port 5001):**
```bash
cd bank-account-query
mvn spring-boot:run
```

## API Endpoints

### Command API (`bank-account-command`)

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/v1/accounts` | `POST` | Open a new bank account |

### Query API (`bank-account-query`)

*Currently under development.*

## Infrastructure Tools

- **Adminer**: Available at `http://localhost:8080` (for database management).
- **PostgreSQL**: `localhost:5432` (Database: `appdb`, User: `admin`, Password: `q`).
- **MongoDB**: `localhost:27017` (Database: `bank_account`, User: `admin`, Password: `secret123`).
- **Kafka**: `localhost:9092, 9094, 9096`.
