
Purpose

Explore market-basket analysis on a public transactional dataset using Apriori and FP-Growth. You’ll prepare data, mine frequent itemsets, generate association rules (support, confidence, lift), and visualize patterns with Seaborn to show how these methods uncover actionable co-purchase signals.

Dataset & Preparation
Source: UCI Online Retail (Excel).


After cleaning:


Transactions: 18,536


Unique items: 3,866


Avg items/transaction: 20.92


Cleaning steps: removed cancellations (InvoiceNo starting with “C”), non-positive quantities, nulls; normalized descriptions (strip + lowercase); grouped by InvoiceNo; deduped items within a basket.


Due to combinatorial growth (L1≈1,174 at support count 100 ⇒ |C2|≈688,551), we sampled 200 transactions for from-scratch implementations and used min_support = 0.05 to keep runtimes reasonable.


EDA Visuals
Top-20 items barplot – highlights most common SKUs (e.g., white hanging heart t-light holder, regency cakestand 3 tier, jumbo bag red retrospot).


Item co-occurrence heatmap (Top-20) – reveals strong pairs among popular gift and kitchen items.


Methods
Apriori
From-scratch candidate-generation and counting on the 200-basket sample.


min_support = 0.05 (relative to sample size).


Output: frequent 1- and 2-itemsets with support.


FP-Growth
From-scratch FP-tree (header table, node links, conditional trees).


Same min_support = 0.05.


Output: frequent itemsets + supports (validated against Apriori on the sample).


Key Results
Frequent Itemsets (examples)
Singles with highest support (sample):
('jumbo bag red retrospot',): 0.13


('regency cakestand 3 tier',): 0.105


('party bunting',), ('lunch bag red retrospot',): 0.085


('set of 3 cake tins pantry design',), ('white hanging heart t-light holder',): 0.08


Pairs (sample):
('jumbo bag red retrospot', 'jumbo storage bag suki'): 0.055


('lunch bag black skull.', 'lunch bag pink polkadot'): 0.055


Barplots for Top-10 itemsets are included for both algorithms.
Association Rules (min_conf = 0.60)
Examples (support, confidence, lift):
('lunch bag black skull.',) → ('lunch bag pink polkadot',)
 support 0.055, confidence 0.9167, lift 13.0952


('jumbo storage bag suki',) → ('jumbo bag red retrospot',)
 support 0.055, confidence 0.7857, lift 6.0440


Scatter (Confidence vs Lift) highlights a couple of high-lift, high-confidence rules around lunch-bag variants and “retrospot” bags.


Comparative Analysis
Output parity: On the 200-basket sample, FP-Growth and Apriori surfaced the same top itemsets (as expected when thresholds match).


Efficiency: FP-Growth was noticeably faster than Apriori due to tree compression and avoiding explicit candidate explosion. Apriori’s cost grows sharply with larger Lk; FP-Growth scales better on dense baskets.


Why: Apriori repeatedly scans the dataset and evaluates a large candidate space (e.g., estimated |C2| in the full set was ~688k); FP-Growth compresses transactions once into an FP-tree and mines conditional trees.


Challenges & Decisions
Combinatorial blow-up: Full dataset with modest support leads to huge candidate sets.
 Mitigation: Sampled 200 baskets and raised min_support to 0.05 for from-scratch code to finish quickly while still producing meaningful patterns.


Memory/visual clarity: Co-occurrence on all items is too large.
 Mitigation: Visualized Top-20 items.


Algorithm correctness: Verified FP-Growth itemsets by recomputing supports and cross-checking with Apriori outputs on the same sample.



Repository Checklist
MSCS_634_Lab_6.ipynb – notebook with code, outputs, and visuals.

README.md – this file.


Insights for a Retailer
Bundle recommendations: High-lift rules between lunch-bag variants and “retrospot” products suggest cross-selling sets and themed bundles.


Inventory planning: Top singles and strong pairs can inform display adjacency and promo pairings.


Email/promo targeting: Rules with lift > 1 identify items to feature together in campaigns.



