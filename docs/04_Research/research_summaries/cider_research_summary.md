# Summary: Cider Research

## Key Findings
This document provides an extensive overview of cider, covering its history, categories, styles, commercial brands, craft producers, production methods, regional traditions, service, rarity, and classification systems. It highlights the complexity and diversity of the cider world.

-   **Categories & Styles**: Cider styles range from traditional (English scrumpy, French cidre doux/brut) to modern (American modern/heritage) and specialty (ice ciders, perry, hopped ciders, wild fermented). Each has distinct ABV, sweetness, and flavor profiles.
-   **Commercial Brands**: Dominated by large players like Strongbow and Angry Orchard, with mid-size producers bridging the gap. Imperial ciders (higher ABV) are a growing trend.
-   **Craft Producers**: Focus on terroir, traditional methods, and unique apple varieties, often with limited production and higher prices (e.g., Eden Specialty Ciders, Shacksbury, Oliver's Cider & Perry).
-   **Production Methods**: Apple selection (bittersweet, bittersharp, sharp), fermentation (commercial yeast vs. wild), and post-fermentation techniques (barrel aging, keeving, dry-hopping, bottle conditioning) significantly impact character.
-   **Regional Traditions**: Distinct traditions exist in England (West Country scrumpy), France (Normandy, Brittany), Spain (Asturian sidra), Germany (Apfelwein), and the US (Pacific Northwest, New England, Michigan).
-   **Service**: Proper temperature and glassware are crucial for appreciation. Professional evaluation guidelines exist.
-   **Rarity**: Driven by limited production, heritage apple scarcity, unique processes (ice cider, keeving), seasonal availability, and limited editions.
-   **Classification Systems**: Evolved from simple sweet/dry to sophisticated frameworks (BJCP, CAMRA, AOC) defining sweetness, alcohol, and geographic origin.

## Actionable Insights for System Design

1.  **Cider Data Model**: Design a comprehensive data model for ciders that captures: 
    -   **Style**: (e.g., English Dry, French Brut, American Modern, Ice Cider, Perry, Hopped) 
    -   **Origin**: Country, Region (e.g., Herefordshire, Normandy, Asturia) 
    -   **ABV**: Alcohol by Volume 
    -   **Sweetness**: (Dry, Semi-Dry, Medium, Semi-Sweet, Sweet) 
    -   **Apple Varieties**: (e.g., Dabinett, Kingston Black, McIntosh) 
    -   **Production Method Tags**: (e.g., Wild Fermented, Barrel Aged, Keeved, Bottle Conditioned, Unfiltered) 
    -   **Rarity Tier**: (Common, Rare, Epic, Legendary, Mythical, Artifact) 
    -   **Commercial/Craft Indicator**.
2.  **Gamification - Artisan's Ring**: Directly use the diverse cider styles and production methods to define progression criteria for the "Artisan's Ring" (Style Mastery). Players can unlock achievements for logging ciders across different styles.
3.  **Gamification - Explorer's Ring**: Leverage regional traditions and origins for the "Explorer's Ring" (Geographic Diversity). Encourage users to log ciders from various countries and specific regions.
4.  **Content Generation**: The detailed descriptions of styles, regions, and production methods can be used to generate rich in-app content, educational snippets, and tasting notes for users.
5.  **Rarity System Integration**: The factors influencing rarity (limited production, heritage apples, unique processes) should directly feed into the app's rarity assignment algorithm for the "Collector's Ring" and overall rarity system.
6.  **Battle System Flavor**: Incorporate cider characteristics into battle mechanics. For example, "tannin structure" from bittersweet apples could provide defensive buffs, "acidity" from sharp apples could be an offensive stat, or "wild fermentation" could introduce unpredictable effects.
7.  **User Education**: Provide information on proper cider service (temperature, glassware) within the app to enhance the user's appreciation and experience.
8.  **Search & Filtering**: Implement robust search and filtering options based on all the detailed cider attributes (style, origin, ABV, sweetness, apple varieties, production methods) to help users discover new ciders and track their progress.
9.  **Dynamic Content**: Use the information on seasonal availability and limited editions to create dynamic in-app events or highlight specific ciders.
10. **Database Population**: The commercial brands and craft producers mentioned can serve as initial data points for populating the app's cider database.