# TicketFlow
TicketFlow is a modern, high-performance, open-source internal IT ticketing system. Built from the ground up with Go (Gin) and GORM, it delivers lightning-fast processing speeds, minimal memory footprint, and native support for complex corporate IT workflows.
Unlike legacy ticketing systems, GoTicket natively supports dynamic Ticket Types, structured Content blocks, and multi-stage Approval Flows required by modern enterprise operations.

------------------------------
## ✨ Core Features

* Dynamic Ticket Types: Define distinct schemas for different request types (e.g., Access Request, Hardware Procurement, Incident Report).
* Structured Content Payload: Leverages JSONB database fields to support varying metadata per ticket type without bloating the relational database schema.
* Multi-Stage Approval Flows: Route tickets to managers, department heads, or security officers for authorization before IT technicians begin their execution workflows.
* Built for Speed: Powered by Go and Gin, capable of handling thousands of ticket inquiries per second with minimal resource consumption.
* Developer-First API: Fully documented, API-first architecture designed to easily hook into your CI/CD pipelines, Git operations, or internal scripts.

------------------------------
## 🏗️ Architecture & Database Design
GoTicket uses GORM for relational mapping. Below is the simplified structure representing how Ticket Types, Custom Content, and Approval Flows interact:
```text
┌─────────────────┐         ┌──────────────┐         ┌─────────────────────┐
│   TicketType    │ ──────> │    Ticket    │ ──────> │    ApprovalFlow     │
│ (Access, Asset) │         │ (JSONB Body) │         │ (Pending/Approved)  │
└─────────────────┘         └──────────────┘         └─────────────────────┘
```

## Core Schema Preview (GORM Models)
```go
type TicketType string
const (
    AccessRequest      TicketType = "ACCESS_REQUEST"
    HardwareAssignment TicketType = "HARDWARE_ASSIGNMENT"
    IncidentReport     TicketType = "INCIDENT_REPORT"
)
type Ticket struct {
    gorm.Model
    Title         string         `gorm:"type:varchar(255);not null" json:"title"`
    Type          TicketType     `gorm:"type:varchar(50);not null" json:"type"`
    Content       string         `gorm:"type:jsonb" json:"content"` // Dynamic fields mapped based on Type
    Status        string         `gorm:"type:varchar(50);default:'OPEN'" json:"status"`
    RequesterID   uint           `json:"requester_id"`
    AssigneeID    *uint          `json:"assignee_id"`
    ApprovalFlows []ApprovalFlow `json:"approval_flows"`
}
type ApprovalFlow struct {
    gorm.Model
    TicketID     uint   `json:"ticket_id"`
    ApproverStep int    `gorm:"not null" json:"approver_step"` // Step 1, Step 2, etc.
    ApproverID   uint   `json:"approver_id"`
    Status       string `gorm:"type:varchar(50);default:'PENDING'" json:"status"` // PENDING, APPROVED, REJECTED
    Comment      string `json:"comment"`
}
```

------------------------------
## 🛠️ Technology Stack

* Backend Framework: Gin Web Framework
* ORM: GORM
* Database: PostgreSQL (Recommended for native JSONB support)
* Containerization: Docker & Docker Compose

------------------------------
## 🚀 Quick Start (60 Seconds Setup)
GoTicket is fully containerized. You can spin up the backend server along with a PostgreSQL database with a single terminal command.
## Prerequisites

* Docker Installed
* Docker Compose Installed

## Running Locally

   1. Clone the repository:
   ```
   git clone https://github.com
   cd goticket
   ```
   
   2. Spin up the stack:
   ```
   docker compose up -d
   ```
   
   3. Verify the server status:
   The backend API will be available at http://localhost:8080. You can test the system health check route using curl:
   ```
   curl http://localhost:8080/api/v1/health
   ```
   
   
------------------------------
## 📡 API Endpoint Reference (Draft)

| Method | Endpoint | Description |
|---|---|---|
| POST | /api/v1/tickets | Create a new ticket with specific Type and Content payloads. |
| GET | /api/v1/tickets/:id | Fetch details of a single ticket including its approval chain history. |
| POST | /api/v1/approvals/:id/action | Update an approval step (APPROVE or REJECT) with optional log notes. |
| PUT | /api/v1/tickets/:id/assign | Assign an open ticket to a designated IT support engineer. |

------------------------------
## 🤝 Contributing
We love open-source contributions! Whether you want to add new ticketing workflows, optimize database indices, or build out frontend UI interfaces, here is how you can jump in:

   1. Fork the Project Repository.
   2. Create your Feature Branch (git checkout -b feature/AmazingFeature).
   3. Commit your Changes (git commit -m 'Add some AmazingFeature').
   4. Push to the Branch (git push origin feature/AmazingFeature).
   5. Open a Pull Request.

Please review our CONTRIBUTING.md file for code styling guidelines, environment standards, and development requirements before creating a PR.
------------------------------
## 📄 License
Distributed under the MIT License. See LICENSE for more detailed information.



## Project Type
[Nuxt.js](https://nuxt.com/) + [Vapor](https://vapor.codes/)
