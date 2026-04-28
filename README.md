# 🚀 AnyPay Tanzania API Documentation

Welcome to the official API documentation for **AnyPay Tanzania**.
This API enables developers to integrate **mobile money payments, order creation, and payment tracking** seamlessly.

---

## 🔐 Authentication

All API requests must include the following headers:

```http
Authorization: Bearer <your_access_token>
API-Key: <your_api_key>
Content-Type: application/json
```

### Example (Python)

```python
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "API-Key": "YOUR_API_KEY",
    "Content-Type": "application/json"
}
```

---

## 📌 Base URL

```
https://anypaytanzania.com/api/payments/
```

---

# 💰 1. Wallet Pull Payment

Initiate a **mobile money payment request** to a customer.

### Endpoint

```
POST /wallet/pull/
```

### Request

```json
{
  "order_id": "ORD-8442355",
  "phone": "0798100380",
  "amount": 1000
}
```

### Response

```json
{
  "status": "success",
  "resultcode": "000",
  "message": "Request in progress. You will receive a callback shortly",
  "order_id": "ORD-8442355",
  "transid": "ORD-8442355",
  "msisdn": "255798100380",
  "payment_reference": "WQ20260428BBD25BEA4407322A",
  "order_response": {
    "result": "SUCCESS",
    "reference": "S20523452275"
  },
  "wallet_response": {
    "result": "SUCCESS",
    "reference": "S20523452284",
    "message": "Wallet push successful"
  }
}
```

### Notes

* Accepts local numbers (`07XXXXXXXX`)
* Automatically converts to international format (`255XXXXXXXXX`)
* Payment is **asynchronous** (user confirms on phone)

---

# 🧾 2. Create Order (Minimal)

Generate a payment link for customer checkout.

### Endpoint

```
POST /create-order-minimal/
```

### Request

```json
{
  "buyer_phone": "0798100380",
  "amount": 1000
}
```

### Response

```json
{
  "order_id": "ORD-ac00cd6466",
  "reference": "S20523453011",
  "result": "SUCCESS",
  "payment_token": "63831626",
  "payment_url": "https://tz.selcom.online/paymentgw/checkout/...",
  "redirect": "http://anypaytanzania.com/payment/success"
}
```

### Usage

* Redirect user to `payment_url`
* Supports mobile money & cards

---

# 🔍 3. Check Order Status

Verify payment status from gateway.

### Endpoint

```
GET /check-order-status/?order_id=ORD-8202355
```

### Response

```json
{
  "success": true,
  "message": "Order status checked successfully",
  "data": {
    "order_id": "ORD-8202355",
    "selcom_payment_status": "COMPLETED",
    "selcom_amount": "500",
    "selcom_transid": "DDQF10OW3W",
    "selcom_channel": "MPESA-TZ",
    "selcom_reference": "1684412135",
    "payment_exists": true,
    "source": "selcom_direct",
    "security_check": "passed"
  }
}
```

### Status Values

* `PENDING`
* `PROCESSING`
* `COMPLETED`
* `FAILED`

---

# 📊 4. List Payments

Retrieve all transactions.

### Endpoint

```
GET /payments/
```

### Response

```json
{
  "count": 15,
  "results": [
    {
      "payment_id": "cbc3ec1a-d520-4afc-ad88-96feab1a148f",
      "amount": 1000.0,
      "currency": "TZS",
      "payment_method": "mobile_money",
      "status": "processing",
      "reference": "WQ20260428BBD25BEA4407322A",
      "created_at": "2026-04-28T23:12:00+03:00"
    }
  ]
}
```

---

# ⚡ Payment Flow

## Wallet Pull Flow

1. Create order (optional)
2. Call `/wallet/pull/`
3. Customer receives prompt
4. Customer enters PIN
5. Confirm via callback or status check

## Hosted Payment Flow

1. Call `/create-order-minimal/`
2. Redirect user to payment page
3. Customer completes payment
4. Redirect to success page

---

# 🔁 Callback (Webhook)

After payment completion, your system should handle callbacks.

### Events

* Payment success
* Payment failure
* Wallet confirmation

---

# ❗ Error Handling

### Example

```json
{
  "status": "error",
  "message": "Invalid amount"
}
```

### Common Errors

| Code | Description     |
| ---- | --------------- |
| 400  | Bad Request     |
| 401  | Unauthorized    |
| 403  | Invalid API Key |
| 500  | Server Error    |

---

# 🛠 Best Practices

* Always verify payments using `/check-order-status/`
* Do not rely only on frontend confirmation
* Store `order_id` and `reference`
* Handle asynchronous responses properly

---

# 🧪 Testing Tips

* Use small amounts (e.g., 500 TZS)
* Use valid Tanzanian numbers
* Log all API responses

---

# 📞 Support

* 🌐 Website: https://anypaytanzania.com
* 📧 Email: [support@anypaytanzania.com](mailto:support@anypaytanzania.com)

---

# ✅ cURL Example

```bash
curl -X POST "https://anypaytanzania.com/api/payments/wallet/pull/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
        "order_id": "ORD-12345",
        "phone": "0798123456",
        "amount": 1000
      }'
```

---

## 🎯 License

MIT License © AnyPay Tanzania

---
