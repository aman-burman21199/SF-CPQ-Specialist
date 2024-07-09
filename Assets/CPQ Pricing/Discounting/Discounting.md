# Discounting -

- `Option Discount` - <img style="display: block; margin: auto;" src="./Option Discount.png"/>

- `Volume-based Discount`  using `Discount Schedules` (DS) -
    - `Range` - applies same discount to total qty (Products are discounted at the rate of the tier that applies to the quoted quantity). Price = $(Total Quantity) * List Price * (1-\% discount)$.
    - `Slab` - applies different discount based on tiers and totals it (Units within a certain bound receive discounts equal to their tier’s discount value.). Total list price for Slab-based DS = $(quantity within Tier 1) * list price * (1 - \% discount) + (quantity within tier 2) * list price * (1 - \% discount)$. Slab discount schedules do not work with cross products and aggregation scope.
    - DS can be applied on various objects - <img style="display: block; margin: auto;" src="./Volume Discounts on Objects.png"/> 
    - By default, discount schedules apply to all quotes, regardless of which price book is used. However, it is possible to identify price books that should not use a given Discount Schedule,by puting pricebook ids in a comma-sparated list on the `Excluded Pricebook IDs` field on DS.
    - `Aggregation Scope` field on DS tells CPQ how to handle multiple quote lines that share the same discount schedule (The DS <ins>should be applied</ins> on QL. Won't consider qtys who don't have DS on QL). It’s worth noting that quote lines flagged as optional are never counted in the aggregated quantity. Optional items are meant to show customers what is possible, not what they actually get.  When an MDQ product falls under an aggregation scope, Salesforce CPQ uses the sum of the quantity in all segments to determine the quote line’s discount tier. <ins> Slab discount schedules don’t support aggregation scopes </ins>.
        - `None`: This setting is the default value, and causes CPQ to treat each quote line independently; no aggregation is performed. 
        - `Quote`: This setting counts the quantities of every quote line across the entire quote. 
        - `Group`: This setting counts quantities of quote lines that are in the same quote line group.
    - By default, the quantities of free products (Bundled checkbox on the product option) are not included in discount schedule calculations. If you need them to count toward a discount, check the `Include Bundled Quantities` checkbox on the Discount Schedule record.
    - `Cross Products Aggregation` - When same DS on different product's QLs.
        - check the field named `Cross Products` on the Discount Schedule record. Then both products are discounted at same rate.
        - <ins>Cross Products only works if the Aggregation Scope field is set to either Quote or Group</ins>
    - `Cross Order Aggregation` - configure DS to count the amount of products sold in previous sales by looking to <ins>asset records associated with the customer’s Account</ins>.
        - check the `Cross Orders` field on the Discount Schedule record.
        - <ins>Cross Orders only affects the price of the product on the current quote. It doesn’t retroactively discount previous sales, nor does it discount the current sale to make up for what the customer would’ve saved if they had bought everything at once.</ins>
        - Be aware that Cross Orders uses all assets records a customer has when aggregating the total quantity to use for the discount schedule. This includes assets that were bought years ago!
        - However, there are times when we don’t want every single record to count toward the total quantity. To filter those assets, create a field on Asset object based on some custom logic (like purchased over the last year). Then you create a field on Quote using same asset field name which return "true". Finally you update `Constraint Field` field picklist on DS to include the asset API, and update you DS record.
    - By default, Salesforce CPQ uses the `Quantity` field on the quote line to determine which discount tier to use. You can update `Quote Line Quantity Field` picklist field on DS to any custom quantity field. Discount schedules using custom field on the Quote Line Quantity field do not support cross order functionality to aggregate quantities from asset records. [More Info](https://help.salesforce.com/s/articleView?id=sf.cpq_custom_ds_concept.htm&type=5)
    - If your org is configured to use multicurrency, discount schedules that use amount-based discounts require discount tiers for each currency.
    - `Override Behavior` - controls if sales reps can modify discount tiers. When sales reps save changes in the discount schedule editor, a clone of the original discount schedule is saved and used only for this one quote. The source discount schedule remains untouched. Cloned discount schedules have a lookup field to the original in case sales reps want to revert. They also have lookups to the quote and account records. Lastly, they have a checkbox field named User Defined that is checked, making it easy to identify custom discount schedules.
        - `--None--`: This makes all values fixed and noneditable by the sales rep.
        - `All`: This lets the sales rep change the upper and lower bounds, as well as the percent or amount for each discount tier. Sales reps have complete control over the discount schedule.
        - `Current Tier Only`: This lets the sales rep change the percent or amount for the Discount Tier based on the current quantity. Although the lower and upper bounds are editable in the user interface, they don’t affect the calculation.

- `Volume-based Discount` using `Compound Discount` on Product - 
    - `Compound Discount` field on Product.
    - CPQ calculates and adjusts the `Regular Price`.
    - $1/(Quantity^(Compound Discount/100))$
    - Both compound discount and discount schedules affect regular price, but only the compound discount applies if both are used at the same time.

- [Manual Discounts](https://trailhead.salesforce.com/content/learn/modules/discounting-tools-in-salesforce-cpq/apply-manual-discounts?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-cpq-specialist-credential)

- [Partner and Distributor Discounts and adjustments to Price Waterfall](https://trailhead.salesforce.com/content/learn/modules/discounting-tools-in-salesforce-cpq/configure-partner-and-distributor-discounts?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-cpq-specialist-credential)