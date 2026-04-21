# NR Dry Fruit

Production-ready full-stack e-commerce web application for a premium dry fruit shop named **NR Dry Fruit**.

## Tech Stack

- Frontend: React + Vite + Tailwind CSS
- Backend: Node.js + Express
- Database: MongoDB + Mongoose
- Auth: JWT + bcrypt
- Payments: Razorpay + Cash on Delivery
- Maps: Google Maps Embed API URL
- Extras: Coupons, email notification hook, image uploads, product reviews, admin panel

## Folder Structure

```text
.
|-- backend
|   |-- package.json
|   |-- uploads
|   `-- src
|       |-- app.js
|       |-- server.js
|       |-- config
|       |-- controllers
|       |-- data
|       |-- middleware
|       |-- models
|       |-- routes
|       |-- services
|       `-- utils
|-- frontend
|   |-- package.json
|   |-- public
|   `-- src
|       |-- api
|       |-- components
|       |-- context
|       |-- hooks
|       |-- layouts
|       |-- pages
|       `-- utils
|-- package.json
`-- README.md
```

## Features

- Premium responsive storefront with warm, natural styling
- Home page with offer banner, featured products, testimonials, owner profile, and embedded map
- Product listing with filters, debounce search, sorting, and pagination
- Product detail pages with reviews and ratings
- JWT-based register/login flow
- Cart and wishlist persistence in local storage
- Checkout with address collection, coupon support, Razorpay, and COD
- Order placement, tracking, summary, and customer order history
- Admin dashboard with stats, product management, order updates, reviews, and shop info editing
- Seed script with sample users, products, reviews, coupons, and shop data
- Local image upload support for products and shop profile photo

## Environment Variables

Create these files before running the project:

### Backend

Copy [backend/.env.example](/C:/Users/irbaz/Documents/Codex/2026-04-18-create-a-complete-full-stack-e/backend/.env.example) to `backend/.env`

Required values:

- `MONGODB_URI`
- `JWT_SECRET`
- `RAZORPAY_KEY_ID`
- `RAZORPAY_KEY_SECRET`
- `GOOGLE_MAPS_EMBED_URL`

Optional email values for order confirmation:

- `EMAIL_HOST`
- `EMAIL_PORT`
- `EMAIL_USER`
- `EMAIL_PASS`
- `EMAIL_FROM`

### Frontend

Copy [frontend/.env.example](/C:/Users/irbaz/Documents/Codex/2026-04-18-create-a-complete-full-stack-e/frontend/.env.example) to `frontend/.env`

- `VITE_API_URL=http://localhost:5000/api`
- `VITE_RAZORPAY_KEY_ID=your_key_id`

## Getting Started

### 1. Install dependencies

```bash
npm run install:all
```

### 2. Seed the database

```bash
npm run seed
```

### 3. Start the backend

```bash
npm run dev:backend
```

### 4. Start the frontend

In a second terminal:

```bash
npm run dev:frontend
```

Frontend URL: `http://localhost:5173`  
Backend URL: `http://localhost:5000`

## Seed Credentials

- Admin: `admin@nrdryfruit.com` / `Admin@123`
- Customer: `aisha@example.com` / `Customer@123`

## API Overview

### Auth

- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/me`
- `PUT /api/auth/me`

### Products

- `GET /api/products`
- `GET /api/products/featured`
- `GET /api/products/:id`
- `POST /api/products` `admin`
- `PUT /api/products/:id` `admin`
- `DELETE /api/products/:id` `admin`

### Reviews

- `GET /api/reviews/:productId`
- `POST /api/reviews/:productId`
- `GET /api/reviews` `admin`

### Orders and Payments

- `POST /api/orders`
- `GET /api/orders/mine`
- `GET /api/orders/:id`
- `GET /api/orders` `admin`
- `PATCH /api/orders/:id/status` `admin`
- `POST /api/payments/create-order`
- `POST /api/payments/verify`

### Shop and Admin

- `GET /api/shop`
- `PUT /api/shop` `admin`
- `GET /api/admin/stats` `admin`
- `POST /api/coupons/validate`

## Notes for Production

- Store uploaded files in Cloudinary or S3 instead of local disk for multi-instance deployments.
- Replace the Google Maps embed URL with your real store location link.
- Configure Razorpay webhook handling if you want server-side payment event reconciliation.
- Add an SMS provider such as Twilio or MSG91 if SMS confirmation is required.
- Set `CLIENT_URL` and CORS settings to your deployed frontend domain.
- Put the frontend behind a CDN and run the backend behind a process manager such as PM2.

## Suggested Next Improvements

- Add refresh tokens and secure httpOnly cookie auth
- Add inventory alerts and low-stock dashboards
- Add webhook-driven payment status sync
- Add image hosting adapter abstraction for Cloudinary
