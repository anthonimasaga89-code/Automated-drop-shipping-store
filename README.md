# Automated E-commerce Dropshipping Store

## 1. Overview

**Business Model:** Retail dropshipping — products are sourced from suppliers (Alibaba/AliExpress) and shipped directly to customers upon purchase. Starts as a web app, with a mobile app planned for a later phase.

**Tech Stack:**
- Frontend: React
- Backend: FastAPI (Python)
- ORM / Database: SQLAlchemy with PostgreSQL (or SQLite for local dev)
- Containerization: Docker
- Payments: M-Pesa (mobile money) + card payments

## 2. Core Features

- **Supplier Sourcing:** Product data (descriptions, images, pricing) imported via CSV for now, with automation (DSers/AutoDS API) planned for a later phase.
- **Inventory & Price Sync:** Product availability and pricing kept in sync with supplier data.
- **Order Routing:** Orders move through a defined lifecycle — pending → processing → shipped → cancelled/returned — so fulfillment status is always clear.
- **Payments:** Checkout supports M-Pesa and card payments.
- **Order Tracking:** Automated email notifications with tracking numbers once an order ships.
- **Admin Dashboard:** Manage products (including CSV imports), orders, customers, and store settings.

## 3. Project Structure

##automated-ecommerce-dropshipping-store/
├── frontend/
│   ├── public/
│   └── src/
│       ├── assets/
│       ├── components/
│       │   ├── common/
│       │   ├── product/
│       │   ├── cart/
│       │   ├── checkout/
│       │   ├── reviews/
│       │   └── admin/
│       │       ├── layout/
│       │       ├── widgets/
│       │       └── forms/
│       ├── pages/
│       │   ├── Home/
│       │   ├── CategoryPage/
│       │   ├── ProductPage/
│       │   ├── Cart/
│       │   ├── Checkout/
│       │   ├── OrderTracking/
│       │   ├── Auth/
│       │   ├── Policies/
│       │   │   ├── ShippingPolicy/
│       │   │   ├── RefundPolicy/
│       │   │   ├── PrivacyPolicy/
│       │   │   └── ContactUs/
│       │   └── Admin/
│       │       ├── Dashboard/
│       │       ├── ProductManagement/
│       │       │   └── CsvImport/
│       │       ├── OrderManagement/
│       │       │   ├── PendingOrders/
│       │       │   ├── ProcessingOrders/
│       │       │   ├── ShippedOrders/
│       │       │   └── CancelledReturned/
│       │       ├── CustomerManagement/
│       │       └── Settings/
│       ├── context/
│       ├── hooks/
│       ├── services/
│       │   ├── api/
│       │   └── payments/
│       ├── utils/
│       └── styles/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── v1/
│   │   │       ├── products/
│   │   │       ├── categories/
│   │   │       ├── cart/
│   │   │       ├── orders/
│   │   │       │   ├── pending/
│   │   │       │   ├── processing/
│   │   │       │   ├── shipped/
│   │   │       │   └── cancelled_returned/
│   │   │       ├── payments/
│   │   │       │   ├── mpesa/
│   │   │       │   └── card/
│   │   │       ├── suppliers/
│   │   │       │   └── csv_import/
│   │   │       ├── shipping_tracking/
│   │   │       ├── reviews/
│   │   │       ├── auth/
│   │   │       └── admin/
│   │   │           ├── dashboard_stats/
│   │   │           ├── product_management/
│   │   │           ├── order_management/
│   │   │           ├── customer_management/
│   │   │           └── settings/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── services/
│   │   │   ├── payment_service/
│   │   │   ├── supplier_service/
│   │   │   ├── inventory_sync/
│   │   │   ├── order_routing/
│   │   │   └── notification_service/
│   │   ├── core/
│   │   ├── db/
│   │   └── tasks/
│   │       └── scheduled_jobs/
│   ├── alembic/
│   │   └── versions/
│   └── tests/
│
├── docker/
├── docs/
└── .env.example
```


## 4. Environment Configuration

Copy `.env.example` to `.env` in both `frontend/` and `backend/` and fill in:

- Database connection string (PostgreSQL or SQLite)
- M-Pesa Daraja API credentials (Consumer Key, Consumer Secret, Shortcode, Passkey)
- Card payment gateway credentials (e.g. Stripe keys)
- Email service credentials (for order tracking notifications)
- JWT secret / auth configuration

## 5. Setup Instructions

1. Clone the repository.
2. **Backend:** create a virtual environment, install dependencies, run Alembic migrations, start the FastAPI server.
3. **Frontend:** install dependencies, start the React dev server.
4. **Docker (optional):** use `docker/` configs to run the full stack (frontend, backend, database) together.

## 6. Docker Usage

The `docker/` folder holds Dockerfiles and a docker-compose configuration to spin up the frontend, backend, and database as containers for local development or deployment.

## 7. Contributing

- Follow the existing folder structure when adding new features.
- Keep supplier/payment integrations isolated in their respective `services/` subfolders so provider swaps (e.g. adding DSers automation later) don't require touching unrelated code.
- Write tests for new backend endpoints under `backend/tests/`.

## 8. Roadmap Notes

- Phase 1: Web app, CSV-based supplier import, M-Pesa + card checkout.
- Phase 2: Automate supplier sync via DSers/AutoDS API.
- Phase 3: Mobile app.
