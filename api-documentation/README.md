# PayBridge Africa Payment API

**Version:** 1.0

**Document Type:** REST API Documentation

---

# Introduction

The **PayBridge Africa Payment API** enables developers to integrate secure digital payment capabilities into web, mobile, and enterprise applications.

Using a RESTful architecture, the API allows applications to create payment requests, verify transactions, process refunds, retrieve transaction history, and manage virtual accounts through secure endpoints.

The API is designed to provide reliable, scalable, and secure payment services for businesses operating across Africa.

---

# Intended Audience

This documentation is intended for:

- Backend Developers
- Frontend Developers integrating payment services
- Mobile Application Developers
- QA Engineers
- DevOps Engineers
- Technical Support Engineers

Readers are expected to have a basic understanding of HTTP requests, JSON, and REST APIs.

---

# Base URL

All API requests must be sent to the following base URL:

```text
https://api.paybridge.africa/v1
```

All endpoints referenced in this documentation are relative to the base URL above.

---

# Authentication

The PayBridge Africa Payment API uses **Bearer Token Authentication** to secure all protected endpoints.

Every request must include a valid API key in the `Authorization` header.

## Request Header

```http
Authorization: Bearer YOUR_API_KEY
```

Replace `YOUR_API_KEY` with the API key assigned to your application.

> **Note:** Never expose your API key in frontend code or public repositories. Store it securely using environment variables or a secure secrets manager.
>
> ---

# Create Payment

Creates a new payment request.

## Endpoint

```http
POST /payments
```

## Request Headers

| Header | Required | Description |
|---------|----------|-------------|
| Authorization | Yes | Bearer API Key |
| Content-Type | Yes | application/json |

## Request Body

```json
{
  "amount": 25000,
  "currency": "NGN",
  "customer": {
    "name": "John Doe",
    "email": "john@example.com"
  },
  "reference": "PAY-10001"
}
```

## Response

### Success (201 Created)

```json
{
  "status": "success",
  "message": "Payment created successfully.",
  "data": {
    "payment_id": "pay_89453",
    "checkout_url": "https://paybridge.africa/pay/pay_89453"
  }
}
```

### Error (400 Bad Request)

```json
{
  "status": "error",
  "message": "Invalid request."
}
```

---

# Verify Payment

Checks the status of a payment using its unique transaction ID.

## Endpoint

```http
GET /payments/{payment_id}
```

## Path Parameter

| Parameter | Type | Description |
|-----------|------|-------------|
| payment_id | String | Unique payment identifier returned after payment creation |

## Example Request

```http
GET /payments/pay_89453
```

## Success Response

```json
{
  "status": "success",
  "data": {
    "payment_id": "pay_89453",
    "status": "completed",
    "amount": 25000,
    "currency": "NGN",
    "customer": {
        "name":"John Doe",
        "email":"john@example.com"
    },
    "paid_at":"2026-07-05T13:41:08Z"
  }
}
```

## Error Response

```json
{
    "status":"error",
    "message":"Payment not found."
}
```

---

# Refund Payment

Refunds a previously completed payment.

## Endpoint

```http
POST /payments/{payment_id}/refund
```

## Request Body

```json
{
  "reason":"Customer requested refund"
}
```

## Success Response

```json
{
    "status":"success",
    "message":"Refund initiated successfully."
}
```

## Error Response

```json
{
    "status":"error",
    "message":"Refund cannot be processed."
}
```

---

# Error Codes

| Status Code | Meaning |
|-------------|----------|
| 200 | Request successful |
| 201 | Resource created |
| 400 | Bad request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Resource not found |
| 409 | Duplicate transaction |
| 429 | Rate limit exceeded |
| 500 | Internal server error |

---

# Rate Limiting

To ensure fair usage and maintain platform stability, API requests are subject to rate limits.

| Plan | Requests |
|------|----------|
| Free | 100 requests per minute |
| Standard | 1,000 requests per minute |
| Enterprise | Custom limits |

If the rate limit is exceeded, the API returns:

```http
429 Too Many Requests
```

Response example:

```json
{
    "status":"error",
    "message":"Rate limit exceeded. Please retry later."
}
```

---

# Pagination

Endpoints that return multiple resources support pagination.

Example request:

```http
GET /payments?page=2&limit=20
```

Example response:

```json
{
    "page":2,
    "limit":20,
    "total":180,
    "pages":9,
    "data":[]
}
```

---

# Webhooks

PayBridge Africa can notify your application whenever important payment events occur.

Supported events include:

- payment.created
- payment.completed
- payment.failed
- refund.completed

Example webhook payload:

```json
{
    "event":"payment.completed",
    "payment_id":"pay_89453",
    "amount":25000,
    "currency":"NGN",
    "status":"completed"
}
```

Your server should respond with:

```http
200 OK
```

---

# Webhooks

PayBridge Africa can notify your application whenever important payment events occur.

Supported events include:

- payment.created
- payment.completed
- payment.failed
- refund.completed

Example webhook payload:

```json
{
    "event":"payment.completed",
    "payment_id":"pay_89453",
    "amount":25000,
    "currency":"NGN",
    "status":"completed"
}
```

Your server should respond with:

```http
200 OK
```

---

# Python Example

```python
import requests

url = "https://api.paybridge.africa/v1/payments"

headers = {
    "Authorization": "Bearer YOUR_API_KEY",
    "Content-Type": "application/json"
}

payload = {
    "amount":25000,
    "currency":"NGN",
    "customer":{
        "name":"John Doe",
        "email":"john@example.com"
    }
}

response = requests.post(url,json=payload,headers=headers)

print(response.json())
```

---

# cURL Example

```bash
curl --request POST \
--url https://api.paybridge.africa/v1/payments \
--header "Authorization: Bearer YOUR_API_KEY" \
--header "Content-Type: application/json" \
--data '{
  "amount":25000,
  "currency":"NGN",
  "customer":{
      "name":"John Doe",
      "email":"john@example.com"
  }
}'
```

---

# Best Practices

To build secure and reliable integrations:

- Store API keys securely.
- Never expose API keys in frontend applications.
- Always use HTTPS.
- Validate API responses before processing.
- Implement retry logic for temporary failures.
- Log API errors for easier debugging.
- Rotate API keys periodically.

- ---

# Changelog

## Version 1.0

- Initial API release
- Payment creation endpoint
- Payment verification
- Refund endpoint
- Webhooks
- Authentication
- SDK examples

- ---

# Support

If you require assistance integrating the PayBridge Africa API, contact:

Email: developers@paybridge.africa

Documentation Version: 1.0

© 2026 PayBridge Africa. All rights reserved.
