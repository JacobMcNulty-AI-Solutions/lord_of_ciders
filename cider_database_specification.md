
# The Fellowship of the Cider - Cider Database Specification

## 1. Introduction

This document outlines the data structure and initial population of the cider database for "The Fellowship of the Cider" application. The specification is derived from the `functional_specification.md`, `cider_research.md`, and `ciders_from_list_wiki.md`.

This revision incorporates feedback to better define sub-styles, represent flavor profiles as a 2D graph, and separate core cider data from user-specific logs.

## 2. Data Model Overview

The data model is now split into three main entities to clearly distinguish between the global catalog, a user's discovered ciders, and their individual tasting logs:

*   **Cider**: A global catalog of all ciders, containing objective information. This is the "undiscovered" list.
*   **UserDiscoveredCider**: A linking table that represents a user's personal collection of "unlocked" ciders.
*   **CiderLog**: A user's personal record of tasting a specific cider, including subjective ratings and context.

## 3. Cider Data Structure (Global Catalog)

Each cider in the global database will have the following metadata fields.

| Field Name | Data Type | Description | Example |
|---|---|---|---|
| `cider_id` | UUID | Unique identifier for each cider. | `f47ac10b-58cc-4372-a567-0e02b2c3d479` |
| `name` | String | The name of the cider. | `Angry Orchard Crisp Apple` |
| `brand` | String | The brand or producer of the cider. | `Angry Orchard` |
| `style` | Enum | The main style of the cider. See Section 7. | `Modern` |
| `sub_style` | Enum | A more specific style category. See Section 8. | `American Craft` |
| `origin_country` | String | The country of origin. | `USA` |
| `origin_region` | String | The region of origin. | `New York` |
| `abv` | Float | Alcohol by volume. | `5.0` |
| `rarity` | Enum | The rarity tier of the cider. See Section 9. | `Common` |
| `base_attack` | Integer | Base attack stat for the battle system. | `10` |
| `base_defense` | Integer | Base defense stat for the battle system. | `5` |
| `special_move` | String | A special move for the battle system. | `Crisp Apple Strike` |
| `flavor_coordinates` | Object | X/Y coordinates on the flavor graph. See Section 4. | `{ "x": 0.8, "y": -0.3 }` |
| `color` | String | The color of the cider. | `Golden` |
| `clarity` | Enum | The clarity of the cider. | `Clear` |
| `carbonation` | Enum | The carbonation level of the cider. | `Highly Carbonated` |
| `package_type` | Enum | The type of packaging. See Section 11. | `Bottle` |
| `description` | Text | A brief description of the cider. | `A classic American hard cider.` |
| `image_url` | String | A URL to an image of the cider. | `https://example.com/angry_orchard.png` |

### Battle Stat Determination

The `base_attack`, `base_defense`, and `special_move` fields are manually curated and not automatically generated. Their values are determined by conducting in-depth research into each cider's history, production methods, and cultural context. For example, a cider with a long history of being enjoyed by soldiers might have a higher `base_defense`, while a cider made with a rare, potent apple variety might have a powerful `special_move`.

## 4. Flavor Profile Graph

The `flavor_coordinates` field represents a cider's position on a 2D flavor graph.

*   **X-Axis (Sweetness):** Ranges from `-1.0` (Very Dry) to `1.0` (Very Sweet).
*   **Y-Axis (Complexity):** Ranges from `-1.0` (Simple/One-dimensional) to `1.0` (Complex/Multi-layered).
*   **UI Guidance:** The user interface should provide contextual help or examples to clarify the meaning of "Simple" vs. "Complex". For example, "Simple" ciders have a straightforward, single-note flavor (e.g., just apple), while "Complex" ciders have multiple layers of flavor and aroma (e.g., notes of oak, spice, and different fruits).

| Quadrant | Description |
|---|---|
| **Top-Right (X > 0, Y > 0)** | Sweet & Complex (e.g., Ice Ciders, Fine Perries) |
| **Top-Left (X < 0, Y > 0)** | Dry & Complex (e.g., Traditional Farmhouse, Wild Ferments) |
| **Bottom-Right (X > 0, Y < 0)** | Sweet & Simple (e.g., Mass-market Ciders) |
| **Bottom-Left (X < 0, Y < 0)** | Dry & Simple (e.g., Basic Dry Ciders) |

## 5. UserDiscoveredCider Data Structure (Linking Table)

This table tracks which ciders a user has "unlocked" and added to their personal collection.

| Field Name | Data Type | Description |
|---|---|---|
| `user_discovered_cider_id` | UUID | Unique identifier for this link. |
| `user_id` | UUID | Foreign key to the `User` table. |
| `cider_id` | UUID | Foreign key to the `Cider` table. |
| `first_logged_date` | Timestamp | The date the user first logged this cider. |

## 6. CiderLog Data Structure

Each time a user logs a cider, a `CiderLog` entry is created with the following fields:

| Field Name | Data Type | Description | Example |
|---|---|---|---|
| `log_id` | UUID | Unique identifier for this specific log entry. | `a1b2c3d4-e5f6-7890-1234-567890abcdef` |
| `user_discovered_cider_id` | UUID | Foreign key to the `UserDiscoveredCider` table. | `...` |
| `personal_rating` | Integer | The user's rating from 1 to 10. | `8` |
| `special_tankards` | Array[Enum] | List of earned special tankards for this log. See Section 10. | `["First Discovery", "Hometown Hero"]` |
| `evolution_tags` | Array[Enum] | List of earned evolution tags. See Section 10. | `["Home Guardian"]` |
| `fellowship_count` | Integer | The number of times this cider has been re-logged. | `3` |
| `price_paid` | Float | The price paid for the cider on this occasion. | `7.50` |
| `currency` | String | The currency of the price paid. | `GBP` |
| `venue_type` | Enum | The type of venue where the cider was purchased/consumed. See Section 11. | `Pub` |
| `location_tried` | String | The name of the venue or location. | `The Green Dragon Inn` |
| `location_coordinates` | Object | GPS coordinates of the location. | `{ "lat": 51.5074, "lon": -0.1278 }` |
| `tasting_notes` | Text | The user's personal tasting notes for this log. | `Surprisingly complex for the price...` |
| `date_logged` | Timestamp | The date and time of the log entry. | `2025-09-02T18:30:00Z` |

## 7. Cider Style & Sub-Style Options

### Main Styles
- Traditional
- Modern
- Perry
- Ice Cider
- Hopped
- Spiced
- Fruit Blend
- Wild Ferment

### Sub-Styles (Examples)
*   **Note:** The available `sub_style` options should be filtered based on the selected `style`.
*   **Traditional:**
    *   English Scrumpy
    *   French Cidre (Doux, Brut, Demi-Sec)
    *   Spanish Sidra
    *   New England Style
    *   Farmhouse
*   **Modern:**
    *   American Craft
    *   Imperial / High-ABV
    *   Rosé Cider
    *   Unfiltered
*   **Perry:**
    *   Traditional Perry
    *   Modern Perry
*   **Fruit Blend:**
    *   Berry Blend
    *   Stone Fruit Blend
    *   Tropical Blend
*   **Spiced:**
    *   Winter/Wassail
    *   Cinnamon & Nutmeg
    *   Ginger & Lemon

## 8. Rarity Tier Options

- Common
- Uncommon
- Rare
- Epic
- Legendary
- Mythical
- Artifact

## 9. Adding a New Cider to the Global Catalog

If a user discovers a cider that is not in the global `Cider` table, they can add it. The process is as follows:

1.  The user searches for the cider and no results are found.
2.  The user is prompted to "Add a New Cider to the Global Catalog".
3.  A form is presented with all the fields from the `Cider` table (Section 3).
4.  The user fills in the metadata for the new cider.
5.  Upon submission, the new cider is added to the global `Cider` table and is immediately available to all users.
6.  A new entry is created in the `UserDiscoveredCider` table for the user and the new cider.
7.  The user is then taken to the standard logging screen to create the first `CiderLog` entry for this new cider.

## 10. Automated Awards System (Tankards & Tags)

The system will automatically award `SpecialTankards` and `EvolutionTags` based on the data in a `CiderLog` and the user's overall history. These awards are stored in the `CiderLog` table for each specific log entry.

### Special Tankards
| Tankard Name | Triggering Event |
|---|---|
| **First Discovery Tankard** | Awarded to the very first `CiderLog` entry for a user. |
| **Century Tankard** | Awarded on the 100th unique cider discovered (i.e., the 100th entry in `UserDiscoveredCider`). |
| **Hometown Hero Tankard** | Awarded to a log if it is the highest-rated cider from the user's home region. |
| **Traveler's Tankard** | Awarded to the first log from each new country. |
| **Crystal Tankard** | Awarded to any log of a cider with `rarity` of `Epic` or higher. |
| **Ancient Relic Tankard** | Awarded to any log of a cider with `style` of `Traditional`. |
| **Collector's Tankard** | Awarded to any log of a cider with `rarity` of `Mythical` or `Artifact`. |
| **Fellowship Tankard** | Awarded to a log with a `venue_type` of `Pub` or `Bar` and a high rating (e.g., > 8). |
| **Solo Quest Tankard** | Awarded to a log with a `venue_type` of `Home` and a high rating (e.g., > 8). |
| **Broken Tankard** | Awarded to any log with a `personal_rating` of 1 or 2. |
| **Legendary Mithril Tankard** | Awarded to any log with a `personal_rating` of 10. |
| **Redemption Tankard** | Awarded to a log if the same cider was previously rated much lower (e.g., 3+ points lower). |

### Evolution Tags
| Tag Name | Triggering Event |
|---|---|
| **Home Guardian** | Awarded to a log if the user has logged 5+ ciders with a `venue_type` of `Home`. |
| **Tavern Warrior** | Awarded to a log if the user has logged ciders from 5+ different `Pub` or `Bar` venues. |
| **Festival Champion** | Awarded to a log if the `venue_type` is `Festival`. |
| **Style Master** | Awarded to a log if the user has logged 20+ ciders of the same `style`. |
| **Regional Lord/Lady** | Awarded to a log if the user has logged 15+ ciders from the same `origin_region`. |
| **Battle-Tested** | Awarded to a log when a cider is re-logged after being defeated in the battle system. |

## 11. Enum Options

### Package Types
- `on-tap`
- `bottle`
- `can`
- `keg`

### Venue Types
- `Pub`
- `Bar`
- `Restaurant`
- `Home`
- `Festival`
- `Winery`
- `Brewery`
- `Other`

## 12. Initial Cider Database

This table remains the same, but now represents the global `Cider` catalog.

| Name | Brand | Style | Sub-Style | Origin Country | Origin Region | ABV | Rarity |
|---|---|---|---|---|---|---|---|
| Strongbow Original | Strongbow | Modern | English Cider | UK | Herefordshire | 4.5 | Common |
| Angry Orchard Crisp Apple | Angry Orchard | Modern | American Craft | USA | New York | 5.0 | Common |
| Magners Original | Magners | Modern | Irish Cider | Ireland | Clonmel | 4.5 | Common |
| 2 Towns Bad Apple | 2 Towns Ciderhouse | Modern | Imperial | USA | Oregon | 10.5 | Uncommon |
| Blake's Triple Jam | Blake's Hard Cider | Fruit Blend | Berry Blend | USA | Michigan | 6.5 | Uncommon |
| Aspall Cyder | Aspall | Traditional | English Cyder | UK | Debenham | 5.5 | Uncommon |
| Downeast Original Blend | Downeast Cider House | Modern | Unfiltered | USA | Massachusetts | 5.1 | Uncommon |
| Oliver's Traditional Cider | Oliver's Cider & Perry | Traditional | Farmhouse | UK | Herefordshire | 6.5 | Rare |
| Eden Heirloom Blend Ice Cider | Eden Specialty Ciders | Ice Cider | Heirloom | USA | Vermont | 10.0 | Epic |
| Shacksbury Deer Snacks | Shacksbury | Wild Ferment | Foraged | USA | Vermont | 6.9 | Epic |
| Eric Bordelet Poiré Granit | Eric Bordelet | Perry | French Perry | France | Normandy | 4.0 | Legendary |
| Farnum Hill Extra Dry | Farnum Hill | Traditional | New England Dry | USA | New Hampshire | 7.5 | Rare |
| Burrow Hill Cider | Burrow Hill Cider | Traditional | Somerset Farmhouse | UK | Somerset | 6.0 | Rare |
| Thatchers Gold | Thatchers | Modern | English Cider | UK | Somerset | 4.8 | Common |
| Kopparbergs Strawberry & Lime | Kopparbergs | Fruit Blend | Swedish Cider | Sweden | Kopparberg | 4.0 | Common |
| Somersby Apple Cider | Somersby | Modern | Danish Cider | Denmark | Copenhagen | 4.5 | Common |
| Woodchuck Amber | Woodchuck Hard Cider | Modern | American Craft | USA | Vermont | 5.0 | Common |
| Ace Perry | Ace Cider | Perry | American Perry | USA | California | 5.0 | Uncommon |
| Bold Rock Virginia Apple | Bold Rock Hard Cider | Modern | American Craft | USA | Virginia | 4.7 | Uncommon |
| Rekorderlig Pear | Rekorderlig | Perry | Swedish Perry | Sweden | Vimmerby | 4.5 | Common |
