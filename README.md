# 🔍 AI-Powered Code Review & Security Audit Tool

> A production-ready REST API that uses **Claude AI** to automatically review code quality, detect security vulnerabilities, and provide actionable fix suggestions — built with Node.js, MongoDB, and Docker.

![Node.js](https://img.shields.io/badge/Node.js-20.x-green?logo=node.js)
![Express](https://img.shields.io/badge/Express-4.x-lightgrey?logo=express)
![MongoDB](https://img.shields.io/badge/MongoDB-7.x-green?logo=mongodb)
![Docker](https://img.shields.io/badge/Docker-ready-blue?logo=docker)
![Claude AI](https://img.shields.io/badge/Claude-Sonnet--4--6-orange?logo=anthropic)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📌 Table of Contents

- [🔍 AI-Powered Code Review \& Security Audit Tool](#-ai-powered-code-review--security-audit-tool)
  - [📌 Table of Contents](#-table-of-contents)
  - [🧠 Overview](#-overview)
  - [✨ Features](#-features)
  - [🛠 Tech Stack](#-tech-stack)
  - [🏗 Architecture](#-architecture)
  - [🚀 Getting Started](#-getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
    - [Environment Variables](#environment-variables)
    - [Run with Docker](#run-with-docker)
    - [Run Locally](#run-locally)
  - [📡 API Reference](#-api-reference)
    - [Authentication](#authentication)
      - [Register a new user](#register-a-new-user)
      - [Login](#login)
    - [Code Review](#code-review)
      - [Submit code for AI review](#submit-code-for-ai-review)
      - [Get review history](#get-review-history)
      - [Get a specific review by ID](#get-a-specific-review-by-id)
  - [📁 Project Structure](#-project-structure)
  - [🤖 How Claude AI Works Here](#-how-claude-ai-works-here)
  - [📊 Example Response](#-example-response)
    - [Severity Levels](#severity-levels)
  - [🗺 Roadmap](#-roadmap)
  - [🤝 Contributing](#-contributing)
  - [📄 License](#-license)
  - [🙏 Acknowledgements](#-acknowledgements)

---

## 🧠 Overview

This project solves a real-world problem: **developers and teams waste hours on manual code review and miss critical security vulnerabilities before deployment.**

This tool provides an intelligent REST API where users submit source code and receive instant, structured feedback powered by **Claude Sonnet 4.6** — including:

- Code quality scoring (0–100)
- Security vulnerability detection (SQL injection, XSS, hardcoded secrets, RCE, etc.)
- Line-by-line issue breakdown with severity levels
- Actionable fix suggestions
- Review history stored per user in MongoDB

---

## ✨ Features

- 🤖 **AI-Powered Analysis** — Uses Anthropic's Claude Sonnet 4.6 for deep code understanding
- 🔐 **JWT Authentication** — Secure user registration, login, and protected routes
- 🗄️ **Review History** — Every review is persisted in MongoDB and retrievable per user
- 🐳 **Dockerized** — Run the entire stack (app + MongoDB) with one command
- 🌐 **Multi-Language Support** — JavaScript, TypeScript, Python, Java, Go, and more
- 📊 **Structured JSON Output** — Machine-readable results with severity scores
- ⚡ **Fast & Scalable** — Non-blocking async architecture built for production
- 🔒 **Security First** — Input validation, rate limiting, and environment-based secrets

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js 20 (LTS) |
| Framework | Express.js 4 |
| AI Engine | Anthropic Claude Sonnet 4.6 |
| Database | MongoDB 7 + Mongoose |
| Authentication | JWT (jsonwebtoken) + bcryptjs |
| Containerization | Docker + Docker Compose |
| API Style | RESTful JSON API |
| Environment | dotenv |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────┐
│                   Client / curl                 │
└──────────────────────┬──────────────────────────┘
                       │ HTTP Request
                       ▼
┌─────────────────────────────────────────────────┐
│              Express REST API (Node.js)          │
│                                                  │
│  ┌─────────────┐    ┌──────────────────────────┐ │
│  │  JWT Auth   │───▶│   /api/review (POST)     │ │
│  │ Middleware  │    │   /api/review/history    │ │
│  └─────────────┘    │   /api/auth/register     │ │
│                     │   /api/auth/login        │ │
│                     └──────────┬───────────────┘ │
└────────────────────────────────┼─────────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                   │
              ▼                  ▼                   ▼
┌─────────────────┐  ┌───────────────────┐  ┌──────────────┐
│  claudeService  │  │     MongoDB       │  │  JWT Service │
│  (Anthropic SDK)│  │  (Review History) │  │  (Auth)      │
│  claude-sonnet  │  │  (Users)          │  │              │
└─────────────────┘  └───────────────────┘  └──────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- [Node.js 20+](https://nodejs.org/)
- [Docker & Docker Compose](https://www.docker.com/)
- [Anthropic API Key](https://console.anthropic.com/) ← Sign up and generate one

---

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/ai-code-review.git
cd ai-code-review

# 2. Install dependencies
npm install
```

---

### Environment Variables

Create a `.env` file in the root directory:

```env
# Server
PORT=3000
NODE_ENV=development

# Anthropic Claude API
ANTHROPIC_API_KEY=sk-ant-your-key-here

# MongoDB
MONGODB_URI=mongodb://localhost:27017/codereview

# JWT
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=7d
```

> ⚠️ **Never commit your `.env` file.** It is already in `.gitignore`.

---

### Run with Docker

The easiest way to start the full stack (app + MongoDB):

```bash
# Build and start all services
docker-compose up --build

# Run in background
docker-compose up -d --build

# Stop all services
docker-compose down
```

The API will be available at: `http://localhost:3000`

---

### Run Locally

If you prefer running without Docker (requires MongoDB installed locally):

```bash
# Start the development server with hot-reload
npm run dev

# Start the production server
npm start
```

---

## 📡 API Reference

### Authentication

#### Register a new user

```http
POST /api/auth/register
Content-Type: application/json

{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": { "id": "64f...", "name": "Jane Doe", "email": "jane@example.com" }
}
```

---

#### Login

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "jane@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### Code Review

#### Submit code for AI review

```http
POST /api/review
Authorization: Bearer <your_jwt_token>
Content-Type: application/json

{
  "language": "javascript",
  "code": "const password = 'admin123';\neval(userInput);"
}
```

**Supported languages:** `javascript`, `typescript`, `python`, `java`, `go`, `php`, `ruby`, `rust`, `c`, `cpp`

---

#### Get review history

```http
GET /api/review/history
Authorization: Bearer <your_jwt_token>
```

**Response:**
```json
[
  {
    "_id": "64f...",
    "language": "javascript",
    "score": 12,
    "createdAt": "2026-04-29T10:00:00.000Z"
  }
]
```

---

#### Get a specific review by ID

```http
GET /api/review/:id
Authorization: Bearer <your_jwt_token>
```

---

## 📁 Project Structure

```
ai-code-review/
├── src/
│   ├── config/
│   │   └── db.js                 # MongoDB connection setup
│   ├── models/
│   │   ├── User.js               # User schema (auth)
│   │   └── Review.js             # Review schema (history)
│   ├── routes/
│   │   ├── auth.js               # /api/auth routes
│   │   └── review.js             # /api/review routes
│   ├── services/
│   │   └── claudeService.js      # 🤖 Claude AI integration
│   ├── middleware/
│   │   ├── auth.js               # JWT verification middleware
│   │   └── rateLimiter.js        # Request rate limiting
│   └── app.js                    # Express app entry point
├── Dockerfile
├── docker-compose.yml
├── .env                          # (not committed)
├── .env.example                  # Template for env vars
├── .gitignore
├── package.json
└── README.md
```

---

## 🤖 How Claude AI Works Here

The `claudeService.js` sends your code to Claude with a **carefully engineered system prompt** that instructs it to act as a senior security engineer and return structured JSON:

```js
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY });

export async function reviewCode(code, language = 'javascript') {
  const message = await client.messages.create({
    model: 'claude-sonnet-4-6',
    max_tokens: 2048,
    system: `You are a senior software engineer and security auditor.
    Analyze the provided code and return ONLY a valid JSON object with:
    - "score": quality score out of 100
    - "summary": brief overview
    - "securityRisks": array of security vulnerabilities
    - "issues": array of { type, severity, line, description, fix }`,
    messages: [
      { role: 'user', content: `Review this ${language} code:\n\n${code}` }
    ]
  });

  return JSON.parse(message.content[0].text);
}
```

---

## 📊 Example Response

Submitting this dangerous JavaScript:
```js
const password = "admin123";
eval(userInput);
```

Returns:
```json
{
  "reviewId": "64f3a2b1c9d8e7f6a5b4c3d2",
  "analysis": {
    "score": 12,
    "summary": "Critical security vulnerabilities detected. Code is unsafe for production.",
    "securityRisks": [
      "Hardcoded password exposed in plain text",
      "eval() with user input enables Remote Code Execution (RCE)"
    ],
    "issues": [
      {
        "type": "security",
        "severity": "critical",
        "line": 1,
        "description": "Hardcoded credential in source code. Anyone with repo access can steal this.",
        "fix": "Use environment variables: process.env.PASSWORD"
      },
      {
        "type": "security",
        "severity": "critical",
        "line": 2,
        "description": "eval() executes arbitrary code from user input — a Remote Code Execution vulnerability.",
        "fix": "Never use eval(). Use JSON.parse() or a safe parser instead."
      }
    ]
  }
}
```

### Severity Levels

| Level | Meaning |
|---|---|
| 🔴 `critical` | Immediate security risk, must fix before any deployment |
| 🟠 `high` | Significant issue affecting security or stability |
| 🟡 `medium` | Code quality or minor security concern |
| 🟢 `low` | Best practice suggestion or style improvement |

---

## 🗺 Roadmap

- [x] Core Claude AI code review engine
- [x] JWT authentication (register/login)
- [x] MongoDB review history
- [x] Docker + Docker Compose setup
- [x] Multi-language support
- [ ] Streaming responses (real-time review output)
- [ ] GitHub Actions integration (auto-review on PR)
- [ ] Swagger / OpenAPI documentation
- [ ] Language auto-detection by Claude
- [ ] Dashboard frontend (React)
- [ ] Team/organization accounts
- [ ] Webhook support (notify Slack on critical findings)

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

```bash
# 1. Fork the repo and clone it
git clone https://github.com/your-username/ai-code-review.git

# 2. Create a feature branch
git checkout -b feature/your-feature-name

# 3. Make your changes and commit
git commit -m "feat: add your feature description"

# 4. Push and open a Pull Request
git push origin feature/your-feature-name
```

Please follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Anthropic](https://www.anthropic.com/) — for Claude, the AI that powers this tool
- [MongoDB](https://www.mongodb.com/) — for the flexible document database
- [Express.js](https://expressjs.com/) — for the minimal and fast Node.js framework

---

<p align="center">Built with ❤️ to help developers write safer, better code.</p>