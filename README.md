# Connectly — Real-Time Communication & Collaboration Platform

[![Live Demo](https://img.shields.io/badge/Live_Demo-onrender.com-success?style=for-the-badge&logo=render&logoColor=white)](https://connectly-real-time-communication-app.onrender.com/)
![Django](https://img.shields.io/badge/Django-5.0%2B-092E20?style=for-the-badge&logo=django&logoColor=white)
![Django Channels](https://img.shields.io/badge/Django_Channels-4.0%2B-25D366?style=for-the-badge&logo=websocket&logoColor=white)
![Daphne](https://img.shields.io/badge/ASGI-Daphne-4A154B?style=for-the-badge&logo=fastapi&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

A full-stack, real-time communication and social collaboration platform designed for students, project teams, and modern community circles.

Connectly seamlessly unites persistent social discourse (feeds, community posts, like/comment engines, social graphs) with bidirectional real-time direct messaging, live presence detection, dynamic typing indicators, and an optimized hybrid file attachment pipeline powered by **Django Channels**, **Daphne ASGI**, and **WebSockets**.

**Organization:** CodeAlpha  
**Live Production URL:** [https://connectly-real-time-communication-app.onrender.com/](https://connectly-real-time-communication-app.onrender.com/)

---

## 📑 Technical Architecture & Blueprints

Connectly has been engineered according to comprehensive visual specifications and blueprints preserved in [`docs/`](docs/):

| Document / Blueprint | Description | Link |
| :--- | :--- | :--- |
| **Technical Blueprint** | Real-Time Chat App End-to-End WebSocket & ASGI Architecture | [View Technical Blueprint](docs/Real-Time%20Chat%20App%20Technical%20Blueprint.png) |
| **Implementation Roadmap** | Step-by-step engineering roadmap and deployment phases | [View Roadmap](docs/Connectly%20Implementation%20Roadmap.png) |
| **Backend Schema Infographic** | Relational data models, foreign keys, constraints & indexing | [View Schema](docs/Connectly%20Backend%20Schema%20Infographic%20(1).png) |
| **Application Flow Infographic** | User lifecycle, authentication, event dispatch, and state handling | [View Application Flow](docs/Connectly%20Application%20Flow%20Infographic.png) |
| **UI/UX Design Brief** | Design tokens, color system, component typography & layouts | [View Design Brief](docs/Connectly%20UI_UX%20Design%20Brief%20(1).png) |
| **Product Requirements (PRD)** | Complete functional requirements, acceptance criteria & metrics | [View PRD](docs/Connectly%20Product%20Requirements%20Infographic.png) |

---

## 🎯 Project Overview & Objectives

Connectly is designed to streamline online communication and team collaboration by providing essential interaction and productivity tools within a unified, responsive web application.

The platform demonstrates:
- **Production-grade full-stack architecture** with Python 3, Django 5, and Daphne ASGI.
- **Low-latency bidirectional WebSockets** via Django Channels for instant real-time message routing.
- **Optimized hybrid transfer pattern** for file and media attachments (< 15MB) preventing WebSocket head-of-line blocking.
- **Robust relational data modeling** with SQLite for zero-dependency local development and production readiness.
- **Strict access control and authorization guards** ensuring private conversation isolation and ownership enforcement.
- **Automated testing** with an asynchronous Channels test suite covering WebSockets, model constraints, and uploads.

---

## 🌟 Key Features

### 1. 👥 User Authentication & Social Graph (`accounts/`)
- **User Registration & Login**: Built-in authentication with encrypted password hashing and session management.
- **Automated Profile Lifecycle**: Django `post_save` signals automatically provision user profiles with avatars, bios, and location.
- **Follow / Unfollow Engine**: Asynchronous follow toggle backed by database-level uniqueness constraints and self-follow prevention.
- **Protected Application Routes**: `@login_required` decorators and session validation protecting all sensitive interaction endpoints.

### 2. 📰 Dynamic Feed & Community Discussions (`feed/`)
- **Interactive Post Creator**: Supports text updates and validated image uploads with file size constraints (< 8MB).
- **Asynchronous Reactions**: Instant Like/Unlike toggle and comment publishing powered by Vanilla JS and AJAX CSRF handling without page reloads.
- **Explore & Discovery**: Live query search across member profiles, usernames, and trending discussion topics.
- **Author Permissions**: Guarded edit and delete actions enforcing strict author ownership.

### 3. 💬 Real-Time WebSockets & Direct Messaging (`chat/`)
- **Bidirectional WebSockets**: Low-latency communication via `AsyncWebsocketConsumer` and ASGI URL routing (`/ws/chat/<room_id>/`).
- **Presence & Status Indicators**: Live online/offline status broadcast when participants connect or disconnect.
- **Interactive Typing Indicators**: Real-time debounce typing feedback displaying when the other party is drafting a response.
- **Read Receipts & Unread Badges**: Real-time unread message counters updated dynamically across conversations.

### 4. 📎 Optimized Hybrid Attachment Handling
Transferring binary files directly over WebSockets can cause head-of-line blocking and memory saturation in channel layers. Connectly implements an **Optimized Hybrid Transfer Pattern**:
1. **Client Staging & Drag-and-Drop**: Users can browse or drag and drop files directly onto the chat window with instantaneous thumbnail previews and size inspection (< 15MB).
2. **HTTP POST Multipart Upload**: Files are transmitted asynchronously via AJAX to `/chat/<room_id>/upload/`.
3. **Storage & Validation**: Safe file classification (image thumbnails vs. downloadable documents with badges) and database persistence.
4. **Channels Broadcast**: The server triggers a lightweight WebSocket event to the room's Channel Layer, instantly rendering formatted message bubbles and download links on both endpoints.

### 5. 🛡️ Security Features
- **XSS Sanitization**: Dynamic HTML escaping in JavaScript preventing script injection in user content and filenames.
- **CSRF Protection**: Integrated Django CSRF tokens across all standard forms and AJAX requests (`X-CSRFToken`).
- **Secure Session Cookies**: HTTP-only cookies preventing client-side script tampering.
- **File Upload Safeguards**: MIME-type classification, size caps, and filename path traversal normalization (`os.path.basename`).
- **Participant Authorization**: Strict validation in both HTTP views and WebSocket connection handshakes rejecting unauthorized non-participants.

---

## 🛠️ Technology Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | HTML5, CSS3, Vanilla JavaScript (ES6+), Tailwind CSS, Material Symbols, Plus Jakarta Sans |
| **Backend Framework** | Django 5.x (Python 3.11+) |
| **ASGI & WebSockets** | Django Channels 4.x, Daphne ASGI Server |
| **Channel Layer** | In-Memory Channel Layer (local / dev) & Redis Channel Layer (production) |
| **Database & ORM** | SQLite (default development) / PostgreSQL ready |
| **Static & Media Asset Delivery** | WhiteNoise with Brotli & Gzip compression, Pillow (image processing) |
| **Testing Suite** | Django Test Framework, Channels `WebsocketCommunicator`, unittest |
| **Deployment & Hosting** | Render (Docker & Native Python runtime), Procfile, Dockerfile |

---

## 🏗️ Directory Layout

```text
Connectly/
├── manage.py
├── requirements.txt
├── Procfile                              # ASGI process runner for Daphne
├── Dockerfile                            # Production container build
├── render.yaml                           # 1-click cloud blueprint
├── build.sh                              # Deployment build & migration script
├── .gitignore
├── LICENSE
├── README.md
├── docs/                                 # Architectural infographics & blueprints
│   ├── Connectly Implementation Roadmap.png
│   ├── Connectly Backend Schema Infographic (1).png
│   ├── Connectly UI_UX Design Brief (1).png
│   ├── Connectly Application Flow Infographic.png
│   ├── Connectly Product Requirements Infographic.png
│   └── Real-Time Chat App Technical Blueprint.png
├── connectly/                            # Project configuration root
│   ├── __init__.py
│   ├── asgi.py                           # ProtocolTypeRouter (HTTP + WebSockets)
│   ├── settings.py                       # Channel layers, WhiteNoise, and auth settings
│   ├── urls.py                           # Root URL routing
│   └── wsgi.py
├── accounts/                             # Authentication, Profiles & Social Graph
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py                         # Profile & Follow models
│   ├── signals.py                        # Automated profile generation signal
│   ├── tests.py                          # Auth & Follow test cases
│   ├── urls.py
│   └── views.py
├── feed/                                 # Post Feed, Likes, Comments & Explore
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py                         # Post, Comment, and Like models
│   ├── tests.py                          # Feed & interaction test cases
│   ├── urls.py
│   ├── views.py
│   └── management/commands/seed_data.py  # Automated demo data seeder
├── chat/                                 # Real-Time Channels & Direct Messaging
│   ├── admin.py
│   ├── apps.py
│   ├── consumers.py                      # AsyncWebsocketConsumer for messaging & presence
│   ├── models.py                         # Conversation & Message with attachments
│   ├── routing.py                        # WebSocket URL patterns
│   ├── tests.py                          # Channels WebSocket & upload test suite
│   ├── urls.py
│   └── views.py
├── static/
│   ├── css/
│   │   ├── style.css                     # Global design tokens and social cards
│   │   └── chat.css                      # Chat room, attachments, and composer styles
│   └── js/
│       ├── main.js                       # AJAX likes, comments, and follow scripts
│       └── chat.js                       # WebSocket client, file staging & drop zone
└── templates/
    ├── base.html                         # Master layout with responsive navbar
    ├── accounts/
    │   ├── login.html
    │   ├── register.html
    │   ├── profile.html
    │   └── edit_profile.html
    ├── feed/
    │   ├── home.html
    │   ├── explore.html
    │   ├── post_detail.html
    │   └── post_edit.html
    └── chat/
        ├── chat_list.html
        └── chat_room.html
```

---

## 🚀 Installation & Local Setup

### Prerequisites
- Python 3.11+
- Git

### 1. Clone the Repository
```bash
git clone https://github.com/shivangsaxena1011/Connectly-Real-Time-Communication-App.git
cd Connectly-Real-Time-Communication-App
```

### 2. Set Up Virtual Environment & Dependencies
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows (PowerShell):
.\venv\Scripts\Activate.ps1
# macOS / Linux:
source venv/bin/activate

# Install required dependencies
pip install -r requirements.txt
```

### 3. Run Database Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### 4. Seed Demo Content & Test Users
Connectly includes a built-in seeder command that generates demo accounts (`shivang`, `ananya`, `rohan`, `priya`), mock posts, comments, likes, and sample conversations:
```bash
python manage.py seed_data
```
> **Default credentials for all seeded demo users:**
> - **Username:** `shivang` (or `ananya`, `rohan`, `priya`)
> - **Password:** `DemoPassword123!`

### 5. Start the Real-Time Server
Because Connectly utilizes **Daphne** as its ASGI server, starting `manage.py runserver` automatically boots Daphne with full WebSocket support:
```bash
python manage.py runserver
```
Visit `http://127.0.0.1:8000/` in your browser.

---

## 🧪 Automated Testing Suite

Connectly includes a comprehensive test suite covering authentication, social graph operations, post access control, WebSocket communication, participant authorization, search queries, and file upload restrictions.

Run all tests:
```bash
python manage.py test
```

Expected output:
```text
Creating test database for alias 'default'...
..................
----------------------------------------------------------------------
Ran 18 tests in ~16s

OK
Destroying test database for alias 'default'...
```

---

## 🎓 Learning Outcomes

Through this project, key competencies developed include:
- Designing and implementing full-stack real-time web applications with Python & Django.
- Architecting asynchronous event-driven pipelines using **Django Channels**, **Daphne**, and **WebSockets**.
- Designing relational data models with custom constraints, foreign key indexes, and signal handlers.
- Structuring hybrid transfer protocols for file uploads to eliminate WebSocket head-of-line blocking.
- Securing real-time applications with CSRF validation, XSS escaping, and authorization middleware.
- Configuring cloud-native deployment pipelines with WhiteNoise, Procfile, Dockerfile, and Render blueprints.

---

## 🔮 Future Enhancements

- **Peer-to-Peer Video & Audio**: WebRTC integration with STUN/TURN signaling over Django Channels.
- **Collaborative Whiteboard**: Synchronized canvas drawing with persistent snapshots.
- **Group Channels & Circles**: Multi-participant rooms with administrative roles and channel permissions.
- **End-to-End Encryption (E2EE)**: Client-side cryptographic message encryption.
- **Message Search & Rich Text**: Full-text message indexing and Markdown formatting.

---

## 🌐 Production Deployment

The project is deployed and actively hosted on **Render**:
- **Live URL:** [https://connectly-real-time-communication-app.onrender.com/](https://connectly-real-time-communication-app.onrender.com/)
- Automatic CI/CD is enabled via GitHub on the `main` branch.

---

## 👥 Acknowledgements

Special thanks to **CodeAlpha** for the project mentorship and guidelines.

---

## 📜 License

This project is licensed under the MIT License — see the [`LICENSE`](LICENSE) file for details.
