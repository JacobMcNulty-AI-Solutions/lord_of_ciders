# Summary: Rarity System Research Results

## Key Findings
This document outlines a comprehensive approach to designing and implementing a rarity system for ciders within the app. It covers data sourcing, objective rarity criteria, database design, user submission workflows, and maintenance strategies.

-   **Data Sourcing**: Global production/consumption data (IARD, NACM, ACA), distribution networks (supermarkets, e-commerce, national beverage authorities), and existing databases (Open Brewery DB) are key sources. Commercial databases are costly.
-   **Objective Rarity Criteria**: Rarity should be determined by a mix of factors beyond raw volume:
    -   **Production Scale**: Small-batch, limited-run ciders are rarer.
    -   **Geographic Exclusivity**: Ciders sold in only one country/region.
    -   **Limited Editions/Seasonal Products**: Special releases.
    -   **Unique Ingredients/Methods**: Rare apple varieties, wild fermentation, special aging.
    -   **Historical/Cultural Significance**: Heritage brands, archival vintages.
    -   **Awards/Recognition**: Prestigious awards.
    -   **Availability Metrics**: Number of countries/points of sale.
-   **Database Design**: A relational schema with `Cider`, `Producer`, `Region`, and `RarityTier` tables is recommended. The `Cider` table should include `rarity_tier` and underlying metrics (e.g., `production_volume`, `distribution_score`, `limited_release_flag`). Auxiliary tables for tags, sources, and user suggestions are also needed.
-   **User Submissions**: Users should be able to add new ciders via a guided form, with search-first, detail entry, duplicate checks, and a submission/review process. This should be similar to Untappd's approach.
-   **Rarity Assignment (User Submissions)**: The system should suggest a rarity tier based on input, preset defaults, or algorithmic suggestions. Community voting and moderator oversight are crucial to prevent abuse.
-   **Maintenance Strategy**: Regular updates are needed through automated data refresh (cron jobs, Supabase Edge functions), community monitoring, and scheduled re-evaluation of rarity scores. ETL pipelines, data quality checks, backend services (Supabase RLS, triggers), and backup/versioning are essential tools.

## Actionable Insights for System Design

1.  **Comprehensive Cider Data Model**: Design the `Cider` table to include fields for `rarity_tier` (enum or FK), `production_volume`, `geographic_distribution`, `is_limited_edition`, `unique_ingredients_methods_tags`, `awards_received`, and `historical_significance_flag`. This will support the objective rarity criteria.
2.  **Initial Database Seeding**: Utilize Open Brewery DB and the `ciders_from_list_wiki.md` data to seed the initial `Cider` and `Producer` tables. Implement an ETL pipeline to regularly pull and upsert data from these and other public sources.
3.  **Algorithmic Rarity Assignment**: Develop an algorithm or scoring rubric that combines the objective rarity criteria to automatically suggest a rarity tier for each cider. This can be run periodically to re-evaluate rarity.
4.  **Guided User Submission Workflow**: Implement a user-friendly form for adding new ciders. This workflow must include:
    -   **Search-first**: Prompt users to search for existing ciders before adding new ones.
    -   **Mandatory Fields**: Ensure essential data (name, producer, type, ABV, origin) is captured.
    -   **Duplicate Check**: Implement fuzzy matching against the existing database to prevent duplicates.
    -   **Rarity Suggestion**: Provide an algorithmic suggestion for rarity based on user input.
    -   **Review Process**: New submissions should be marked as "pending" and require verification by moderators or trusted users.
5.  **Moderation Tools**: Develop a backend interface for moderators to review, approve, reject, and manually adjust rarity tiers for user-submitted ciders. Implement community voting or flagging mechanisms.
6.  **Automated Data Refresh & Quality Checks**: Set up scheduled jobs (e.g., Supabase Edge Functions) to automatically refresh data from external APIs and perform data quality checks (e.g., detect anomalies, flag records for review).
7.  **Soft Deletes for Ciders**: Implement soft deletes (`deleted_at` timestamp) for ciders to manage discontinued products and ensure proper data reconciliation during sync.
8.  **Gamification - Collector's Ring Integration**: Ensure the rarity system directly feeds into the "Collector's Ring" progression, allowing users to track and be rewarded for discovering rare, epic, legendary, and mythical ciders.
9.  **Provenance Tracking**: Include fields like `source_api` or `entered_by_user` to track the origin of each cider entry, aiding in data validation and maintenance.
10. **Scalable Database (Supabase)**: Leverage Supabase's Postgres capabilities, including JSONB for flexible attributes and Row-Level Security (RLS) for managing user submissions and moderation workflows.