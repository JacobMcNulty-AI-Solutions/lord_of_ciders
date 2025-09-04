# Summary: Ciders from List Wiki

## Key Findings
This document is a list of cider brands, including perry, compiled from a Wikipedia article. It categorizes brands into "Operational brands" and "Defunct brands," providing their town, country, type (hard cider, soft cider, pear cider), and some notes.

-   **Extensive List**: Contains a broad list of cider and perry brands from various countries (USA, UK, Australia, Ireland, Canada, Poland, Finland, Sweden, South Africa, Germany, New Zealand, Belgium).
-   **Geographic Diversity**: Highlights the global presence of cider production and consumption.
-   **Brand Information**: Provides basic details for each brand, such as origin, type, and sometimes ownership or specific product lines.
-   **Operational vs. Defunct**: Differentiates between currently active and discontinued brands, which can be useful for historical data or avoiding outdated entries.
-   **Commercial Focus**: The list primarily features commercial brands, including some very large players (e.g., Strongbow, Angry Orchard, Magners/Bulmers).

## Actionable Insights for System Design

1.  **Initial Database Population**: This list serves as an excellent starting point for populating the app's initial cider database. Brands, their origin countries, and types can be directly imported.
2.  **Producer/Brand Data Model**: The information provided (Name, Town, Country, Type) can directly inform the `Cider` and `Producer` tables in the database schema. Consider adding fields for `is_perry` or `is_soft_cider`.
3.  **Geographic Data**: The country and town information can be used to build out the geographic data for the "Explorer's Ring" progression system.
4.  **Brand Recognition**: The inclusion of major commercial brands indicates their importance in the market. The app should be able to easily identify and log these common ciders.
5.  **Data Validation**: When users submit new ciders, this list can be used for initial fuzzy matching to prevent duplicate entries for well-known brands.
6.  **Historical Context**: The "Defunct brands" section can be used to inform users if a logged cider is no longer in production, adding a layer of historical context.
7.  **Expansion**: While a good starting point, the list is not exhaustive. The system should be designed to allow for easy expansion through user submissions and other data sources (as discussed in the Rarity System research).