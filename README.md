# SplitEase — Shared Expenses App

A cross-platform mobile app for roommates, friends and teams to split shared expenses, track who owes whom, and settle debts — with group analytics and a full activity history. The repository contains both the mobile app and its API server.

![React Native](https://img.shields.io/badge/React_Native-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Expo](https://img.shields.io/badge/Expo_SDK_54-000020?style=flat-square&logo=expo&logoColor=white)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-FF4154?style=flat-square&logo=reactquery&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)

## Features

- **Secure accounts** — JWT access and refresh tokens, stored on the device with Expo SecureStore and rotated automatically by an Axios interceptor
- **Groups** — create groups (household, trips, food…), assign admin or member roles, and invite people by email with pending-invitation handling
- **Expense splitting** — add an expense once and it is split equally among the selected members
- **Debt tracking** — net balances between members, active vs. settled debts, and one-tap "Settle all" between two people
- **Analytics** — total group spending, monthly trends and per-person spending patterns
- **Activity log** — an audit trail of every action (expenses, joins, settlements) for full transparency

## Architecture

```text
Expo app (React Native)
  screens ─▶ hooks (TanStack Query) ─▶ utils/api.js (Axios + token refresh)
                                              │  REST /api
                                              ▼
Express API ─▶ routes ─▶ validation ─▶ controllers ─▶ Mongoose models ─▶ MongoDB
              (Helmet, CORS, rate limiting, JWT auth, centralized errors, Morgan logging)
```

```text
Sp-itEsae/
├── SplitEsae/            # Expo React Native app
│   ├── app/              # Expo Router entry
│   ├── screens/          # Login, Main, GroupDetail, Debts, Transactions, Analytics, Activities…
│   ├── components/       # Reusable UI
│   ├── contexts/         # Auth and theme providers
│   ├── hooks/            # React Query hooks
│   └── utils/            # API client and secure storage helpers
├── server/               # Express API
│   ├── models/           # User, Group, Invitation, Debt, Transaction, Activity
│   ├── controllers/ routes/ middleware/ validation/
│   └── config/           # Database connection and .env
└── simulate-all.js       # End-to-end scenario simulator
```

## Getting Started

**Requirements:** Node.js 18+, MongoDB (Atlas or local), Expo Go on a phone or an emulator

### 1. API server

```bash
cd server
npm install
```

Create `server/config/.env`:

```env
PORT=5001
MONGO_URI=mongodb+srv://...
JWT_SECRET=change-me
REFRESH_TOKEN_SECRET=change-me-too
```

```bash
npm run dev        # http://localhost:5001 — health check at /health
```

### 2. Mobile app

```bash
cd SplitEsae
npm install
npm run start      # scan the QR code with Expo Go
```

Point the app at your API in `SplitEsae/utils/api.js`:

| Device | Base URL |
|---|---|
| Android emulator | `http://10.0.2.2:5001/api` |
| iOS simulator | `http://localhost:5001/api` |
| Physical device | `http://<your-computer-LAN-IP>:5001/api` |

## End-to-End Simulation

`simulate-all.js` exercises the whole API with a realistic scenario: five students sharing a flat across four groups. It registers users, creates groups, sends and accepts invitations, splits rent, internet and dinners, computes balances and settles debts — printing colored step-by-step logs.

```bash
node simulate-all.js    # requires a valid database connection in server/config/.env
```

## Author

**Abdellah Sheikh** — [abdellahsheikh.dev](https://abdellahsheikh.dev) · [LinkedIn](https://www.linkedin.com/in/abdellahsheikh)
