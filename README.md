<p align="center">
  <img src="https://img.icons8.com/external-flatart-icons-outline-flatarticons/128/external-video-conference-work-from-home-flatart-icons-outline-flatarticons.png" width="120"/>
</p>

<h1 align="center">MeetSpace</h1>

<p align="center">
Real-Time Video Conferencing Platform built with WebRTC, Socket.IO and the MERN Stack
</p>

<p align="center">

![React](https://img.shields.io/badge/Frontend-React-blue)
![Node](https://img.shields.io/badge/Backend-Node.js-green)
![Express](https://img.shields.io/badge/Framework-Express-black)
![MongoDB](https://img.shields.io/badge/Database-MongoDB-brightgreen)
![WebRTC](https://img.shields.io/badge/Realtime-WebRTC-orange)
![Socket.IO](https://img.shields.io/badge/Realtime-Socket.IO-black)

</p>

---

# 🚀 Overview

MeetSpace is a **full-stack real-time video conferencing web application** that allows users to create and join meeting rooms using shareable links.

The platform uses **WebRTC for peer-to-peer audio/video streaming** and **Socket.IO for real-time signaling**, enabling seamless communication between participants.

This project demonstrates how modern web technologies can be used to build scalable collaboration tools similar to professional video conferencing platforms.

---

# ✨ Features

- Real-time video and audio communication  
- Join meetings using shareable meeting URLs  
- Multi-user meeting rooms  
- Real-time signaling with Socket.IO  
- Peer-to-peer media streaming using WebRTC  
- Meeting history tracking  
- User authentication system  

---

# 🛠 Tech Stack

### Frontend
- React.js  
- React Router  
- Context API  
- HTML5  
- CSS3  

### Backend
- Node.js  
- Express.js  
- Socket.IO  

### Database
- MongoDB  
- Mongoose  

### Real-Time Communication
- WebRTC  
- Socket.IO  

---

# 🏗 System Architecture

```
User A (Browser)
     │
     │  WebRTC Offer / Answer
     │
Socket.IO Signaling Server
     │
     │  ICE Candidate Exchange
     │
User B (Browser)
```

### Communication Flow

1. User creates or joins a meeting room.  
2. Client establishes a Socket.IO connection with the backend server.  
3. Signaling messages are exchanged between participants.  
4. WebRTC establishes a peer-to-peer connection.  
5. Video and audio streams are transmitted directly between users.

---

# 📂 Project Structure

```
MeetSpace
│
├── backend
│   └── src
│       ├── controllers
│       │      socketManager.js
│       │
│       ├── models
│       │
│       ├── routes
│       │      users.routes.js
│       │
│       └── app.js
│
└── frontend
    └── src
        ├── contexts
        │      AuthContext.js
        │
        ├── pages
        │      landing
        │      authentication
        │      home
        │      VideoMeet
        │      history
        │
        ├── styles
        ├── utils
        └── App.js
```

---

# ⚙️ Installation

## Clone the Repository

```bash
git clone https://github.com/yourusername/meetspace.git
cd meetspace
```

---

## Backend Setup

```bash
cd backend
npm install
```

Create `.env` file

```
PORT=8000
MONGO_URI=your_mongodb_connection_string
```

Run backend server

```bash
npm start
```

---

## Frontend Setup

```bash
cd frontend
npm install
npm start
```

Frontend runs on

```
http://localhost:3000
```

Backend runs on

```
http://localhost:8000
```

---

# 🔗 API Endpoints

```
POST /api/v1/users/register
POST /api/v1/users/login
GET  /api/v1/users/history
```

---

# 🔮 Future Improvements

- Screen sharing support  
- In-meeting chat system  
- Meeting recording feature  
- TURN server integration for NAT traversal  
- Docker-based deployment  

---

# 👨‍💻 Author

Garv Gupta

---

# 📜 License

This project is licensed under the MIT License.