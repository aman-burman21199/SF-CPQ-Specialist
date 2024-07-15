# CPQ Pricing -

1. [Basics](./Basics/CPQ%20Pricing%20Basics.md)

2. [Subscription Pricing](./Subscription%20Pricing/Subscription%20Pricing.md)

3. [Discounting](./Discounting/Discounting.md)

4. [Price Rule](./Price%20Rule/Price%20Rule.md)

- Quote lines flagged as `optional` do not sync to the opportunity, which keeps forecasting accurate. `Optional` flag is set on QLE. The QL's total is not added to Quote's total. It’s worth noting that quote lines flagged as optional are never counted in the aggregated quantity for a DS (field - Aggregation Scope). Optional items are meant to show customers what is possible, not what they actually get.

- To know which Quote fields are `Calculating Fields` (one that CPQ knows might affect prices in some way), go to Calculating Fields Field Set on the quote object. The following fields are part of Calculating Fields by default and <ins>still trigger calculations even if you remove them from the field set</ins>.

    - Additional Discount (%)
    - Distributor Discount
    - End Date
    - First Segment Term End Date
    - Markup (%)
    - Partner Discount
    - Primary
    - Start Date
    - Subscription Term

- By default, Salesforce CPQ calculates quote prices in real-time based on user actions and changes to fields in the quote’s Calculating Fields field set. If you want Salesforce CPQ to calculate prices only when a sales rep clicks Quick Save or Save in the quote line editor, you can disable real-time calculation. In Package settings, on the Pricing and Calculations tab, deselect the `Calculate Immediately` field.