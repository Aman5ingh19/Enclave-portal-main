# 🛡️ Enclave Portal — Secure Neo-Brutalist Contact Portal

Enclave Portal is a production-ready, highly secure full-stack web application designed for processing contact requests. Built using the **MERN** stack (MongoDB, Express, React, Node.js), this application combines robust security principles (including request validation, rate limiting, and HTTP security headers) with a striking, modern **Neo-Brutalist** visual interface.

---

## 🎨 Design Aesthetic & Styling
The user interface is designed with a **Neo-brutalism** theme, providing a contrast to typical minimal SaaS layouts. Key aesthetic features include:
*   **Typography**: Powered by the **Space Grotesk** font from Google Fonts.
*   **Color Palette**: Balanced primary colors with black accents (`#111111`) for a card-based comic feel:
    *   `--background`: `#fff8ef` (cream canvas)
    *   `--yellow` (`#ffd43b`) & `--blue` (`#4f7cff`) for navigation and primary action prompts
    *   `--green` (`#57cc99`) & `--red` (`#ff5d5d`) for validation states and errors
*   **Containers & Interactions**: Heavy solid borders (`border: 4px solid #111`), card offsets, flat thick box-shadows (`box-shadow: 8px 8px 0 #111`), custom retro form controls, and micro-interactions for input focus transitions.

---

## 🌟 Key Features

### 💻 Client (Frontend)
*   **Public Contact Form**: Collects `name`, `email`, `subject`, and `message`.
*   **Real-time & Server Validation**: Input fields are validated in real-time. Validation errors from the backend are parsed and attached to their corresponding form fields.
*   **Interactive Admin Dashboard**: A clean dashboard displaying all submitted messages in a table format with options to delete entry logs.
*   **Responsive Layout**: Custom query breakpoints optimized for Mobile (up to `480px` & `768px`), Tablet (`1024px`), and Desktop (`1440px`) viewing.

### 🛡️ Server (Backend)
*   **API Security**: Configured with `helmet()` to implement standard HTTP security headers and block cross-site request attacks.
*   **Rate Limiting**: Protects the contact submission route using `express-rate-limit` to prevent denial-of-service (DoS) and message spam.
*   **Payload Validation**: Strictly enforces constraints on incoming JSON payloads using **Zod** schema validation.
*   **Auditing & Logging**: Implements **Winston** and **Morgan** logger to capture traffic stream and persist files locally under `src/logs/`.

---

## 🛠️ Tech Stack

| Layer | Component | Technologies |
|---|---|---|
| **Frontend** | Build Tool & Library | React 19.x, Vite 8.x |
| | Routing | React Router DOM 7.x |
| | HTTP Client | Axios |
| **Backend** | Runtime/Framework | Node.js, Express 5.x |
| | Object Modeling | Mongoose ODM |
| | Schema Validation | Zod |
| **Security** | Headers & Origin rules | Helmet, CORS |
| | Traffic Throttling | Express Rate Limit |
| **Logs** | File & Console | Winston, Morgan |

---

## 📂 Codebase Directory Structure

```text
Enclave-portal-main/
├── client/                     # React Single Page Application (SPA)
│   ├── public/                 # Static assets
│   ├── src/
│   │   ├── assets/             # Images, SVGs
│   │   ├── components/         # Reusable UI Blocks (ContactForm, ContactTable)
│   │   ├── pages/              # Main Route components (Admin dashboard view)
│   │   ├── services/           # Axios HTTP client requests mapping
│   │   ├── App.css             # Main stylesheet modifications
│   │   ├── App.jsx             # Layout structure & routing mappings
│   │   ├── index.css           # Neo-brutalist variables & CSS design system
│   │   └── main.jsx            # React root application bootstrap
│   ├── .env                    # Client environment secrets
│   ├── package.json            # Node dependencies and scripts
│   └── vite.config.js          # Vite build details
│
└── server/                     # Express REST API Backend
    ├── src/
    │   ├── config/             # MongoDB connection configuration (db.js)
    │   ├── controllers/        # Request handlers (contact controller)
    │   ├── logs/               # Generated log files (combined, error, access logs)
    │   ├── middlewares/        # Express handlers (error handler, rate limiter, validators)
    │   ├── models/             # Mongoose schemas (Contact mongodb schema)
    │   ├── routes/             # Express routes mappings (contacts & admin)
    │   ├── schemas/            # Zod validation models (message validation schema)
    │   ├── utils/              # Winston logging initialization
    │   ├── app.js              # Express app middleware configuration
    │   └── server.js           # Server initializer & port launcher
    ├── .env                    # System secret configuration
    ├── nodemon.json            # Nodemon hot reload settings
    └── package.json            # Node backend packages
```

---

## ⚙️ Environment Variables Setup

Configure the `.env` settings for both applications to boot the project successfully.

### 🔴 Server Configuration (`server/.env`)
Create a `.env` file in the `server` directory:

```env
PORT=8888
NODE_ENV=development
MONGO_URI=your_mongodb_connection_string
client_URL=http://localhost:5173
RATE_LIMIT_WINDOW_MS=60000
RATE_LIMIT_MAX_REQUESTS=5
```

*   `PORT`: Port the node server binds to (default: `8888`).
*   `MONGO_URI`: The connection URI for your MongoDB cluster instance.
*   `client_URL`: The origin URI of your frontend for CORS setup (default: `http://localhost:5173`).
*   `RATE_LIMIT_WINDOW_MS`: Time duration window for request counts (in MS).
*   `RATE_LIMIT_MAX_REQUESTS`: Max submission attempts allowed per IP within the window interval.

### 🔵 Client Configuration (`client/.env`)
Create a `.env` file in the `client` directory:

```env
VITE_API_BASE_URL=http://localhost:8888/api
```

*   `VITE_API_BASE_URL`: Endpoint prefix linking your front-end actions to the Express instance backend.

---

## 🚀 Installation & Running the Project

Follow these steps to run the application locally:

### 1. Prerequisites
Make sure you have node and mongodb:
*   [Node.js](https://nodejs.org/en) (v18.x or above recommended)
*   [MongoDB](https://www.mongodb.com/) (Local server or MongoDB Atlas Cluster)

### 2. Set Up the Server
Navigate into the server folder, install the packages, and launch development mode:
```bash
cd server
npm install
npm run dev
```
The server will bind and start running on: `http://localhost:8888`

### 3. Set Up the Client
Open another terminal instance, navigate to the client folder, install packages, and boot Vite client dev-server:
```bash
cd client
npm install
npm run dev
```
The application will launch on: `http://localhost:5173`

---

## 🔌 API Reference Document

### Health Status Check
Check connection availability:
*   **URL**: `/api/health`
*   **Method**: `GET`
*   **Response**:
    ```json
    {
      "success": true,
      "message": "Server is running successfully."
    }
    ```

---

### Message Submission (Public Form)
Submit feedback or query message:
*   **URL**: `/api/contact`
*   **Method**: `POST`
*   **Rate-Limited**: Yes (Max 5 checks per minute by default template configuration)
*   **Body Parameters (JSON)**:
    *   `name` (string, min 3 chars, max 50)
    *   `email` (string, valid email layout)
    *   `subject` (string, min 5 chars, max 100)
    *   `message` (string, min 20 chars, max 500)
*   **Response (201 Success)**:
    ```json
    {
      "success": true,
      "message": "Contact message submitted successfully.",
      "data": {
        "_id": "6472df...",
        "name": "Jane Doe",
        "email": "jane@example.com",
        "subject": "Inquiry regarding services",
        "message": "This is a detailed inquiry containing at least twenty characters...",
        "createdAt": "2026-07-04T15:10:00.000Z",
        "updatedAt": "2026-07-04T15:10:00.000Z"
      }
    }
    ```
*   **Response (400 Validation Failure)**:
    ```json
    {
      "success": false,
      "message": "Validation failed.",
      "errors": [
        {
          "field": "message",
          "message": "Message must contain at least 20 characters."
        }
      ]
    }
    ```

---

### Retrieve Submissions list (Admin Panel)
Retrieve submitted messages chronologically (newest first):
*   **URL**: `/api/admin/contacts`
*   **Method**: `GET`
*   **Response (200 Success)**:
    ```json
    {
      "success": true,
      "count": 1,
      "data": [
        {
          "_id": "6472df...",
          "name": "Jane Doe",
          "email": "jane@example.com",
          "subject": "Inquiry regarding services",
          "message": "This is a detailed inquiry containing at least twenty characters...",
          "createdAt": "2026-07-04T15:10:00.000Z"
        }
      ]
    }
    ```

---

### Delete Submission (Admin Action)
Delete a submission from the database:
*   **URL**: `/api/admin/contacts/:id`
*   **Method**: `DELETE`
*   **Response (200 Success)**:
    ```json
    {
      "success": true,
      "message": "Contact deleted successfully."
    }
    ```

---

## 🪵 Log Management System
The backend utilizes [Winston](https://github.com/winstonjs/winston) to maintain logs across different log files located in `server/src/logs/`:
1.  **`access.log`**: Standard request trails redirected from the Express Morgan HTTP pipeline context.
2.  **`error.log`**: Errors trapped within catch handlers or uncaught app faults.
3.  **`combined.log`**: Compiled tracking combining all logger streams (HTTP logs, Info indicators, Warning items, and Error stacks).
