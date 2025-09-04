# Cider Data Model

This document defines the data model for a "cider" object in The Fellowship of the Cider application.

## Object Structure

```json
{
  "id": "uuid",
  "name": "string",
  "brand": "string",
  "rarity": "string",
  "style": "string",
  "origin_country": "string",
  "origin_region": "string",
  "created_at": "timestamp"
}
```

## Field Descriptions

*   **`id`**: A unique identifier for the cider (UUID).
*   **`name`**: The name of the cider.
*   **`brand`**: The brand or producer of the cider.
*   **`rarity`**: The rarity tier of the cider (e.g., "Common", "Uncommon", "Rare", "Epic", "Legendary", "Mythical", "Artifact").
*   **`style`**: The style of the cider (e.g., "Traditional", "Modern", "Perry", "Ice Cider", "Hopped", "Spiced", "Fruit Blend", "Wild Ferment").
*   **`origin_country`**: The country where the cider is produced.
*   **`origin_region`**: The region within the country where the cider is produced.
*   **`created_at`**: The timestamp when the cider was first added to the database.
