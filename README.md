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

### Typical User Workflow

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

### Receiving and Processing Webhooks
Once a bin is created, external services can send webhooks to the generated endpoint URL.

When a webhook is received at the `/endpoint/:binId` route, the backend:

1. Extracts the request data (method, URL, headers, body, query parameters, IP address, timestamp)
2. Stores the complete request data in MongoDB
3. Retrieves the endpoint ID from PostgreSQL
4. Inserts a summary record in the PostgreSQL `requests` table with a reference to the MongoDB document
5. Emits a `newRequest` event via Socket.IO with basic request information
6. The frontend listens for `newRequest` events and updates the UI in real-time when a new request is received

The dual-database approach allows efficient storage and retrieval of both structured metadata and complete request payloads.
