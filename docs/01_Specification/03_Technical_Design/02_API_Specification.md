# The Fellowship of the Cider - API Specification

This document defines the comprehensive API specification for "The Fellowship of the Cider" application, including REST endpoints, GraphQL schema, real-time subscriptions, and integration patterns.

## 1. API Overview

### 1.1 API Architecture Philosophy
The Fellowship of the Cider API follows a hybrid approach combining REST for CRUD operations, GraphQL for flexible data fetching, and WebSocket subscriptions for real-time updates. This design optimizes for mobile performance while providing the flexibility needed for complex battle mechanics and social features.

### 1.2 API Design Principles
- **Mobile-First**: Optimized for mobile device constraints and capabilities
- **Offline-Compatible**: Support for offline operation with eventual consistency
- **Version-Tolerant**: Backward-compatible evolution with semantic versioning
- **Performance-Optimized**: Efficient data transfer and minimal round trips
- **Security-Integrated**: Authentication and authorization at every endpoint
- **Standards-Compliant**: Following REST, GraphQL, and OpenAPI standards

### 1.3 API Standards & Conventions
- **HTTP Methods**: Standard REST verbs (GET, POST, PUT, DELETE, PATCH)
- **Status Codes**: Appropriate HTTP status codes with detailed error messages
- **Content Types**: JSON for data exchange, multipart/form-data for file uploads
- **Naming Conventions**: camelCase for JSON fields, kebab-case for URLs
- **Pagination**: Cursor-based pagination for consistent performance
- **Filtering**: Query parameters following JSON API specification patterns
- **Rating System**: 1-10 tankard rating scale with visual representations (1-3: wooden peasant mugs, 4-6: iron tavern tankards, 7-8: silver noble chalices, 9-10: golden royal goblets)

## 2. Authentication & Authorization

### 2.1 Authentication Strategy

#### 2.1.1 OAuth 2.0 + OpenID Connect
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securePassword123"
}

Response:
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "username": "ciderlover123"
  }
}
```

#### 2.1.2 Social Authentication
```http
POST /auth/oauth/{provider}
Content-Type: application/json

{
  "provider": "google|apple|facebook",
  "token": "provider_access_token",
  "code": "authorization_code"
}
```

#### 2.1.3 Token Refresh
```http
POST /auth/refresh
Content-Type: application/json
Authorization: Bearer {refresh_token}

{
  "refresh_token": "current_refresh_token"
}
```

### 2.2 Authorization Patterns

#### 2.2.1 Role-Based Access Control
- **USER**: Standard user permissions (logging, battles, basic features)
- **PREMIUM**: Premium subscriber permissions (advanced analytics, tournaments)
- **MODERATOR**: Content moderation permissions (review submissions)
- **ADMIN**: Administrative permissions (user management, system configuration)

#### 2.2.2 Resource-Based Authorization
```http
GET /api/v1/users/{userId}/cider-logs
Authorization: Bearer {access_token}
X-User-ID: {authenticated_user_id}

# Access Control:
# - Users can only access their own cider logs
# - Moderators can access any user's logs
# - Premium users can access additional metadata
```

## 3. Core REST API Endpoints

### 3.1 User Management API

#### 3.1.1 User Profile Operations
```http
# Get current user profile
GET /api/v1/users/me
Authorization: Bearer {access_token}

Response: 200 OK
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "username": "ciderlover123",
  "email": "user@example.com",
  "created_at": "2024-01-15T10:30:00Z",
  "preferences": {
    "privacy_level": "public",
    "notification_settings": {
      "quest_reminders": true,
      "battle_invitations": false
    }
  },
  "subscription": {
    "tier": "premium",
    "expires_at": "2024-12-15T10:30:00Z"
  }
}

# Update user profile
PATCH /api/v1/users/me
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "username": "newUsername",
  "preferences": {
    "privacy_level": "private"
  }
}
```

#### 3.1.2 User Statistics
```http
GET /api/v1/users/me/stats
Authorization: Bearer {access_token}

Response: 200 OK
{
  "total_ciders_logged": 247,
  "unique_ciders_tried": 189,
  "favorite_style": "Traditional Dry",
  "seven_rings_progress": {
    "explorers_ring": {
      "tier": "silver",
      "progress": 18,
      "next_milestone": 25
    },
    "artisans_ring": {
      "tier": "copper",
      "progress": 12,
      "next_milestone": 15
    }
  },
  "battle_stats": {
    "total_battles": 45,
    "victories": 32,
    "win_rate": 71.1
  }
}
```

### 3.2 Cider Catalog API

#### 3.2.1 Cider Search & Discovery
```http
# Search ciders with filters
GET /api/v1/ciders?search={query}&style={style}&region={region}&abv_min={min}&abv_max={max}&limit={limit}&cursor={cursor}
Authorization: Bearer {access_token}

Response: 200 OK
{
  "data": [
    {
      "id": "cider-550e8400-e29b-41d4-a716-446655440001",
      "name": "Autumn Harvest Traditional",
      "producer": {
        "id": "producer-123",
        "name": "Misty Mountain Cidery",
        "region": "Vermont, USA"
      },
      "style": {
        "id": "style-traditional-dry",
        "name": "Traditional Dry",
        "category": "Traditional"
      },
      "abv": 6.5,
      "description": "A crisp, dry cider made from heirloom apples...",
      "attributes": {
        "sweetness": 3,
        "complexity": 7,
        "body": 6,
        "rarity": "uncommon"
      },
      "average_rating": 8.4,
      "total_logs": 1247,
      "images": [
        "https://cdn.fellowshipcider.com/ciders/autumn-harvest/label.jpg"
      ]
    }
  ],
  "pagination": {
    "has_next": true,
    "next_cursor": "eyJpZCI6IjU1MGU4NDAwIiwic29ydCI6InJhdGluZyJ9",
    "total_count": 15420
  }
}
```

#### 3.2.2 Cider Details
```http
GET /api/v1/ciders/{ciderId}
Authorization: Bearer {access_token}

Response: 200 OK
{
  "id": "cider-550e8400-e29b-41d4-a716-446655440001",
  "name": "Autumn Harvest Traditional",
  "producer": {
    "id": "producer-123",
    "name": "Misty Mountain Cidery",
    "region": "Vermont, USA",
    "established": "2010",
    "website": "https://mistymountaincidery.com"
  },
  "style": {
    "id": "style-traditional-dry",
    "name": "Traditional Dry",
    "category": "Traditional",
    "description": "Fermented apple juice with no added sugars..."
  },
  "attributes": {
    "sweetness": 3,
    "complexity": 7,
    "body": 6,
    "alcohol_strength": 7,
    "rarity": "uncommon",
    "base_attack": 65,
    "base_defense": 58,
    "special_move": "Orchard Slam"
  },
  "production_details": {
    "apples_used": ["Northern Spy", "Granny Smith", "McIntosh"],
    "fermentation_method": "Traditional",
    "aging_process": "6 months in oak barrels",
    "production_volume": "medium",
    "seasonal_availability": ["fall", "winter"]
  },
  "community_data": {
    "average_rating": 8.4,
    "total_logs": 1247,
    "popularity_rank": 342,
    "recent_activity": "trending_up"
  }
}
```

### 3.3 Cider Logging API

#### 3.3.1 Create Cider Log Entry
```http
POST /api/v1/cider-logs
Authorization: Bearer {access_token}
Content-Type: multipart/form-data
X-Request-ID: {unique_request_id}
X-Offline-Timestamp: {client_timestamp}

FormData:
- cider_id: "cider-550e8400-e29b-41d4-a716-446655440001"
- rating: 8
- notes: "Crisp and refreshing with notes of green apple and oak"
- location: {"latitude": 44.2601, "longitude": -72.5806}
- venue: "The Thirsty Hobbit Tavern"
- price: 8.50
- serving_type: "pint"
- photo: [File Upload - auto-compressed to <1MB]
- logged_at: "2024-01-20T19:30:00Z"
- tags: ["with_friends", "dinner_pairing"]
- offline_uuid: {client_generated_uuid}
- device_info: {"platform": "ios", "version": "16.4"}

Response: 201 Created
{
  "id": "log-550e8400-e29b-41d4-a716-446655440002",
  "cider": {
    "id": "cider-550e8400-e29b-41d4-a716-446655440001",
    "name": "Autumn Harvest Traditional"
  },
  "rating": 8,
  "notes": "Crisp and refreshing with notes of green apple and oak",
  "location": {
    "latitude": 44.2601,
    "longitude": -72.5806,
    "address": "Main Street, Montpelier, VT"
  },
  "venue": "The Thirsty Hobbit Tavern",
  "price": 8.50,
  "serving_type": "pint",
  "photos": [
    {
      "id": "photo-123",
      "url": "https://cdn.fellowshipcider.com/photos/user-logs/photo-123.jpg",
      "thumbnail_url": "https://cdn.fellowshipcider.com/photos/user-logs/thumb-photo-123.jpg"
    }
  ],
  "logged_at": "2024-01-20T19:30:00Z",
  "tags": ["with_friends", "dinner_pairing"],
  "created_at": "2024-01-20T19:35:00Z"
}
```

#### 3.3.2 Get User's Cider Logs
```http
GET /api/v1/users/me/cider-logs?sort={field}&order={asc|desc}&limit={limit}&cursor={cursor}
Authorization: Bearer {access_token}

Response: 200 OK
{
  "data": [
    {
      "id": "log-550e8400-e29b-41d4-a716-446655440002",
      "cider": {
        "id": "cider-550e8400-e29b-41d4-a716-446655440001",
        "name": "Autumn Harvest Traditional",
        "producer": "Misty Mountain Cidery"
      },
      "rating": 8,
      "notes": "Crisp and refreshing...",
      "logged_at": "2024-01-20T19:30:00Z",
      "venue": "The Thirsty Hobbit Tavern",
      "photos": [
        {
          "thumbnail_url": "https://cdn.fellowshipcider.com/photos/thumb-photo-123.jpg"
        }
      ]
    }
  ],
  "pagination": {
    "has_next": true,
    "next_cursor": "eyJpZCI6IjU1MGU4NDAwIiwic29ydCI6ImxvZ2dlZF9hdCJ9",
    "total_count": 247
  }
}
```

### 3.4 Battle System API

#### 3.4.1 Battle Initialization
```http
POST /api/v1/battles
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "battle_type": "orc_beer_skirmish",
  "difficulty": "normal",
  "selected_ciders": [
    {
      "slot": 1,
      "cider_id": "cider-550e8400-e29b-41d4-a716-446655440001"
    },
    {
      "slot": 2,
      "cider_id": "cider-550e8400-e29b-41d4-a716-446655440003"
    }
  ]
}

Response: 201 Created
{
  "id": "battle-550e8400-e29b-41d4-a716-446655440010",
  "battle_type": "orc_beer_skirmish",
  "status": "active",
  "turn": 1,
  "current_player": "user",
  "participants": {
    "user_team": [
      {
        "slot": 1,
        "cider": {
          "id": "cider-550e8400-e29b-41d4-a716-446655440001",
          "name": "Autumn Harvest Traditional",
          "battle_stats": {
            "hp": 100,
            "max_hp": 100,
            "attack": 65,
            "defense": 58,
            "special_move": "Orchard Slam",
            "type_effectiveness": ["weak_vs_bitter", "strong_vs_sweet"]
          }
        }
      }
    ],
    "opponent_team": [
      {
        "slot": 1,
        "enemy": {
          "id": "orc-beer-basic-lager",
          "name": "Mordor Pale Lager",
          "battle_stats": {
            "hp": 80,
            "max_hp": 80,
            "attack": 55,
            "defense": 70,
            "special_move": "Bitter Strike"
          }
        }
      }
    ]
  },
  "created_at": "2024-01-20T20:00:00Z"
}
```

#### 3.4.2 Execute Battle Move
```http
POST /api/v1/battles/{battleId}/moves
Authorization: Bearer {access_token}
Content-Type: application/json
X-Battle-Version: {battle_state_version}

{
  "move": {
    "action": "attack",
    "attacker_slot": 1,
    "target_slot": 1,
    "move_type": "special_attack"
  },
  "client_timestamp": "2024-01-20T20:01:30.123Z"
}

Response: 200 OK
{
  "battle_id": "battle-550e8400-e29b-41d4-a716-446655440010",
  "move_result": {
    "action": "attack",
    "attacker": {
      "name": "Autumn Harvest Traditional",
      "slot": 1
    },
    "target": {
      "name": "Mordor Pale Lager",
      "slot": 1
    },
    "damage": 28,
    "effectiveness": "super_effective",
    "critical": false,
    "special_effects": []
  },
  "battle_state": {
    "status": "active",
    "turn": 2,
    "current_player": "opponent",
    "participants": {
      "user_team": [
        {
          "slot": 1,
          "cider": {
            "name": "Autumn Harvest Traditional",
            "battle_stats": {
              "hp": 100,
              "max_hp": 100
            }
          }
        }
      ],
      "opponent_team": [
        {
          "slot": 1,
          "enemy": {
            "name": "Mordor Pale Lager",
            "battle_stats": {
              "hp": 52,
              "max_hp": 80
            }
          }
        }
      ]
    }
  }
}
```

### 3.5 Progression & Achievements API

#### 3.5.1 Seven Rings Progress
```http
GET /api/v1/users/me/progression/rings
Authorization: Bearer {access_token}

Response: 200 OK
{
  "rings": {
    "explorers_ring": {
      "tier": "silver",
      "current_value": 18,
      "tier_requirement": 25,
      "next_tier": "gold",
      "next_requirement": 35,
      "progress_percentage": 72,
      "unlocked_at": "2024-01-10T15:30:00Z",
      "benefits": [
        "Regional discovery bonuses",
        "Geographic analytics access"
      ]
    },
    "artisans_ring": {
      "tier": "copper",
      "current_value": 12,
      "tier_requirement": 15,
      "next_tier": "silver",
      "next_requirement": 25,
      "progress_percentage": 80,
      "unlocked_at": "2024-01-05T12:00:00Z"
    },
    "warriors_ring": {
      "tier": "copper",
      "current_value": 32,
      "tier_requirement": 75,
      "next_tier": "silver",
      "next_requirement": 200,
      "progress_percentage": 42.7,
      "unlocked_at": "2024-01-08T18:45:00Z"
    }
  },
  "overall_progress": {
    "total_rings_unlocked": 6,
    "highest_tier": "silver",
    "rings_completed": 0,
    "estimated_one_ring_date": "2026-03-15T00:00:00Z"
  }
}
```

#### 3.5.2 Quest System
```http
GET /api/v1/users/me/quests
Authorization: Bearer {access_token}

Response: 200 OK
{
  "active_quests": [
    {
      "id": "quest-weekly-explorer-001",
      "type": "weekly",
      "title": "Journey to Three Realms",
      "description": "Log ciders from 3 different regions this week",
      "objectives": [
        {
          "id": "obj-1",
          "description": "Log a cider from New England",
          "progress": 1,
          "target": 1,
          "completed": true
        },
        {
          "id": "obj-2", 
          "description": "Log a cider from the Pacific Northwest",
          "progress": 0,
          "target": 1,
          "completed": false
        },
        {
          "id": "obj-3",
          "description": "Log a cider from the United Kingdom",
          "progress": 0,
          "target": 1,
          "completed": false
        }
      ],
      "rewards": {
        "experience": 250,
        "ring_progress": {
          "explorers_ring": 1
        },
        "special_items": ["Regional Explorer Badge"]
      },
      "expires_at": "2024-01-27T23:59:59Z",
      "difficulty": "medium"
    }
  ],
  "completed_quests_this_week": 2,
  "available_quest_slots": 3
}
```

### 3.6 Analytics & Insights API

#### 3.6.1 Personal Analytics ("Palantír")
```http
GET /api/v1/users/me/analytics/insights
Authorization: Bearer {access_token}

Response: 200 OK
{
  "taste_profile": {
    "preferred_sweetness": {
      "average": 4.2,
      "range": [2, 7],
      "trend": "slightly_sweeter_over_time"
    },
    "preferred_complexity": {
      "average": 6.8,
      "range": [4, 9],
      "trend": "increasingly_complex"
    },
    "style_preferences": [
      {
        "style": "Traditional Dry",
        "percentage": 28.5,
        "average_rating": 8.2
      },
      {
        "style": "Hopped Cider",
        "percentage": 22.3,
        "average_rating": 8.6
      }
    ]
  },
  "consumption_patterns": {
    "most_active_day": "friday",
    "most_active_time": "evening",
    "seasonal_trends": {
      "spring": 18,
      "summer": 35,
      "fall": 28,
      "winter": 19
    },
    "venue_types": {
      "home": 45,
      "restaurant": 32,
      "brewery_taproom": 15,
      "festival": 8
    }
  },
  "geographic_journey": {
    "regions_explored": 18,
    "countries_explored": 5,
    "favorite_region": "Vermont, USA",
    "exploration_map_data": [
      {
        "region": "Vermont, USA",
        "ciders_tried": 45,
        "average_rating": 8.4,
        "coordinates": [44.2601, -72.5806]
      }
    ]
  }
}
```

## 4. GraphQL API Schema

### 4.1 GraphQL Schema Definition
```graphql
# User and Authentication Types
type User {
  id: ID!
  username: String!
  email: String!
  createdAt: DateTime!
  preferences: UserPreferences!
  subscription: Subscription
  stats: UserStats!
  sevenRings: SevenRingsProgress!
}

type UserPreferences {
  privacyLevel: PrivacyLevel!
  notificationSettings: NotificationSettings!
  displayUnits: DisplayUnits!
}

type UserStats {
  totalCidersLogged: Int!
  uniqueCidersTried: Int!
  favoriteStyle: CiderStyle
  totalBattles: Int!
  battleVictories: Int!
  winRate: Float!
}

# Cider Domain Types
type Cider {
  id: ID!
  name: String!
  producer: Producer!
  style: CiderStyle!
  abv: Float
  description: String
  attributes: CiderAttributes!
  images: [CiderImage!]!
  communityData: CiderCommunityData!
  battleStats: CiderBattleStats!
}

type CiderAttributes {
  sweetness: Int!
  complexity: Int!
  body: Int!
  alcoholStrength: Int!
  rarity: RarityLevel!
}

type CiderBattleStats {
  baseAttack: Int!
  baseDefense: Int!
  specialMove: String!
  typeEffectiveness: [TypeEffectiveness!]!
}

# Battle System Types
type Battle {
  id: ID!
  battleType: BattleType!
  status: BattleStatus!
  turn: Int!
  currentPlayer: Player!
  participants: BattleParticipants!
  moves: [BattleMove!]!
  result: BattleResult
  createdAt: DateTime!
}

type BattleParticipants {
  userTeam: [BattleUnit!]!
  opponentTeam: [BattleUnit!]!
}

type BattleUnit {
  slot: Int!
  cider: Cider
  enemy: Enemy
  currentHp: Int!
  maxHp: Int!
  statusEffects: [StatusEffect!]!
}

# Progression Types
type SevenRingsProgress {
  explorersRing: RingProgress!
  artisansRing: RingProgress!
  warriorsRing: RingProgress!
  chroniclersRing: RingProgress!
  collectorsRing: RingProgress!
  celebrantsRing: RingProgress!
  scholarsRing: RingProgress!
  oneRingProgress: OneRingProgress!
}

type RingProgress {
  tier: RingTier!
  currentValue: Int!
  tierRequirement: Int!
  nextTier: RingTier
  nextRequirement: Int
  progressPercentage: Float!
  unlockedAt: DateTime
  benefits: [String!]!
}

# Query Root
type Query {
  # User Operations
  me: User
  user(id: ID!): User
  
  # Cider Operations
  cider(id: ID!): Cider
  ciders(
    search: String
    style: ID
    producer: ID
    region: String
    abvMin: Float
    abvMax: Float
    rarity: RarityLevel
    sortBy: CiderSortField
    sortOrder: SortOrder
    first: Int
    after: String
  ): CiderConnection!
  
  # Battle Operations
  battle(id: ID!): Battle
  myBattles(
    status: BattleStatus
    first: Int
    after: String
  ): BattleConnection!
  
  # Analytics Operations
  myAnalytics(
    timeRange: TimeRange
    includePrivate: Boolean
  ): UserAnalytics!
}

# Mutation Root
type Mutation {
  # Authentication
  login(email: String!, password: String!): AuthPayload!
  register(input: RegisterInput!): AuthPayload!
  refreshToken: AuthPayload!
  
  # User Management
  updateProfile(input: UpdateProfileInput!): User!
  updatePreferences(input: UpdatePreferencesInput!): User!
  
  # Cider Logging
  createCiderLog(input: CreateCiderLogInput!): CiderLog!
  updateCiderLog(id: ID!, input: UpdateCiderLogInput!): CiderLog!
  deleteCiderLog(id: ID!): Boolean!
  
  # Battle System
  createBattle(input: CreateBattleInput!): Battle!
  executeBattleMove(battleId: ID!, input: BattleMoveInput!): BattleMoveResult!
  surrenderBattle(battleId: ID!): Battle!
  
  # Quest System
  acceptQuest(questId: ID!): Quest!
  completeQuestObjective(questId: ID!, objectiveId: ID!): Quest!
}

# Subscription Root
type Subscription {
  # Real-time battle updates
  battleUpdated(battleId: ID!): Battle!
  
  # Quest progress notifications
  questProgressUpdated(userId: ID!): QuestProgress!
  
  # Ring progression notifications
  ringProgressUpdated(userId: ID!): SevenRingsProgress!
  
  # Community notifications
  newCiderAdded(region: String): Cider!
}
```

### 4.2 GraphQL Query Examples

#### 4.2.1 Comprehensive User Profile Query
```graphql
query GetUserProfile {
  me {
    id
    username
    email
    stats {
      totalCidersLogged
      uniqueCidersTried
      favoriteStyle {
        name
        category
      }
      winRate
    }
    sevenRings {
      explorersRing {
        tier
        currentValue
        tierRequirement
        progressPercentage
        benefits
      }
      artisansRing {
        tier
        currentValue
        progressPercentage
      }
      oneRingProgress {
        estimatedCompletionDate
        overallProgress
      }
    }
  }
}
```

#### 4.2.2 Cider Search with Battle Stats
```graphql
query SearchCiders($search: String!, $first: Int!, $after: String) {
  ciders(search: $search, first: $first, after: $after) {
    edges {
      node {
        id
        name
        producer {
          name
          region
        }
        style {
          name
          category
        }
        attributes {
          sweetness
          complexity
          rarity
        }
        battleStats {
          baseAttack
          baseDefense
          specialMove
        }
        communityData {
          averageRating
          totalLogs
          popularityRank
        }
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

## 5. Real-Time WebSocket API

### 5.1 WebSocket Connection
```javascript
// WebSocket connection establishment
const ws = new WebSocket('wss://api.fellowshipcider.com/ws');

// Authentication message
ws.send(JSON.stringify({
  type: 'auth',
  token: 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'
}));

// Subscribe to battle updates
ws.send(JSON.stringify({
  type: 'subscribe',
  channel: 'battle',
  battle_id: 'battle-550e8400-e29b-41d4-a716-446655440010'
}));
```

### 5.2 Real-Time Event Types

#### 5.2.1 Battle Events
```javascript
// Opponent move received
{
  "type": "battle_move",
  "battle_id": "battle-550e8400-e29b-41d4-a716-446655440010",
  "move": {
    "attacker_slot": 1,
    "target_slot": 1,
    "action": "attack",
    "damage": 15,
    "effectiveness": "normal"
  },
  "new_state": {
    "turn": 3,
    "current_player": "user",
    "participants": { /* updated battle state */ }
  }
}

// Battle completed
{
  "type": "battle_completed",
  "battle_id": "battle-550e8400-e29b-41d4-a716-446655440010",
  "result": {
    "winner": "user",
    "rewards": {
      "experience": 150,
      "ring_progress": {
        "warriors_ring": 1
      }
    }
  }
}
```

#### 5.2.2 Progression Events
```javascript
// Ring tier advancement
{
  "type": "ring_advanced",
  "user_id": "550e8400-e29b-41d4-a716-446655440000",
  "ring": "explorers_ring",
  "old_tier": "copper",
  "new_tier": "silver",
  "benefits_unlocked": [
    "Regional discovery bonuses",
    "Geographic analytics access"
  ]
}

// Quest completion
{
  "type": "quest_completed",
  "user_id": "550e8400-e29b-41d4-a716-446655440000",
  "quest": {
    "id": "quest-weekly-explorer-001",
    "title": "Journey to Three Realms",
    "rewards": {
      "experience": 250,
      "ring_progress": {
        "explorers_ring": 1
      }
    }
  }
}
```

## 6. Error Handling & Status Codes

### 6.1 HTTP Status Codes
- **200 OK**: Successful GET, PATCH, PUT requests
- **201 Created**: Successful POST requests creating new resources
- **204 No Content**: Successful DELETE requests
- **400 Bad Request**: Invalid request format or parameters
- **401 Unauthorized**: Missing or invalid authentication
- **403 Forbidden**: Valid authentication but insufficient permissions
- **404 Not Found**: Resource not found
- **409 Conflict**: Resource conflict (e.g., duplicate entries)
- **422 Unprocessable Entity**: Valid format but semantic errors
- **429 Too Many Requests**: Rate limit exceeded
- **500 Internal Server Error**: Unexpected server error
- **503 Service Unavailable**: Temporary service outage

### 6.2 Error Response Format
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request contains invalid data",
    "details": {
      "field": "email",
      "issue": "Email address is not in valid format"
    },
    "request_id": "req_550e8400e29b41d4a716446655440000",
    "timestamp": "2024-01-20T20:30:00Z"
  }
}
```

### 6.3 Common Error Codes
- **AUTH_REQUIRED**: Authentication token missing
- **AUTH_INVALID**: Authentication token invalid or expired
- **AUTH_INSUFFICIENT**: Insufficient permissions for operation
- **VALIDATION_ERROR**: Request data validation failed
- **RESOURCE_NOT_FOUND**: Requested resource does not exist
- **RESOURCE_CONFLICT**: Resource already exists or conflict detected
- **RATE_LIMIT_EXCEEDED**: Too many requests from client
- **BATTLE_INVALID_STATE**: Battle operation not valid in current state
- **CIDER_NOT_FOUND**: Specified cider does not exist
- **USER_NOT_FOUND**: Specified user does not exist
- **NETWORK_TIMEOUT**: Request timed out due to slow connection
- **STORAGE_FULL**: Device storage insufficient for operation
- **CAMERA_PERMISSION_DENIED**: Camera access not granted
- **LOCATION_PERMISSION_DENIED**: Location access not granted
- **SYNC_CONFLICT**: Data conflict during offline/online synchronization

## 7. Rate Limiting & Performance

### 7.1 Rate Limiting Strategy
```http
# Rate limit headers in responses
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642712400
X-RateLimit-Retry-After: 60
```

### 7.2 Rate Limit Tiers
- **Unauthenticated**: 100 requests per hour
- **Basic User**: 1,000 requests per hour
- **Premium User**: 5,000 requests per hour
- **Enterprise**: 50,000 requests per hour

### 7.3 Performance Optimizations
- **Response Caching**: ETag and Last-Modified headers
- **Compression**: Gzip compression for all responses
- **Pagination**: Cursor-based pagination for large datasets
- **Field Selection**: GraphQL field-level optimization
- **CDN Integration**: Static asset delivery via CDN
- **Request Batching**: Combine multiple operations into single requests
- **Delta Synchronization**: Send only changed data during sync operations
- **Image Optimization**: Automatic image compression and format selection
- **Offline Queue**: Queue requests for execution when connectivity restored
- **Prefetching**: Anticipatory loading of likely-needed data

#### 7.3.1 Mobile-Specific Optimizations
- **Request Coalescing**: Batch multiple API calls within 100ms window
- **Background Sync**: Defer non-critical operations to background app state
- **Network-Aware Quality**: Adjust image quality based on connection speed
- **Retry Logic**: Exponential backoff with jitter for failed requests
- **Connection Pooling**: Reuse HTTP connections to reduce latency

## 8. API Versioning Strategy

### 8.1 Versioning Approach
- **URL Versioning**: `/api/v1/`, `/api/v2/` in URL path
- **Backward Compatibility**: Maintain previous version for 12 months
- **Deprecation Warnings**: Clear communication of deprecated endpoints
- **Semantic Versioning**: Major.Minor.Patch version numbering

### 8.2 Version Lifecycle
```http
# Version headers
GET /api/v1/ciders
API-Version: 1.2.0
API-Supported-Versions: 1.0.0, 1.1.0, 1.2.0
API-Deprecated-Versions: 1.0.0
```

### 8.3 Migration Support
- **Automatic Migration**: Transparent upgrade for compatible changes
- **Migration Guides**: Detailed documentation for breaking changes
- **Dual-Version Support**: Temporary support during transition periods
- **Client SDK Updates**: Coordinated SDK releases with API versions

## 9. Testing & Documentation

### 9.1 API Testing Strategy
- **Unit Tests**: Individual endpoint testing with mocked dependencies
- **Integration Tests**: End-to-end workflow testing
- **Contract Tests**: API contract validation between services
- **Performance Tests**: Load testing and benchmark validation
- **Security Tests**: Vulnerability scanning and penetration testing

### 9.2 Documentation Standards
- **OpenAPI 3.0**: Complete API specification with examples
- **Interactive Documentation**: Swagger UI with try-it-out functionality
- **Code Examples**: Multi-language client implementation examples
- **Changelog**: Detailed version history and migration guides
- **Postman Collection**: Ready-to-use API testing collection

---

*This API Specification provides comprehensive technical documentation for all "Fellowship of the Cider" API endpoints, ensuring consistent, secure, and performant integration for the mobile application and potential third-party developers.*