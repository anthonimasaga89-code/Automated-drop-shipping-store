# Automated E-commerce Dropshipping Store

## 1. Overview

**Business Model:** Retail dropshipping — products are sourced from suppliers (Alibaba/AliExpress) and shipped directly to customers upon purchase. Starts as a web app, with a mobile app planned for a later phase.

**Tech Stack:**
- Frontend: React
- Backend: FastAPI (Python)
- ORM / Database: SQLAlchemy with PostgreSQL
- Payments: M-Pesa (Daraja API) + card payments (Stripe)

## 2. Core Features

- **Supplier Sourcing:** Product data imported via CSV for now, with DSers/AutoDS API automation planned for a later phase.
- **Inventory & Price Sync:** Product availability and pricing kept in sync with supplier data.
- **Product Catalog:** Published/draft/archived product lifecycle, categories, search, filtering, pagination.
- **Cart:** Per-customer cart with stock validation and price locked at time of adding.
- **Orders:** Order lifecycle — pending → processing → shipped → cancelled/returned.
- **Payments:** Checkout supports M-Pesa and card payments.
- **Order Tracking:** Automated email notifications with tracking numbers once an order ships.
- **Role-Based Access:** Customer, Admin, and Super Admin roles.

## 3. Architecture

This project uses a simplified 3-layer backend architecture — no separate repository layer. Services handle both business logic and database queries directly against the models.

```
Request → routes/ → services/ (business logic + DB queries) → models/
```

- **`routes/`** — thin FastAPI endpoint definitions. No business logic here; each route calls a service function and returns the result.
- **`services/`** — where the actual work happens: validation, permission checks, and the SQLAlchemy queries themselves.
- **`models/`** — SQLAlchemy table definitions.
- **`schemas/`** — Pydantic request/response models, kept separate from the database models.
- **`core/`** — cross-cutting setup: configuration, database session, and security (JWT, password hashing).
- **`dependencies.py`** — a single file holding shared FastAPI dependencies like `get_current_user`, `admin_required`, and `super_admin_required`.

## 4. Project Structure

```
automated-ecommerce-dropshipping-store/
├── frontend/
│   ├── public/
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── services/
│       ├── hooks/
│       ├── context/
│       └── utils/
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── core/
│   │   │   ├── config.py
│   │   │   ├── database.py
│   │   │   └── security.py
│   │   ├── models/
│   │   │   ├── user.py
│   │   │   ├── product.py
│   │   │   ├── cart.py
│   │   │   ├── order.py
│   │   │   └── payment.py
│   │   ├── schemas/
│   │   │   ├── user.py
│   │   │   ├── product.py
│   │   │   ├── cart.py
│   │   │   └── order.py
│   │   ├── services/
│   │   │   ├── auth_service.py
│   │   │   ├── product_service.py
│   │   │   ├── cart_service.py
│   │   │   └── order_service.py
│   │   ├── routes/
│   │   │   ├── auth.py
│   │   │   ├── products.py
│   │   │   ├── cart.py
│   │   │   └── orders.py
│   │   └── dependencies.py
│   ├── alembic/
│   ├── requirements.txt
│   └── .env
│
├── .gitignore
└── README.md
```

## 5. Environment Configuration

Copy `.env.example` to `.env` in `backend/` and fill in:

- `DATABASE_URL` — PostgreSQL connection string
- `JWT_SECRET_KEY`, `JWT_ALGORITHM`, `ACCESS_TOKEN_EXPIRE_MINUTES`
- M-Pesa Daraja API credentials (Consumer Key, Consumer Secret, Shortcode, Passkey)
- `STRIPE_SECRET_KEY`
- Email service credentials (order tracking notifications)

## 6. Setup Instructions

1. Clone the repository.
2. **Backend:**
   ```bash
   cd backend
   python -m venv venv
   source venv/bin/activate   # venv\Scripts\activate on Windows
   pip install -r requirements.txt
   alembic upgrade head
   uvicorn app.main:app --reload
   ```
3. **Frontend:**
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

## 7. Contributing

- Keep routes thin — no business logic or SQLAlchemy queries inside route handlers.
- All validation, permission checks, and database queries belong in `services/`.
- One file per feature per layer (e.g. `product.py` in `models/`, `schemas/`, `services/`, `routes/`) — don't create nested subfolders per feature.
- Reuse `dependencies.py` (`get_current_user`, `admin_required`, `super_admin_required`) instead of writing new permission checks inline.
- Write tests for new service functions under `backend/tests/`.

## 8. Roadmap

- **Phase 1:** Web app, CSV-based supplier import, M-Pesa + card checkout.
- **Phase 2:** Automate supplier sync via DSers/AutoDS API.
- **Phase 3:** Mobile app.

