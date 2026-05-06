# Usage Gating: Accounting Sync — Developer Handoff

> Prototype: [`pay/index.html`](index.html) | Live: [GitHub Pages](https://anacaligagan.github.io/venue-prototypes/pay/)

---

## Overview

The accounting sync feature (Xero, MYOB, QuickBooks) is gated by subscription tier. Free-tier venues have a **30 invoice sync limit per calendar month**. Pro subscribers get **unlimited syncing**. The UI adapts across 4 states to communicate quota status, prompt upgrades, and handle limit enforcement.

---

## States

| State | Condition | Syncs Used | UI Treatment |
|---|---|---|---|
| **Normal** | Under threshold | < 27/30 | No warning. Upsell hidden in dropdown. |
| **Approaching** | Near limit | 27–29/30 | Yellow "Near limit" badge. Upsell visible. |
| **Limit reached** | At or over limit | 30/30 | Red "Syncing paused" badge. Sync disabled. Upsell prominent. |
| **Pro** | Active subscription | N/A | No limit shown. "Unlimited syncing" label. All upsells hidden. |

### State transitions

```
Normal (< 27) ──→ Approaching (27-29) ──→ Limit Reached (30)
                                              │
                                              ▼
                                     Quota resets on 1st of month → Normal

Any state ──→ Pro (on subscription activation)
Pro ──→ Normal (on subscription cancellation/expiry)
```

---

## Rules

### Quota

- **Free tier limit**: 30 invoice syncs per calendar month
- **Pro tier**: Unlimited (no quota tracking needed in UI)
- **Reset**: 1st of each month at midnight (server time)
- **Counting**: Each invoice sync attempt counts toward the quota, regardless of success or failure

### Sync behaviour at limit

- **Do not auto-sync** once limit is reached — queue invoices instead
- **Resume auto-sync** when quota resets or venue upgrades to Pro
- **Never delete or lose** queued invoices — they must sync when capacity is available
- Last sync timestamp should reflect actual last successful sync, not last attempt

### Threshold for "approaching" state

- Trigger at **27/30** (90% of quota)
- This gives the venue ~3 syncs of runway to see the warning before hitting the wall

---

## UI Components

### 1. Sync button (always visible in Pay tab header)

Shows the connected provider (Xero/MYOB/QuickBooks) with live status.

| State | Subtitle text | Colour | Badge |
|---|---|---|---|
| Normal | "23/30 syncs used" | Default | None |
| Approaching | "27/30 syncs used" | Default | Yellow "Near limit" |
| Limit reached | "30/30 sync limit reached" | Error red | Red "Syncing paused" |
| Pro | "Unlimited syncing" | Default | None |

**Pending actions dot**: Shows when there are unresolved issues (failed syncs, missing expense codes). Displays count. Visible in all states.

### 2. Sync dropdown (opens on button click)

4 sections, top to bottom:

1. **Status row** — Last sync timestamp + help link
2. **Pending actions** (conditional) — Yellow background, lists items needing attention
3. **Links** — "Sync settings", "See usage history"
4. **Upsell** (free only) — Crown icon + "Try unlimited syncing for free"

| State | Last sync text | Pending section | Upsell |
|---|---|---|---|
| Normal | "Last sync: 1 minute ago" | If pending items exist | Hidden |
| Approaching | "Last sync: 1 minute ago" | If pending items exist | Visible |
| Limit reached | "Last sync: 3 days ago. Limit resets next month." (red) | If pending items exist | Visible |
| Pro | "Last sync: 1 minute ago" | If pending items exist | Hidden |

### 3. Usage history modal

Opened from dropdown "See usage history" link.

**Content:**
- Monthly breakdown table showing `synced/total` per month
- Months that hit the limit show a red "Limit reached" badge
- Months near the limit show a yellow "Near limit" badge
- "View unsynced invoices" link filters the invoice table

**Pro state**: Shows confirmation message — "You have unlimited invoice syncs with your Pro subscription. All invoices will sync automatically."

**Free state**: Shows usage table + upsell CTA at bottom — "Get unlimited sync with Pro" + "Try Pro for Free" button

### 4. Invoice table sync status

Each invoice row shows a sync icon:

| Icon | Meaning | When shown |
|---|---|---|
| Checkmark (green) | Synced successfully | Invoice synced to provider |
| Warning (yellow) | Failed to sync | Sync attempted but errored |
| Blocked (grey) | Not synced | Limit reached or not yet attempted |

---

## Pending Actions

Pending actions surface in the sync dropdown when issues need attention. These are independent of the quota state.

| Item | Example | CTA |
|---|---|---|
| Missing expense codes | "12 products missing expense codes" | "View" → expense code mapping screen |
| Failed syncs | "5 invoices failed to sync" | "View" → filtered invoice list |

**Display rules:**
- Show the section only if `pendingCount > 0`
- Badge count on the sync button dot = total pending items
- Yellow background to distinguish from other dropdown sections

---

## Edge Cases

### Sync fails at limit boundary
If a venue is at 29/30 and a sync fails, it still counts toward the quota (now 30/30, limit reached). The failed invoice should be retryable when quota resets.

### Bulk sync pushes over limit
If a venue has 25/30 used and triggers a bulk sync of 10 invoices, sync the first 5 and queue the remaining 5. Show the limit-reached state. Do **not** sync more than the remaining quota.

### Provider disconnected
If the accounting provider is disconnected (token expired, revoked), show an error state separate from usage gating. Do not conflate "disconnected" with "limit reached."

### Mid-month upgrade to Pro
When a venue upgrades to Pro mid-month:
- Immediately switch to unlimited state
- Auto-sync any queued/unsynced invoices
- Usage history should still show the month's count for record-keeping

### Pro subscription expires/cancels
When Pro lapses:
- Revert to free-tier gating
- Calculate remaining quota: `30 - syncs already used this month`
- If already over 30 for the month, immediately show limit-reached state

### Multiple providers
If a venue connects multiple providers (e.g. Xero + MYOB):
- Quota is shared across all providers (30 total, not 30 per provider)
- UI should show the aggregate count

### Month boundary during active session
If a user is viewing the Pay tab when the month rolls over:
- Quota should reset on next API call or page refresh
- No need for real-time WebSocket update

### Zero usage
If a venue has 0/30 syncs used:
- Show "0/30 syncs used" (normal state)
- Do not show "Near limit" or any warning

---

## Upgrade CTAs

All upgrade interactions should navigate to the subscription settings page or trigger the trial modal.

| Location | CTA Text | Action |
|---|---|---|
| Sync dropdown | "Try unlimited syncing for free" | Navigate to subscriptions page |
| Usage history modal | "Try Pro for Free" | Navigate to subscriptions page |
| Navbar | "Try Pro for Free" | Open trial modal |

**When Pro**: All upgrade CTAs are hidden. Navbar shows "Pro" badge or nothing.

---

## API Considerations

### Required endpoints

| Endpoint | Purpose |
|---|---|
| `GET /sync/quota` | Returns `{ used, limit, resetDate, state }` |
| `GET /sync/history` | Monthly breakdown for usage history modal |
| `GET /sync/pending` | Pending actions count + details |
| `POST /sync/trigger` | Trigger manual sync (respect quota) |

### Response shape suggestion

```json
{
  "quota": {
    "used": 23,
    "limit": 30,
    "resetDate": "2026-04-01T00:00:00Z",
    "isUnlimited": false
  },
  "lastSync": "2026-03-31T10:15:00Z",
  "pending": {
    "count": 2,
    "items": [
      { "type": "missing_expense_codes", "count": 12 },
      { "type": "failed_syncs", "count": 5 }
    ]
  }
}
```

### State derivation (client-side)

```
if subscription.isPro → "pro"
else if quota.used >= quota.limit → "limit-reached"
else if quota.used >= 27 → "approaching"
else → "normal"
```

The threshold (27) should be configurable server-side in case we want to tune it.

---

## Testing Checklist

- [ ] Free tier: verify 30/30 blocks further syncing
- [ ] Free tier: verify "approaching" badge appears at 27/30
- [ ] Free tier: verify quota resets on 1st of month
- [ ] Pro: verify unlimited syncing, no quota UI
- [ ] Upgrade mid-month: queued invoices sync immediately
- [ ] Downgrade: quota enforced based on current month usage
- [ ] Bulk sync: respects remaining quota, queues overflow
- [ ] Failed sync: counts toward quota
- [ ] Pending actions: badge count matches items
- [ ] Usage history: correct monthly breakdown with badges
- [ ] All upgrade CTAs navigate correctly
- [ ] Provider disconnect: does not show as usage gating issue
