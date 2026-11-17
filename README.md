# Buy-Sell IIITH

A marketplace where IIIT Hyderabad students buy and sell items with each other. Students sign in with the institute's CAS single sign-on, list items, add them to a cart, check out, and confirm the hand-over with a one-time code.

## Features

- **Sign-in:** IIIT CAS single sign-on, plus email and password. Sessions use a JWT in an HTTP cookie, with bcrypt-hashed passwords and server-side sessions stored in MongoDB.
- **Listings:** create, edit and delete items, with name, price, description and category.
- **Browsing:** search by name or description, filter by category and price range, and sort.
- **Cart and checkout:** add and remove items, check out a single item or the whole cart. Each purchase creates one order per seller.
- **Orders:** separate views for orders you placed, items you sold, and cancelled orders. Orders can be completed or cancelled.
- **Delivery confirmation:** every order gets a 6-digit OTP. The buyer gives it to the seller at hand-over, and the seller enters it to complete the order. The server checks it against a bcrypt hash.

## Tech stack

- **Frontend:** React 19, Vite, React Router, Redux Toolkit, Tailwind CSS, Radix UI
- **Backend:** Node.js, Express, MongoDB with Mongoose, express-session with connect-mongo, `cas-authentication`

## Running locally

Requires Node.js 18+ and a MongoDB instance (local or Atlas).

```bash
# 1. Environment variables: copy the values listed in .env.example
#    into backend/.env and frontend/.env

# 2. Backend (http://localhost:8000)
cd backend
npm install
npm run dev

# 3. Frontend, in a second terminal (http://localhost:5173)
cd frontend
npm install
npm run dev
```

CAS login only works with a callback URL registered with the IIIT CAS server. Email and password login works anywhere.

## API overview

All routes are under `/api/v1`.

| Area | Routes |
|---|---|
| User | `POST /user/register`, `POST /user/login`, `GET /user/logout`, `GET /user/profile`, `PUT /user/update`, `GET /user/cas-login` |
| Items | `GET /item`, `GET /item/categories`, `GET /item/:id`, `POST /item/create`, `PUT /item/update/:id`, `DELETE /item/delete/:id` |
| Cart | `GET /order/cart`, `POST /order/cart/add`, `POST /order/cart/remove` |
| Orders | `POST /order/checkout`, `POST /order/checkoutallItems`, `GET /order/placed/:userId`, `GET /order/sold/:userId`, `GET /order/cancelled/:userId`, `PATCH /order/complete/:transactionId`, `PATCH /order/cancel/:transactionId`, `POST /order/verify-delivery` |

## Known limitations

- A plaintext copy of each OTP is stored on the order so the buyer's Orders page can show it. A production version would show the code once and keep only the hash.
- Some order-listing routes do not yet check that the requester is the user named in the URL.
