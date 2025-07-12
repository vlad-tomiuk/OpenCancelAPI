# OpenCancel API

> A modern open standard for subscription cancellation.
> Simple. Transparent. User-first.

## 🌍 Why OpenCancel?

The internet is full of apps and services — subscribing is easy.
But canceling subscriptions? Often confusing, hidden, or intentionally difficult.

**OpenCancel API** is a universal standard that solves this.
It allows users (and tools they trust) to send a unified unsubscribe request directly to a service.

## 📊 How It Works

Every service that supports OpenCancel must implement:

* A `.well-known/opencancel` discovery file (public JSON metadata)
* A standard API endpoint for cancellation:

  ```
  POST /api/openCancel/unsubscribe
  ```
* The endpoint must accept a JSON body with required fields:

  ```json
  {
    "user_id": "abc123",
    "subscription_id": "netflix-premium",
    "access_token": "Bearer xyz456"
  }
  ```
* The response must confirm cancellation:

  ```json
  {
    "status": "cancelled",
    "cancelled_at": "2025-07-12T18:05:00Z"
  }
  ```

## 📁 Repository Structure

```
OpenCancelAPI/
├── README.md                    ← This file
├── LICENSE                      ← Apache 2.0 (recommended)
├── openapi.yaml                 ← OpenAPI spec for the unsubscribe endpoint
├── .well-known-template/
│   └── opencancel.json          ← JSON template for providers
├── directory/
│   └── services.json            ← List of supported apps
├── docs/
│   ├── how-to-integrate.md      ← Integration guide for developers
│   ├── how-to-discover.md       ← How discovery and scanning works
│   └── api-reference.md         ← Formal API description
├── examples/
│   ├── cancel-request.http      ← Example unsubscribe request
│   └── fastapi-server/          ← Minimal working example
├── .gitignore                   ← Common ignored files (node_modules, .env, etc)
```

## 🔧 For Developers

To support OpenCancel in your app:

1. Create an endpoint: `POST /api/openCancel/unsubscribe`
2. Publish a discovery file at: `https://yourapp.com/.well-known/opencancel`
3. Add your service to `directory/services.json`
4. Submit a Pull Request to join the public directory

## 🤝 Join the Movement

We believe every user deserves full control over their subscriptions.
Let’s make unsubscribe as easy as subscribe.

**License:** Apache 2.0
**Website:** *(coming soon)*
