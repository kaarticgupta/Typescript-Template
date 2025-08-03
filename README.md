# 🚀 TypeScript Backend Project Template

A modern, scalable, and production-ready **TypeScript-based backend API template** designed for rapid development and maintainability. Includes robust error handling, layered architecture, Prisma ORM, middleware utilities, and token-based authentication.

---

## 📁 Built With

- **Node.js**
- **TypeScript**
- **Express**
- **Prisma ORM**
- **PostgreSQL**
- **Joi** (for validation)
- **JWT** (for authentication)

---

## 🧭 Table of Contents

- [🔧 Installation & Usage](#-installation--usage)
- [🧱 Project Structure](#-project-structure)
- [🚨 Error Handling & Logging](#-error-handling--logging)
- [🧩 Middlewares](#-middlewares)
- [📡 API Reference](#-api-reference)
- [📘 Models](#-models)
- [🔐 Authentication](#-authentication)

---

## 🔧 Installation & Usage

### 🛠 Prerequisites

- Node.js `^18.12.1`
- npm `^8.19.2` or yarn `^1.22.19`
- PostgreSQL `^12.11`

### 🚀 Getting Started

#### Option 1: Use GitHub Template

Click the **"Use this template"** button on GitHub.

#### Option 2: Manual Setup

```bash
wget https://github.com/NivaldoFarias/typescript-project-template/archive/main.zip
unzip main.zip
cd typescript-project-template-main
```

### Install Dependencies

```bash
# Using npm
npm install

# Or using yarn
yarn install
```

### Start Development Server

```bash
# Using npm
npm run dev

# Or using yarn
yarn dev
```

> 🔄 Don't forget to update your `package.json` and `.env` with your project's information.

---

## 🧱 Project Structure

```
typescript/
├── controllers/
├── middlewares/
├── models/
├── repositories/
├── routes/
├── services/
├── utils/
├── events/
│   ├── AppError.ts
│   └── AppLog.ts
└── index.ts
```

---

## 🚨 Error Handling & Logging

### AppError

A custom error handler class designed to standardize error responses.

```ts
new AppError(
  "User not found", // log message (server-side)
  404, // status code
  "User not found", // client-facing message
  "Ensure to provide a valid user ID." // details
);
```

### AppLog

A logging utility that categorizes logs using color-coded prefixes for easy debugging:

| Method       | Color   | Prefix         |
| ------------ | ------- | -------------- |
| `controller` | Green   | `[Controller]` |
| `repository` | Blue    | `[Repository]` |
| `middleware` | Magenta | `[Middleware]` |
| `service`    | Cyan    | `[Service]`    |
| `error`      | Red     | `[Error]`      |
| `server`     | Yellow  | `[Server]`     |
| `utils`      | Cyan    | `[Utils]`      |

```ts
AppLog.middleware("Password is valid");
AppLog.controller("User created successfully");
```

---

## 🧩 Middlewares

The project supports modular middleware through the `useMiddleware` utility.

### useMiddleware Options

```ts
useMiddleware(
  {
    schema, // Joi schema
    header, // e.g. 'admin-api-key'
    token, // boolean - validate JWT
  },
  endpoint
);
```

### Built-in Middlewares

- `validateSchema()` – Joi schema validation
- `processHeader()` – Custom header validation
- `requireToken()` – JWT verification

```ts
adminRouter.post(
  "/admin/create",
  useMiddleware(
    {
      schema: schema.create,
      header: "admin-api-key",
      token: true,
    },
    "/admin/create"
  ),
  middleware.createValidations,
  controller.create
);
```

---

## 📡 API Reference

### POST `/auth/register`

Registers a new user.

**Request Body:**

```json
{
  "username": "johndoe",
  "email": "john_doe@gmail.com",
  "password": "123456789"
}
```

**Responses:**

- `201 Created`: `{ data: {} }`
- `409 Conflict`: `{ error: { message, details } }`
- `422 Unprocessable Entity`
- `500 Internal Server Error`

---

### POST `/auth/sign-in`

Logs in a user and returns a JWT token.

**Request Body:**

```json
{
  "email": "john_doe@gmail.com",
  "password": "123456789"
}
```

**Responses:**

- `200 OK`: `{ data: { token } }`
- `403 Forbidden`: Invalid password
- `404 Not Found`: User not found
- `422 Unprocessable Entity`
- `500 Internal Server Error`

---

## 📘 Models

### User

| Field        | Type        | Description                |
| ------------ | ----------- | -------------------------- |
| `id`         | `serial4`   | Unique user identifier     |
| `username`   | `text`      | Username                   |
| `email`      | `text`      | Must be unique             |
| `password`   | `text`      | Hashed password            |
| `created_at` | `timestamp` | Account creation timestamp |

---

## 🔐 Authentication

- Token-based using **JWT**
- Tokens verified via `requireToken()` middleware
- Secure endpoints with headers and token validation

---

## 🧠 Final Notes

- You can freely extend the project to include more modules, routes, or services.
- The `AppError` and `AppLog` structures are reusable in any project.
- Follow the modular architecture for clean, maintainable code.

---

## 💡 License

MIT — feel free to use and modify for your own purposes.
