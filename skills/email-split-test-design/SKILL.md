---
name: email-split-test-design
description: Design an email split test, A/B test or holdout test that will give a trustworthy answer. Use when someone wants to test a discount or offer, subject line, send timing, number of emails, or whether a flow or message earns money at all, in Klaviyo or another email platform.
---

# Email split test design

A test is worth running only if it can change a decision. Most email tests fail before they start: too many arms for the traffic, a metric that does not track money, or a split that leaks.

## 1. Pin down the decision

Ask what the person will do differently depending on the result. "See which subject line wins" is not a decision; "decide whether abandoned cart needs a discount at all" is. The decision picks the metric and the arms.

## 2. Pick the shape

- **Flow test:** the treatment lives inside an existing journey (a different offer in email 2 of abandoned cart). Split inside the flow at the treatment moment.
- **Campaign test:** the treatment is a send to an audience (a win-back offer to lapsed customers). Split the segment and give each group its own campaign.
- **Holdout test:** one group gets nothing at that moment. This is the only way to measure whether an email or flow earns money at all, because attribution windows credit an email for orders that would have happened anyway.

## 3. Choose the metric

Use money per person who entered the test, measured over a fixed window (7 days is a good default for flows, longer for replenishment products):

- **Profit per recipient** when product costs are known, because a bigger discount can win revenue and lose margin.
- **Revenue per recipient** otherwise.
- **Conversion rate** when order values are similar across arms.

Avoid open rate. Apple Mail Privacy Protection marks most Apple Mail messages as opened, so open rate measures audience mix more than the email. Click rate is acceptable for a pure subject-line or layout question, but say that it does not show money.

Measure everyone assigned to an arm, not only people who clicked. A holdout sends nothing, so an attribution model that needs a click or open credits it $0 by construction. Compare holdout and treatment on all orders by assigned people in the window.

## 4. Size it

Fewer arms beat more arms. Each extra arm splits traffic and adds another chance of a fluke winner. As a rule of thumb:

- Under about 300 people a day entering the test: 2 arms, maybe 3.
- A few thousand a day: up to 4 or 5.
- Expect conversion-based tests to need thousands of people per arm to detect a 10 to 15% difference. Money metrics are noisier than conversion, and a few big orders can swing them.

State how long the test must run before anyone reads a result, and agree on that up front.

## 5. Assign cleanly

- Assign each person once and keep them in their group (sticky assignment), written onto their profile, so later emails and other flows can respect it.
- If the same people can enter two related flows (browse and cart abandonment), use one assignment for both, or the arms contaminate each other.
- Every arm, the control included, should route on its own explicit group, so a person without an assignment is visible as an error rather than silently becoming control.
- Assignment by a webhook is asynchronous. Leave a wait of at least 30 minutes between assigning and splitting, or a share of people reach the split before their group is written.

## 6. Write it down

Before launch, record: the decision, arms and weights, the metric and window, the minimum run time, and what result would change the decision. Then hand off to `read-test-results` when data arrives.

## With the Zaymo connector

Zaymo builds and runs these tests on Klaviyo for organizations with testing enabled; if `proposeTestSetup` is not among the tools, testing is not enabled for this organization, so design the test here and build it by hand. Read the `zaymo://guides/ltv-test-design` and `zaymo://guides/ltv-test-creation` resources, call `listLtvTests` to see what already runs, `listFlows` and `listSegments` to ground the ids, then `proposeTestSetup`. Show the person the preview and build only after they confirm. Zaymo can also move traffic toward the better arm as evidence builds (adaptive allocation, never below a 5% floor per arm).
