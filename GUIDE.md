# Guide

Everything you need to set up and run your subscription business.

---

## Table of Contents

- [First Steps](#first-steps)
- [Payment Methods](#payment-methods)
- [Plan Settings Explained](#plan-settings-explained)
- [Managing Subscribers](#managing-subscribers)
- [Notifications](#notifications)
- [Subscriber Experience](#subscriber-experience)
- [Deleting Plans](#deleting-plans)
- [Archive](#archive)
- [Tips](#tips)
- [More Resources](#more-resources)

---

## First Steps

### 1. Select Your Wallet

Choose which LNbits wallet receives subscription payments from the dropdown at the top.

### 2. Create a Plan

Click **New Plan** and fill in:

| Field | Required | Description |
|-------|----------|-------------|
| Name | Yes | What you're selling |
| Price | Yes | Amount per billing cycle |
| Interval | Yes | Weekly, monthly, or yearly |
| Description | No | Shown to subscribers on checkout |
| Trial Days | No | Free period before first payment |
| Grace Period | No | Days after failed payment before marking failed |

### 3. Share Your Link

Every plan has a unique URL. Click the copy icon next to your plan and share it anywhere.

---

## Payment Methods

### Lightning (Default)

Always available. Subscribers scan a QR code or copy the invoice.

### On-chain Bitcoin

Requires additional setup:

1. Install **WatchOnly** extension
2. Add your wallet's xpub/zpub to WatchOnly
3. Install **SatsPay** extension
4. When creating a plan, enable on-chain and select your WatchOnly wallet

Subscribers see a unified QR code that works with both Lightning and on-chain wallets.

### Card & PayPal

Server-level feature. Your LNbits admin must enable Stripe/PayPal in server settings. If enabled, these options appear automatically in plan creation.

---

## Plan Settings Explained

### Trial Days

Give new subscribers free access before their first payment.

- `0` = No trial, payment required immediately
- `7` = 7 days free, then first payment

Subscribers are **active** during trial. Use trials for higher-priced plans to reduce friction.

### Grace Period

Extra time after a payment fails before marking the subscription as failed.

- `0` = Strict, fails immediately
- `3` = 3 days to fix payment issues

During grace period, status shows "Past Due". Subscribers keep access. Use 3-7 days to reduce accidental churn.

---

## Managing Subscribers

### Status Reference

| Status | What it means | Has access? |
|--------|---------------|-------------|
| Active | Paid and current | Yes |
| Pending | Waiting for first payment | No |
| Paused | Temporarily suspended | Until period ends |
| Past Due | Failed payment, in grace period | Yes |
| Payment Failed | Grace period ended | No |
| Cancelled | Ended by you or subscriber | No |
| Expired | Period ended | No |

### Actions

| Action | Effect |
|--------|--------|
| **Pause** | Stops future billing. Access continues until current period ends. |
| **Resume** | Reactivates a paused subscription |
| **Cancel** | Ends subscription immediately. Subscriber notified. |
| **Add Days** | Extends subscription with free time |
| **Copy Portal Link** | Get subscriber's self-service URL |

### Filtering

- **Search**: Find by email or name
- **Plan filter**: Show subscribers of specific plan
- **Status filter**: Active / Needs Attention / Inactive

---

## Notifications

### How It Works

Notifications follow a **global → plan** inheritance model:

1. **Global Settings** (gear icon) define which channels are available
2. **Plan Settings** can override or customize per plan
3. **Subscribers** only see contact options you've enabled

```
Global Settings (Email + Telegram enabled)
    │
    ├── Plan A: Uses global defaults
    │       → Subscribers see: Email, Telegram
    │
    └── Plan B: Email only (override)
            → Subscribers see: Email only
```

If you haven't configured a notification channel globally, it won't appear as an option anywhere to subscribers. This ensures that only the notification methods you've explicitly enabled will be available to your subscribers.

### Setup

1. Click the **gear icon** (Settings)
2. Enable your preferred channel(s)
3. Fill in required details
4. Click **Test** to verify
5. Save

### Channels

| Channel | You need |
|---------|----------|
| Email | SMTP server, port, username, password |
| Telegram | Bot token, chat ID |
| Webhook | URL endpoint that accepts POST |
| Nostr | Your private key (nsec) |

### Per-Plan Customization

When creating or editing a plan, you can:

- **Use global defaults** — Inherits all channels from global settings
- **Customize** — Enable/disable specific channels for this plan only

Use this to offer different contact options per plan. Example: Premium plans might include Telegram support while basic plans are email-only.

### Events

Choose which events trigger notifications:

- New subscription created
- Payment received
- Payment failed
- Subscription cancelled
- Subscription expired

Both you (merchant) and subscribers can receive notifications.

---

## Subscriber Experience

### What They See

1. **Subscribe page**: Plan details, price, payment options
2. **Payment**: QR code for Bitcoin, redirect for card/PayPal
3. **Confirmation**: Success message with details
4. **Portal**: Self-service management

### Self-Service Portal

Every subscriber gets a unique link (no password needed):

```
/subscriptions_manager/manage?token=<unique_token>
```

From the portal they can:
- View subscription status
- See payment history
- Cancel their subscription

The link is included in confirmation emails. You can also copy it from the subscriber row in admin.

---

## Deleting Plans

When you delete a plan:

1. **All active subscriptions are cancelled**
2. **Subscribers are notified** via their preferred channel
3. **Plan moves to Archive**
4. **Subscriptions move to Archive**

The confirmation dialog shows exactly how many subscriptions will be affected.

---

## Archive

The Archive tab contains:

- Deleted plans
- Cancelled/expired subscriptions

Each section has its own search. Archived subscriptions show:
- Subscriber info
- How long they were active
- When they ended

Nothing is permanently deleted. Use Archive for record-keeping.

---

## Tips

### Before Launch

- [ ] Create your plan with clear name and description
- [ ] Test the subscribe flow yourself
- [ ] Set up at least one notification channel
- [ ] Verify notifications arrive

### For Success

- Use trials for plans over 50k sats
- Set grace period to 3-7 days
- Write descriptions that explain what subscribers get

### Troubleshooting

**Subscriber stuck on "Pending"**
→ They didn't complete payment. Ask them to try again.

**Notifications not arriving**
→ Use Test button in Settings. Check spam folder for email.

**On-chain option not showing**
→ Verify WatchOnly and SatsPay are installed. Check plan has on-chain enabled.

**Subscriber can't access portal**
→ Copy fresh portal link from admin UI and send to them.

---

## More Resources

- [Use Cases](USE_CASES.md) — Examples and benefits
- [Full Documentation](https://docs-subscriptions.netlify.app) — Complete docs including API reference
- [GitHub Issues](https://github.com/DoktorShift/subscriptions-manager-public/issues) — Report problems
