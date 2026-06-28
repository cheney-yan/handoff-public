# WeChat Mini-Program: Gym Check-In

A WeChat mini-program that lets gym members check in by scanning a QR code at the door and view their visit streak. The system must support roughly 500 members. Identity is handled through WeChat login (openid), and check-in fraud is mitigated by a per-day rotating QR code at the door.

## Problem

Gym members need a lightweight way to check in when they arrive and to see their attendance streak as motivation. The gym has ~500 members. A naive QR code could be photographed and reused to "check in" from home, so the check-in mechanism must prove physical presence at the door. The gym also wants to avoid building and maintaining separate user accounts.

## Solution

- **Front-end:** WeChat mini-program (WXML/WXSS + JS) calling a small backend.
- **Check-in flow:** Members scan a per-day rotating QR code at the door. The QR encodes a token; the backend validates the token and records a visit. The rotation prevents check-in from home, since a photographed/old QR will no longer be valid.
- **Identity:** WeChat login via openid — no separate accounts to create or manage.
- **Streak:** Consecutive days with at least one recorded visit. One visit per day counts; the streak resets if a day is missed.

## User stories

1. **Member check-in:** As a member, I scan the door QR and see "checked in, streak: N days".
2. **Staff dashboard:** As staff, I see today's check-in count.
3. **Idempotent scans:** As a member, if I scan more than once on the same day, the duplicate scans are idempotent — only one visit is counted and the streak is not affected.

## Decisions

- **Per-day rotating QR (chosen):** The door QR rotates so a captured image cannot be reused to check in remotely. This proves physical presence. (Implicitly rejected: a static QR, which could be photographed and reused from home.)
- **WeChat openid for identity (chosen):** Use WeChat login (openid) rather than building separate accounts. Reduces account-management burden and friction for members.
- **Streak rules (chosen):** Streak counts consecutive days with a visit; one visit per day counts; the streak resets when a day is missed.
- **Duplicate scans idempotent (chosen):** Multiple scans on the same day record at most one visit.
- **openid only, no phone numbers (chosen, privacy):** We reject storing phone numbers. Identity is keyed on openid only, to minimize collection of personal data.
- **Secrets handling:** The backend key for the WeChat API lives in the secrets manager. (Test/credential value omitted: `<redacted>`.)

## Testing

- **Token expiry boundary:** Verify QR/token validation at the expiry boundary (valid just before expiry, rejected after).
- **Duplicate-scan idempotency:** Verify repeated scans on the same day produce a single visit.
- **Streak reset across a missed day:** Verify the streak resets correctly when a day is missed.

## Out of scope

Explicit non-goals for v1:

- Payments
- Class booking
- Multi-branch support

## Open questions

- **QR rotation interval:** How often should the door QR rotate — every 60s or every 5 minutes?
