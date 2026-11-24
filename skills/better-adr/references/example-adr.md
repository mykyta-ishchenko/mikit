# 2026-10-18 Push goes straight to Apple: direct APNs, no OneSignal

The backend sends every push itself through Apple's push service with one team key, and drops
OneSignal before launch.

## TL;DR

The app sends push notifications **directly through APNs**. The backend keeps **each person's
devices** and signs requests with **one .p8 team key** that covers the development and production
push services. A push carries **only its kind and an id**, and its words live in the app.
**OneSignal is dropped**: its free plan now caps mobile push at 1,000 monthly users, and it would be
one more SDK and one more processor of personal data. **FCM is deferred**, not rejected: it joins
behind the same port if Android ever comes.

## Status

**Accepted** (2026-10-18). Supersedes [2026-03-12 Push through OneSignal](2026-03-12-push-onesignal.md). Tasks: APP-339
(backend), APP-348 (sending in production), APP-360 (silent pushes).

## Context

- The app needs four kinds of news at launch: analysis ready, review ready, the daily reminder and,
  later, data export ready. Reminders must fire at the person's chosen hour in their own time zone.
- The app is iOS only. Android is not planned.
- Entries are health-adjacent personal data (GDPR Art. 9). Every processor of personal data must be
  named in the legal texts and covered by a DPA.
- OneSignal was chosen in March 2026. Since 2026-10-01 its free plan caps mobile push at 1,000
  monthly active users.
- What exists in the backend (checked 2026-11-24):
  - The notification context with its devices, sending and reminders:
    `src/app/notification/`, documented in `docs/notification.md`.
  - The APNs adapter: `src/app/notification/apns/` (client, gateway, provider
    token).
  - Temporal already runs multi-step and scheduled work
    ([2026-03-20 Workflows on Temporal](2026-03-20-workflows-temporal.md)).

## 🪛 Decision drivers

Hard filters:

- **F1 No content leaves:** a push carries nothing the person wrote or the analysis said.
- **F2 No new processor of personal data** before launch.
- **F3 No per-user cost at launch scale.**

Tie-breakers:

- **T1 Setup and ops effort** (SDKs, keys, dashboards).
- **T2 Server-side reminders** in the person's time zone.
- **T3 Path to Android** without a rewrite.

## Options

| Driver               | OneSignal                                | ⭐ Direct APNs                    | FCM for iOS                         |
|----------------------|------------------------------------------|-----------------------------------|-------------------------------------|
| F1 No content        | 🟢 payload is ours                       | 🟢 payload is ours                | 🟢 payload is ours                  |
| F2 No new processor  | 🔴 device ids and activity at OneSignal | 🟢 Apple only                     | 🔴 Google becomes a processor       |
| F3 No per-user cost  | 🔴 capped at 1,000 MAU on free           | 🟢 free                           | 🟢 free                             |
| T1 Setup and ops     | 🟡 SDK in the app, dashboard, webhooks   | 🟢 one .p8 key, one adapter       | 🟡 Firebase SDK and project         |
| T2 Reminders         | 🟡 their scheduler, our data copied over | 🟢 Temporal, data already here    | 🟢 Temporal                         |
| T3 Path to Android   | 🟢 built in                              | 🟡 FCM added behind the same port | 🟢 built in                         |

- **OneSignal, rejected:** fails F2 and F3. The 1,000 MAU cap ends the free plan within the
  first weeks after launch.
- **FCM for iOS, rejected:** fails F2. It routes through APNs anyway and adds Google.
- **FCM for Android, deferred, not rejected:** right if an Android app comes.

## Decision

Adopt **direct APNs**:

- **Devices:** the app registers each install with its token, bundle id and push service. A person
  can have several devices, and a push goes to all of them. A token belongs to one person at a time.
- **Sending:** each fact (analysis completed, review written, export ready) goes through the outbox
  and a Temporal workflow, one per kind and subject, so it is told once.
- **Payload:** only the kind and the id of what it opens. Title and body are the app's localized
  strings `push.<kind>.title` and `push.<kind>.body`.
- **Reminders:** sent by the server at the profile's hour (8, 14, 20 or 0) in the person's zone,
  skipped if they wrote that day.
- **Switches:** one per kind in the profile. A kind that is off goes out silently, and a reminder
  is not sent at all.
- **Key:** one .p8 team key for every environment and both push services, kept as a secret in
  the secret store.
- **Not now:** Android and FCM.

| Pros                                          | Cons                                                    |
|-----------------------------------------------|---------------------------------------------------------|
| No processor besides Apple                    | Delivery, retries and dead tokens are ours to handle    |
| Free at any scale                             | No dashboard: delivery shows only in logs and Sentry    |
| No push SDK in the app                        | Android would need FCM added later                      |
| Reminders use data the backend already has    | A second app or key change needs a worker restart       |

Direct APNs beats OneSignal on the two filters it fails, and the work OneSignal would save
(devices, retries, dead tokens) is one context of about 1,500 lines, 265 of them the APNs adapter
(counted 2026-11-24).

## Consequences

**Harder / risks**

- **Dead tokens:** Apple refuses tokens of reinstalled or restored phones. → The device is
  forgotten, unless the app registered it again after Apple's moment.
- **Key problems stop every push:** a wrong or revoked key fails for every device. → The worker
  does not start with a key it cannot sign with, and a refused request is reported as an error.
- **Silent pushes are best effort:** Apple may delay or drop them. → The app never counts on them
  and refreshes on open.
- **Late reminders are worthless:** → A reminder whose hour passed while the worker was down is
  dropped, and Apple holds it only for a configured while.

**Mitigations & future**

- **Android:** FCM joins behind the same sending port; devices already store their push service.
- **Rollout switch:** an admin turns sending off at runtime without a deploy.
- **Manual setup, once:** create the .p8 key in the Apple Developer account for both push services,
  store it in the secret store for every environment, list the allowed bundle ids in the backend config.

## References

- `docs/notification.md` on `main` (checked 2026-11-24)
- [2026-03-12 Push through OneSignal](2026-03-12-push-onesignal.md) (superseded)
- OneSignal pricing page: free plan capped at 1,000 MAU from 2026-10-01 (checked 2026-10-18)
- Readable page: [Push Straight to Apple](https://example.com/adr/push-direct-apns)
