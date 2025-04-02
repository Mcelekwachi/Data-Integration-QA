# Data-Integration-QA
This project focused on unifying and analyzing customer data across three core systems — Stripe (Payments), Salesforce (CRM), and NetSuite (Finance) — to assess integration accuracy, identify mismatches, and suggest system-wide improvements.
Part 1 – Entity Relationship Modeling & Data Cleaning 
I began with an ERD to map how Stripe_ID, Account_ID, and Customer_ID connect across systems. After that each dataset was cleaned independently:
Stripe: Fixed inconsistent European/US number and date formats, flagged future dates, and normalized currency values.
Salesforce: Cleaned and standardized key fields (Account_ID, Email, Phone_Number), ensuring they matched with Stripe.
NetSuite: Parsed revenue and date fields, then aligned Customer_ID as the primary key while preserving Stripe_ID for joins.

Part 2 – Data Integration, Analysis & Findings:
I performed two merges:
Initial Merge: Linked Stripe → Salesforce via Customer_ID and then joined NetSuite using Stripe_ID. Low overlap suggested inconsistencies.
Improved Merge (used for analysis): A structured two-step approach joining Stripe → Salesforce via Metadata_SAL_ID, then to NetSuite via Stripe_ID. This provided a cleaner and broader match across systems.
Findings:
Largest Payment: Jamie Taylor made the highest Stripe payment of €1,984.31.
Most Transactions: Riley Moore had the highest number of transactions (2), based on Customer_ID.
Revenue vs Cash: Stripe cash received per customer was calculable, but NetSuite revenue returned NaN due to join inconsistencies.
Connection Issues: Inconsistent identifiers and partial overlaps across datasets caused incomplete integration and hindered analysis.
Part 3 – System Improvements
To ensure 100% matching going forward:
Architectural Change: Introduce a universal Customer_ID (UUID) created at CRM level (Salesforce) and propagated to Stripe and NetSuite via APIs.
Real-Time QA: Build reconciliation pipelines and alerts for mismatches or missing values. Validate relationships daily.
AWS Automation:
Glue/DataBrew: For data prep and cataloging
Lambda: Matching & validation logic
S3 + CloudWatch + SNS: Storage, monitoring, and alerts
QuickSight: QA dashboard
Result: Automated, scalable data governance across systems with faster issue detection and reliable reporting.
Part 4 – Expanding connections
New Data Source: CustomerSupportActivity
A centralized support system adds behavioral and retention insights:
Links complaints or failed payments with Stripe refunds or churn risk.
Reveals root causes behind revenue loss (e.g., billing issues).
Enables proactive fixes by surfacing trends in support queries.
Schema Highlights:
Customer_ID (FK to other systems)
Timestamps (Date_Opened, Date_Closed)
Fields like Interaction_Type, Channel, Status, Satisfaction_Score
100 mock rows were generated using Faker and exported as customer_support_activity.json. The updated ERD now includes this source, using Customer_ID as a unified relational key across all systems.
