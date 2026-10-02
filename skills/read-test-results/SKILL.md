---
name: read-test-results
description: Interpret email A/B, split or holdout test results and decide whether a difference is real. Use when someone asks if a variant won, whether a lift is significant, when to stop a test, or why results changed from last week, for Klaviyo or any email platform.
---

# Reading email test results

The goal is an honest call: winner, loser, or "not enough evidence yet". Most mistakes come from reading noise as a result.

## 1. Check the data before the numbers

- **Assignment balance:** arm sizes should match their intended weights. A big mismatch means a broken split, and the test cannot be trusted.
- **Unassigned people:** anyone who reached the email without a group is missing from the result. Count them.
- **Maturity:** count only people whose full measurement window has passed. Including people who entered yesterday biases toward arms that convert faster.
- **Same population:** if traffic shares changed during the test, compare arms within the same enrollment periods, or pooled averages mislead.

## 2. Use the right comparison

- **Conversion (yes/no):** a chi-square test across arms.
- **Revenue or profit per person:** Welch's t-test on per-person values, zeros included. Do not compare only buyers.
- **Big orders:** a few large orders can decide a money metric. Cap per-person values at the 99th percentile of non-zero values (winsorize, do not drop) and check whether the call changes. If it does, the result rests on a handful of orders.

With three or more arms, first ask whether any arm differs at all (an omnibus test across every arm). Picking the best of several arms and testing it against control overstates its significance. With six identical arms, one will look like a winner surprisingly often.

## 3. Avoid the two classic traps

- **Peeking:** checking every day and stopping the first time p < 0.05 produces false winners far more often than 5%. Decide the run time in advance, or use a method built for repeated looks.
- **Open rate:** Apple Mail Privacy Protection inflates opens, so an open-rate "win" says little about money.

## 4. Holdouts

A holdout arm gets no email, so click- or open-based attribution gives it zero revenue by definition. Compare all orders by everyone assigned to each arm in the window. The gap is the email's true incremental value, usually much smaller than the attributed revenue in the platform's dashboard.

## 5. Say what you found

Report, for each arm: people, conversion, money per person with a confidence interval, and the lift against control. Then give one of:

- **Winner:** the difference is significant on the pre-agreed metric and holds with outliers capped.
- **No detectable difference:** say how large a difference the test could have detected. "No difference" from a small test is not proof of equality.
- **Keep running:** with an estimate of how much longer.

If one arm is clearly worse, it is reasonable to stop sending it even before the others are separated.

## With the Zaymo connector

Call `listLtvTests` and `getLtvTest` for the test's arms, metric and current traffic split, and `runSql` for per-person outcomes. Zaymo only moves traffic between arms when there is evidence that they differ, so an unchanged split is itself information: the arms are not yet distinguishable.
