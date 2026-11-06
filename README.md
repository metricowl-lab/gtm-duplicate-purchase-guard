# Duplicate Purchase Guard

A web variable template that returns `true` when a transaction ID was already seen on an earlier page view in this browser. A refreshed, revisited or bookmarked thank-you page then doesn't count the sale twice in GA4, Google Ads, Meta or anywhere else.

## Why

Duplicate purchases are one of the most common reasons GA4 revenue doesn't match the shop. GA4 can remove duplicate purchases that carry the same transaction ID, but ad platforms each have their own rules, and some tags have none. Stopping the duplicate before any tag fires covers them all. More in [How to fix duplicate conversions in GA4](https://www.metricowl.co.uk/guides/fix-duplicate-conversions-in-ga4).

## Set up

1. Add the template from the gallery, or import `template.tpl`.
2. Create a variable from it, for example **Purchase is duplicate**, and set **Transaction ID** to your transaction ID variable, such as a Data Layer Variable for `ecommerce.transaction_id`.
3. On the trigger for your purchase tags, add the condition **Purchase is duplicate equals false**.
4. Test: place an order, then refresh the thank-you page. In Preview, the purchase tags shouldn't fire the second time.

## How it works

- **The first time an ID is seen,** it's stored in the browser's local storage, with the time and a token for that page view. The variable returns `false`.
- **On the same page view** it always returns `false`, however many times and on whichever dataLayer message Tag Manager evaluates it. Preview mode and several purchase tags on different triggers are all fine.
- **On a later page view,** such as a refresh, a revisit or a bookmark, the same ID returns `true` until it expires.
- **Limits:** IDs are kept for 30 days by default, and the newest 100 at most.
- **No transaction ID:** the variable returns `false` and stores nothing.

It doesn't catch a purchase pushed twice on the same page view. That's a tagging bug to fix at the source, and GA4 removes those duplicates itself when they carry the same transaction ID.

Local storage is per browser, so this catches repeats on the same device, not the same order in another browser. Storing a transaction ID this way is usually treated as part of the purchase itself, but check it against your own consent approach.

## Settings

| Setting | What it does |
|---|---|
| Transaction ID | The order's unique ID (required) |
| Remember transaction IDs for (days) | 30 by default |

## Permissions

- **Local storage:** reads and writes the key `mo_purchase_guard`.
- **Window:** reads and writes `mo_purchase_guard_page`, the page view token.

## Licence

Apache License 2.0. See [LICENSE](LICENSE).

Built and maintained by Marc Alexander at [Metric Owl](https://www.metricowl.co.uk), a GA4 and Google Tag Manager consultancy in London. Issues and suggestions are welcome.
