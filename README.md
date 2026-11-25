# Revenue-Intelligence-Dashboard
Revenue Intelligence Dashboard for NovaTech Solutions - AWS AI &amp; ML Scholars Program (Udacity)

<h3>Project Overview & Business Objective</h3>

NovaTech's data is siloed across three systems: 
1. CRM (Salesforce-style) — tracks the sales pipeline: deals, stages, reps, win/loss outcomes, and deal values.
2. Marketing analytics platform — tracks campaign performance: leads generated, channel attribution, spend, and response rates.
3. Support ticketing system — tracks customer issues: priorities, resolution times, product areas, and customer sentiment. These systems do not talk to each other. Every Monday morning, the revenue team manually pulls reports from all three tools to answer basic cross-functional questions. This process wastes roughly an hour per week and still leaves gaps — nobody can easily answer questions that span systems, such as:
   
 "Which marketing campaigns are actually driving closed deals?" 
 
 "Are our highest-value customers also the ones filing the most support tickets?" 
 
 "What's the relationship between campaign channel and eventual deal win rate?"

To eliminate manual reporting and establish a single source of truth across the customer lifecycle, we designed and deployed the NovaTech Revenue Intelligence Dashboard and a dedicated Amazon QuickSight Q Topic (an artificial intelligence natural language querying layer that enables business users to ask plain-English questions of underlying datasets). Together, these tools unify marketing funnel performance, sales pipeline velocity, and customer health indicators into one interactive decision-support environment.

<h3> Data Strategy, Preparation & Unified Join Architecture</h3>

Our data architecture strategy centered on transforming raw transactional records into a unified, analysis-ready semantic model designed for executive decision-making:
1. Data Ingestion & Verification: We ingested historical CRM deals (499 records, $707,201 in closed won revenue), marketing campaigns (2,240 records, $12.36 million in marketing expenditure), and customer support tickets (3,000 records). Pre-indexed knowledge bases were rigorously verified against the company data dictionary, achieving a 100% pass rate across record counts, revenue benchmarks, and priority distributions.
2. Transformations & Calculated Business Metrics: Date fields were standardized into analytical calendar dates, and Account Identifiers were classified as categorical dimensions rather than numerical measures to prevent erroneous mathematical summation. We engineered key calculated business indicators, including Sales Cycle Duration (measuring the elapsed days between deal creation and deal closing), Marketing Campaign Return on Investment (measuring the net financial return generated relative to marketing campaign spend), and Support Ticket Resolution Duration (tracking the elapsed days required to resolve customer issues).
3. Unified Three-Way SPICE Join: Using CRM Deals as our foundational anchor dataset, we performed Left Outer Joins to both marketing campaigns and customer support tickets on matching Account Identifiers. This structural decision ensures that every sales deal is fully preserved regardless of whether an account has logged an active marketing interaction or support ticket, while providing complete cross-functional visibility into customer accounts.


<h3> Dashboard Design Rationale & Interactive Architecture</h3>

<b>Marketing Funnel</b>
The sheet provides a comprehensive overview of campaign performance, tracking lead-to-deal conversion, spend, ROI, revenue, and channel effectiveness across marketing campaigns.
* Despite a total campaign spend of 12,359,497.34, only 1,127,223.09 in revenue was generated, with all campaigns showing negative average ROI—the best being NovaPulse Launch at -0.81.
* Of 2,240 leads in the funnel, only 412 reached Closed Won, revealing a steep drop-off from the 848 at the Qualified Lead stage.
* Partner Referral dominated as the top channel, driving 375 of 1,076 total leads and 277 of 609 campaign responses.
* Paid Social and Organic Search followed as notable response drivers with 131 and 97 responses respectively.
  
<b>Sales Pipeline</b>
The sheet provides a comprehensive sales performance dashboard analyzing deal values, win rates, revenue segmentation, loss reasons, and closure timelines across products and representatives.
* NovaPulse Ultimate has the highest average deal value at 26,588 and a perfect win rate of 1, yet contributes the lowest total revenue at 14,439.4, suggesting very few deals closed.
* A significant performance gap exists among sales representatives, with Marcus Webb leading at a 0.8 win rate while Finn O'Brien trails at 0.32.
* Enterprise companies generate the highest average deal value at 1,589.17, while Software and Technology are the top revenue-contributing industries at 293,362.73 and 289,430.85 respectively.
* Most deals close quickly, with the highest record counts at 2 and 3 days to close, while Budget Constraints and Timing Not Right are among the least frequent loss reasons.

<b>Customer Health</b>
The sheet provides an overview of support ticket operations, analyzing resolution times by priority, ticket distribution across products and accounts, and customer sentiment trends.
* Resolution times are nearly identical across priority levels, with low at 1.97 days, high at 1.92, and medium at 1.92, suggesting priority has minimal impact on resolution speed.
* Notifications generates the most tickets at 601, while Data Pipeline has the fewest at 310 out of 3,000 total tickets.
* ACCT-041 dominates ticket volume with 334 tickets, while several accounts have just 1 ticket, revealing highly uneven distribution.
* Neutral sentiment overwhelmingly dominates with 1,953 records, while positive sentiment is the smallest segment at only 304.


<h3> Impact of Topic Configuration on AI Natural Language Querying (Quick Chat)</h3>

We tested Quick Chat both before and after configuring the NovaTech Revenue Intelligence Topic:
* Baseline Performance (Before Topic Configuration): When scoped only to the published dashboard without a configured Topic, Quick Chat accurately answered questions whose exact numbers appeared on KPI cards (such as confirming $707,201 in closed-won revenue and a 63.1% win rate). However, when asked about unplotted dataset columns or business synonyms—such as total ad budget and average Campaign ROI by marketing channel—Quick Chat returned overall spend ($12,359,497.34) and channel response counts (Partner Referral 277, Paid Social 131, Organic Search 97, Direct Mail 79, Email 25), but stated that Campaign ROI by channel was unavailable because it was not displayed on a dashboard visual.
* Post-Topic Improvements (After Semantic Enrichment): We configured the Topic by enabling underlying analytical fields, excluding noisy system identifiers, and mapping business synonyms (such as "ad budget" for Campaign Spend, "ROI" for Campaign ROI, "sales cycle" for Days to Close). After configuration, Q answered the same ROI question it had previously declined. It returned total spend of $12,359,497.34 and an average Campaign ROI of −0.90 across five channels, with Direct Mail performing best (−0.64) and Organic Search worst (−0.96). Resolution Time Days (1.95 days across 2,941 resolved tickets).
