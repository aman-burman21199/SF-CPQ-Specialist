# CPQ Pricing Basics -

- Pricing Methods/Tools - <img style="display: block; margin: auto;" src="./Pricing Methods or Tools.png"/>

- QL Pricing Fields / Price Waterfall - (Left to Right Precendence in LS) OLSRCPN <img style="display: block; margin: auto;" src="./QL Pricing Fields and Pricing Waterfall.png"/> 

- Sequence to calculate Net Price :-

| PRICE AND DISCOUNT | DESCRIPTION |
| --- | :---: |
| List Unit Price |	The list unit price inherits this product's price book entry by default. |
Contracted Price (if any) |	If the quote line has an associated contracted price on the account, the contracted price overrides the list unit price. |
| Special Price |	The special price is inherited from the contracted price, a custom price set by the Salesforce admin, or the list price. Pricing calculations start with this value. |
| Prorate Multiplier |	The prorate multiplier is the subscription term divided by the default term. |
| Prorated List Price |	The price after the prorate multiplier is applied to the list price. |
| Volume Discount Schedules |	The volume discounts set on the quote line. |
| Option Discounts |	The discounts on the product option record. |
| Term Discount Schedules |	The discount schedules based on subscription product terms. |
| Discount Schedules |	If the quote line has discount schedules, Salesforce CPQ applies the appropriate discount to the prorated price. The resulting value becomes the regular unit price. |
| Regular Unit Price |	The unit price before additional discount. If it’s a special price, the type is overridden with Special Price. |
| Additional Discounts |	The discounts entered by the sales rep or using price rules. |
| Customer Unit Price |	The net price before partner discounts. Calculated by applying discretionary discounts to the regular unit price. |
| Partner Discounts |	Discounts entered by sales reps or using price rules. |
| Partner Unit Price |	The partner price is the customer unit price with a partner discount applied. |
| Distributor Discounts |	The discounts entered by sales reps or using price rules. |
| Net Unit Price |	The net price is the unit price after all discounts are applied. |

- `Block Pricing` - price a product based on several different quantity ranges, called block prices. It is defined in Related Tab of `Product` record. Then change Pricing Method to `Block`. An `overage rate` is a per-unit price for any quantity above our highest tier. To use overage rates, we must create a special custom field `OverageRate` on the Block Price object. This is a one-time step for any org using Salesforce CPQ. Be sure to create Block Price records for every currency. Block pricing isn’t compatible with an asset conversion of One per Unit.

- `Percent Of Total` - 
    - Update these 3 fields on Product - `Pricing Method`, `Percent Of Total (%)` and `Percent Of Total Base`. 
    - `Percent Of Total Base` determines which quote line prices are summed for the total. Each price is affected by different types of discounts: <img style="display: block; margin: auto;" src="./Applied Discounts on QL Fields.png"/> 
    - The `Percent Of Total Scope` field, found on product option records, determines which products inside or outside the bundle are included in the price calculation. <img style="display: block; margin: auto;" src="./Percent Of Total Scope field results.png"/> 
    - <ins> By default, percent of total uses only nonsubscription quote lines as part of the total used in the price calculation. </ins> 
    - However, you can tell CPQ to include certain subscription products in the percent of total calculation by checking the box named `Include in Percent Of Total`, found on the subscription product record. Be aware that when using Include in Percent Of Total, CPQ uses an unprorated price for the subscription in the total calculation.
    - Checking `Exclude from Percent Of Total` box excludes the product from all percent of total calculations.
    - Percent of total products never include other percent of total products in their calculations. 
    - We can also use `Percent Of Total Category`, which connects a percent of total product to other products that should contribute to the total. This fields needs to be set on all included/required products.
    - You can also limit the amount of Percent of Total. Use `Percent Of Total Constraint` to limit the amount by comparing it to it's price book price.

- **`Option pricing` does not support multicurrency**. If you need to adjust prices of options while supporting multicurrency, you can use option discounting.

- **[IMP] `Bundled`** PO Field - When that field is checked, CPQ ignores all pricing methods and zeros out the Regular Price, Customer Price, Partner Price, and Net Prices fields. The List Unit Price field states `Included` on QLE. By default, the quantities of these free products are not included in discount schedule calculations. If you need them to count toward a discount, check the `Include Bundled Quantities` checkbox on the Discount Schedule record. If the Bundled field is checked, CPQ prices the option at $0.00. If the Bundled field is left unchecked, then block pricing and percent of total pricing take precedence over option pricing. <img style="display: block; margin: auto;" src="./Bundled and Option Pricing.png"/>

- `Cost Plus Markup` - 
    - There’s no built-in limits to the markup value.
    - Values can be negative, to sell under cost. 
    - List Price field is not affected by cost plus markup.
    - We define `Unit Cost` on `Costs` object in Related tab of Product record. If you forget to add a cost record, CPQ treats the product as though its cost is $0. If your org supports multicurrency, you create a cost record for each different currency. Otherwise there’s no need to create multiple cost records for a single product.
    - Then update Pricing Method to `Cost` on Product record.

- `Contracted Pricing` - 
    - Account -> Related -> Contracted Prices -> Product and Price.
    - You can also use this to apply Account-wide Discount for Multiple Products. Instead of a single Product use a product filter, and instead of Price use Discount. The Filter Field should be on Product and twinned to QL.
    - `Effective Date` is a field on the contracted pricing record that's typically set to some day in the future. If a **quote line** is added to a quote on or after the effective date, the contracted price is applied, irrespective of Quote/Opportunity creation date.
    - `Expiration Date` field also respects when the quote line is created.
    - Quote lines have a lookup to the Contracted Price object, so that data can drive conditional logic.
    - By default, a contracted price created at the parent level is inherited by all of the children. It’s easy to disable inheritance by editing the child account record and checking the `Ignore Parent Contracted Prices` field.
    - Child contracted prices take precedence over parent contracted prices.
    - **Do not create two contracted prices on the same account that act on a single product**. The danger here is that there’s no way to dictate which contracted price is used.
    - At this time, products that use block pricing or percent of total pricing do not support contracted pricing. Avoid creating contracted prices for products using these price methods.

- The Price Editable checkbox does not unlock list price when using block pricing or percent of total pricing.

- Set the details of alternate pricing methods if the `Pricing Method Editable` field value is true. For example, if the Percent Of Total (%) field is left blank, the list unit price of the quote line will be set to $0 when percent of total is the selected pricing method. Similarly, selecting the block pricing method produces an error message if no block pricing records exist related to the product.

- The `custom pricing method` becomes available when a product is flagged as `Pricing Method Editable`. This allows the sales rep to manually change the customer unit price and overrides every pricing tool we’ve discussed throughout this module. It even overrides discounting methods.

- When a manual discount is added to a subscription product, CPQ deducts the amount from Customer Unit Price, Net Unit Price, and Net Total.