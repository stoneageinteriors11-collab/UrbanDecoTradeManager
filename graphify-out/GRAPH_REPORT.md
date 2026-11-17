# Graph Report - urban-deco-trade-manager  (2026-11-03)

## Corpus Check
- Corpus is ~28,346 words - fits in a single context window. You may not need a graph.

## Summary
- 454 nodes · 621 edges · 38 communities (25 shown, 13 thin omitted)
- Extraction: 97% EXTRACTED · 3% INFERRED · 0% AMBIGUOUS · INFERRED: 19 edges (avg confidence: 0.95)
- Token cost: local AST extraction; host semantic token usage unmetered

## Community Hubs (Navigation)
- Trade Account Editor
- Project Setup and Sessions
- Trade Application Editor
- PostgreSQL Trade Data Model
- Linting and Developer Tools
- TypeScript Compiler Configuration
- Trade Account Listing
- Trade Application Listing
- Build and Deployment Commands
- Shopify Authentication and Webhooks
- Package Metadata
- Runtime Dependencies
- Trade Account Detail Components
- Server Rendering
- Application Route Layouts
- Commission Ledger
- Application Submission Emails
- Trade Approval and Customer Management
- Workspace Build Permissions
- Vite Development Server
- Trade Pricing Discounts
- Login Flow
- GraphQL Code Generation
- Cursor Shopify MCP
- Project Shopify MCP
- Application Detail Components
- Accessibility Lint Plugin
- React Lint Plugin
- Node Type Definitions
- TypeScript Lint Plugin

## God Nodes (most connected - your core abstractions)
1. `compilerOptions` - 18 edges
2. `scripts` - 17 edges
3. `react-router` - 17 edges
4. `authenticate` - 15 edges
5. `TradeAccountDetail()` - 13 edges
6. `PostgreSQL Datasource` - 13 edges
7. `@shopify/shopify-app-react-router` - 10 edges
8. `Shopify React Router App Template` - 10 edges
9. `TradeAccount` - 10 edges
10. `action()` - 9 edges

## Surprising Connections (you probably didn't know these)
- `Filesystem Route Discovery` --conceptually_related_to--> `React Router`  [INFERRED]
  app/routes.js → README.md
- `Shared Prisma Client` --conceptually_related_to--> `PostgreSQL Datasource`  [INFERRED]
  app/db.server.js → prisma/schema.prisma
- `Prisma Migration Deployment` --conceptually_related_to--> `Active Baseline Migration`  [INFERRED]
  shopify.web.toml → prisma/migrations/00000000000000_baseline/migration.sql
- `Session Baseline Table` --conceptually_related_to--> `Session`  [INFERRED]
  prisma/migrations/00000000000000_baseline/migration.sql → prisma/schema.prisma
- `Session Historical Table` --conceptually_related_to--> `Session`  [INFERRED]
  prisma/migrations_backup/20260817055038_init_postgresql/migration.sql → prisma/schema.prisma

## Import Cycles
- None detected.

## Communities (38 total, 13 thin omitted)

### Community 0 - "Trade Account Editor"
Cohesion: 0.04
Nodes (53): actionsContainerStyle, allocationRuleStyle, backStyle, badgeStyle, copyReferralButtonStyle, dangerButton, emptyStateStyle, errorMessageStyle (+45 more)

### Community 1 - "Project Setup and Sessions"
Cohesion: 0.06
Nodes (32): Project Agent Instructions, Shopify AI Toolkit, Filesystem Route Discovery, Upstream Template Change History, Expiring Offline Access Tokens, Claude Instructions, @react-router/fs-routes@^7.12.0, Session Baseline Table (+24 more)

### Community 2 - "Trade Application Editor"
Cohesion: 0.06
Nodes (24): actionsContainerStyle, approveButtonStyle, approvedMessageStyle, backStyle, badgeStyle, cardStyle, deleteCustomerButtonStyle, deleteCustomerContainerStyle (+16 more)

### Community 3 - "PostgreSQL Trade Data Model"
Cohesion: 0.12
Nodes (26): Shared Prisma Client, Commission Baseline Table, CommissionStatus Baseline Enum, Active Baseline Migration, PricingOption Baseline Enum, Referral Baseline Table, TradeAccount Baseline Table, TradeAccountStatus Baseline Enum (+18 more)

### Community 4 - "Linting and Developer Tools"
Cohesion: 0.10
Nodes (21): devDependencies, eslint@^8.57.1, eslint-import-resolver-typescript@^3.10.1, eslint-plugin-import@^2.32.0, graphql-config@^5.1.1, prettier@^3.6.2, @shopify/polaris-types@^1.0.1, @types/eslint@^9.6.1 (+13 more)

### Community 5 - "TypeScript Compiler Configuration"
Cohesion: 0.10
Nodes (19): compilerOptions, allowJs, allowSyntheticDefaultImports, baseUrl, forceConsistentCasingInFileNames, isolatedModules, jsx, lib (+11 more)

### Community 6 - "Trade Account Listing"
Cohesion: 0.12
Nodes (15): badgeStyle, countStyle, emptyStyle, headerStyle, pageStyle, referralStyle, StatusBadge(), subtitleStyle (+7 more)

### Community 7 - "Trade Application Listing"
Cohesion: 0.13
Nodes (15): badgeStyle, countStyle, emptyStyle, formatDate(), headerStyle, pageStyle, StatusBadge(), subtitleStyle (+7 more)

### Community 8 - "Build and Deployment Commands"
Cohesion: 0.12
Nodes (17): scripts, build, config:link, config:use, deploy, dev, docker-start, env (+9 more)

### Community 9 - "Shopify Authentication and Webhooks"
Cohesion: 0.16
Nodes (6): apiVersion, authenticate, registerWebhooks, sessionStorage, shopify, unauthenticated

### Community 10 - "Package Metadata"
Cohesion: 0.13
Nodes (14): author, eslint-plugin-react-hooks@^4.6.2, @typescript-eslint/parser@^6.21.0, engines, node, name, overrides, p-map (+6 more)

### Community 11 - "Runtime Dependencies"
Cohesion: 0.13
Nodes (15): dependencies, pg@^8.23.0, prisma@^6.16.3, @prisma/client@^6.16.3, react-router@^7.12.0, @react-router/serve@^7.12.0, @shopify/app-bridge-react@^4.2.4, @shopify/shopify-app-react-router@^1.2.1 (+7 more)

### Community 12 - "Trade Account Detail Components"
Cohesion: 0.15
Nodes (13): CommissionBadge(), EmptyState(), formatDate(), formatDateTime(), formatPricingOption(), InfoCard(), InfoGrid(), InfoItem() (+5 more)

### Community 14 - "Server Rendering"
Cohesion: 0.20
Nodes (9): handleRequest(), streamTimeout, addDocumentResponseHeaders, isbot@^5.1.31, react-dom@^18.3.1, @react-router/node@^7.12.0, isbot, react-dom (+1 more)

### Community 16 - "Commission Ledger"
Cohesion: 0.24
Nodes (6): CommissionLedger(), formatDate(), money(), StatusBadge(), react@^18.3.1, react

### Community 17 - "Application Submission Emails"
Cohesion: 0.36
Nodes (8): action(), escapeHtml(), getTeamNotificationEmails(), resend, sendApplicationReceivedEmail(), sendNewApplicationTeamEmail(), resend@^6.22.1, resend

### Community 18 - "Trade Approval and Customer Management"
Cohesion: 0.22
Nodes (10): action(), rollbackShopifyCustomer(), addTradeAccountTag(), deleteShopifyCustomer(), findExistingShopifyCustomer(), generateReferralCode(), normalisePhone(), removeTradeAccountTag() (+2 more)

### Community 19 - "Workspace Build Permissions"
Cohesion: 0.20
Nodes (9): allowBuilds, core-js, esbuild, @parcel/watcher, prisma, @prisma/client, @prisma/engines, unrs-resolver (+1 more)

### Community 20 - "Vite Development Server"
Cohesion: 0.22
Nodes (6): @react-router/dev@^7.12.0, vite-tsconfig-paths@^5.1.4, vite@^6.3.6, @react-router/dev, vite, vite-tsconfig-paths

### Community 21 - "Trade Pricing Discounts"
Cohesion: 0.50
Nodes (6): action(), createTradePriceDiscount(), deleteTradePriceDiscount(), shopifyGraphQL(), updateTradePriceDiscount(), validateDiscountPercent()

### Community 22 - "Login Flow"
Cohesion: 0.46
Nodes (5): loginErrorMessage(), action(), loader(), login, @shopify/shopify-app-react-router

### Community 23 - "GraphQL Code Generation"
Cohesion: 0.33
Nodes (3): config, @shopify/api-codegen-preset@^1.2.0, @shopify/api-codegen-preset

### Community 29 - "Cursor Shopify MCP"
Cohesion: 0.50
Nodes (3): npx, @shopify/dev-mcp, shopify-dev-mcp

### Community 30 - "Project Shopify MCP"
Cohesion: 0.50
Nodes (3): npx, @shopify/dev-mcp, shopify-dev-mcp

### Community 31 - "Application Detail Components"
Cohesion: 0.67
Nodes (3): Detail(), StatusBadge(), TradeApplicationDetail()

## Knowledge Gaps
- **15 isolated node(s):** `npx`, `@shopify/dev-mcp`, `npx`, `@shopify/dev-mcp`, `Shopify CLI` (+10 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 242 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **13 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `react-router` connect `Application Route Layouts` to `Trade Account Editor`, `Trade Application Editor`, `Trade Account Listing`, `Trade Application Listing`, `Package Metadata`, `Runtime Dependencies`, `Storefront Referrals and Orders`, `Server Rendering`, `Commission Ledger`, `Login Flow`, `Embedded App Shell`, `Admin Home`?**
  _High betweenness centrality (0.223) - this node is a cross-community bridge._
- **What connects `npx`, `@shopify/dev-mcp`, `npx` to the rest of the system?**
  _15 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Trade Account Editor` be split into smaller, more focused modules?**
  _Cohesion score 0.03636363636363636 - nodes in this community are weakly interconnected._
- **Why does `PostgreSQL Datasource` connect `PostgreSQL Trade Data Model` to `Project Setup and Sessions`?**
  _High betweenness centrality (0.158) - this node is a cross-community bridge._
- **Should `Project Setup and Sessions` be split into smaller, more focused modules?**
  _Cohesion score 0.06218487394957983 - nodes in this community are weakly interconnected._
- **Why does `Shared Prisma Client` connect `PostgreSQL Trade Data Model` to `Storefront Referrals and Orders`?**
  _High betweenness centrality (0.154) - this node is a cross-community bridge._
- **Should `Trade Application Editor` be split into smaller, more focused modules?**
  _Cohesion score 0.06060606060606061 - nodes in this community are weakly interconnected._

## Generation Notes

- Code AST extraction was local. Semantic extraction used this host session; host token usage is unmetered. Zero semantic token fields are placeholders, not measured zero cost.
- SQL parser dependency was unavailable; all six migration files were read completely and represented through semantic extraction. Prisma schema, deployment TOML and Dockerfile were also included semantically.
- No missing endpoints or self-loops. The undirected graph collapses six same-endpoint relationships; EXTRACTION.json preserves all 627 original edges and their evidence. See GRAPH_HEALTH.json.
- README SQLite references describe the upstream template; active Prisma datasource is PostgreSQL.
- Secrets and ignored generated files were excluded; .npmrc was flagged sensitive.
- graphify-out/ was written into the working tree and is not ignored; commit only the outputs the team should share, or add it to .gitignore.
