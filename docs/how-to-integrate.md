# 📦 How to Integrate OpenCancel API

OpenCancel is a lightweight, open standard that makes **subscription cancellation simple, transparent, and programmable**.

Follow these two steps to integrate:

---

## ✅ Step 1: Create `.well-known/opencancel.json`

Each service must expose a discovery file at:

```
https://yourdomain.com/.well-known/opencancel
```

### 📁 Setup

1. Create a folder in your public root:

   ```
   .well-known
   ```
2. Inside it, create the file:

   ```
   opencancel.json
   ```
3. Fill it using this structure:

```json
{
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
			"cancel": "https://www.mycompany.com/account/cancel"
		},
		"api": {
			"subscriptions": {
				"url": "https://api.mycompany.com/opencancel/subscriptions",
				"method": "GET",
				"auth_required": true,
				"response_type": "application/json"
			},
			"activate": {
				"url": "https://api.mycompany.com/opencancel/activate",
				"method": "POST",
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
			}
		}
	},
	"metadata": {
		"discovery": "https://www.mycompany.com/.well-known/opencancel",
		"updated_at": "2025-07-13T00:00:00Z"
	}
}
```

---

## ✅ Step 2: Implement the Required API Endpoints

All endpoints must return JSON and follow a consistent, extendable response structure.

### ✅ Common Response Envelope

```json
{
	"status": "success",
	"schema_version": "1.0",
	"data": {
		...
	}
}
```

### ❌ Common Error Format

```json
{
	"status": "error",
	"error": {
		"http_status": 404,
		"code": "subscription_not_found",
		"message": "The requested subscription could not be found.",
		"request_id": "req_8a9e7c",
		"details": {
			"subscription_id": "sub_xyz"
		},
		"timestamp": "2025-07-12T21:30:00Z"
	}
}
```

---

## 🔍 1. Get All Active Subscriptions

```
GET /opencancel/subscriptions
Authorization: Bearer <token>
```

```json
{
	"status": "success",
	"schema_version": "1.0",
	"data": {
		"subscriptions": [
			{
				"id": "sub_123",
				"status": "active",
				"plan": {
					"name": "Premium",
					"description": "Full access to premium features"
				},
				"state": {
					"is_active": true,
					"is_cancelled": false,
					"is_expired": false
				},
				"lifecycle": {
					"activated_at": "2025-01-01T00:00:00Z",
					"current_period": {
						"start": "2025-07-01T00:00:00Z",
						"end": "2025-08-01T00:00:00Z"
					}
				},
				"billing": {
					"cycle": "monthly",
					"auto_renew": true,
					"next_payment": "2025-08-01T00:00:00Z"
				},
				"meta": {
					"created_by": "user",
					"last_updated": "2025-07-01T12:00:00Z"
				}
			}
		]
	}
}
```

---

## ✅ 2. Activate Subscription

```
POST /opencancel/activate
Authorization: Bearer <token>
Content-Type: application/json
```

### Request

```json
{
	"subscription_id": "sub_123"
}
```

### Response

Same response envelope with updated `status = "active"` and populated `lifecycle.reactivated_at`.

---

## ❌ 3. Cancel Subscription

```
POST /opencancel/cancel
Authorization: Bearer <token>
Content-Type: application/json
```

### Request

```json
{
	"subscription_id": "sub_123",
	"reason": "No longer needed"
}
```

### Response

Same base structure with:

* `status = "cancelled"`
* `billing.auto_renew = false`
* `lifecycle.cancelled_at` populated

---

## 📊 4. Get Subscription Status

```
GET /opencancel/status?subscription_id=sub_123
Authorization: Bearer <token>
```

### Response

```json
{
	"status": "success",
	"schema_version": "1.0",
	"data": {
		"subscription": {
			"id": "sub_123",
			"status": "cancelled",
			"state": {
				"is_active": true,
				"is_cancelled": true,
				"is_expired": false
			},
			"plan": {
				"name": "Premium",
				"description": "Full access to premium features"
			},
			"lifecycle": {
				"activated_at": "2025-01-01T00:00:00Z",
				"cancelled_at": "2025-07-14T09:30:00Z",
				"current_period": {
					"start": "2025-07-01T00:00:00Z",
					"end": "2025-08-01T00:00:00Z"
				}
			},
			"billing": {
				"cycle": "monthly",
				"auto_renew": false,
				"next_payment": null
			},
			"meta": {
				"last_updated": "2025-07-14T09:30:00Z",
				"message": "Subscription was cancelled and remains active until the period end."
			}
		}
	}
}
```

---