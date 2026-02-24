# 🏦 AuraPay - Global Payments Platform

**AuraPay** is a high-performance, bank-grade fintech infrastructure designed for global payments, fraud detection, and immutable ledger recording.

---

## 🚀 Key Features

### 1. 🛡️ Bank-Grade CoreLedger
- **Double-Entry Accounting**: Every transaction has equal debit and credit entries.
- **Immutable Records**: Ledger entries are append-only; no history is ever overwritten.
- **ACID Transactions**: Financial data integrity is guaranteed via atomic database transactions.
- **Reconciliation**: Nightly jobs verify wallet balances against the ledger journal.

### 2. 👁️ Sentinel Fraud Scoring
- **Real-Time Analysis**: Risk scoring happens *before* money moves.
- **Weighted Risk Factors**:
    - Transaction Amount
    - Velocity
    - Geo-Location
    - Device Fingerprinting
- **Automated Decisions**: `AUTO_APPROVE`, `STEP_UP`, `MANUAL_REVIEW`, or `AUTO_BLOCK`.

### 3. 🕸️ Microservices Architecture
- **API Gateway**: Central entry point.
- **Auth Service**: OAuth2/JWT security.
- **Transaction Service**: Orchestrates payments.
- **Ledger Service**: The source of truth for funds.
- **Fraud Service**: Risk calculation engine.
- **Wallet Service**: User balance management.

---

## 🛠️ Technology Stack

- **Java 17** & **Spring Boot 3.2**
- **PostgreSQL** (Transactional Data)
- **Apache Kafka** (Event Streaming)
- **Redis** (Caching)
- **Docker & Kubernetes** (Containerization)
- **Spring Cloud Gateway** & **Eureka** (Service Discovery)

---

## ⚙️ Setup & Installation

### Prerequisites
- JDK 17+
- Maven 3.8+
- Docker & Docker Compose

### 1. Start Infrastructure
Run the core infrastructure services (Database, Kafka, Redis):
```bash
docker-compose up -d
```

### 2. Build Services
Build the entire project:
```bash
mvn clean install -DskipTests
```

### 3. Run Microservices
Start them in this order for best results:
1.  `aurapay-gateway`
2.  `aurapay-auth-service`
3.  `aurapay-ledger-service`
4.  `aurapay-fraud-service`
5.  `aurapay-transaction-service`

---

## 🔌 API Usage

### Initiate Transaction
**POST** `http://localhost:8080/transactions`

```json
{
  "senderWalletId": 1001,
  "receiverWalletId": 2005,
  "amount": 500.00,
  "currency": "USD",
  "idempotencyKey": "uuid-1234-5678"
}
```

### Response
```json
{
  "transactionId": "tx_987654321",
  "status": "COMPLETED",
  "createdAt": "2023-10-27T10:00:00"
}
```

---

## 📂 Project Structure

| Service | Description | Port |
| :--- | :--- | :--- |
| **aurapay-gateway** | API Gateway & Routing | `8080`* |
| **aurapay-auth-service** | Authentication & JWT | `8081` |
| **aurapay-user-service** | User Profile Management | `8082` |
| **aurapay-wallet-service** | Wallet & Balance Logic | `8083` |
| **aurapay-transaction-service** | Transaction Orchestrator | `8084` |
| **aurapay-ledger-service** | Immutable Core Ledger | `8085` |
| **aurapay-fraud-service** | Risk Scoring Engine | `8086` |

*> Note: `docker-compose.yml` may expose Kafka UI on 8080. Ensure no port conflicts or adjust Gateway port.*

---

## 🛡️ Security & Compliance
- **Audit Trails**: All financial actions are logged in the `journal_entries` table.
- **Validation**: Strict input validation on all APIs.
- **Resilience**: Circuit breakers and fallbacks for external dependencies.
