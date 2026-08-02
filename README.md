# MeetSpace

MeetSpace is a full-stack real-time video conferencing application built with React, Node.js, Express, MongoDB, Socket.IO, and WebRTC. It supports authenticated users, instant meeting creation, shareable meeting codes, meeting activity history, real-time signaling, multi-user rooms, and in-meeting chat.

The project is structured as a MERN-style application with a separate React client and Express API/signaling server.

## Table Of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [Available Scripts](#available-scripts)
- [API Reference](#api-reference)
- [Realtime Events](#realtime-events)
- [Data Models](#data-models)
- [Development Notes](#development-notes)
- [Known Improvements](#known-improvements)

## Features

- User registration and login with bcrypt password hashing
- Token-based session persistence using browser local storage
- Protected routes for authenticated app pages
- Modern auth UI with sign-in/sign-up switching
- Home dashboard with user greeting, meeting stats, recent rooms, and quick actions
- Create instant meetings from the dashboard
- Join meetings by room code
- Copy shareable meeting links
- Meeting history persisted in MongoDB
- Multi-user video rooms powered by WebRTC
- Socket.IO signaling for offer/answer and ICE candidate exchange
- In-meeting chat message relay through Socket.IO
- Responsive frontend layouts for desktop and mobile

## Architecture

```text
Browser Client
  |
  | React Router / Context API
  |
  | REST API: auth, dashboard, history, meeting creation
  v
Express API Server
  |
  | Mongoose
  v
MongoDB

Browser Client A
  |
  | Socket.IO signaling: join-call, signal, chat-message
  v
Socket.IO Server
  |
  | WebRTC offer/answer + ICE exchange
  v
Browser Client B

After signaling completes, media streams travel peer-to-peer through WebRTC.
```

## Tech Stack

**Frontend**

- React 18
- React Router 6
- Material UI
- Axios
- Socket.IO Client
- WebRTC browser APIs
- CSS modules and page-level CSS

**Backend**

- Node.js
- Express
- Socket.IO
- Mongoose
- bcrypt
- http-status

**Database**

- MongoDB

## Project Structure

```text
.
├── backend
│   ├── package.json
│   └── src
│       ├── app.js
│       ├── controllers
│       │   ├── socketManager.js
│       │   └── user.controller.js
│       ├── models
│       │   ├── meeting.model.js
│       │   └── user.model.js
│       └── routes
│           └── users.routes.js
│
└── frontend
    ├── package.json
    ├── public
    └── src
        ├── App.js
        ├── contexts
        │   └── AuthContext.jsx
        ├── pages
        │   ├── authentication.jsx
        │   ├── home.jsx
        │   ├── history.jsx
        │   ├── landing.jsx
        │   └── VideoMeet.jsx
        ├── styles
        └── utils
            └── withAuth.jsx
```

## Getting Started

### Prerequisites

- Node.js 18 or newer recommended
- npm
- MongoDB connection string

### 1. Install Backend Dependencies

```bash
cd backend
npm install
```

### 2. Install Frontend Dependencies

```bash
cd frontend
npm install
```

### 3. Configure The Frontend API Target

For local backend development, set `IS_PROD` to `false` in:

```text
frontend/src/environment.js
```

Expected local API target:

```js
let IS_PROD = false;
```

This points the frontend to:

```text
http://localhost:8000
```

### 4. Start The Backend

```bash
cd backend
npm start
```

Backend runs on:

```text
http://localhost:8000
```

### 5. Start The Frontend

```bash
cd frontend
npm start
```

Frontend runs on:

```text
http://localhost:3000
```

## Environment Configuration

Recommended backend environment variables:

```env
PORT=8000
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/meetspace
```

Current implementation note: `backend/src/app.js` still contains a hardcoded MongoDB connection string. For production-quality configuration, move that value into `process.env.MONGO_URI` before deploying or sharing the project publicly.

Recommended backend connection pattern:

```js
await mongoose.connect(process.env.MONGO_URI);
```

## Available Scripts

### Backend

```bash
npm start
```

Runs the Express and Socket.IO server with Node.

```bash
npm run dev
```

Runs the backend with nodemon.

```bash
npm run prod
```

Runs the backend with pm2.

### Frontend

```bash
npm start
```

Starts the React development server.

```bash
npm run build
```

Creates an optimized production build.

```bash
npm test
```

Runs the Create React App test runner.

## API Reference

Base URL:

```text
/api/v1/users
```

### Register

```http
POST /register
```

Request body:

```json
{
  "name": "Garv Gupta",
  "username": "garv",
  "password": "password123"
}
```

Success response:

```json
{
  "message": "User Registered"
}
```

### Login

```http
POST /login
```

Request body:

```json
{
  "username": "garv",
  "password": "password123"
}
```

Success response:

```json
{
  "token": "generated-session-token"
}
```

### Get Meeting History

```http
GET /get_all_activity?token=generated-session-token
```

Success response:

```json
[
  {
    "_id": "meeting-id",
    "user_id": "garv",
    "meetingCode": "meet-a1b2c3",
    "title": "Team Sync",
    "status": "created",
    "date": "2026-08-02T13:30:00.000Z"
  }
]
```

### Add Joined Meeting To History

```http
POST /add_to_activity
```

Request body:

```json
{
  "token": "generated-session-token",
  "meeting_code": "meet-a1b2c3",
  "title": "Joined Meeting"
}
```

Success response:

```json
{
  "message": "Added code to history"
}
```

### Get Dashboard Summary

```http
GET /dashboard?token=generated-session-token
```

Success response:

```json
{
  "user": {
    "name": "Garv Gupta",
    "username": "garv"
  },
  "stats": {
    "totalMeetings": 12,
    "uniqueRooms": 8,
    "recentMeetings": 6
  },
  "recentMeetings": []
}
```

### Create Instant Meeting

```http
POST /create_meeting
```

Request body:

```json
{
  "token": "generated-session-token",
  "title": "Team Sync"
}
```

Success response:

```json
{
  "message": "Meeting created",
  "meeting": {
    "_id": "meeting-id",
    "user_id": "garv",
    "meetingCode": "meet-a1b2c3",
    "title": "Team Sync",
    "status": "created",
    "date": "2026-08-02T13:30:00.000Z"
  }
}
```

## Realtime Events

Socket.IO is initialized on the same backend HTTP server.

### `join-call`

Client joins a room path.

```js
socket.emit("join-call", window.location.href);
```

Server broadcasts:

```js
socket.emit("user-joined", socketId, roomConnections);
```

### `signal`

Used for WebRTC offer, answer, and ICE candidate exchange.

```js
socket.emit("signal", targetSocketId, message);
```

Server relays:

```js
socket.emit("signal", senderSocketId, message);
```

### `chat-message`

Used for in-room chat relay.

```js
socket.emit("chat-message", message, sender);
```

Server broadcasts:

```js
socket.emit("chat-message", message, sender, senderSocketId);
```

### `disconnect`

Server removes the socket from its active room and broadcasts:

```js
socket.emit("user-left", socketId);
```

## Data Models

### User

```js
{
  name: String,
  username: String,
  password: String,
  token: String
}
```

### Meeting

```js
{
  user_id: String,
  meetingCode: String,
  title: String,
  status: String,
  date: Date
}
```

## Development Notes

- Authentication currently uses a generated token stored on the user document.
- Frontend route protection is handled by `withAuth`.
- Meeting history and dashboard data are persisted in MongoDB.
- Socket room state and chat messages are kept in server memory.
- WebRTC media streams are peer-to-peer after signaling.
- For local end-to-end testing, run both backend and frontend at the same time.

## Known Improvements

- Move MongoDB connection string to `MONGO_URI`.
- Replace custom token storage with JWT or secure server-managed sessions.
- Add request validation for auth and meeting endpoints.
- Add centralized API error handling middleware.
- Persist chat messages if long-term history is required.
- Add TURN server support for production-grade NAT traversal.
- Add automated backend tests for auth, dashboard, and meeting creation.
- Add frontend tests for protected routes and dashboard actions.
- Add Docker Compose for local MongoDB, backend, and frontend orchestration.

## Author

Garv Gupta
