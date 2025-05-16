<img src="https://github.com/user-attachments/assets/930b8614-7c71-4288-a458-c853c5af6281" width="256" alt="Dump Truck">

# Dumpi

Dumpi is a TypeScript/React/Express webhook inspection tool designed to help developers debug webhook interactions. It provides a simple yet powerful interface for creating webhook endpoints, capturing incoming webhook requests, and inspecting their contents in real-time with WebSockets.

### Tech Stack

| Component      | Technologies                          |
|:--------------|:--------------------------------------|
| Frontend      | React, TypeScript, Socket.IO Client, Tailwind CSS |
| Backend       | Express.js, Node.js, Socket.IO Server |
| Databases     | PostgreSQL (structured data), MongoDB (document storage) |
| Communication | RESTful APIs, WebSockets (Socket.IO) |

# What Are Webhooks?

Webhooks are automated messages sent from one application to another when a specific event occurs. They enable real-time data flow between systems without constant polling (repeatedly checking for updates).

### How Webhooks Work

1. **Event Occurs** (e.g., a payment is processed, a new user signs up).
2. **Source App Sends Data** to a predefined URL (your "webhook endpoint").
3. **Destination App Receives & Acts** on the data (e.g., updates a database, triggers a notification).

### Webhooks vs. APIs

| Feature       | Webhooks | APIs (REST/GraphQL) |
|--------------|----------|---------------------|
| **Initiation** | Server-triggered (pushes data) | Client-triggered (pulls data) |
| **Timing**    | Real-time (event-driven) | On-demand (manual requests) |
| **Efficiency** | No wasted polling | Requires frequent polling for updates |

### Common Use Cases

- **Payment Gateways** (e.g., Stripe sends payment success/failure alerts)
- **CI/CD Pipelines** (e.g., GitHub triggers a build on code push)
- **Chat Apps** (e.g., Slack gets notified when a Trello task is updated)
- **User Authentication** (e.g., Auth0 sends user login events)

# What Might a Typical Dumpi User Workflow Look Like?

1. Create a Webhook Bin
   - User requests a new bin via the frontend
   - System creates a unique endpoint in PostgreSQL
   - System returns the endpoint URL to the user

2. Receive Webhooks
   - External systems send requests to the endpoint
   - Backend captures all request data (headers, body, query parameters)
   - Complete request is stored in MongoDB
   - Request summary is stored in PostgreSQL
   - Real-time notification is sent via Socket.IO

3. Inspect Webhook Details
   - User selects a request from the history
   - Frontend fetches complete details from MongoDB
   - Details are displayed in a structured, readable format

# How Does Dumpi Receive and Process Webhooks?
Once a bin is created, external services can send webhooks to the generated endpoint URL.

When a webhook is received at the `/endpoint/:binId` route, the backend:

1. Extracts the request data (method, URL, headers, body, query parameters, IP address, timestamp)
2. Stores the complete request data in MongoDB
3. Retrieves the endpoint ID from PostgreSQL
4. Inserts a summary record in the PostgreSQL `requests` table with a reference to the MongoDB document
5. Emits a `newRequest` event via Socket.IO with basic request information
6. The frontend listens for `newRequest` events and updates the UI in real-time when a new request is received

The dual-database approach allows efficient storage and retrieval of both structured metadata and complete request payloads.
