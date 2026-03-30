# SmartLink — Your Bus, Your Way!

<img width="4096" height="2140" alt="Gemini_Generated_Image_tc5juctc5juctc5j" src="https://github.com/user-attachments/assets/d6d3b412-6464-4511-a19a-92c70a4af814" />

# SmartLink — Real-Time Bus Tracking System

SmartLink is a full-stack, real-time bus tracking platform designed to make 
commuting effortless. Passengers can track buses live, while admins and 
operators manage fleets, routes, and users from a unified dashboard.

---

## Features

### For Passengers
- **Live Bus Tracking** — Watch buses move in real-time on an interactive map
- **Instant Notifications** — Get alerts on bus location, route changes, and arrival times
- **Mobile-Friendly** — Fully responsive interface for commuting on the go

### For Admins & Operators
- **Fleet Management** — Add buses, update routes, and monitor operations
- **Role-Based Access** — Assign Admin, Operator, or Passenger roles
- **Real-Time Dashboard** — Track bus availability, status, and performance

### Technical Highlights
- **Smart ETA Predictions** — AI-powered arrival time estimates using historical data
- **Real-Time Location Streaming** — Low-latency updates via Redis in-memory caching
- **Interactive Maps** — Built with Leaflet.js for smooth, live bus visualization
- **Secure Authentication** — Firebase-powered login with role-based access control
- **RESTful API** — Clean, well-structured endpoints for all bus, route, and user data
- **Scalable Architecture** — Modular design built to handle high traffic

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React.js | Dynamic UI with Hooks, Context API, and Zustand |
| Tailwind CSS / Shadcn UI | Responsive styling and reusable components |
| Leaflet.js | Interactive real-time maps |
| Google Maps API | Route visualization and location services |
| OSMR | OpenStreetMap routing for optimal paths |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express.js | Server-side logic and API handling |
| MongoDB Atlas | Cloud database for buses, routes, and users |
| Redis | In-memory caching for real-time location data |

### Auth & Security
| Technology | Purpose |
|---|---|
| Firebase Authentication | Secure sign-in for all user roles |
| JWT | Token-based session management |
| Role-Based Access Control | Admin, Operator, and Passenger permissions |

---

## Architecture Overview
```mermaid
flowchart LR
    A[Bus GPS Device] -->|Sends Location Data| B[Backend API - Node.js/Express]
    B --> C[Redis Cache - Real-time Data]
    B --> D[Database - MongoDB Atlas]
    C --> E[Frontend - React UI]
    D --> E
    E -->|Interactive Map| F[User / Admin]
```

- GPS data is ingested by the backend API
- Redis caches live bus locations for instant retrieval
- MongoDB stores persistent data — buses, routes, and users
- Frontend fetches live data via REST APIs and updates maps in real time

---

## Installation & Setup

### Prerequisites
Make sure the following are installed and configured before proceeding:
- Node.js (v18+)
- MongoDB Atlas account
- Redis (local or cloud)
- Firebase project

---

### 1. Clone the Repository
```bash
git clone https://github.com/RahulSingh044/Smart-Link.git
cd Smart-Link
```

---

### 2. Backend Setup
```bash
cd backend
npm install
```

Create a `.env` file inside the `backend` folder:
```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
REDIS_URL=redis://localhost:6379
JWT_SECRET=your_secret_key
FIREBASE_SERVICE_ACCOUNT='{
  "type": "service_account",
  "project_id": "your_project_id",
  "private_key_id": "your_private_key_id",
  "private_key": "-----BEGIN PRIVATE KEY-----\nYOUR_KEY\n-----END PRIVATE KEY-----\n",
  "client_email": "your_client_email",
  "client_id": "your_client_id",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token"
}'
```

Start the backend server:
```bash
npm start
```

The server runs on `http://localhost:5000`.

---

### 3. Frontend Setup
```bash
cd ../frontend
npm install
```

Create a `.env` file inside the `frontend` folder:
```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_AUTH_DOMAIN=your_firebase_auth_domain
NEXT_PUBLIC_PROJECT_ID=your_firebase_project_id
NEXT_PUBLIC_STORAGE_BUCKET=your_firebase_storage_bucket
NEXT_PUBLIC_MESSAGING_SENDER_ID=your_firebase_messaging_sender_id
NEXT_PUBLIC_APP_ID=your_firebase_app_id
```

Start the development server:
```bash
npm run dev
```

The frontend runs on `http://localhost:3000`.

---

### 4. Open the App

Navigate to `http://localhost:3000` in your browser.

> **Note:** Ensure MongoDB Atlas, Redis, and Firebase are correctly configured 
> before starting. Misconfigured services will cause authentication or 
> data-fetching failures.

---

## API Reference

### Authentication

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `POST` | `/api/auth/register` | Register a new user account | None |
| `POST` | `/api/auth/login` | Log in and receive a JWT token | None |
| `POST` | `/admin/set-admin` | Grant admin privileges to a user | Admin |

---

### Bus Management

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `GET` | `/api/buses` | List all buses with live location and status | User |
| `GET` | `/api/buses/:id` | Get details of a specific bus | User |
| `POST` | `/api/buses` | Add a new bus | Admin |
| `POST` | `/api/buses/bulk` | Add multiple buses in one request | Admin |
| `PATCH` | `/api/buses/:id` | Update bus details | Admin |
| `PUT` | `/api/buses/:id/change-route` | Assign a new route to a bus | Admin |
| `PUT` | `/api/buses/:id/change-status` | Update bus operational status | Admin |
| `PUT` | `/api/buses/:id/change-driver` | Assign a driver to a bus | Admin |

---

### Route Management

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `GET` | `/api/routes` | List all routes | User |
| `GET` | `/api/routes/:code` | Get details of a specific route | User |
| `GET` | `/api/routes/:code/assigned-buses` | List buses on a route | User |
| `POST` | `/api/routes` | Create a new route | Admin |
| `POST` | `/api/routes/bulk` | Add multiple routes at once | Admin |
| `PUT` | `/api/routes/:code/assign-bus` | Assign buses to a route | Admin |
| `PUT` | `/api/routes/:code/unassign-bus` | Remove buses from a route | Admin |
| `POST` | `/api/routes/update-connectivitys` | Update route connectivity | Admin |

---

### Station & Stop Management

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `GET` | `/api/stations` | List all stations | User |
| `POST` | `/api/stations` | Add a new station | Admin |
| `POST` | `/api/stations/bulk` | Add multiple stations | Admin |
| `GET` | `/api/stops` | List all bus stops | User |
| `POST` | `/api/stops` | Add a new stop | Admin |
| `POST` | `/api/stops/bulk` | Add multiple stops | Admin |

---

### Driver Management

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `GET` | `/api/drivers` | List all drivers | Admin |
| `GET` | `/api/drivers/:id` | Get details of a specific driver | Admin |
| `GET` | `/api/drivers/status/:status` | Filter drivers by status | Admin |
| `POST` | `/api/drivers` | Add a new driver | Admin |

---

### Admin Dashboard

| Method | Endpoint | Description | Auth |
|:---:|:---|:---|:---:|
| `GET` | `/admin/dashboard` | Fetch analytics and key metrics | Admin |

---

## Contributing

Contributions are welcome. To get started:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the **MIT License**.

---

Built with hard work and collaboration by the SmartLink team.
