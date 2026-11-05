# kdd_online_retail.ipynb

*A complete Knowledge Discovery in Databases workflow using online retail transactions*

## Introduction

Association-rule mining is often reduced to one line of code: run Apriori and print the rules. That approach misses the most important part of Knowledge Discovery in Databases. Useful knowledge depends on selecting the right population, cleaning the records, choosing a meaningful representation, interpreting patterns, and translating them into decisions.

This project applies the KDD process to the Kaggle E-Commerce Data dataset. The objective is to discover products purchased together and recommend possible bundles or cross-selling experiments.

## The Dataset

The uploaded file contains 541,909 transaction-line records and eight columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, and Country.

The file includes product descriptions with non-UTF-8 characters, so the notebook loads it with latin1 encoding.

## Domain Understanding and Selection

The business question is: which products are repeatedly purchased together strongly enough to justify a bundle or cross-selling experiment?

The initial analysis uses the United Kingdom as the market scope. This creates a more coherent population for support calculations and avoids treating all countries as one identical market.

The output is not an automatic recommendation system. It is a ranked list of merchandising hypotheses for review.

## Preprocessing

The source file contains cancellations and returns. Invoice numbers beginning with C are treated as cancellations and excluded from the positive-purchase analysis.

The preprocessing pipeline also removes exact duplicate rows, missing product descriptions, non-positive quantities, and non-positive unit prices. The 541,909 source rows become 524,878 cleaned rows. The UK subset contains 18,019 transactions and 3,996 products.

These choices are assumptions, not universal truths. A separate KDD iteration could analyze returns, cancellation behavior, customer segments, or country-specific rules.

## Transformation

Association mining requires a transaction-by-item representation. The notebook groups records by invoice and product, sums quantities, and converts the result to Boolean presence. Each row represents one invoice. Each product column is True when the product appears in that invoice.

The most common products include the White Hanging Heart T-Light Holder, Jumbo Bag Red Retrospot, Regency Cakestand 3 Tier, and Party Bunting.

## Mining

The project uses Apriori with minimum support of 0.03, minimum confidence of 0.35, and lift as the ranking metric.

The algorithm produced 145 frequent itemsets and 20 association rules.

## Pattern Evaluation

Support measures the proportion of transactions containing an itemset. Confidence measures how often the consequent appears when the antecedent appears. Lift compares the rule’s conditional frequency with the consequent’s baseline frequency.

A lift greater than 1 indicates a positive association. It does not prove that one product causes the other to be purchased.

One of the strongest discovered rules was:

Customers who bought PINK REGENCY TEACUP AND SAUCER also bought GREEN REGENCY TEACUP AND SAUCER with approximately 82.1% confidence, 15.86 lift, and 3.2% support.

This is a strong merchandising hypothesis, but it still needs operational review. The products may already be part of a coordinated collection, and showing them together may not create incremental revenue.

## Knowledge Consolidation

The final artifact is a ranked recommendation table. For each rule, the merchandising team should check product compatibility, gross margin, inventory availability, seasonal effects, customer segment, existing catalog placement, average order value, and return behavior.

The safest next step is a controlled experiment. Compare a treatment group that sees a bundle or cross-sell recommendation with a control group that does not. Measure incremental revenue, margin, conversion, and returns.

## Lessons Learned

KDD is broader than association-rule mining. Apriori produces patterns, but the KDD process turns patterns into knowledge by connecting them to a domain decision and requiring interpretation.

The most important limitation is causality. A product association describes observed purchase behavior. It does not establish that recommending the products together will cause customers to buy both.

## Reproduce the Project

- GitHub: Replace with your public repository URL
- Google Colab: Replace with your Colab URL
- Walkthrough video: Replace with your YouTube URL

## References

- Dataset: https://www.kaggle.com/carrie1/ecommerce-data/data
- KDD framework: https://cdn.aaai.org/KDD/1996/KDD96-014.pdf
