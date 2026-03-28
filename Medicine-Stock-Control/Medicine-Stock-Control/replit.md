# Smart Hospital Pharmacy System

## Overview

Full-stack hospital pharmacy management system with two roles: Admin and Staff.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite + Tailwind CSS + shadcn/ui
- **Auth**: JWT tokens with bcryptjs password hashing

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (all backend routes)
│   └── pharmacy/           # React+Vite frontend
├── lib/
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
└── scripts/                # Utility scripts
```

## Modules Implemented

### Module 1 – Authentication
- Login (Admin/Staff), account lockout after 3 failed attempts
- Forgot password via OTP (demo OTP shown in response)
- JWT-based auth, Bearer token in headers
- Admin can unlock locked staff accounts + reset password

### Module 2 – Inventory Management
- Add, view, update medicines
- Stock In / Stock Out with transaction logging
- Validation (SKU unique, cost > 0, active status, etc.)

### Module 3 – Alerts
- Auto-generated LOW_STOCK, OUT_OF_STOCK, EXPIRY_SOON alerts
- Low stock threshold: stockQuantity ≤ minStockLevel (default: 5)
- Expiry alert: medicines expiring within 10 days
- Alerts accessible via notification bell in header
- Admin/Staff can resolve alerts

### Module 4 – Transaction History
- Complete audit trail for all stock in/out operations
- Filterable by medicine, type, date range, performer

### Module 5 – Reports & Analytics
- Summary dashboard (total, active, low stock, out of stock, expiring counts)
- Stock trend chart (daily stock in/out)
- Top moving medicines
- Low stock & expiring reports
- PDF export capability

## Admin Panel Sidebar
- Dashboard
- New Staff Account
- Staff Details
- Locked Accounts
- Medicine Details
- Low Stock Medicines
- Reports

## Staff Panel Sidebar
- Dashboard (profile info top-right)
- Add New Medicine
- Medicine Details
- Medicine Stock In/Out
- Low Stock Medicine
- Expiry Medicine Alert
- Reports

## Default Credentials

- **Admin**: username: `admin`, password: `admin123`
- Staff accounts created by admin

## Database Schema

Tables:
- `users` — Admin + Staff with role, lockout tracking, OTP reset
- `medicines` — Medicine inventory with SKU, category, expiry, stock levels
- `alerts` — System-generated stock and expiry alerts
- `transactions` — Full audit trail of all stock movements

## API Routes

All under `/api`:
- `POST /auth/login` — Login
- `POST /auth/forgot-password` — Request OTP
- `POST /auth/verify-otp` — Reset password with OTP
- `GET /auth/me` — Current user profile
- `GET/POST /staff` — List/create staff (admin only)
- `GET/PUT /staff/:id` — Get/update staff (admin only)
- `POST /staff/:id/unlock` — Unlock + reset password (admin only)
- `GET/POST /medicines` — List/create medicines
- `GET/PUT /medicines/:id` — Get/update medicine
- `POST /medicines/:id/stock-in` — Add stock
- `POST /medicines/:id/stock-out` — Deduct stock
- `GET /alerts` — List alerts (filterable)
- `POST /alerts/:id/resolve` — Resolve alert
- `GET /transactions` — Transaction history (filterable)
- `GET /reports/summary` — Full analytics report
- `GET /reports/low-stock` — Low stock medicines
- `GET /reports/expiring` — Expiring soon medicines
