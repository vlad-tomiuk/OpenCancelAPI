# OpenCancel API

> A modern open standard for subscription cancellation.  
> Simple. Transparent. User-first.

## 🌍 Why OpenCancel?

The internet is full of apps and services — subscribing is easy.  
But canceling subscriptions? Often confusing, hidden, or intentionally difficult.

**OpenCancel API** is a universal standard that solves this.  
It allows users (and tools they trust) to send a unified unsubscribe request directly to a service.

## 📐 How It Works

Every service that supports OpenCancel must implement:

- A `.well-known/opencancel` discovery file (public JSON metadata)
- A standard API endpoint for cancellation:
