# Subscription Pricing -

- Set `subscription term unit` in the CPQ package settings that applies to every product.

- CPQ prorates Regular Unit Price, Customer Unit Price, Net Unit Price, and Net Total (maybe Prorated List Price and Prorated Unit Price).

- `Subscription Pricing` field on Product.
    - `Fixed Price` - use the price book price in calculations.

- `Subscription Term` field on Product has default value of 12. This value gets copied to `Default Subscription Term` field on QL which CPQ uses to calculate subscription pricing.

- The length of subscription can either come from `Subscription Term` (on Quote or QL) or `Start Date` and `End Date` (on Quote, Quote Line Group or QL). CPQ determines the length of a subscription quote line by first looking for the most specific start and end dates. If CPQ can’t find either of those dates, it looks for the most specific subscription term. By default, CPQ does the same thing when deciding if a quote line qualifies for a term discount schedule.
    - On every product there is a field named `Term Discount Level`. Blank value means `Line`.
    - One important thing to note about term discount level is that CPQ looks up the TDL value from the product record every time a recalculation of the quote occurs. Be careful when changing the TDL on a product because the new value is applied every time you recalculate a quote that contains that product.
    - [More info](https://trailhead.salesforce.com/content/learn/modules/subscription-pricing-in-salesforce-cpq/give-discounts-for-long-subscriptions?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-cpq-specialist-credential)

- For one-time products, you set the Pricing Method field to Percent Of Total. For subscription products, you leave Pricing Method as List and set the Subscription Pricing field to Percent Of Total.

- **`Prorate Multiplier`** : 
    - $Subscription Term / Default Subscription Term$.
    - You can tell CPQ to prorate the `Additional Discount` amount as long as you make a quote line field `ProrateAmountDiscount` ([formula] checkbox/number) to give CPQ the message. This field doesn’t exist by default. 