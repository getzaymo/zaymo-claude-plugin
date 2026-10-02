---
name: klaviyo-flow-audit
description: Audit Klaviyo flows for lost revenue and customer annoyance. Use when a flow underperforms or drops off, subscribers say they got the same email twice, browse and cart abandonment overlap, re-entry or smart sending settings are in question, or someone is cleaning up or inheriting a Klaviyo account.
---

# Klaviyo flow audit

Most underperforming Klaviyo accounts are not short of flows. They send the wrong flows to the same person, send one message twice, or have a flow that quietly stopped matching anyone. Audit structure first, then evidence, then performance.

## 1. Get the facts

Work from real data, never from what a flow is "supposed" to do.

- With the Zaymo connector: `flows_list` and `flows_get` for structure (trigger, trigger filters, profile filters, re-entry, each action), `klaviyoReport` for Klaviyo's own performance numbers, `runSql` over the warehouse for per-profile send history.
- With another Klaviyo connector, use its flow and reporting tools the same way.
- Without either, ask for: each live flow's trigger and filters, its re-entry setting, the message timeline, and 30 days of per-flow recipients, revenue and unsubscribes. A CSV export is fine.

Exclude transactional messages (order confirmations, shipping updates) from every duplicate check. They are supposed to stack.

## 2. Structural checks

For each live flow, check:

1. **Duplicate trigger.** Two live flows on the same trigger with the same trigger filter send everyone both. A different trigger filter (for example a shipping status split) is legitimate.
2. **Browse and cart overlap.** Browse abandonment, site abandonment, abandoned cart and abandoned checkout all fire on one shopping session. Without exclusions a shopper who checks out halfway gets three or four emails in a day. The fix is a flow filter on each lower-intent flow excluding people who reached the higher-intent event since starting the flow (for example browse excludes "Started Checkout since starting this flow").
3. **Re-entry.** A flow that lets people re-enter every few hours re-sends its first message on every trigger. Compare the re-entry window with how often the trigger fires per person.
4. **Smart sending.** Off on marketing flows means back-to-back sends to the same person. Check it on every message, and check again after any flow was created or cloned through the API, since that path can leave it off.
5. **Dead flows.** A live flow with zero recipients in 30 days has a broken trigger, a filter that excludes everyone, or a metric that stopped arriving (an integration disconnected).
6. **Draft leftovers.** Setting a whole flow live through the API turns every draft action in it live too. Look for half-built branches that started sending.

## 3. Evidence checks

Structure says what could go wrong; send history says what did. From per-profile sends over 30 days, count:

- **Same message re-sent** to one profile within 24 hours.
- **Two flows, same trigger,** both sending to one profile within 24 hours.
- **Back to back:** any two marketing emails to one profile within an hour (a warning, not always a bug).

Report a finding only when it touches enough people to matter. A useful bar is at least 25 profiles and at least 5% of the flow's recipients. Below that it is noise.

Before blaming the customer's setup, check whether one of the flows is a test copy someone built (a cloned flow running next to its source sends both).

## 4. Performance

Judge flows on revenue per recipient and unsubscribe rate, not open rate. Apple Mail Privacy Protection marks most Apple Mail messages as opened, so opens overstate engagement and swing with audience mix. Compare each flow with its own previous period and with the same flow type in similar accounts, not with campaigns.

A flow whose revenue per recipient falls while recipients stay flat usually has a content or offer problem. A flow whose recipients fall usually has a trigger, filter or list-growth problem.

## 5. Report

Give a short table: flow, finding, people affected in 30 days, and the fix. Order by people affected. Recommend fixes; do not apply any change to a live flow without the person confirming it, because every change reaches customers immediately.

When a fix is a judgment call (does this offer help or just give margin away?), say that it should be tested rather than guessed, and hand off to the `email-split-test-design` skill.
