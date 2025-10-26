# ZendFi API Documentation

Welcome to the ZendFi API! Accept crypto payments as easily as using Stripe.

## What is ZendFi?

ZendFi is a payment infrastructure for accepting cryptocurrency payments on Solana. We handle all the complexity of blockchain transactions, settlement, and wallet management so you can focus on building your product.

## Getting Started in 3 Steps

```bash
# 1. Create your merchant account
curl -X POST https://api.zendfi.tech/api/v1/merchants \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Store",
    "email": "hello@mystore.com",
    "business_address": "123 Main St, San Francisco, CA",
    "webhook_url": "https://mystore.com/webhooks/zendfi"
  }'

# 2. Save your API key from the response
# API_KEY=your_api_key_here

# 3. Create your first payment
curl -X POST https://api.zendfi.tech/api/v1/payments \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "amount": 50.00,
    "currency": "USD",
    "description": "Premium Plan - Monthly"
  }'
```

That's it! You'll receive a QR code and payment URL that your customers can use to pay with any Solana wallet.

## Core Concepts

### Payments
Create one-time payment requests. Customers scan a QR code or click a payment link to complete the transaction.

### Payment Links
Reusable payment pages perfect for products, donations, or tips. Share one link for multiple customers.

### Webhooks
Real-time notifications when payments are confirmed, settled, or fail. Essential for order fulfillment.

### Settlements
Automatic transfers of funds to your wallet. We handle the complexity - you just receive your money.

### MPC Wallets (Optional)
Secure, non-custodial wallets with passkey authentication. No seed phrases to manage.

## API Endpoint Categories

| Category | Description | Status | Documentation |
|----------|-------------|--------|---------------|
| **Getting Started** | Merchant onboarding & authentication | ✅ Complete | [01-getting-started.md](./01-getting-started.md) |
| **Payments** | Create and manage payment requests | ✅ Complete | [02-payments.md](./02-payments.md) |
| **Payment Links** | Reusable payment pages | ✅ Complete | [03-payment-links.md](./03-payment-links.md) |
| **Webhooks** | Real-time event notifications | ✅ Complete | [04-webhooks.md](./04-webhooks.md) |
| **Wallet Management** | MPC wallet operations | ✅ Complete | [05-wallet-management.md](./05-wallet-management.md) |
| **Advanced Features** | Subscriptions, escrows, installments | ✅ Complete | [06-advanced-features.md](./06-advanced-features.md) |

## Authentication

All API requests require authentication using your API key:

```bash
Authorization: Bearer YOUR_API_KEY
```

Get your API key when you create your merchant account or from your dashboard at https://api.zendfi.tech/dashboard.

## Base URL

```
Production: https://api.zendfi.tech
```

## Rate Limits

- **Default:** 100 requests per hour per API key
- **Rate limit headers** are included in all responses
- Contact us for higher limits

## Error Handling

All errors return a JSON response with details:

```json
{
  "error": "invalid_request",
  "message": "Amount must be greater than 0",
  "details": {
    "field": "amount",
    "provided": -10
  }
}
```

Common HTTP status codes:
- `200` - Success
- `400` - Bad Request (invalid parameters)
- `401` - Unauthorized (missing/invalid API key)
- `404` - Not Found
- `429` - Rate Limit Exceeded
- `500` - Internal Server Error

## Idempotency

Prevent duplicate payments by including an idempotency key:

```bash
curl -X POST https://api.zendfi.tech/api/v1/payments \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Idempotency-Key: unique-key-123" \
  -d '{"amount": 100, "currency": "USD"}'
```

If you retry with the same key, you'll get the original response instead of creating a duplicate payment.

## Supported Tokens

- **USDC** (Primary) - Stablecoin pegged to USD
- **SOL** - Solana's native token
- More tokens coming soon!

## Network

We operate on **Solana Mainnet** for production and **Solana Devnet** for testing.

## Need Help?

- Email: support@zendfi.tech
- Discord: [Join our community](https://discord.gg/zendfi)
- Guides: [zendfi.tech/guides](https://zendfi.tech/guides)
- Issues: [GitHub](https://github.com/zendfi)

## Quick Links

- [Getting Started Guide](./01-getting-started.md)
- [Payment Flow Diagram](./diagrams/payment-flow.md)
- [Webhook Events Reference](./04-webhooks.md#events)
- [Error Codes Reference](./reference/error-codes.md)
- [SDKs & Libraries](#sdks)

---

**Developer Experience Matters**

We obsess over making your integration smooth. If something's confusing or could be better, please tell us. We're here to help!
