# Database Schema

This document defines the database schema for The Fellowship of the Cider application, using Supabase (PostgreSQL).

## Tables

### `users`

Stores user account information.

| Column      | Type      | Description                               |
|-------------|-----------|-------------------------------------------|
| `id`        | `uuid`    | Primary key (from Supabase Auth)          |
| `username`  | `text`    | User's chosen name                        |
| `created_at`| `timestamp`| Timestamp of account creation             |

### `ciders`

Stores the global list of all known ciders.

| Column        | Type      | Description                               |
|---------------|-----------|-------------------------------------------|
| `id`          | `uuid`    | Primary key                               |
| `name`        | `text`    | Name of the cider                         |
| `brand`       | `text`    | Brand or producer of the cider            |
| `rarity`      | `text`    | Rarity tier (e.g., Common, Rare, Mythical)|
| `style`       | `text`    | Cider style (e.g., Traditional, Modern)   |
| `origin_country`| `text`    | Country of origin                         |
| `origin_region` | `text`    | Region of origin                          |
| `created_at`  | `timestamp`| Timestamp of creation                     |

### `cider_logs`

Stores each instance of a user logging a cider.

| Column            | Type        | Description                                      |
|-------------------|-------------|--------------------------------------------------|
| `id`              | `uuid`      | Primary key                                      |
| `user_id`         | `uuid`      | Foreign key to `users.id`                        |
| `cider_id`        | `uuid`      | Foreign key to `ciders.id`                       |
| `personal_rating` | `integer`   | User's rating (1-10)                             |
| `photo_url`       | `text`      | URL to the photo of the cider                    |
| `location`        | `geography` | GPS coordinates of where the cider was logged    |
| `tasting_notes`   | `text`      | Detailed tasting notes                           |
| `price`           | `numeric`   | Price paid for the cider                         |
| `created_at`      | `timestamp` | Timestamp of the log                             |

### `quests`

Stores the available quests.

| Column      | Type      | Description                               |
|-------------|-----------|-------------------------------------------|
| `id`        | `uuid`    | Primary key                               |
| `title`     | `text`    | Title of the quest                        |
| `description`| `text`    | Description of the quest                  |
| `category`  | `text`    | Quest category (e.g., Geographic, Style)  |
| `created_at`| `timestamp`| Timestamp of creation                     |

### `user_quests`

Tracks user progress on quests.

| Column      | Type      | Description                               |
|-------------|-----------|-------------------------------------------|
| `id`        | `uuid`    | Primary key                               |
| `user_id`   | `uuid`    | Foreign key to `users.id`                 |
| `quest_id`  | `uuid`    | Foreign key to `quests.id`                |
| `completed` | `boolean` | Whether the user has completed the quest  |
| `created_at`| `timestamp`| Timestamp of when the user started the quest|

### `achievements`

Stores the available achievements.

| Column      | Type      | Description                               |
|-------------|-----------|-------------------------------------------|
| `id`        | `uuid`    | Primary key                               |
| `name`      | `text`    | Name of the achievement                   |
| `description`| `text`    | Description of the achievement            |
| `hidden`    | `boolean` | Whether the achievement is hidden         |
| `created_at`| `timestamp`| Timestamp of creation                     |

### `user_achievements`

Tracks user's earned achievements.

| Column      | Type      | Description                               |
|-------------|-----------|-------------------------------------------|
| `id`        | `uuid`    | Primary key                               |
| `user_id`   | `uuid`    | Foreign key to `users.id`                 |
| `achievement_id`| `uuid`| Foreign key to `achievements.id`          |
| `created_at`| `timestamp`| Timestamp of when the achievement was earned|
