---
name: interactive-email
description: Plan, build or debug interactive email (AMP for Email or Apple Mail interactive HTML) such as in-email product pickers, carousels, quizzes, forms, reviews or subscription management. Use when someone asks what interactive email can do, which inboxes support it, why interactivity does not show, or how to test it.
---

# Interactive email

Interactive email lets a shopper act inside the message: choose a size, answer a question, leave a review, change a subscription. It reaches only part of an audience, so every interactive email also needs a good static version.

## 1. Who sees what

- **AMP for Email** renders in Gmail and a few other webmail clients that support it. The sender must be registered with Google for AMP, the email must pass the AMP validator, and SPF, DKIM and DMARC must pass. Any failure silently falls back to the HTML version.
- **Interactive HTML** (CSS-driven, no JavaScript) works in Apple Mail, which is often the largest client for consumer brands.
- **Everyone else** (Outlook, many mobile apps) sees the static fallback.

Look at the audience's client mix before promising results. The interactive share is the Gmail plus Apple Mail share, minus anyone whose setup blocks it.

## 2. Design rules

- Make the fallback a complete email with a clear call to action, not a "view in browser" stub.
- One interaction per email. A picker plus a quiz plus a carousel overwhelms people and multiplies failure points.
- Gmail treats AMP content as dynamic for a limited period after sending, then shows the HTML version. Do not rely on AMP for evergreen flow messages without checking this.
- AMP strips some CSS, including `!important`, and Gmail drops `pointer-events`. Decorative overlays such as a dropdown arrow on top of a control can become dead tap areas; give the control the top layer.
- In Apple Mail, deliver important images as `<img>` elements. CSS background images may not load (see `email-qa-preflight`).

## 3. Measure it properly

Interactive emails tempt people to count interactions as success. Measure money per recipient against a static version of the same email in a split test (see `email-split-test-design`), across the whole audience, including people who saw the fallback.

## 4. Debugging "the interactivity doesn't show"

Work down this list:

1. Is the recipient's client one that supports the format?
2. AMP: is the sender registered, does the message pass the validator, do SPF, DKIM and DMARC pass, and is the email inside Gmail's dynamic window?
3. Did the sending platform rewrite the HTML? Some editors strip unknown attributes or reformat code.
4. Are you looking at a forwarded copy? Forwarding drops AMP.
5. Test with fresh, cache-busted image URLs and a fresh send, not a resend of an old message.

## With the Zaymo connector

Zaymo builds interactive emails as Klaviyo templates with AMP, Apple Mail and static versions generated together. Read `zaymo://guides/builder-vocabulary` first, call `builder_inspect` to see a template's structure, and edit with `builder_mutate`, which previews every change before saving. `templates_sendTest` sends a test copy.
