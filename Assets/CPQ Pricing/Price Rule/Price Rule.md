# Price Rule -

1. There is a big limitation to price action formulas: **You may only reference data on objects immediately related to the quote, quote line, or quote line group**.

2. Try targeting only `List Price` and `Special Price` in Price Actions. Rest all are prorated prices. You usually use `Special Price` when applying action after price from PO.

3. `Special Price` has a unique quirk that you must address. Anytime you use a price rule to update Special Price, you must also update its companion field, `Special Price Type`, to have a value of "Custom". If you don’t change Special Price Type to Custom, the price rule won’t work.
    - If the special price type is `Contract`, your special price uses the value of the current contracted price record that is active for the account associated with your quote. Remember that the contracted price uses either a price or a discount. If it uses a price, your special price inherits that value directly. If it uses a discount, Salesforce CPQ applies that discount to your product’s list unit price and sends the result to your special price.
    - If the special price type is `Custom`, admins can provide their own value in the special price field. Salesforce CPQ uses the special price in calculating the quote line item price. However, the quote line item still maintains a value for the list unit price. If the list price is zero, the special price doesn’t affect the calculation.
    - Amendment quote lines don’t use special price calculations from the source subscription or original quote line. They use the original quote line’s list unit price instead. If your company supports amendments, we recommend not using the special price field.

4. It is critical that your combination of lookup queries never return more than one row. If they do, an error message appears in the Quote Line Editor and calculation will fail, which is a showstopper!

5. Quote Calculation Sequence Summary: [Complete List Here](https://help.salesforce.com/s/articleView?id=sf.cpq_quote_calc_process.htm&type=5) <img style="display: block; margin: auto;" src="./Quote Calculation Sequence Summary.png"/> 

6. Every condition is evaluated before any actions are taken in a given evaluation event. So if 2nd rule condition relies on 1st condition action, then 2nd rule will not run on first calculate. To solve this, you can move the first rule into an earlier evaluation event. Or you could move the second rule into a later evaluation event. 

7. [Lookup Queries for Large Datasets](https://help.salesforce.com/s/articleView?id=sf.cpq_lq_large_data.htm&type=5)

8. Salesforce CPQ doesn’t support On Initialization, Before Calculate, or On Calculate formulas that reference the Prorate Multiplier field or any unit price other than the List Price field. The values of those fields aren’t accurate until after the rule event’s completion.

9. Salesforce CPQ doesn’t support On Initialization, Before Calculate, or On Calculate formulas that reference total price fields other than component totals and package totals. Noncomponent and nonpackage totals get their values following calculation, so a price rule can’t reference them during the first calculation sequence.

10. When a price action without price conditions targets the Quote, the action evaluates the quote line or quote line group with the lowest Number__c value. We recommend using price conditions to ensure that your price action targets the correct record.

11. Price rules targeting the Additional Discount (%) or Additional Discount (Amt) fields must have an action that sends a null value to the other Additional Discount field.

12. When you’re using price conditions, and a picklist field in the Field parameter then comparing against a filter value, the filter value must be surrounded by single quotes.