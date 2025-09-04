# The Fellowship of the Cider - System Architecture Specification

This document defines the comprehensive system architecture for "The Fellowship of the Cider" application, establishing the technical foundation for a scalable, secure, and maintainable cider logging platform.

## 1. Architecture Overview

### 1.1 System Vision
Design a cloud-native, microservices-based architecture that supports a cross-platform mobile application with offline-first capabilities, real-time synchronization, and battle system mechanics, capable of scaling from thousands to millions of users.

### 1.2 Architectural Principles
- **Offline-First**: Core functionality available without internet connectivity
- **Mobile-Optimized**: Designed specifically for mobile device constraints and capabilities
- **Microservices**: Loosely coupled services with single responsibilities
- **Cloud-Native**: Built for elastic scaling and high availability
- **Security-First**: Security integrated at every architectural layer
- **Data Consistency**: Eventually consistent with conflict resolution strategies

### 1.3 Architecture Style
**Primary Pattern**: Event-Driven Microservices Architecture with CQRS (Command Query Responsibility Segregation)
**Secondary Patterns**: 
- Clean Architecture for mobile app structure
- API Gateway pattern for service orchestration
- Database-per-service for data isolation
- Eventual consistency with event sourcing

## 2. System Context & External Interfaces

### 2.1 System Context Diagram
```
[User Mobile Device] ←→ [API Gateway] ←→ [Microservices Cluster]
                                              ↓
[Third-party Services] ←→ [Event Bus] ←→ [Database Cluster]
                                              ↓
[Content Delivery Network] ←→ [File Storage] ←→ [Monitoring & Analytics]
```

### 2.2 External System Interfaces

#### 2.2.1 Authentication Providers
- **Supabase Auth**: Primary OAuth2/JWT authentication
- **Apple Sign-In**: iOS platform integration
- **Google Sign-In**: Android platform integration
- **Email/Password**: Traditional authentication fallback

#### 2.2.2 Data Sources
- **OpenBreweryDB API**: Brewery and producer information
- **Government Databases**: Regulatory and compliance data
- **Geolocation Services**: GPS coordinate and reverse geocoding
- **Image Recognition APIs**: OCR for cider label detection

#### 2.2.3 Platform Services
- **Apple App Store**: iOS distribution and in-app purchases
- **Google Play Store**: Android distribution and billing
- **Push Notification Services**: APNs (iOS) and FCM (Android)
- **Analytics Platforms**: Crash reporting and user behavior tracking

#### 2.2.4 Infrastructure Services
- **Supabase**: PostgreSQL database, real-time subscriptions, edge functions
- **Cloudflare CDN**: Global content delivery and DDoS protection
- **AWS S3**: Object storage for images and file assets
- **Stripe**: Payment processing for premium subscriptions

## 3. High-Level Architecture

### 3.1 Architectural Layers

#### 3.1.1 Presentation Layer (Mobile App)
- **React Native Application**: Cross-platform mobile client
- **Offline Storage**: SQLite with sync capabilities
- **State Management**: Redux Toolkit with RTK Query
- **Navigation**: React Navigation with deep linking support

#### 3.1.2 API Gateway Layer
- **Supabase API Gateway**: Request routing, authentication, rate limiting
- **Load Balancer**: Traffic distribution across service instances
- **API Versioning**: Backward-compatible API evolution
- **Request/Response Transformation**: Data format standardization

#### 3.1.3 Application Services Layer
- **User Service**: Authentication, profiles, preferences
- **Cider Service**: Catalog management, search, recommendations
- **Logging Service**: Cider entries, ratings, notes, photos
- **Battle Service**: Combat mechanics, matchmaking, tournaments
- **Quest Service**: Challenge generation, progress tracking, rewards
- **Analytics Service**: Data processing, insights, reporting

#### 3.1.4 Data Layer
- **Primary Database**: PostgreSQL with read replicas
- **Cache Layer**: Redis for session management and temporary data
- **File Storage**: S3-compatible object storage for images
- **Search Engine**: Elasticsearch for full-text cider search

#### 3.1.5 Infrastructure Layer
- **Container Orchestration**: Docker containers with Kubernetes
- **Service Mesh**: Istio for service-to-service communication
- **Monitoring Stack**: Prometheus, Grafana, Jaeger for observability
- **CI/CD Pipeline**: GitHub Actions with automated testing and deployment

### 3.2 Service Architecture

#### 3.2.1 Core Services

**User Management Service**
- **Responsibilities**: User registration, authentication, profile management
- **Database**: User profiles, preferences, authentication tokens
- **APIs**: REST endpoints for CRUD operations
- **Events**: User created, updated, deleted, authenticated

**Cider Catalog Service**
- **Responsibilities**: Global cider database, producer information, metadata
- **Database**: Cider definitions, styles, producers, regions
- **APIs**: Search, filtering, recommendation endpoints
- **Events**: Cider added, updated, popularity changed

**Logging Service**
- **Responsibilities**: Personal cider logs, ratings, photos, notes
- **Database**: User cider entries, ratings, timestamps, locations
- **APIs**: CRUD operations for cider logs, batch operations
- **Events**: Cider logged, rating updated, photo uploaded

**Battle System Service**
- **Responsibilities**: Combat mechanics, matchmaking, tournaments, battle state management
- **Database**: Battle configurations, results, statistics, combat formulas
- **APIs**: Battle initiation, move execution, result recording, state synchronization
- **Events**: Battle started, move made, battle completed, state changed

**Battle Engine Architecture**:
- **Combat Calculator**: Damage formulas, type effectiveness, status effects
- **State Manager**: Battle state persistence, recovery, synchronization
- **AI Engine**: Orc beer opponent behavior and difficulty scaling
- **Balance Manager**: Win rate monitoring, automatic difficulty adjustment

**Progression Service**
- **Responsibilities**: Seven Rings tracking, achievements, experience
- **Database**: User progression, ring status, achievement timestamps
- **APIs**: Progress queries, achievement unlocking
- **Events**: Ring advanced, achievement unlocked, milestone reached

**Quest Service**
- **Responsibilities**: Dynamic quest generation, progress tracking
- **Database**: Quest templates, user quest states, completion records
- **APIs**: Active quest retrieval, progress updates
- **Events**: Quest assigned, progress updated, quest completed

#### 3.2.2 Supporting Services

**Analytics Service**
- **Responsibilities**: Data aggregation, insight generation, reporting
- **Database**: Aggregated metrics, user behavior patterns
- **APIs**: Dashboard data, export functionality
- **Events**: Analytics computed, insights generated

**Notification Service**
- **Responsibilities**: Push notifications, email communications
- **Database**: Notification templates, delivery logs
- **APIs**: Notification scheduling and delivery
- **Events**: Notification sent, delivery confirmed

**File Management Service**
- **Responsibilities**: Image upload, processing, optimization
- **Database**: File metadata, processing status
- **APIs**: Upload endpoints, image serving
- **Events**: File uploaded, processed, deleted

## 4. Data Architecture

### 4.1 Database Design Strategy

#### 4.1.1 Database-per-Service Pattern
Each microservice maintains its own database to ensure loose coupling and independent scaling:

- **User Service Database**: PostgreSQL with user profiles and authentication data
- **Cider Service Database**: PostgreSQL with full-text search capabilities
- **Logging Service Database**: PostgreSQL optimized for time-series data
- **Battle Service Database**: PostgreSQL with complex relational data
- **Analytics Database**: ClickHouse for high-volume analytical queries

#### 4.1.2 Data Consistency Strategy
- **Strong Consistency**: Within service boundaries using ACID transactions
- **Eventual Consistency**: Between services using event-driven updates
- **Conflict Resolution**: Last-writer-wins with user override capabilities
- **Data Synchronization**: Event sourcing with replay capabilities

### 4.2 Core Data Models

#### 4.2.1 User Domain
```sql
-- User Service Database
TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(50) UNIQUE NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

TABLE user_preferences (
    user_id UUID REFERENCES users(id),
    preference_key VARCHAR(100),
    preference_value JSONB,
    PRIMARY KEY (user_id, preference_key)
);
```

#### 4.2.2 Cider Domain
```sql
-- Cider Service Database
TABLE ciders (
    id UUID PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    producer_id UUID REFERENCES producers(id),
    style_id UUID REFERENCES cider_styles(id),
    abv DECIMAL(4,2),
    description TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    search_vector TSVECTOR
);

TABLE cider_attributes (
    cider_id UUID REFERENCES ciders(id),
    sweetness INTEGER CHECK (sweetness BETWEEN 1 AND 10),
    complexity INTEGER CHECK (complexity BETWEEN 1 AND 10),
    body INTEGER CHECK (body BETWEEN 1 AND 10),
    PRIMARY KEY (cider_id)
);
```

#### 4.2.3 Logging Domain
```sql
-- Logging Service Database
TABLE cider_logs (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    cider_id UUID NOT NULL,
    rating INTEGER CHECK (rating BETWEEN 1 AND 5),
    notes TEXT,
    location POINT,
    logged_at TIMESTAMPTZ DEFAULT NOW(),
    synced_at TIMESTAMPTZ
);

TABLE cider_photos (
    id UUID PRIMARY KEY,
    log_id UUID REFERENCES cider_logs(id),
    file_path VARCHAR(255),
    uploaded_at TIMESTAMPTZ DEFAULT NOW()
);
```

#### 4.2.4 Battle Domain
```sql
-- Battle Service Database
TABLE battles (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    opponent_type VARCHAR(50), -- 'orc_beer', 'boss', 'tournament'
    status VARCHAR(20) DEFAULT 'active',
    current_turn INTEGER DEFAULT 1,
    battle_seed INTEGER NOT NULL, -- For deterministic RNG
    difficulty_level INTEGER DEFAULT 1,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    completed_at TIMESTAMPTZ,
    winner VARCHAR(20), -- 'user', 'opponent', 'draw'
    total_turns INTEGER
);

TABLE battle_participants (
    battle_id UUID REFERENCES battles(id),
    slot_number INTEGER,
    cider_id UUID,
    orc_beer_id UUID,
    current_hp INTEGER,
    max_hp INTEGER,
    attack_stat INTEGER,
    defense_stat INTEGER,
    status_effects JSONB DEFAULT '[]',
    PRIMARY KEY (battle_id, slot_number)
);

TABLE battle_moves (
    id UUID PRIMARY KEY,
    battle_id UUID REFERENCES battles(id),
    turn_number INTEGER NOT NULL,
    attacker_slot INTEGER NOT NULL,
    target_slot INTEGER NOT NULL,
    move_type VARCHAR(50) NOT NULL, -- 'attack', 'special_attack', 'defend'
    damage_dealt INTEGER,
    effectiveness_multiplier DECIMAL(3,2),
    critical_hit BOOLEAN DEFAULT FALSE,
    status_effects_applied JSONB DEFAULT '[]',
    executed_at TIMESTAMPTZ DEFAULT NOW()
);

TABLE orc_beers (
    id UUID PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL, -- 'lager', 'ale', 'stout', 'pilsner'
    base_attack INTEGER NOT NULL,
    base_defense INTEGER NOT NULL,
    base_hp INTEGER NOT NULL,
    special_abilities JSONB DEFAULT '[]',
    rarity VARCHAR(20) DEFAULT 'common'
);

TABLE type_effectiveness (
    cider_type VARCHAR(50) NOT NULL,
    orc_beer_type VARCHAR(50) NOT NULL,
    effectiveness_multiplier DECIMAL(3,2) NOT NULL,
    PRIMARY KEY (cider_type, orc_beer_type)
);
```

### 4.3 Data Flow Architecture

#### 4.3.1 Command Flow (Write Operations)
1. **Mobile App** → **API Gateway** → **Service** → **Database**
2. **Service** publishes **Domain Events** → **Event Bus**
3. **Other Services** subscribe to relevant events → **Update Local Data**
4. **Analytics Service** processes events → **Generate Insights**

#### 4.3.2 Query Flow (Read Operations)
1. **Mobile App** → **API Gateway** → **Query Service** → **Database/Cache**
2. **Cache Layer** provides fast access to frequently requested data
3. **Read Replicas** handle complex analytical queries
4. **Search Engine** provides full-text search capabilities

#### 4.3.3 Synchronization Flow
1. **Mobile App** stores data locally in **SQLite**
2. **Background Sync Service** detects connectivity
3. **Conflict Resolution** handles concurrent modifications
4. **Event Replay** ensures data consistency

## 5. Security Architecture

### 5.1 Security Framework

#### 5.1.1 Authentication & Authorization
- **OAuth 2.0 + OpenID Connect**: Standard authentication protocols
- **JWT Tokens**: Stateless authentication with short-lived access tokens
- **Refresh Token Rotation**: Enhanced security with automatic token renewal
- **Role-Based Access Control (RBAC)**: Fine-grained permission management

#### 5.1.2 Data Protection
- **Encryption at Rest**: AES-256 encryption for all stored data
- **Encryption in Transit**: TLS 1.3 for all network communications
- **Data Masking**: Sensitive data protection in logs and non-production environments
- **Key Management**: Automated key rotation and secure key storage

#### 5.1.3 Application Security
- **Input Validation**: Comprehensive validation at API gateway and service levels
- **SQL Injection Prevention**: Parameterized queries and ORM protections
- **XSS Protection**: Content Security Policy and input sanitization
- **API Rate Limiting**: Prevents abuse and ensures fair resource usage

#### 5.1.4 Infrastructure Security
- **Network Segmentation**: Isolated network zones for different service tiers
- **Container Security**: Image scanning, runtime protection, and least-privilege access
- **Secrets Management**: Secure storage and injection of configuration secrets
- **Monitoring & Alerting**: Real-time security event detection and response

#### 5.1.5 Mobile Security
- **Certificate Pinning**: Prevent man-in-the-middle attacks
- **Jailbreak/Root Detection**: Identify compromised devices and limit functionality
- **App Store Code Signing**: Prevent tampering and ensure app authenticity
- **Local Data Encryption**: SQLite database encryption with device keystore
- **Binary Obfuscation**: Protect against reverse engineering of battle formulas
- **Anti-Debugging**: Runtime application self-protection (RASP)

### 5.2 Privacy & Compliance

#### 5.2.1 Data Privacy
- **Privacy by Design**: Minimal data collection and processing
- **Data Anonymization**: User data protection in analytics and reporting
- **Right to Deletion**: Complete data removal capabilities
- **Data Portability**: User data export in standard formats

#### 5.2.2 Regulatory Compliance
- **GDPR Compliance**: European data protection regulation adherence
- **CCPA Compliance**: California privacy law requirements
- **SOC 2 Type II**: Security and availability certification
- **PCI DSS**: Payment card data security (for subscription billing)

## 6. Performance & Scalability

### 6.1 Performance Requirements

#### 6.1.1 Response Time Targets
- **API Responses**: <200ms for 95% of requests
- **App Launch**: <3 seconds cold start, <1 second warm start
- **Image Upload**: <10 seconds for 5MB photos
- **Search Operations**: <500ms for complex queries
- **Offline Sync**: <30 seconds for typical sync operations

#### 6.1.2 Throughput Targets
- **Concurrent Users**: 10,000 simultaneous active users
- **API Requests**: 50,000 requests per minute peak capacity
- **Database Operations**: 5,000 transactions per second
- **File Uploads**: 1,000 simultaneous image uploads
- **Real-time Updates**: 100,000 WebSocket connections

#### 6.1.3 Scalability Strategy
- **Horizontal Scaling**: Auto-scaling service instances based on demand
- **Database Scaling**: Read replicas and sharding for high-volume tables
- **Content Delivery**: Global CDN for static assets and images
- **Caching Strategy**: Multi-layer caching (CDN, API, application, database)

### 6.2 Availability & Reliability

#### 6.2.1 Availability Targets
- **Service Uptime**: 99.9% availability (8.77 hours downtime per year)
- **Data Durability**: 99.999999999% (11 9's) data durability
- **Recovery Time**: <15 minutes recovery from infrastructure failures
- **Data Backup**: Real-time replication with 4-hour point-in-time recovery

#### 6.2.2 Fault Tolerance Design
- **Circuit Breakers**: Service failure isolation and graceful degradation
- **Retry Mechanisms**: Exponential backoff with jitter for transient failures
- **Bulkhead Pattern**: Resource isolation to prevent cascade failures
- **Health Checks**: Automated service health monitoring and recovery

## 7. Technology Stack

### 7.1 Mobile Application Stack

#### 7.1.1 Core Framework
- **React Native**: Cross-platform mobile development framework
- **Expo**: Development toolchain and managed services
- **TypeScript**: Type-safe JavaScript development
- **Metro**: JavaScript bundler optimized for React Native

#### 7.1.2 State Management & Data
- **Redux Toolkit**: Predictable state management with modern Redux patterns
- **RTK Query**: Data fetching and caching solution
- **SQLite**: Local database for offline data storage
- **React Query**: Server state management and synchronization

#### 7.1.3 UI & User Experience
- **React Navigation**: Navigation library with deep linking
- **React Native Paper**: Material Design components
- **Lottie**: Animation library for micro-interactions
- **React Native Fast Image**: Optimized image loading and caching

#### 7.1.4 Device Integration
- **React Native Camera**: Camera integration for photo capture
- **React Native Geolocation**: Location services integration
- **React Native Async Storage**: Persistent local storage
- **React Native Push Notifications**: Push notification handling

### 7.2 Backend Services Stack

#### 7.2.1 Runtime & Framework
- **Node.js**: JavaScript runtime environment
- **TypeScript**: Type-safe server-side development
- **Express.js**: Web application framework
- **Supabase**: Backend-as-a-Service platform

#### 7.2.2 Database & Storage
- **PostgreSQL**: Primary relational database
- **Redis**: In-memory caching and session storage
- **Elasticsearch**: Full-text search engine
- **AWS S3**: Object storage for images and files

#### 7.2.3 Communication & Integration
- **GraphQL**: Flexible API query language
- **Socket.io**: Real-time bidirectional communication
- **Bull Queue**: Background job processing
- **Axios**: HTTP client for external API integration

### 7.3 Infrastructure & DevOps Stack

#### 7.3.1 Containerization & Orchestration
- **Docker**: Application containerization
- **Kubernetes**: Container orchestration and management
- **Helm**: Kubernetes package manager
- **Istio**: Service mesh for microservices communication

#### 7.3.2 Cloud Services
- **Supabase**: Database, authentication, and real-time features
- **Cloudflare**: CDN, DNS, and DDoS protection
- **AWS S3**: Object storage and file hosting
- **Stripe**: Payment processing and subscription management

#### 7.3.3 Monitoring & Observability
- **Prometheus**: Metrics collection and monitoring
- **Grafana**: Metrics visualization and dashboards
- **Jaeger**: Distributed tracing
- **Sentry**: Error tracking and performance monitoring

#### 7.3.4 CI/CD & Development
- **GitHub Actions**: Continuous integration and deployment
- **ESLint**: JavaScript/TypeScript linting
- **Prettier**: Code formatting
- **Jest**: Unit testing framework

## 8. Deployment Architecture

### 8.1 Environment Strategy

#### 8.1.1 Environment Configuration
- **Development**: Local development with hot reloading
- **Testing**: Automated testing with isolated test data
- **Staging**: Production-like environment for final validation
- **Production**: Multi-region deployment with high availability

#### 8.1.2 Infrastructure as Code
- **Terraform**: Infrastructure provisioning and management
- **Kubernetes Manifests**: Application deployment configurations
- **Helm Charts**: Package management for Kubernetes applications
- **GitHub Actions**: Automated deployment pipelines

### 8.2 Deployment Strategy

#### 8.2.1 Mobile App Deployment
- **CodePush**: Over-the-air updates for JavaScript bundles
- **App Store Deployment**: Managed through Expo Application Services
- **Staged Rollout**: Gradual release to percentage of users
- **A/B Testing**: Feature flag-driven experimentation

#### 8.2.2 Backend Deployment
- **Blue-Green Deployment**: Zero-downtime deployments
- **Canary Releases**: Gradual traffic shifting to new versions
- **Database Migrations**: Backward-compatible schema changes
- **Feature Flags**: Runtime configuration for gradual feature rollout

### 8.3 Monitoring & Operations

#### 8.3.1 Application Monitoring
- **Application Performance Monitoring**: Real-time performance metrics
- **Error Tracking**: Crash reporting and error analysis
- **User Analytics**: Usage patterns and feature adoption
- **Business Metrics**: KPI tracking and alerting

#### 8.3.2 Mobile-Specific Monitoring
- **Crash Reporting**: Real-time crash detection with stack traces and device context
- **ANR Detection**: Application Not Responding events on Android
- **Memory Leak Detection**: Automatic memory usage pattern analysis
- **Battery Usage Monitoring**: Track app power consumption impact
- **Network Performance**: API response times across different connection types
- **Battle System Monitoring**: Combat calculation performance and balance metrics

#### 8.3.3 Infrastructure Monitoring
- **Resource Utilization**: CPU, memory, and network monitoring
- **Service Health**: Endpoint availability and response times
- **Database Performance**: Query performance and connection pooling
- **Security Monitoring**: Intrusion detection and compliance scanning

## 9. Integration Architecture

### 9.1 Internal Service Integration

#### 9.1.1 Service Communication Patterns
- **Synchronous**: REST APIs for real-time request/response
- **Asynchronous**: Event-driven messaging for loose coupling
- **GraphQL**: Flexible data fetching for mobile applications
- **gRPC**: High-performance service-to-service communication

#### 9.1.2 Event-Driven Architecture
- **Event Bus**: Central message broker for service coordination
- **Event Sourcing**: Audit trail and state reconstruction capabilities
- **CQRS**: Separate read and write models for optimal performance
- **Saga Pattern**: Distributed transaction management

### 9.2 External Service Integration

#### 9.2.1 Third-Party APIs
- **OpenBreweryDB**: Brewery and producer data integration
- **Geolocation Services**: Reverse geocoding and location data
- **Image Recognition**: OCR and label detection services
- **Payment Processing**: Stripe integration for subscriptions

#### 9.2.2 Platform Integration
- **App Store APIs**: In-app purchase and subscription management
- **Push Notification Services**: Cross-platform notification delivery
- **Analytics Platforms**: User behavior and performance tracking
- **Social Media APIs**: Optional social sharing capabilities

## 10. Evolution & Future Architecture

### 10.1 Architectural Roadmap

#### 10.1.1 Phase 1: MVP Foundation (Months 1-6)
- Basic microservices architecture with core functionality
- PostgreSQL with simple caching layer
- React Native app with offline-first design
- Essential third-party integrations

#### 10.1.2 Phase 2: Scale & Performance (Months 7-12)
- Advanced caching strategies and database optimization
- Event-driven architecture implementation
- Enhanced monitoring and observability
- Geographic distribution preparation

#### 10.1.3 Phase 3: Advanced Features (Months 13-18)
- Machine learning integration for recommendations
- Real-time multiplayer battle features
- Advanced analytics and business intelligence
- Multi-platform expansion (web, desktop)

#### 10.1.4 Phase 4: Enterprise & Ecosystem (Months 19-24)
- API ecosystem for third-party developers
- Enterprise features and white-label solutions
- Global scaling and compliance certifications
- Advanced AI and personalization features

### 10.2 Technology Evolution Strategy

#### 10.2.1 Emerging Technology Adoption
- **Machine Learning**: Recommendation engines and personalization
- **Artificial Intelligence**: Natural language processing for reviews
- **Blockchain**: Potential for cider authenticity and provenance
- **Edge Computing**: Reduced latency for global users

#### 10.2.2 Architecture Modernization
- **Serverless Architecture**: Function-as-a-Service for specific workloads
- **Event Mesh**: Advanced event-driven patterns
- **API-First Design**: Comprehensive API ecosystem
- **Micro-Frontends**: Modular frontend architecture

---

*This System Architecture Specification provides the comprehensive technical foundation for "The Fellowship of the Cider" platform, ensuring scalable, secure, and maintainable system design that supports the application's unique requirements and long-term growth objectives.*