# Aria Fashion API — Developer Setup Guide

This guide walks you through setting up and running Aria locally on your machine. By the end, you will have a fully functional instance of Aria running at `http://localhost:3000`.

**Who this guide is for:** Developers who want to run, explore, or contribute to the Aria codebase.

**Time to complete:** approximately 15 minutes.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Quick Start](#quick-start)
3. [Environment Configuration](#environment-configuration)
4. [Project Structure](#project-structure)
5. [Running Locally](#running-locally)
6. [API Keys Setup](#api-keys-setup)
7. [Troubleshooting](#troubleshooting)
8. [Deployment](#deployment)

---

## Prerequisites

Before you begin, make sure you have the following installed:

| Tool | Version | Purpose |
|------|---------|---------|
| Node.js | v18 or higher | Runtime environment |
| npm | v9 or higher | Package manager |
| Git | Any recent version | Clone the repository |

You will also need free accounts on the following platforms to obtain API keys:

- [Mistral AI](https://console.mistral.ai) — conversational AI
- [Groq](https://console.groq.com) — vision AI
- [MongoDB Atlas](https://mongodb.com/atlas) — database

To verify your Node.js and npm versions, run:

```bash
node --version
npm --version
```

---

## Quick Start

If you want to get up and running as fast as possible, follow these four steps:

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/aria-fashion-stylist.git
cd aria-fashion-stylist

# 2. Install dependencies
npm install

# 3. Create your environment file
cp .env.example .env
# Open .env and add your API keys

# 4. Start the server
node server.js
```

Then open `http://localhost:3000` in your browser. You should see the Aria landing page.

> **Note:** You must add valid API keys to your `.env` file before the app will function correctly. See [Environment Configuration](#environment-configuration) for details.

---

## Environment Configuration

Aria uses a `.env` file to manage all sensitive credentials. Create a file named `.env` in the root of the project with the following variables:

```
MISTRAL_API_KEY=your_mistral_api_key_here
GROQ_API_KEY=your_groq_api_key_here
JWT_SECRET=your_jwt_secret_here
MONGODB_URI=your_mongodb_connection_string_here
```

### Variable Reference

| Variable | Required | Description |
|----------|----------|-------------|
| `MISTRAL_API_KEY` | Yes | API key for Mistral AI. Used for all chat responses. |
| `GROQ_API_KEY` | Yes | API key for Groq. Used for vision/image analysis. |
| `JWT_SECRET` | Yes | A secret string used to sign and verify JWT tokens. Can be any long random string. |
| `MONGODB_URI` | Yes | MongoDB Atlas connection string. Includes username, password, and cluster URL. |

> **Security:** Never commit your `.env` file to version control. The `.gitignore` file in this project already excludes it.

---

## Project Structure

```
aria-fashion-stylist/
├── public/                  # Frontend files served statically
│   ├── index.html           # Landing page
│   ├── login.html           # Login and signup page
│   ├── onboarding.html      # Style profile setup
│   ├── chat.html            # Main chat interface
│   ├── profile.html         # Edit profile page
│   ├── app.js               # Frontend JavaScript
│   └── style.css            # Global styles
├── server.js                # Express server and all API routes
├── package.json             # Project metadata and dependencies
├── .env                     # Environment variables (not committed)
└── .gitignore               # Files excluded from version control
```

### Key files explained

**`server.js`** is the heart of the application. It contains:
- Express server setup and middleware configuration
- MongoDB connection and Mongoose schema definitions
- All API route handlers
- JWT authentication middleware
- Groq Vision API integration
- Mistral AI chat integration

**`public/app.js`** handles all frontend logic including:
- Loading chat history and profile data on page load
- Sending messages and displaying responses
- Image upload and preview handling
- Theme toggling and menu interactions

**`public/style.css`** uses CSS custom properties (variables) for theming, making it easy to switch between light and dark mode and customize the color palette.

---

## Running Locally

### Step 1 — Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/aria-fashion-stylist.git
cd aria-fashion-stylist
```

### Step 2 — Install dependencies

```bash
npm install
```

This installs all packages listed in `package.json` including Express, Mongoose, Multer, Jimp, Mistral AI SDK, bcryptjs, and jsonwebtoken.

### Step 3 — Configure environment variables

Create your `.env` file and add your API keys as described in [Environment Configuration](#environment-configuration).

### Step 4 — Start the server

```bash
node server.js
```

You should see the following output in your terminal:

```
Fashion AI server running on port 3000
Connected to MongoDB
```

If you see `MongoDB connection error` instead, double-check your `MONGODB_URI` in the `.env` file.

### Step 5 — Open the app

Open your browser and navigate to:

```
http://localhost:3000
```

You should see the Aria landing page. Click **Get Started** to create an account and complete your style profile.

![Aria landing page](./screenshots/landing-page.jpg)

---

## API Keys Setup

### Mistral AI

1. Go to [console.mistral.ai](https://console.mistral.ai)
2. Create a free account
3. Navigate to **API Keys** in the sidebar
4. Click **Create new key**
5. Copy the key and paste it as `MISTRAL_API_KEY` in your `.env` file

Aria uses the `mistral-small-latest` model for all chat responses.

### Groq

1. Go to [console.groq.com](https://console.groq.com)
2. Create a free account
3. Navigate to **API Keys**
4. Click **Create API Key**
5. Copy the key and paste it as `GROQ_API_KEY` in your `.env` file

Aria uses the `meta-llama/llama-4-maverick-17b-128e-instruct` model for image analysis. The free tier allows up to 7,000 requests per day.

### MongoDB Atlas

1. Go to [mongodb.com/atlas](https://mongodb.com/atlas)
2. Create a free account and select the **M0 free tier**
3. Under **Database Access**, create a new user with **Read and Write** permissions
4. Under **Network Access**, add `0.0.0.0/0` to allow connections from anywhere
5. Click **Connect** on your cluster, choose **Connect your application**, and copy the connection string
6. Replace `<password>` in the connection string with your database user's password
7. Paste the full string as `MONGODB_URI` in your `.env` file, appending `/aria-fashion` as the database name:

```
MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/aria-fashion
```

### JWT Secret

The `JWT_SECRET` can be any long random string. It is used to sign authentication tokens. You can generate one by running:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Copy the output and paste it as your `JWT_SECRET`.

---

## Troubleshooting

### Server fails to start

**Error:** `Cannot find module 'express'`

**Fix:** Run `npm install` in the project root. Dependencies are missing.

---

### MongoDB connection fails

**Error:** `MongoServerError: bad auth : authentication failed`

**Fix:** Your `MONGODB_URI` password is incorrect or contains special characters that need URL encoding. Reset your MongoDB Atlas database user password to use only letters and numbers, then update your `.env` file.

---

### Groq vision times out

**Error:** `Vision error: This operation was aborted`

**Fix:** The image being uploaded may be too large or the Groq API is under load. The app will continue without the analysis — the profile will still be saved. Try again with a smaller image (under 2MB recommended).

---

### Chat returns no response

**Error:** `Could not reach the server. Is it running?`

**Fix:** Make sure `node server.js` is running in your terminal. If the server is running, check that your `MISTRAL_API_KEY` is valid and has not exceeded its rate limit.

---

### Port already in use

**Error:** `Error: listen EADDRINUSE: address already in use :::3000`

**Fix:** Another process is using port 3000. Either stop that process or change the port in `server.js`:

```javascript
const PORT = process.env.PORT || 3001;
```

---

## Deployment

Aria is configured for deployment on [Railway](https://railway.app). Follow these steps to deploy your own instance.

### Step 1 — Push to GitHub

Make sure your code is pushed to a public or private GitHub repository. Do not commit your `.env` file.

### Step 2 — Create a Railway project

1. Go to [railway.app](https://railway.app) and sign in with GitHub
2. Click **New Project** → **Deploy from GitHub repo**
3. Select your `aria-fashion-stylist` repository
4. Railway will automatically detect it as a Node.js project

### Step 3 — Add environment variables

In your Railway project:

1. Click on your service
2. Go to the **Variables** tab
3. Add each variable from your `.env` file:

```
MISTRAL_API_KEY
GROQ_API_KEY
JWT_SECRET
MONGODB_URI
```

### Step 4 — Generate a public domain

1. Go to the **Settings** tab of your service
2. Scroll to **Networking**
3. Click **Generate Domain**

Railway will provide a URL like `your-app.up.railway.app`. Your app is now live.

> **Note:** Railway's free tier does not provide persistent file storage. This is why Aria uses MongoDB Atlas for all data persistence rather than local JSON files.

---

## Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| express | ^5.2.1 | Web framework |
| mongoose | ^8.x | MongoDB ODM |
| @mistralai/mistralai | ^1.14.0 | Mistral AI SDK |
| multer | ^2.1.0 | File upload handling |
| jimp | ^1.6.0 | Image compression |
| bcryptjs | ^3.0.3 | Password hashing |
| jsonwebtoken | ^9.0.3 | JWT authentication |
| dotenv | ^17.3.1 | Environment variable loading |
| cors | ^2.8.6 | Cross-origin resource sharing |

---

*For API endpoint documentation, see the [Aria Fashion API Reference](https://shruti-rajpurohit.github.io/Technical-writing-portfolio/docs/aria-fashion-api/index.html).*
