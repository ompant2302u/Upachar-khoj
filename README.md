# UpacharKhoj Nepal

**Healthcare Availability & Referral Coordination Platform**

A full-stack web application built with Django (Backend) and React + TypeScript (Frontend) to help patients, health workers, and hospitals coordinate healthcare services in Nepal.

## 🎯 Project Overview

UpacharKhoj Nepal solves a critical problem: **patients traveling long distances to hospitals only to find required services unavailable**. This platform allows:

- **Patients/Families**: Search hospitals by service (ICU, MRI, Dialysis, etc.) and check real-time availability
- **Health Workers**: Create referral requests and track responses before patient transfer
- **Hospital Staff**: Update bed/service availability and respond to incoming referrals
- **System Admins**: Manage hospitals, services, users, and monitor platform health

---

## 🏗️ Tech Stack

### Backend
- **Python 3.11** + **Django 4.2**
- **Django REST Framework** (DRF) for APIs
- **Django Channels** for WebSocket real-time updates
- **SimpleJWT** for authentication
- **SQLite** (development) / **PostgreSQL** (production-ready)
- **Redis** (optional, for production Channels layer)

### Frontend
- **React 18** + **TypeScript**
- **Vite** (build tool)
- **Tailwind CSS** (styling)
- **React Router v6** (routing)
- **Zustand** (state management)
- **Axios** (API calls)
- **React Hot Toast** (notifications)
- **date-fns** (date formatting)
- **Leaflet** (maps - placeholder ready)

---

## 🚀 Quick Start

### Prerequisites
- **Python 3.11+**
- **Node.js 20+** and **npm**
- **Git**

### Option 1: Run with Docker (Recommended)

```bash
cd C:\Users\rajju\Music\UpacharKhoj

# Build and start all services
docker-compose up --build

# Backend will be at: http://localhost:8000
# Frontend will be at: http://localhost:3000
# API docs at: http://localhost:8000/api/
```

The `seed_data` command runs automatically and creates all demo accounts.

### Option 2: Run Manually (Development)

#### Backend Setup

```bash
cd C:\Users\rajju\Music\UpacharKhoj\backend

# Create virtual environment
python -m venv venv
venv\Scripts\activate  # On Windows
# source venv/bin/activate  # On Linux/Mac

# Install dependencies
pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Create seed data (demo hospitals, services, users)
python manage.py seed_data

# Start Django server
python manage.py runserver
```

Backend runs at: **http://localhost:8000**

#### Frontend Setup

```bash
cd C:\Users\rajju\Music\UpacharKhoj\frontend

# Install dependencies
npm install

# Start Vite dev server
npm run dev
```
---

## 🌐 API Endpoints

### Authentication
- `POST /api/auth/login/` - Login (returns JWT tokens)
- `POST /api/auth/logout/` - Logout
- `POST /api/auth/register/` - Register health worker
- `GET /api/auth/profile/` - Get current user
- `PATCH /api/auth/profile/` - Update profile
- `POST /api/auth/token/refresh/` - Refresh access token

### Hospitals
- `GET /api/hospitals/` - List hospitals (public, filterable by service/district)
- `GET /api/hospitals/{id}/` - Hospital detail (public)
- `POST /api/hospitals/` - Create hospital (system admin only)
- `PATCH /api/hospitals/{id}/` - Update hospital
- `DELETE /api/hospitals/{id}/` - Delete hospital

### Services
- `GET /api/services/` - List services (public)
- `GET /api/services/{id}/` - Service detail
- `POST /api/services/` - Create service (system admin only)

### Availability
- `GET /api/availability/?hospital={id}` - Get hospital availability (public)
- `POST /api/availability/` - Create availability (hospital staff)
- `PATCH /api/availability/{id}/` - Update availability
- `POST /api/hospitals/{id}/availability/bulk_update/` - Bulk update

### Referrals
- `GET /api/referrals/` - List referrals (filtered by role)
- `POST /api/referrals/` - Create referral (health worker)
- `GET /api/referrals/{id}/` - Referral detail
- `PATCH /api/referrals/{id}/respond/` - Respond to referral (hospital staff)
- `PATCH /api/referrals/{id}/update_status/` - Update status (health worker)

### WebSocket
- `ws://localhost:8000/ws/availability/` - Global availability updates
- `ws://localhost:8000/ws/availability/{hospital_id}/` - Hospital-specific updates

---

## 📁 Project Structure

```
UpacharKhoj/
├── backend/                    # Django backend
│   ├── manage.py
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── upacharkhoj/           # Django project settings
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── asgi.py
│   │   ├── wsgi.py
│   │   └── routing.py         # WebSocket routing
│   └── apps/
│       ├── accounts/          # User authentication & roles
│       │   ├── models.py      # Custom User model
│       │   ├── serializers.py
│       │   ├── views.py
│       │   ├── permissions.py
│       │   └── urls.py
│       ├── hospitals/         # Hospital & service management
│       │   ├── models.py      # Hospital, Service, HospitalService, Availability
│       │   ├── serializers.py
│       │   ├── views.py
│       │   ├── consumers.py   # WebSocket consumers
│       │   ├── management/
│       │   │   └── commands/
│       │   │       └── seed_data.py  # Demo data creation
│       │   └── urls.py
│       ├── referrals/         # Referral workflow
│       │   ├── models.py      # Referral, ReferralEvent
│       │   ├── serializers.py
│       │   ├── views.py
│       │   └── urls.py
│       └── audit/             # Audit logging
│           ├── models.py      # AuditLog
│           ├── views.py
│           └── urls.py
│
├── frontend/                   # React frontend
│   ├── package.json
│   ├── vite.config.ts
│   ├── tsconfig.json
│   ├── tailwind.config.js
│   ├── Dockerfile
│   ├── index.html
│   └── src/
│       ├── main.tsx           # Entry point
│       ├── App.tsx            # Router & routes
│       ├── index.css          # Tailwind + custom styles
│       ├── types/
│       │   └── index.ts       # TypeScript interfaces
│       ├── lib/
│       │   └── api.ts         # Axios instance & API functions
│       ├── store/
│       │   ├── authStore.ts   # Zustand auth state
│       │   └── notificationStore.ts
│       ├── hooks/
│       ├── components/
│       │   ├── layout/
│       │   │   ├── Navbar.tsx
│       │   │   ├── Footer.tsx
│       │   │   ├── Layout.tsx
│       │   │   └── DashboardLayout.tsx
│       │   └── common/
│       │       ├── FreshnessTag.tsx
│       │       ├── StatusBadge.tsx
│       │       ├── ReferralStatusBadge.tsx
│       │       ├── LoadingSpinner.tsx
│       │       ├── DataFreshnessAlert.tsx
│       │       └── ProtectedRoute.tsx
│       └── pages/
│           ├── public/
│           │   ├── HomePage.tsx
│           │   ├── SearchPage.tsx
│           │   ├── HospitalDetailPage.tsx
│           │   └── LoginPage.tsx
│           ├── healthworker/
│           │   ├── HWDashboard.tsx
│           │   ├── HWReferrals.tsx
│           │   ├── HWNewReferral.tsx
│           │   └── HWReferralDetail.tsx
│           ├── hospitalstaff/
│           │   └── StaffDashboard.tsx
│           ├── hospitaladmin/
│           └── admin/
│
└── docker-compose.yml         # Docker orchestration
```

---

## 🎨 Design Principles

- **Mobile-first**: All pages work on 360px width and up
- **Accessible**: ARIA labels, keyboard navigation, high contrast
- **Bilingual-ready**: Text structured for English/Nepali translation
- **Clear data freshness**: Every changing data point shows "last updated"
- **Minimal cognitive load**: Focused workflows, no unnecessary features
- **Safety warnings**: Stale data alerts, "hospital-reported" disclaimers

---

## 🚢 Deployment (Production)

### Backend
- **Hosting**: Render, Railway, AWS EC2, DigitalOcean
- **Database**: PostgreSQL (required for production)
- **Static/Media**: AWS S3 or local storage
- **WebSocket**: Redis + Daphne (Channels production setup)

### Frontend
- **Hosting**: Vercel, Netlify, or serve via Django static
- **Build**: `npm run build` → `dist/` folder
- **Env**: Set `VITE_API_URL` to production backend URL
