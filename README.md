# SaaSFlow

**A Universal API Gateway & Metered Billing Console (Simulation)**

SaaSFlow is a client-side simulated API gateway that demonstrates how enterprise platforms manage API traffic, enforce rate limits, and automate metered billing, all while strictly maintaining data privacy.

Whether a company is serving live financial tickers, e-commerce data, or AI models, SaaSFlow provides the infrastructure to monetize and protect those endpoints. This project was built by Neel Raj and Divyanshu Baghel for the PST_PD_004 OJT Track.

---

## ⚠️ The Problem: Unmanaged API Infrastructure

APIs are the backbone of the modern internet. However, exposing an API without a protective layer introduces massive risks:

1. **Infrastructure Vulnerability:** A single buggy script from a client can send thousands of requests per second, instantly crashing the provider's backend servers.
2. **Revenue Leakage:** Providers lack built-in tools to precisely meter API usage, making it nearly impossible to confidently charge customers for what they consume.
3. **Access Chaos:** Managing authentication, issuing API keys, and enforcing plan-specific quotas (e.g., Free vs. Pro tiers) becomes a logistical nightmare as the user base grows.

---

## 💡 The SaaSFlow Solution

SaaSFlow acts as a smart, universal "toll booth" sitting between external developers and a company's backend infrastructure. It standardizes the API lifecycle:

- **The Bouncer (Authentication):** Intercepts requests and validates masked API keys before traffic ever touches the core servers.
- **The Speed Limit (Rate Limiting):** Enforces strict, plan-based RPM (Requests Per Minute) limits in real-time. If a client exceeds their quota, SaaSFlow instantly blocks them with a `429 Too Many Requests` error, protecting backend uptime.
- **The Cashier (Metered Billing):** Tracks every byte of data passing through the gateway, automatically aggregating usage to generate transparent, Stripe-style invoices at the end of the month.

---

## ✨ Core Features

- **🔀 Universal API Routing:** A dynamic Playground where consumers can simulate sending requests through the gateway and viewing the simulated response and data consumption.
- **🚦 In-Browser Rate Limiting:** Tracks API keys and enforces subscription limits (e.g., 20 requests/min). Exceeding the limit triggers a dynamic `429` block with a live countdown timer.
- **💳 Automated Billing Engine:** Automatically tracks usage per tenant, calculating base subscription fees plus Rupee (₹) overage charges for the monthly invoice.
- **🛡️ Zero-Inspection Privacy:** The system architecture is designed to log only operational metadata (latency, status codes, data cost) while completely discarding the actual payload strings.
- **👥 Role-Based Access (RBAC):** Three distinct views:
  - **Founder:** High-level KPI dashboard (Total Calls, Blocked 429s, Active MRR).
  - **API Consumer:** Developer playground and virtual key management.
  - **Finance:** Itemized usage tracking and invoice settlement.

---

## 🛠️ Tech Stack (Phase 1: Simulation)

Because this project demonstrates architectural logic without requiring real backend deployment, the entire gateway engine runs directly in the browser:

- **UI/Styling:** HTML5, CSS3, Bootstrap 5.3
- **Core Logic:** Vanilla JavaScript (ES6)
- **Database Simulation:** Browser `localStorage` (JSON seeding)
- **Routing Engine:** JS Promises & `setTimeout` to mimic network latency.

---

## 🆕 Backend Technology Stack (Planned Phase 2)

The planned second phase will extend SaaSFlow beyond its browser-based simulation by introducing a backend server and a persistent database.

### 1. Node.js

Node.js will provide the runtime environment for executing JavaScript on the backend, outside the browser.

**Its role in SaaSFlow:**
- Run the backend server.
- Execute API request-processing and business logic.
- Handle asynchronous operations and communication with external API providers.
- Coordinate database operations and other backend services.

### 2. Express.js

Express.js is a web framework built on Node.js that simplifies the development of web servers, API endpoints, and middleware.

**Its role in SaaSFlow:**
- **API Routing:** Create endpoints for projects, API keys, usage statistics, and billing.
- **Authentication:** Validate API keys and identify the associated customer.
- **Rate Limiting:** Enforce plan-specific request limits and return `429 Too Many Requests` when limits are exceeded.
- **Usage Tracking:** Record operational metadata such as timestamps, response times, and HTTP status codes.
- **Billing:** Calculate subscription fees and usage-based overage charges.
- **Access Control:** Restrict Founder, API Consumer, and Finance operations according to user permissions.
- **Error Handling:** Return appropriate HTTP status codes and error messages.

Express.js will act as the main API layer connecting the frontend, database, and external services.

### 3. MongoDB

MongoDB is a document-oriented NoSQL database that stores data in flexible, JSON-like BSON documents.

Unlike browser `localStorage`, MongoDB will allow SaaSFlow to persist shared data across users, browsers, and devices.

**Its role in SaaSFlow:**
- **Tenant Management:** Store customer accounts and organization details.
- **API Key Management:** Store securely represented API-key credentials, project associations, and key status.
- **Subscription Plans:** Store plan limits, subscription prices, and overage rates.
- **Usage Logs:** Persist request metadata for usage tracking and analytics.
- **Billing Records:** Store invoice details, aggregated usage, calculated charges, and payment status.
- **Dashboard Analytics:** Provide persistent data for total API calls, blocked requests, usage trends, and billing summaries.

Express.js will communicate with MongoDB through a MongoDB driver or an optional library such as Mongoose.

---

## 🔄 Planned Backend Workflow

The backend-enabled version will process API requests through the following workflow:

1. **Receive Request:** The Express.js server receives an incoming API request.
2. **Authenticate:** The backend validates the API key and identifies the associated tenant and project.
3. **Check Rate Limits:** The rate-limiting logic checks the customer's plan and current request usage.
4. **Forward Request:** If the request is authorized and permitted, the gateway forwards it to the configured external API or protected service.
5. **Record Usage:** SaaSFlow records operational metadata, including timestamps, response status, latency, and billable usage.
6. **Store Data:** MongoDB persists the relevant usage records and billing information.
7. **Return Response:** The gateway returns the response to the client, while the dashboard retrieves updated statistics through the backend API.

Requests rejected by authentication or rate limiting will also be handled appropriately, with sensitive information excluded from operational logs.

---

## 📂 Folder Structure

### Existing Frontend Simulation

```text
SaaSFlow/
├── index.html          # Main login/entry point
├── dashboard.html      # The core UI wrapper for all roles
├── css/
│   └── styles.css      # Custom styling overrides for Bootstrap
├── js/
│   ├── gateway.js      # The simulated API routing engine & fake delays
│   ├── rateLimiter.js  # Math logic for fixed-window rate limiting
│   ├── billing.js      # Data-to-Rupee math calculations
│   └── app.js          # UI state management and DOM manipulation
└── data/
    ├── tenants.json    # Seed data for companies (ShopKart, Zomato, etc.)
    └── plans.json      # Subscription tier definitions
```

### Proposed Backend Structure (Phase 2)

```text
backend/
├── server.js                 # Express server entry point
├── routes/
│   ├── authRoutes.js         # Authentication endpoints
│   ├── projectRoutes.js      # Project management endpoints
│   ├── apiKeyRoutes.js       # API key management endpoints
│   ├── usageRoutes.js        # Usage tracking and analytics endpoints
│   └── billingRoutes.js      # Billing and invoice endpoints
├── middleware/
│   ├── authenticate.js       # API key and user authentication
│   ├── authorize.js          # Role-based access control
│   └── rateLimiter.js        # Request rate-limiting logic
├── models/
│   ├── User.js               # User records
│   ├── Project.js            # Project records
│   ├── ApiKey.js             # API key records
│   ├── UsageLog.js           # API usage metadata
│   └── Invoice.js            # Billing and invoice records
├── services/
│   ├── gatewayService.js     # API gateway processing
│   ├── usageService.js       # Usage aggregation
│   └── billingService.js     # Billing calculations
├── config/
│   └── database.js           # MongoDB connection configuration
├── .env                      # Local environment variables
├── .gitignore                # Files excluded from version control
└── package.json              # Backend dependencies and scripts
```

*Note: The backend structure above is proposed for Phase 2. The modules will be implemented as backend development progresses.*

---

## 🔐 Backend Security and Privacy (Planned)

The backend implementation will introduce additional security measures:

- **Secure API Keys:** Generate keys securely, reveal secret values only when appropriate, and avoid storing recoverable plaintext API-key secrets.
- **Server-Side Authentication:** Validate credentials and enforce permissions on the backend instead of trusting frontend state.
- **Environment Variables:** Keep database connection strings, secrets, and other sensitive configuration out of source control.
- **Privacy-Focused Logging:** Exclude request and response payloads, API keys, authorization headers, and other sensitive information from operational logs.
- **Server-Side Rate Limiting:** Enforce usage limits on the backend rather than relying on browser-only counters.
- **Reliable Billing:** Calculate charges from trusted server-side usage records and configured pricing rules.

---

## 💻 How to Run Locally

Since this is a client-side simulation, there are no heavy servers or databases to spin up.

1. **Clone the repository:**

   ```bash
   git clone https://github.com/YourUsername/SaaSFlow.git
   ```

2. **Navigate to the folder:**

   ```bash
   cd SaaSFlow
   ```

3. **Run the app:**

   Simply double-click `index.html` to open it in your web browser, or use a tool like VS Code Live Server for hot reloading.

---

## ⚙️ Running the Planned Backend (Phase 2)

Once the backend has been implemented, the following setup process will apply:

1. **Install Node.js:** Install Node.js and npm on your development machine.
2. **Navigate to the Backend:** Open a terminal in the `backend/` directory.
3. **Install Dependencies:** Run `npm install` after the backend's `package.json` has been configured.
4. **Configure MongoDB:** Set up a local MongoDB instance or a hosted MongoDB database and configure the connection string in your local `.env` file.
5. **Start the Server:** Run the configured npm start script or the backend entry point to launch the Express.js server.
6. **Connect the Frontend:** Configure the frontend to communicate with the backend API instead of relying exclusively on browser `localStorage`.
7. **Test the Features:** Verify authentication, API routing, rate limiting, usage recording, and billing calculations.

*These instructions describe the planned backend setup. The backend must be implemented and configured before these steps can be used successfully.*

---

*Built with 💡 in Bengaluru, India.*
