# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains **"The Fellowship of the Cider"** - a gamified mobile application that transforms cider logging into an epic Lord of the Rings-inspired adventure. The project is currently in the design and specification phase, with comprehensive research and documentation driving the development approach.

### Core Application Concept
- **Theme**: Medieval fantasy inspired by LOTR with parchment textures, hand-drawn map aesthetics
- **Purpose**: Personal cider logging app with Pokemon-style battle mechanics (ciders vs orc beers)
- **Key Features**: Seven Rings progression system, rarity tiers (Common → Mythical/Artifact), quest system, achievement tracking
- **Target**: Solo user comprehensive cider journey tracker with gamified collection mechanics

## Repository Structure

### Primary Documentation
- `functional_specification.md` - Core functional specification with complete feature details
- `cider_database_specification.md` - Database schema and cider data model design

### Research Foundation (`docs/04_Research/`)
The project is built on extensive research across multiple domains:
- **LOTR Integration**: Comprehensive lore framework for authentic theming
- **Cider Knowledge**: Detailed research on styles, regions, production methods
- **Battle Mechanics**: Pokemon-inspired turn-based combat system design
- **Progression Systems**: Seven Rings system with balanced engagement curves
- **Technical Systems**: Image recognition, rarity algorithms, offline sync

### Design Documents (`docs/03_Design/`)
- Architecture design documents (system, component, data, API, security)
- UI/UX design specifications (wireframes, mockups, user flows)
- Data models for users, ciders, and battle mechanics
- Feature-specific designs (quest system, battle system, achievements)

### Planning Documents (`docs/02_Planning/`)
- High-level development plan and task decomposition
- Resource allocation and risk assessment strategies

### Agent System (`agents/`)
Sophisticated internal capability system with specialized modules:
- **Core Modules**: `coder.md`, `planner.md`, `researcher.md`, `reviewer.md`, `tester.md`
- **SPARC Methodology**: `architecture.md`, `pseudocode.md`, `refinement.md`, `specification.md`
- **Development Types**: Backend, frontend, mobile (React Native focus)
- **Specialized Capabilities**: Testing (unit, integration), DevOps (CI/CD), documentation

## Architecture Philosophy

### SPARC Methodology Integration
The agent system follows the SPARC methodology:
- **S**pecification → **P**seudocode → **A**rchitecture → **R**efinement → **C**oding
- Each phase has dedicated modules with specific capabilities and responsibilities
- Architecture phase emphasizes microservices, scalability, and security-first design

### Technology Stack Expectations
Based on research and specifications:
- **Mobile**: React Native for cross-platform development
- **Backend**: Node.js/TypeScript with microservices architecture
- **Database**: PostgreSQL with potential Supabase integration
- **Battle System**: Turn-based mechanics with type effectiveness matrices
- **Image Recognition**: OCR capabilities for cider label detection

### Data Architecture Principles
- **Three-tier model**: Global Cider catalog → UserDiscoveredCider → CiderLog entries
- **Rarity System**: Algorithm-driven assignment based on production volume, geographic exclusivity, limited editions
- **Flavor Profiling**: 2D coordinate system for battle mechanics and recommendations
- **Battle Stats**: Manually curated base_attack, base_defense, special_move fields

## Agent System Instructions

**CRITICAL**: When working in this repository, you MUST follow the agent system instructions defined in the `agents/` folder. This system provides specialized internal capabilities and structured development approaches.

### SPARC Methodology (Primary Development Approach)
All development tasks must follow the SPARC methodology:

1. **Specification** (`agents/sparc/specification.md`): Define clear, measurable requirements with acceptance criteria
2. **Pseudocode** (`agents/sparc/pseudocode.md`): Design algorithms and logic flow before implementation
3. **Architecture** (`agents/sparc/architecture.md`): Create scalable system designs with proper component separation
4. **Refinement** (`agents/sparc/refinement.md`): Use TDD, optimize performance, and improve code quality
5. **Coding** (`agents/core/coder.md`): Implement using best practices and clean code principles

### Core Internal Capabilities
- **Planner** (`agents/core/planner.md`): Break complex tasks into manageable components with clear dependencies
- **Researcher** (`agents/core/researcher.md`): Conduct thorough codebase analysis and pattern recognition
- **Coder** (`agents/core/coder.md`): Write clean, maintainable code following SOLID principles
- **Reviewer** (`agents/core/reviewer.md`): Ensure code quality through comprehensive review processes
- **Tester** (`agents/core/tester.md`): Implement comprehensive testing strategies (unit, integration, E2E)

### Mobile Development Requirements
Given this is a React Native cider app, follow the specialized mobile agent (`agents/specialized/mobile/spec-mobile-react-native.md`):
- Use functional components with hooks
- Implement React Navigation for navigation
- Handle platform-specific differences (iOS/Android)
- Optimize for mobile performance and memory usage
- Test on both iOS and Android platforms
- Follow mobile UI/UX best practices

### Code Quality Standards (Enforced by Agents)
- **SOLID Principles**: Single responsibility, open/closed, Liskov substitution, interface segregation, dependency inversion
- **TDD Approach**: Red-Green-Refactor cycle (write failing tests → implement → refactor)
- **Test Coverage**: >80% coverage with unit, integration, and E2E tests
- **Error Handling**: Comprehensive error boundaries with user-friendly messages
- **TypeScript**: Strong typing for better documentation and error prevention
- **Performance**: O(log n) or better for critical paths, implement caching where appropriate

### Security Requirements (Enforced by Review Agent)
- Never hardcode secrets or API keys
- Implement proper authentication/authorization (JWT, OAuth2)
- Validate all inputs and sanitize outputs
- Use parameterized queries for database operations
- Apply encryption at rest (AES-256) and in transit (TLS 1.3)
- Follow OWASP Top 10 security practices

### Architecture Requirements (Enforced by Architecture Agent)
- **Microservices**: Loose coupling, high cohesion, single responsibility per service
- **Scalability**: Horizontal scaling with load balancers and auto-scaling
- **Observability**: Comprehensive logging, monitoring, and alerting
- **Caching Strategy**: Multi-layer caching (CDN, API gateway, application, database)
- **Database Design**: Proper indexing, partitioning for large datasets
- **API Design**: RESTful APIs with OpenAPI specifications

## Key Implementation Areas

### Battle System Complexity
The Pokemon-inspired battle system requires:
- Type effectiveness matrices between cider styles and orc beer types
- Stat specialization (Sweetness, Complexity, Alcohol Strength, Body, Rarity)
- Status effects (Wild Yeast Infection, Oxidation, Brett Character)
- Turn-based mechanics with priority systems and damage variance
- Evolution/aging mechanics for long-term progression

### Seven Rings Progression
Balanced progression system with four tiers (Copper → Silver → Gold → Mithril):
- Explorer's Ring (geographic diversity): 5 → 15 → 35 → 75 regions
- Artisan's Ring (style mastery): 8 → 25 → 60 → 120 styles
- Warrior's Ring (battle victories): 20 → 75 → 200 → 500 victories
- Chronicler's Ring (logging streaks): 7 → 30 → 90 → 365 days
- Plus Collector's, Celebrant's, and Scholar's rings with specific mechanics

### Rarity System Implementation
Data-driven approach using multiple factors:
- Production scale and geographic exclusivity
- Limited editions and seasonal availability
- Unique ingredients/methods and cultural significance
- Integration with OpenBreweryDB API and government databases
- Community-driven submissions with moderation workflows

## File Structure Patterns

### Agent Module Naming
- Pattern: `[type]-[specialization]-[capability].md`
- Examples: `dev-backend-api.md`, `test-unit-jest.md`, `arch-cloud-aws.md`

### Documentation Organization
- Research results and summaries are clearly separated
- Functional specifications drive technical implementation
- Architecture documents provide scalable system designs

## Agent Workflow Requirements

### Task Execution Process
1. **Planning Phase**: Use the Planner agent to break down complex requests into manageable tasks with clear dependencies
2. **Research Phase**: Apply the Researcher agent to analyze existing code patterns and gather context
3. **SPARC Implementation**: Follow the complete SPARC methodology for any substantial development work
4. **Quality Assurance**: Use the Reviewer and Tester agents to ensure code quality and comprehensive testing
5. **Documentation**: Maintain clear documentation throughout the development process

### Agent Coordination Patterns
- **Sequential Execution**: Complex tasks should progress through SPARC phases in order
- **Quality Gates**: Each phase must meet completion criteria before advancing
- **Continuous Integration**: All changes must pass automated tests and code quality checks
- **Cross-Agent Communication**: Agents should share context and findings with each other

### Memory and Context Management
The agent system includes memory capabilities:
- Store architectural decisions and patterns for reuse
- Track progress through SPARC phases
- Maintain testing strategies and performance benchmarks  
- Document lessons learned and optimization approaches

## Working in this Repository

### Priority Order for Development
1. **Understand Research Foundation**: Review the comprehensive research in `docs/04_Research/`
2. **Follow Agent Instructions**: Strictly adhere to the agent system capabilities and workflows
3. **Apply SPARC Methodology**: Use the structured development approach for all features
4. **Respect Project Vision**: Create an authentic, engaging experience that respects LOTR lore and cider culture
5. **Maintain Quality Standards**: Deliver sophisticated gamification mechanics with proper testing and documentation

### Critical Success Factors
- Deep understanding of Pokemon-inspired battle mechanics translated to cider vs orc beer combat
- Authentic LOTR theming throughout the user experience
- Sophisticated yet accessible progression systems (Seven Rings)
- Comprehensive cider knowledge base integration
- Mobile-optimized performance for React Native platform

## Tool Usage Notes
- **Read tool error prevention**: NEVER use Read tool on directories - it will throw "EISDIR: illegal operation on a directory, read". Always use Bash with `ls` or Glob tool to explore directory contents first, then Read specific files.