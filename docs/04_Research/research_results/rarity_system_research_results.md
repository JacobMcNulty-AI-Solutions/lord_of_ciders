Data Sourcing for Rarity

Global production and consumption data: Cider production and consumption is tracked by a few specialized sources. For example, the International Alliance for Responsible Drinking (IARD) publishes beer & cider statistics compiled by GlobalData covering “over 200 countries”
iard.org
. National trade associations also collect data – e.g. the UK’s National Association of Cider Makers (NACM) reports the UK industry produces about 750 million litres of cider annually
en.wikipedia.org
. Likewise industry bodies like the American Cider Association (ACA) commission annual surveys of US cider-makers. Most detailed volumes by country come from paid market‐research firms (GlobalData, Euromonitor, IBISWorld, etc.), but some national stats are public – for example, Statistics Canada reports provincial cider‐and‐cooler sales (377 million litres in 2023/24)
www150.statcan.gc.ca
.

Distribution networks and availability: Information on distribution is sparser and usually inferred. Market reports note that cider is sold mainly through supermarkets/hypermarkets and e-commerce in leading markets
fortunebusinessinsights.com
fortunebusinessinsights.com
. For example, one analysis finds that in North America and Europe major retailers (supermarkets, hypermarkets) and online channels carry most cider brands
fortunebusinessinsights.com
fortunebusinessinsights.com
. National beverage authorities also track where cider is sold – e.g. the U.S. TTB and state liquor boards list permitted brands and distributors (often via downloadable CSVs). Open datasets exist too: Open Brewery DB provides a free, public API listing breweries and cideries worldwide (over 30,000 entries)
openbrewerydb.org
. This lets us identify which countries and regions have cider producers, even if volumes are unknown.

Existing cider databases/APIs: Aside from OpenBreweryDB, there is no single open global “cider product” database. Wine and beer reference sites (like Wine-Searcher or RateBeer) have large lists but limited APIs. Trade groups (NACM, ACA, European AICV) maintain member lists of producers, but these are often not public. In practice, we expect to populate our app database from multiple sources: the OpenBreweryDB (CC0 license) for breweries/cideries and locations
openbrewerydb.org
, supplemented by scraped data from government registries (e.g. TTB/TTB’s importer lists, European Union export/import stats) and user submissions. Commercial databases (GlobalData, Nielsen, Mintel, etc.) exist but require costly subscriptions; they may be referenced indirectly (e.g. citing their reported figures). We will rely on publicly available data and industry association reports whenever possible, noting licensing (openbrewerydb is open, most gov’t data is public domain, private market reports are not).

Objective rarity criteria: To assign cider rarity tiers (Common → Artifact) we must go beyond raw volume. Useful factors include:

Production scale: small-batch or limited-run ciders are rarer. For example, a micro‑cidery releasing 1000 bottles is rarer than a mass-market brand.

Geographic exclusivity: ciders sold in only one country, region, or locale should score higher rarity. (E.g. a farm-produced cider only in one county, vs. a UK-wide brand.)

Limited editions or seasonal products: special releases (festive specials, barrel‑aged vintages, one-off batches) are inherently rarer.

Unique ingredients or methods: ciders made from rare apple varieties, wild fermentation, or aged in special barrels can be flagged rare.

Historical or cultural significance: heritage brands or archival vintages (like a legendary “first vintage” or a famous cider from 19th century) might merit higher tiers.

Awards and recognition: a cider that won a prestigious award (Golden apple, ICS medal) could be considered “higher rarity” in gameplay.

Availability metrics: number of countries or points of sale carrying the cider. For example, a global brand like Heineken’s Strongbow is common (low rarity) because it’s sold everywhere; a local craft perry might be legendary or mythical by contrast.

In short, we would codify rarity using mix of data: reported production volumes (where available), distribution breadth, and qualitative tags (limited-edition, local‑only, organic, etc.). These factors can be combined into an algorithm or scoring rubric to assign tiers consistently. (For instance, our system might auto-flag any cider with production <1000 cases and only one retail location as “Epic” or higher.)

Database Design for Rarity

Core schema: We recommend a relational schema with separate tables for Cider, Producer (Cidery), Region, and RarityTier. For example:

Producer table (id, name, country, subregion, coordinates, website, etc.). Producers link to regions (countries/continents).

Cider table (id, name, producer_id, style/type (still/sparkling, fruit, perry, etc.), ABV, ingredients, year, description).

Rarity fields: In the cider table include a field like rarity_tier (enum or foreign key referencing Common→Artifact), plus fields for underlying metrics (e.g. production_volume, distribution_score, limited_release_flag). We can update rarity_tier programmatically or via moderation later.

Auxiliary tables: Tags or categories (e.g. “seasonal”, “heritage”, “organic”), citations/links to data sources, and user-suggestions.

User submissions table: If users add new ciders, store them in a pending_ciders table or have a status column, along with the submitting user and timestamp.

This mirrors open databases like beer.db, which links beers to breweries and countries (see JSON example)
openbeer.github.io
. We would also index fields that support search and filtering: name (for text search), producer, country, ABV range, style, and rarity tier. Postgres full-text search or trigram indexes can make name lookup robust. Our Supabase Postgres can also use JSONB for flexible attributes (e.g. tagging rare features).

Integrating multiple sources: Data will be merged from APIs, CSVs, and user input. To unify entries:

Normalize names and regions: Match producers by name and location (e.g. “XYZ Cidery, Somerset”). Use geocoding or consistent country codes.

Deduplicate: Before adding a user-supplied cider, run a fuzzy match against existing entries (match on brand + producer). This avoids duplicates (similar to Untappd’s “Can’t find” check).

Source tracking: Keep provenance (e.g. mark which entries came from OpenBreweryDB, which from manual entry). We can have fields like source_api or entered_by_user.

Data import pipeline: For semi-automated updates, build an ETL (e.g. a Python or Node.js script) to fetch data (e.g. nightly query OpenBrewery API, or parse a CSV of global trade stats) and upsert into Postgres. Use standard identifiers where possible (e.g. if a UK cidery has a HMRC ID or EU organic cert, use that as key).

By enforcing a clear schema and data-cleaning routines (trimming whitespace, standardizing abbreviations, etc.), we can merge disparate lists into a unified cider database. Metadata fields (like last_updated, verified_by_admin) help track reliability.

User Submissions and Rarity Assignment

Adding new ciders (user workflow): Users should be able to add a cider when they “can’t find” it. The app will prompt them through a guided form:

Search First: User searches for the cider name and producer. If not found, they tap “Add new cider.”

Enter Details: They fill in mandatory fields (name, producer, type, ABV, origin region, etc.). The form can auto-suggest producers or regions based on partial input (e.g. dropdown of countries).

Check for Duplicates: The system runs a quick similarity check. If a likely match exists, the user is warned or asked to confirm it’s truly a different cider.

Submission and Review: The new entry is saved as “pending.” Experienced users or moderators receive an alert to verify and either approve or reject it.

This is similar to Untappd’s approach: they allow users to add beers/cider if not in the database, but enforce creation guidelines and can revoke privileges for bad entries
help.untappd.com
. We will likewise require users to follow naming standards (e.g. include proof of brand and origin if possible) to prevent nonsense entries.

Guiding rarity assignment: When a user submits a cider, the system should suggest a rarity tier but not blindly trust the user’s choice. Possible methods:

AI/algorithmic suggestion: Based on input (e.g. if the user notes “limited release of 500 bottles”), the system can auto-fill a likely rarity tier (Common if wide-release, Rare/Epic if very limited). A simple formula might weigh “production volume vs. distribution.”

Preset defaults: For example, new user submissions default to “Uncommon” or “Rare” and must be confirmed. If the app recognizes the producer (e.g. global brand), it might default to Common.

Community voting: Once submitted, other users can vote or comment if they think the rarity should be adjusted. If many users from different regions flag it as too high or low, the system can demote or promote the tier.

Moderation: Admins or trusted moderators (maybe elected or experienced users) can finalize the tier. For instance, if someone tries to mark a very common cider as “Legendary,” moderators can override it.

These mechanisms prevent abuse. We should treat rarity like any other user-editable field with checks. (Untappd notes that if submissions violate guidelines or appear fraudulent, they are flagged and privileges can be revoked
help.untappd.com
.) Our app can similarly lock submissions behind reputation – e.g., a brand-new user’s cider entry might be “unverified” until others confirm it. Over time, an AI feature (using metadata or even computer vision on label images) could assist suggestions, but initially clear rules and peer review will be key.

Maintenance Strategy

Regular updates: The cider database must be kept current as new ciders are launched and old ones discontinued. We will implement a mixed strategy:

Automated data refresh: Schedule periodic jobs (cron jobs or Supabase Edge functions) to pull fresh data from reliable feeds. Examples include: new brewery openings from OpenBreweryDB API, government import/export updates, and global market news sources (RSS feeds or APIs from IARD, trade journals, etc.). Any importer/distributor lists (e.g. TTB’s importer CSVs) can be reloaded quarterly to catch new brand registrations. Supabase’s Postgres can store a “date_added” and “discontinued” flag, which our pipeline can set if an item vanishes from authoritative lists.

Community monitoring: Encourage users to report changes (e.g. “this cider is discontinued” or “new batch released”). We can gamify this (badges for reporting) so the community self-updates records.

Scheduled re-evaluation of rarity: Rarity isn’t static – a once-limited cider can become common if re-released broadly. We will periodically recompute rarity scores (e.g. annually) using the latest data (production figures, sales, number of regions available). This could be an automated recalculation job that flags any tier changes for moderator review.

Tools and automation: To ease upkeep:

ETL pipelines: As mentioned, use scripts or integration tools to ingest external data automatically. For example, use Postgres foreign data wrappers or REST APIs to sync with OpenBreweryDB and government databases on a schedule.

Data quality checks: Automated routines can detect anomalies (e.g. a cider’s producer country changed, or ABV suddenly off) and mark those records “needs review.”

Backend services: Supabase supports row-level permissions and triggers – we can leverage those to send notifications (e.g. an admin alert when a user adds a very high-tier cider).

Monitoring market releases: Subscribing to cider news sites or producers’ press releases could feed into a light intelligence system; e.g. webhooks or Zapier connectors that parse a new cider announcement into our staging DB.

Backup and versioning: We will keep historical snapshots of the database to track how rarity tiers evolved. This also helps audit changes (e.g. if a new survey shows volume doubled, we can raise the tier accordingly).

By combining automated feeds (for constant vigilance) with community and moderator input (for nuanced judgment), we can keep the cider rarity database accurate and up-to-date. This ensures the app’s rarity-based features (battles, quests, achievements) remain meaningful as the real-world cider market changes.

Sources: Authoritative data include IARD/GlobalData (global production)
iard.org
, national trade bodies (e.g. NACM for the UK)
en.wikipedia.org
, industry research reports
openpr.com
openpr.com
, and open datasets like OpenBreweryDB
openbrewerydb.org
. Where possible, we’ll cite official publications (e.g. government alcohol sales reports
www150.statcan.gc.ca
) to guide schema and criteria. App guidelines (similar to Untappd’s) help shape user contribution workflows
help.untappd.com
. The above strategy draws on these sources to define a robust, scalable rarity and database framework.