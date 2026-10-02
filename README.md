# Zaymo for Klaviyo

Skills and a connector for people who run email on Klaviyo: design split and holdout tests that give a real answer, read test results without fooling yourself, audit flows for duplicate and overlapping sends, and catch the inbox rendering traps that only show up after you hit send.

The skills work on their own, with any Klaviyo data you paste in or reach through another connector. With the Zaymo connector set up, Claude can also read your Zaymo organization's live data and act in it: build and launch tests on your Klaviyo flows, report on results, and edit interactive emails, each write shown to you for confirmation first.

## Skills

| Skill | Use it when |
|---|---|
| `klaviyo-flow-audit` | A flow underperforms, people get the same email twice, or you are cleaning up an account |
| `email-split-test-design` | You want to test a discount, a subject line, timing, or whether an email earns its keep at all |
| `read-test-results` | A test has data and you need to know whether a difference is real |
| `email-qa-preflight` | An email or flow is about to go out and you want it checked against real client behaviour |
| `interactive-email` | You are planning or debugging AMP or Apple Mail interactive email |

## Use it

Ask in plain words, for example:

- "Why is my abandoned cart flow making less than last quarter?"
- "Design a test of 10% vs $10 off in my browse abandonment flow."
- "Variant B is up 12% after a week. Is that real?"
- "QA this email before Thursday's send." (attach the HTML or a screenshot)

## Connect Zaymo

Add the plugin, then connect **Zaymo** from the plugin's Connectors tab and sign in with your Zaymo account. You choose which Zaymo organization the connection starts in. Disconnect any time from Integrations in Zaymo, or remove the connector in Claude.

A Zaymo account is required for the connector; the skills need nothing. Get started at [zaymo.com](https://www.zaymo.com).

## Data

The skills are instructions only: they run nothing on your computer and send nothing anywhere.

The Zaymo connector sends the requests Claude makes in your conversation, and the data those requests need, to `https://app.zaymo.com/api/mcp`, which acts on your Zaymo organization and the Klaviyo and Shopify accounts you have connected to Zaymo. Zaymo stores an audit record of each tool call. Changes that reach your customers (launching a test, publishing a template, changing a flow) are previewed and need your confirmation in the conversation. See the [Zaymo privacy policy](https://www.zaymo.com/privacy).

## Support

support@zaymo.com
