# Notes -

<ins>Terms</ins> - **Guided Selling** (ques Sales rep ask), **Bundle** (group of products), **MDQ** (Mulit-Dimensional Quoting), **AOM** (Advanced Order Management), **SKU proliferation** (when the number of similar products balloons to a level that’s very difficult to maintain), **Prorated Pricing** (CPQ calculates the price for just the portion of the term, not the whole term), **OverageRate** (per-unit price for any quantity above our highest tier). 

- The primary quote pushes the total quote amount into the Amount field on your opportunity. The Products related list also updates with the products from your quote. If you later make a different quote primary, your opportunity automatically updates to reflect the new details.

- When Salesforce CPQ creates a quote line from an opportunity product, the bundle products aren’t included in the quote line because the bundle structure data isn’t available in the opportunity product.

- Salesforce CPQ allows you to split a single quote into several orders so products and services are delivered across that same time period you set up.

- On amendment quotes, quote lines inherit twin field values from the first subscription record, not any amended subscriptions made from that record.

- A blank quantity field on Product Option (PO) causes CPQ to leave quantity editable, so the Quantity Editable field is ignored.

- To create `Configuration Field set`, I need to create a new Field Set on PO. Then I need to add Field Set Name in either Product object pickist or Product Feature picklist. You can only put fields from the Product Option object into the field set. If you want information from the Product record to appear, create a formula field on the Product Option object that returns the value pulled from the Product field. A feature-level field set overrides a product-level field set.

- It’s important to understand that `configuration attributes` are not actually fields, but are visible manifestations of fields. Think of it this way: When the Product Configuration page loads, what you see is really a page built from records—Product records, Option records, and Feature records. The page itself is not actually a record! And if the page isn’t a record, it can’t have its own fields. But what it can have is a representation of a field, and that’s what a configuration attribute is — a representation.

- **Product Feature** - If you try to use both (Shown Values and Hidden Values) at the same time, CPQ will default to the Hidden Values field. If you use Default Field to pre-set the value of the picklist to something that’s supposed to be hidden, it will still appear as a choice. If the sales rep chooses a different value and saves it, the defaulted value is hidden and can’t be selected again.

- **Global vs Configuration Attributes** - ![Global vs Configuration Attributes](./Assets/Global%20vs%20Configuration%20Attributes.png)

- After creating `global attributes`, they’re kind of floating out there with no home. To give them a little structure, you’re going to put them together into what is called an `attribute set`. Attribute sets are collections of global attributes that have something in common. Global attributes are related to attribute sets through a junction object called an `attribute item`, which has lookups to each of the other objects. This kind of relationship allows you to create more than one attribute set from the same collection of global attributes. Think of an attribute set as a playlist. One playlist has your favorite workout songs, another your favorite dance songs. Both draw from the same library, and some songs appear in both lists. The attribute item is like a playlist entry. The `product attribute set` junction object allows you to relate an attribute set to more than one product option. Also, it allows you to associate more than one attribute set to the same product option.

- **[Considerations for Global Attributes](https://help.salesforce.com/s/articleView?id=sf.cpq_global_attr_considerations.htm&type=5)**

-  **Product Rules** caveats :
    - CPQ only tests selected options, ignoring anything that’s unchecked.
    - The second caveat involves testing two separate options in the same rule. For example, you create 2 conditions : PC = 'Cream' and PC = 'Sugar'. When CPQ gets to the Cream option and performs the tests, the Sugar test will fail. It’s impossible for a single option to be both Cream and Sugar, so the rule will never run. The workaround is to use a summary variable to count one of the options.

- [Info on Subscription/Asset Target Object in Summary Variable](https://help.salesforce.com/s/articleView?id=000389486&type=1)

- The Discount (Amt) cannot be used on Product Options with multi-currency orgs.

- **[CPQ Package Settings](https://help.salesforce.com/s/articleView?id=sf.cpq_package_settings.htm&type=5)**
    - `Net Total` is Default value in `Totals Field` and `Line Subtotals Total Field` picklist in Line Editor.
    - `Actions Column Placement` - Places the Delete and Edit actions to the left or right of quote lines in the quote line editor.
    - Default value for `Quote batch size` is 150.
    - `Default Quote Validity (Days)` - The **quote’s expiration date** equals the quote’s creation date plus the default quote validity value. If the default quote validity is null, the quote’s expiration date is null. The quote’s expiration date has no default functionality, but it can be used in custom automation.
    - You can also set `Default Order Start Date`.

- **[CPQ Pricing](./Assets/CPQ%20Pricing/CPQ%20Pricing.md)**

- Review the following Product Record Fields when encountering sync issues between Opportunity and CPQ Quote in Salesforce :

    - Active checkbox is True.
    - Optional checkbox is False.
    - Exclude from Opportunity checkbox is False.
    - Verify that the Product record has an active price for the Price Book on the Quote.

- When you work with split orders, review important guidelines.

    - Percent of Total products can’t be ordered separately from their covered assets.
    - Component product options can’t be ordered separately from their parent bundles.
    - You can split a bundle with accessories or related products into multiple orders. However, if you contract both orders into one contract and renew or amend the contract, Salesforce CPQ doesn’t reassemble the bundle on renewal or amendment quotes.
    - You can order different quantities of an entire bundle. However, if the bundle contains an asset, Salesforce CPQ doesn’t reassemble the product options under their respective parent bundle on renewal or amendment quotes.

- To enable percent of total pricing on a subscription product, set its Subscription Pricing field to Percent of Total and its Pricing Method to List. You can also add percent of total pricing to a one-time product by changing its Pricing Method field to Percent of Total.

- [QnA](./QnA.md)