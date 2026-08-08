# food-donation-platform-node-express-mongodb 🍲

A full-stack web application that connects people who have surplus food to donate with people who need it — built on Node.js, Express, EJS, and MongoDB, with JWT-based authentication and image uploads for donation listings.

## Table of Contents
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Tools and Technologies](#tools-and-technologies)
- [Methods](#methods)
- [Key Insights](#key-insights)
- [Dashboard/Model/Output](#dashboardmodeloutput)
- [How to Run this Project?](#how-to-run-this-project)
- [Results & Conclusion](#results--conclusion)
- [Future Work](#future-work)
- [Author & Contact](#author--contact)

## Overview

Food Donation Platform is a server-rendered web app where registered users can list food they want to donate — with photos, food type, and pickup address — and other users can browse and claim those listings. Authentication is handled with JWT stored in HTTP-only cookies, and a guest view lets anyone browse registered users without logging in.

## Problem Statement

Surplus food from households, events, and small businesses often goes to waste simply because there's no easy way to connect the person who has it with someone nearby who needs it. This project builds a lightweight platform to close that gap — letting a donor list food in minutes and a receiver find and claim it just as fast.

## Dataset

Instead of a static dataset, the app is backed by a live MongoDB database with three core collections:

- **Users** — name, username, email, phone, password, auth tokens, and a list of their donations
- **Donations** — donor name, email, phone, food type, food name, address, image, and status (e.g., "click to need")
- **Fruit** *(early/sample schema used during development)*

## Tools and Technologies

- **Node.js & Express** — server and routing
- **EJS** — server-side templating for views (home, login, signup, donate-food, receive-food, guest, profile)
- **MongoDB & Mongoose** — database and object modeling
- **JSON Web Tokens (jsonwebtoken)** — stateless authentication
- **cookie-parser** — reading JWTs from HTTP-only cookies
- **Multer** — handling image uploads for donation listings
- **dotenv** — environment variable management
- **body-parser** — parsing form submissions

## Methods

1. **User registration & login** — new users sign up with name, username, email, phone, and password; on login, a JWT is generated and stored in an HTTP-only cookie for session persistence
2. **Auth middleware** — protected routes (`/donate-food`, `/recive-food`, `/logout`) verify the JWT cookie before granting access
3. **Donation creation** — authenticated users submit a form (with an image upload via Multer) describing the food, quantity/type, and pickup address; the record is saved to MongoDB and linked to the donor's user profile
4. **Browsing donations** — the home page and "receive food" page query and render all current donations from the database
5. **Guest directory** — a public route lists all registered users without requiring login
6. **Logout** — invalidates the current session token and clears the auth cookie

## Key Insights

- The auth flow is fully token-based (JWT + HTTP-only cookies) rather than server-side sessions, making it straightforward to scale horizontally later
- Donations are tied directly to the donor's user document (via an embedded array) in addition to their own collection, which keeps a donor's history queryable from their profile
- Image upload is handled per-listing, so each donation is visually verifiable rather than being a bare text entry

## Dashboard/Model/Output

The app exposes the following pages:
- **Home** — lists all current food donations
- **Login / Signup** — authentication entry points
- **Donate Food** — form to create a new donation listing (with image upload)
- **Receive Food** — browse all available donations
- **Profile** — logged-in user's details and donation history
- **Guest** — public directory of all registered users

## How to Run this Project?

**Prerequisites:**
- Node.js and npm
- A MongoDB connection string (MongoDB Atlas or local)

**Steps:**

```bash
# 1. Clone the repository
git clone https://github.com/kaushik-path/food-donation-platform-node-express-mongodb.git
cd food-donation-platform-node-express-mongodb

# 2. Install dependencies
npm install

# 3. Create a .env file in the root directory with:
PORT=3000
SECRET_KEY=your_jwt_secret_here
MONGO_URI=your_mongodb_connection_string_here

# 4. Start the server
npm start
```

Then open `http://localhost:3000` in your browser.

> **Security note:** Make sure your MongoDB connection string and JWT secret are loaded from environment variables (via `.env`, which should be in `.gitignore`) and never committed to the repo in plain text. Passwords should also be hashed (e.g., with bcrypt) before being stored — the current implementation stores them as plain text, which is fine for local learning but should be fixed before any real deployment.

## Results & Conclusion

The project delivers a working end-to-end flow — from user registration and authentication through to listing, browsing, and claiming food donations — using a standard Node/Express/MongoDB stack. It demonstrates practical experience with server-side rendering, JWT authentication, file uploads, and MongoDB schema design in a real, multi-user application.

## Future Work

- Hash passwords with bcrypt instead of storing them in plain text
- Move all secrets (DB URI, JWT secret) fully into environment variables, with a `.env.example` for setup guidance
- Add status tracking so a donation is marked "claimed" once a receiver accepts it, instead of remaining open indefinitely
- Add form validation (client- and server-side) for signup, login, and donation submissions
- Add a proper admin view to moderate listings and manage users
- Responsive/mobile-friendly UI polish

## Author & Contact

**Kaushik Pathak**
📧 Reach out via [LinkedIn](https://www.linkedin.com/in/kaushikpath/) | [GitHub](https://github.com/kaushik-path)

*Feel free to open an issue or reach out for questions, feedback, or collaboration.*
