---
name: email-qa-preflight
description: Pre-send QA for a marketing email, campaign or flow message. Use when an email is about to go out, when someone shares email HTML or a screenshot to check, or when images, buttons, coupons or dark mode look wrong in Gmail, Apple Mail or Outlook.
---

# Email QA before sending

Check what recipients will actually see, not what the editor preview shows. Go through every section; report each problem with where it is and how to fix it, most damaging first.

## 1. Rendering traps by client

- **Apple Mail and CSS background images:** when Mail Privacy Protection's proxy is unavailable, Apple Mail loads `<img src>` images but not CSS `background-image` URLs it has not already cached. Images delivered only as backgrounds render as empty boxes. Any important image (product, hero, offer) should be an `<img>`, or have an `<img>` fallback.
- **Gmail clipping:** Gmail clips messages whose HTML exceeds about 102 KB and hides the rest behind "View entire message". Content after the clip, often the footer and unsubscribe link, is lost. Check the size of the final HTML.
- **Gmail and `<style>`:** Gmail supports embedded styles, but can discard a whole style block over one unsupported rule. Keep critical layout inline.
- **Classic Outlook for Windows:** renders with Word (the newer Outlook does not). No background images without VML, no `max-width`, limited padding on `<div>`. Use tables for layout and check buttons are "bulletproof" (padded table cells, not images).
- **Dark mode:** some clients invert colors. Transparent PNG logos with dark artwork vanish on dark backgrounds. Text baked into images does not adapt. Check both modes.

## 2. Content

- Every image has descriptive `alt` text, and the email still makes sense with images off.
- The subject line and preheader work together and are not truncated on mobile (keep the key words in the first 30 to 40 characters).
- Personalisation has fallbacks (`{{ first_name|default:"there" }}`), so nobody reads "Hi ,".

## 3. Links and offers

- Every link resolves, and goes where its text says. Product links point at live products.
- Tracking parameters are present and consistent.
- **Coupon codes:** the code applies at checkout, works for this audience (some discounts reject subscription carts), and outlasts what the copy promises. If the email says "valid for 7 days", the code must be valid for at least 7 days after the last email in the flow that mentions it.
- The unsubscribe link and physical address are present (legal requirement).

## 4. Sending settings

- Marketing vs transactional is set correctly. Marking a promotional email transactional to skip consent breaks the rules and risks the sending account.
- Smart sending is on for marketing flow messages unless there is a reason.
- The audience and exclusions are right, and the send time is in the audience's time zone.

## 5. Testing images honestly

When checking whether images load in a client, add a unique query string to each image URL (`?v=check42`) for every test. Otherwise the client's cache answers and a broken image looks fine.

## With the Zaymo connector

`templates_previewHtml` returns a template's final HTML for these checks, and `templates_sendTest` sends a test copy to an inbox. Zaymo's interactive emails also have their own checks; see the `interactive-email` skill.
