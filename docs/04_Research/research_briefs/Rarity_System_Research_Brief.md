# Research Brief: Rarity System & Database Integration

## Project Introduction: The Fellowship of the Cider - An Epic Journey of Cider Discovery

"The Fellowship of the Cider" is not just another logging app; it's a **gamified mobile application** designed to transform the simple act of trying ciders into an **epic medieval fantasy adventure**, deeply inspired by the rich lore of **Lord of the Rings**. Our core vision is to create a compelling, long-term engagement loop that encourages real-world cider exploration while providing a rich, immersive experience.

**Core Purpose**: To be the ultimate personal cider logging and documentation app, evolving into a comprehensive "cider dictionary" and gamifying the discovery process through unique mechanics.

**Key Gamified Mechanics**:
*   **Cider Logging & Rating**: Document every cider with detailed notes and a unique "tankard" rating system.
*   **Rarity System**: Discover and collect ciders across tiers from Common to Mythical/Artifact.
*   **Battle System**: Engage in "Pokemon-style" turn-based combat against "Orc beers" using your collected ciders.
*   **Quest & Achievement Systems**: Undertake weekly challenges and earn prestigious titles and rewards.
*   **Seven Rings of Cider**: Progress through a unique "ring" system tied to geographic exploration, style mastery, and battle victories.

This research is foundational to building the immersive and rewarding experience we envision.

## Research Area: Rarity System & Database Integration

### Research Goal
To determine the most effective, objective, and sustainable methods for building, populating, and maintaining the app's comprehensive cider rarity database. This database is crucial for the app's core gamification loop, directly impacting the collection, battle, and achievement systems, and must be scalable and perceived as fair by users.

### Specific Questions to Answer

1.  **Data Sourcing for Rarity:**
    *   What are the most reliable and comprehensive public or commercial data sources for cider production volumes, distribution networks, and market availability on a global scale? We are particularly interested in data that can help objectively classify ciders into our defined rarity tiers (Common, Uncommon, Rare, Epic, Legendary, Mythical, Artifact).
    *   Are there existing cider databases or APIs (e.g., Untappd, RateBeer, industry associations) that could be leveraged for initial data population? What are their licensing terms, data quality, and how well do they align with our need for rarity-defining metrics?
    *   What objective criteria (beyond production volume) can be used to assign rarity tiers (Common, Uncommon, Rare, Epic, Legendary, Mythical, Artifact) to ciders? (e.g., regional exclusivity, limited releases, historical significance, specific ingredients/processes).

    *   **Geographic Focus:** Should the research prioritize global cider coverage, or focus on specific regions (e.g., Europe, North America) initially?

2.  **Database Design for Rarity:**
    *   What is the optimal schema for storing cider data, including rarity attributes, to support efficient search, filtering, and gamification mechanics (e.g., Battle System, Ring Progression, Achievements)? The schema should be flexible enough to accommodate future data points.
    *   How can we integrate data from various sources into a unified, consistent database structure?

3.  **Personal Additions & Rarity Assignment:**
    *   What is the proposed workflow for users to add ciders not found in the existing database?
    *   How can the system guide users to assign a reasonable rarity to their personal additions, ensuring consistency with our established rarity tiers? What mechanisms can prevent arbitrary or abusive rarity assignments that could unbalance the game mechanics (e.g., community voting, moderation, AI-assisted suggestions based on metadata)?

4.  **Maintenance Strategy:**
    *   What is a sustainable and efficient strategy for updating the rarity database over time (e.g., new releases, discontinued ciders, changes in distribution/rarity)? This is crucial for maintaining the integrity and relevance of the gamified experience.
    *   What tools or processes can automate or semi-automate data updates and rarity re-evaluation?

5.  **Constraints & Preferences:**
    *   **Data Source Accessibility:** Are we open to using paid data sources/APIs, or should the focus be strictly on publicly accessible data only?
    *   **Preferred Technologies for Database Integration:** Are there any preferred technologies or platforms for the database integration (e.g., PostgreSQL, Firebase, AWS RDS, Supabase)?

### Purpose of this Research for Design & Specification

This research is absolutely critical for the success and integrity of "The Fellowship of the Cider" app. It will:
*   **Data Model Specification (`03_Design/02_Data_Models/Cider_Data_Model.md`):** Directly inform the schema and data integrity rules for cider entries, especially rarity-related fields, ensuring our core data foundation supports gamification.
*   **Functional Specification Refinement:** Provide concrete, actionable details for the "Rarity System" (Section 6) and "Cider Logging System" (Section 4), particularly regarding objective rarity assignment, duplicate prevention, and the seamless integration of personal additions.
*   **Game Balancing & Immersion:** Rarity directly impacts battle benefits and strategy, quest difficulty, and achievement unlocks. A well-researched rarity system is essential for balancing the game, preventing exploits, and enhancing the immersive fantasy experience.
*   **Technical Architecture:** Guide crucial decisions on data storage solutions, external API integrations, and potential backend services for data management, ensuring the system is robust and scalable enough to handle a growing global cider database.
*   **User Trust & Engagement:** A transparent and fair rarity system will build user trust and encourage long-term engagement, as users will perceive their collection efforts as meaningful and rewarding.
