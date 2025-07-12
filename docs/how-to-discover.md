# 🧭 How to Discover OpenCancel Support

This guide explains how clients (apps, browsers, aggregators) can discover whether a service supports **OpenCancel API**, using the standardized `.well-known/opencancel.json` endpoint.

---

## ✅ What is `.well-known/opencancel.json`?

`.well-known/opencancel.json` is a **machine-readable manifest** that signals a service or platform supports the **OpenCancel API protocol** — a standard for subscription cancellation, status checking, and subscription management.

It lives under the [IETF well-known URI path](https://datatracker.ietf.org/doc/html/rfc8615), like this:

```
https://www.mycompany.com/.well-known/opencancel.json
```

---

## 🎯 Why it's useful

| Benefit                 | Description                                                                                    |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| 🔍 Discoverability      | Lets clients, browsers, or subscription managers **detect cancellation support automatically** |
| 🔐 Transparency         | Shows if API-based cancellation is available (`cancel_api`, `status`, `subscriptions`)         |
| ⚡ User empowerment      | Enables 1-click cancellation or management from third-party tools                              |
| 🌍 Regulation readiness | Supports compliance with laws like DMA (EU), CCPA (US), or Open Access policies                |

---

## 🗂 Example Manifest

```json
{
	"opencancel": {
		"version": "1.0",
		"provider": {
			"name": "MyCompanyName",
			"website": "https://www.mycompany.com",
			"terms": "https://www.mycompany.com/legal/terms",
			"privacy": "https://www.mycompany.com/legal/privacy"
		},
		"actions": {
			"url": {
				"subscriptions": "https://www.mycompany.com/account/subscriptions",
				"cancel": "https://www.mycompany.com/account/cancel",
				"activate": "https://www.mycompany.com/account/activate"
			},
			"api": {
				"subscriptions": {
					"url": "https://api.mycompany.com/opencancel/subscriptions",
					"method": "GET",
					"auth_required": true,
					"response_type": "application/json"
				},
				"cancel": {
					"url": "https://api.mycompany.com/opencancel/cancel",
					"method": "POST",
					"auth_required": true,
					"response_type": "application/json"
				},
				"status": {
					"url": "https://api.mycompany.com/opencancel/status",
					"method": "GET",
					"auth_required": true,
					"response_type": "application/json"
				},
				"activate": {
					"url": "https://api.mycompany.com/opencancel/activate",
					"method": "POST",
					"auth_required": true,
					"response_type": "application/json"
				}
			}
		},
		"metadata": {
			"added_at": "2025-07-13T10:00:00Z",
			"updated_at": "2025-07-13T10:00:00Z"
		}
	}
}
```

---

## 🔄 How Discovery Works

1. **Client loads base domain** (e.g., `https://www.mycompany.com`)
2. It tries fetching:

   ```
   GET /.well-known/opencancel.json
   ```
3. If the file exists and is valid:

   * The client knows OpenCancel is supported.
   * It reads the `actions` object to find URLs and API endpoints.
   * It can offer instant cancel buttons, show subscription info, or automate status checks.

---

## 📦 Response Handling

If the file does **not exist** or returns an HTTP 404:

* The client should assume **OpenCancel is not supported**.

If the file exists but is **malformed** or missing required fields:

* The client should fallback to manual management (UI only).

---

## ✅ Tips for Implementers

* Always host `.well-known/opencancel.json` under **HTTPS**
* Make sure it is publicly accessible (no auth required to fetch it)
* Keep fields like `cancel_url` and `api.cancel` up-to-date
* Include `version` to ensure compatibility with future changes

---

## 📣 Want to support OpenCancel?

* Build your own projects with the same subscription structure — let’s kickstart a new open standard together.
* If you're an engineer, architect, or just someone with a vision — I'm happy to hear your ideas and improvements.
* Already improved the structure? Join the GitHub, open a pull request, and let’s build it together!

---

> © 2025 OpenCancel Initiative – open standard for subscription transparency