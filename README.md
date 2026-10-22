SaaSFlow

A Universal API Gateway & Metered Billing Console

SaaSFlow is a simulated API gateway and metered billing platform designed to demonstrate how enterprise systems manage API traffic, enforce rate limits, and automate usage-based billing while prioritizing data privacy.

Whether a company provides financial market data, e-commerce services, or AI model APIs, SaaSFlow demonstrates how an intermediary gateway can help protect backend infrastructure, monitor consumption, and monetize API access.

This project was built by Neel Raj and Divyanshu Baghel for the PST_PD_004 OJT Track.

⚠️ The Problem: Unmanaged API Infrastructure

APIs are the backbone of the modern internet. However, exposing an API without a protective layer introduces several risks:

Infrastructure Vulnerability: A buggy script or malicious client can send excessive requests, potentially overwhelming backend servers and degrading service availability.

Revenue Leakage: Providers need reliable usage metering to calculate consumption and charge customers accurately.

Access Chaos: Managing authentication, API keys, and plan-specific quotas, such as Free and Pro tiers, becomes increasingly difficult as a customer base grows.

💡 The SaaSFlow Solution

SaaSFlow acts as a smart, universal API gateway, functioning like a digital toll booth between external developers and protected backend services. It standardizes API access, usage tracking, and billing through the following components:

The Bouncer (Authentication): Validates API keys and verifies that requests belong to authorized customers before allowing access to protected resources.

The Speed Limit (Rate Limiting): Enforces plan-based Requests Per Minute (RPM) limits. When a client exceeds the permitted rate, the gateway can return a 429 Too Many Requests response, protecting backend availability.

The Cashier (Metered Billing): Records API usage, aggregates consumption by customer, and calculates subscription fees and overage charges for transparent monthly invoices.

The Privacy Guard (Metadata-Only Logging): Records operational information such as timestamps, latency, status codes, and usage costs without retaining request or response payload contents.

✨ Core Features

🔀 Universal API Routing: A developer playground that simulates sending requests through the gateway and displays the response, latency, status code, and estimated data consumption.

🚦 Rate Limiting: Tracks API usage by key and enforces subscription-specific limits, such as 20 requests per minute. Exceeding a limit triggers a 429 response and a countdown until requests are permitted again.

💳 Metered Billing Engine: Aggregates usage by customer and calculates invoices using base subscription fees and configurable rupee-denominated overage rates.

🛡️ Privacy-Focused Logging: Stores operational metadata rather than actual payload strings, reducing the amount of potentially sensitive information retained.

👥 Role-Based Access Control (RBAC): Provides three distinct interfaces:

Founder: High-level KPIs, including total API calls, blocked requests, active Monthly Recurring Revenue (MRR), and system usage.

API Consumer: Developer playground, project management, virtual API key management, and usage analytics.

Finance: Itemized usage reports, invoice calculations, payment status, and billing history.

🛠️ Technology Stack and Architecture

SaaSFlow is planned in two development phases: a browser-based simulation and a backend-enabled implementation.

Phase 1: Frontend Simulation

The initial version demonstrates the core gateway and billing concepts directly in the browser without requiring a deployed backend.

HTML5: Defines the application structure, including login screens, dashboards, tables, and forms.

CSS3: Provides custom styling, layouts, and visual presentation.

Bootstrap 5.3: Supplies responsive UI components and a consistent design system.

Vanilla JavaScript (ES6): Implements frontend interactions, simulated routing, rate-limit calculations, billing logic, and dashboard updates.

Browser localStorage: Persists simulated application state and sample records between browser sessions.

JSON Seed Data: Supplies initial demonstration tenants, subscription plans, and other sample records.

JavaScript Promises and setTimeout: Mimic network delays and asynchronous API responses for demonstration purposes.

Phase 2: Backend and Database Integration

The next phase introduces a real backend using Node.js, Express.js, and MongoDB. This transforms SaaSFlow from a browser-only simulation into a server-backed application capable of managing persistent records and processing API requests.

1. Node.js: Backend Runtime

Node.js provides the runtime environment for executing JavaScript outside the browser. It allows SaaSFlow to run its backend code, communicate with the database, and handle incoming network requests.

Functions in SaaSFlow:

Runs the Express.js server.

Executes authentication, request-processing, rate-limiting, and billing logic.

Coordinates database operations and communication with external API providers.

Handles asynchronous operations such as network calls and database queries.

2. Express.js: Backend Framework and API Layer

Express.js is a web framework built on Node.js. It simplifies the creation of HTTP servers, API endpoints, and middleware used to process incoming requests.

Functions in SaaSFlow:

API Routing: Defines endpoints such as GET /api/usage, POST /api/projects, and POST /api/keys.

Authentication Middleware: Validates API credentials and identifies the associated customer or project.

Rate Limiting: Checks request counts against configured subscription limits and rejects excess traffic with HTTP 429.

Usage Metering: Records request metadata, including timestamps, response status, latency, and billable usage.

Billing Calculations: Aggregates usage records and calculates subscription fees and overage charges.

Access Control: Enforces permissions so Founder, API Consumer, and Finance users can access only the features and data they are authorized to use.

Error Handling: Returns consistent HTTP status codes and error responses when requests fail.

Express.js will act as the primary communication layer between the frontend, MongoDB, and any protected API services connected through the gateway.

3. MongoDB: Persistent Database

MongoDB is a document-oriented NoSQL database that stores application data in flexible, JSON-like BSON documents. Unlike browser localStorage, MongoDB allows the backend to maintain shared, persistent records across users, browsers, and devices.

Functions in SaaSFlow:

Tenant Management: Stores customer accounts, organization details, and account settings.

API Key Management: Stores securely represented API-key credentials, their associated projects, status, and permissions.

Subscription Plans: Stores plan configurations, request limits, subscription prices, and overage rates.

Usage Logs: Stores operational metadata for API requests without retaining actual request or response payloads.

Billing Records: Stores aggregated usage, invoice line items, calculated charges, and payment status.

Analytics: Supplies data for dashboard KPIs, usage trends, blocked-request counts, and customer-level reports.

MongoDB will serve as the central source of truth for persistent application data. Express.js will access it through a MongoDB driver or an optional object modeling library such as Mongoose.

4. How the Backend Components Work Together

When a customer sends an API request through SaaSFlow, the planned backend workflow is:

The request reaches the Express.js server running on Node.js.

Authentication middleware validates the customer's API key and identifies the associated tenant and project.

The rate-limiting layer checks the applicable plan limits and recent request counts.

If the request is allowed, the gateway forwards it to the configured upstream API, where applicable.

The gateway receives the response and records operational metadata, including status code, latency, and billable usage.

MongoDB stores the relevant usage records and updates the data used for analytics and billing.

The response is returned to the customer, and the dashboard can retrieve updated statistics from the backend.

Requests rejected during authentication or rate limiting should also be logged appropriately, without exposing credentials or sensitive payloads.

5. Proposed Backend Architecture

SaaSFlow/
├── frontend/
│   ├── index.html
│   ├── dashboard.html
│   ├── css/
│   │   └── styles.css
│   └── js/
│       └── app.js
│
└── backend/
    ├── server.js
    ├── routes/
    │   ├── authRoutes.js
    │   ├── projectRoutes.js
    │   ├── apiKeyRoutes.js
    │   ├── usageRoutes.js
    │   └── billingRoutes.js
    ├── middleware/
    │   ├── authenticate.js
    │   ├── authorize.js
    │   └── rateLimiter.js
    ├── models/
    │   ├── User.js
    │   ├── Project.js
    │   ├── ApiKey.js
    │   ├── UsageLog.js
    │   └── Invoice.js
    ├── services/
    │   ├── gatewayService.js
    │   ├── usageService.js
    │   └── billingService.js
    ├── config/
    │   └── database.js
    └── .env

This is a proposed structure for the backend-enabled version. The files and modules will be introduced as the project develops.

🔐 Security and Privacy Considerations

The backend implementation will incorporate security measures appropriate for an API gateway:

API keys will be generated securely, displayed carefully, and stored using a design that avoids retaining recoverable plaintext secrets.

Authentication and authorization checks will run on the server rather than relying on frontend role selections.

Database credentials, signing secrets, and other sensitive configuration will be kept in environment variables rather than committed to source control.

Request and response payloads will not be retained by the usage-logging system. Credentials, authorization headers, and sensitive query parameters must also be excluded from logs.

Rate limiting will be enforced server-side using a shared or distributed store when required for multi-instance deployments.

Billing calculations will use trusted server-side usage records and consistent pricing rules.

External API calls will use configured destinations and appropriate safeguards against unauthorized forwarding and server-side request forgery.

💻 How to Run Locally

Phase 1: Frontend Simulation

Clone the repository:

git clone https://github.com/YourUsername/SaaSFlow.git

Navigate to the project directory:

cd SaaSFlow

Open index.html in a browser or use VS Code Live Server.

The simulated version does not require Node.js, Express.js, or MongoDB to run.

Phase 2: Backend-Enabled Version

After the backend has been implemented:

Install Node.js and npm.

Navigate to the backend directory.

Install the project's dependencies using npm install.

Configure the MongoDB connection string and other secrets in a local .env file.

Start the Express.js server using the configured npm script or Node.js entry point.

Configure the frontend to communicate with the backend API.

Test authentication, rate limiting, usage recording, and billing calculations.

MongoDB must be available locally or through a configured hosted database. The exact commands will depend on the final implementation.

🚀 Project Vision

SaaSFlow aims to demonstrate how an API gateway can combine traffic control, customer access management, usage analytics, and metered billing in one platform.

The first phase validates the core business logic through a browser simulation. The planned second phase introduces a Node.js and Express.js backend with MongoDB persistence, enabling more realistic request processing, centralized usage tracking, and a foundation for future deployment.

Built with 💡 in Bengaluru, India.

Project contributors: Neel Raj and Divyanshu Baghel | PST_PD_004 OJT Track
