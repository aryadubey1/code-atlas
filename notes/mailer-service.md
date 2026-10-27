# ✉️ mailer-service

> Demo notes. Everything here is fictional.

## What it does

Renders and sends every shopper email for the main app.

```
northwind-web ──event──► mailer-service ──► email provider
```

## How to run it locally

```
bin/setup
bin/dev   # http://localhost:4000/previews
```

## 🧠 Things I asked about

- **Event**: the app says "this happened"; the mailer decides what to send.
