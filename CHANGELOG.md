# Changelog

All notable changes to the Skills Collection will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-01-11

### Added

#### New Skills (46 total)

**Foundation & DevEx (10 skills)**
- **skill-creator** - Generate new Claude Code skills from scratch
- **conventional-commits** - Enforce conventional commit message format
- **monorepo-setup** - Turborepo/Nx monorepo configuration
- **eslint-prettier-config** - ESLint and Prettier setup with custom rules
- **typescript-strict-migrator** - Migrate to strict TypeScript gradually
- **hot-reload-optimizer** - Optimize HMR for faster development
- **vscode-workspace-setup** - VS Code workspace configuration and extensions

**Frontend (6 skills)**
- **tailwind-gradient-builder** - Generate beautiful Tailwind CSS gradients
- **dark-mode-implementer** - Implement dark mode with CSS variables
- **framer-motion-animator** - Advanced animations with Framer Motion
- **responsive-design-system** - Mobile-first responsive design system
- **zustand-state-builder** - Zustand state management setup
- **react-server-components** - React Server Components patterns
- **tanstack-query-setup** - TanStack Query configuration and patterns

**Backend (5 skills)**
- **graphql-schema-designer** - GraphQL schema design and resolvers
- **websocket-realtime-builder** - WebSocket real-time features
- **queue-job-processor** - Background job processing with Bull/BullMQ
- **event-driven-architect** - Event-driven architecture patterns
- **rest-to-graphql-migrator** - Migrate REST APIs to GraphQL

**AI Engineering (5 skills)**
- **langchain-workflow-builder** - LangChain workflow pipelines
- **vector-db-setup** - Vector database setup (Pinecone, Weaviate, etc.)
- **structured-output-extractor** - Extract structured data from LLM outputs
- **ai-agent-orchestrator** - Multi-agent orchestration patterns
- **embedding-pipeline-builder** - Document embedding pipelines

**Infrastructure (4 skills)**
- **kubernetes-manifest-generator** - Kubernetes deployment manifests
- **terraform-module-builder** - Terraform module development
- **nginx-config-optimizer** - NGINX configuration optimization
- **redis-patterns** - Redis caching and data structure patterns

**Documentation (4 skills)**
- **mermaid-diagram-generator** - Mermaid.js diagram generation
- **readme-generator** - Comprehensive README documentation
- **storybook-setup** - Storybook component documentation
- **jsdoc-typescript-docs** - JSDoc and TypeScript documentation

**Security (4 skills)**
- **oauth2-oidc-implementer** - OAuth2/OIDC implementation
- **cors-configuration** - CORS setup and security
- **api-security-hardener** - API security best practices
- **env-secrets-manager** - Environment and secrets management

**Testing (4 skills)**
- **visual-regression-tester** - Visual regression testing setup
- **api-mock-server** - API mocking with MSW patterns
- **load-test-builder** - Load testing with k6
- **cypress-playwright-setup** - E2E testing framework setup

**Other (4 skills)**
- **accessibility-auditor** - WCAG accessibility compliance audits
- **mcp-server-builder** - Model Context Protocol server development
- **react-hook-builder** - Custom React hooks with TypeScript
- **dockerfile-optimizer** - Optimize Dockerfiles for production
- **pr-template-builder** - PR template generation
- **api-docs-generator** - OpenAPI/Swagger documentation

### Fixed

- Fixed leading space in SKILL.md filenames (renamed 32 files from ` SKILL.md` to `SKILL.md`)

### Changed

- Total skill count increased from 100 to 155 skills

---

## [1.0.0] - 2026-01-11

### Added

Initial release with **100 production-ready development skills** across 10 categories:

#### Foundation (11 skills)
- project-scaffolder
- dependency-doctor
- code-formatter-installer
- git-hygiene-enforcer
- dev-environment-bootstrapper
- dev-onboarding-builder
- docs-starter-kit
- changelog-writer
- repo-structure-linter
- codebase-summarizer
- explaining-code

#### Frontend (10 skills)
- component-scaffold-generator
- page-layout-builder
- form-wizard-builder
- table-builder
- modal-drawer-system
- animation-micro-interaction-pack
- i18n-frontend-implementer
- state-ux-flow-builder
- design-to-component-translator
- frontend-refactor-planner

#### Backend (10 skills)
- api-endpoint-generator
- auth-module-builder
- rbac-permissions-builder
- webhook-receiver-hardener
- rate-limiting-abuse-protection
- api-contract-normalizer
- caching-strategist
- error-handling-standardizer
- service-layer-extractor
- background-jobs-designer

#### AI Engineering (10 skills)
- prompt-template-builder
- rag-pipeline-builder
- doc-to-vector-dataset-generator
- guardrails-safety-filter-builder
- evaluation-harness
- agent-orchestration-planner
- cost-latency-optimizer
- tool-function-schema-designer
- llm-debugger
- prompt-regression-tester

#### Architecture (10 skills)
- adr-writer
- system-design-generator
- domain-model-boundaries-mapper
- api-versioning-deprecation-planner
- scalability-playbook
- migration-planner
- reliability-strategy-builder
- tech-debt-prioritizer
- rfc-generator
- performance-budget-setter

#### CI/CD (10 skills)
- github-actions-pipeline-creator
- caching-strategy-optimizer
- release-automation-builder
- preview-environments-builder
- secrets-env-manager
- quality-gates-enforcer
- rollback-workflow-builder
- deployment-checklist-generator
- monorepo-ci-optimizer
- artifact-sbom-publisher

#### Database Management (10 skills)
- prisma-migration-assistant
- schema-consistency-checker
- sql-query-optimizer
- data-seeding-fixtures-builder
- backup-restore-runbook-generator
- multi-tenant-safety-checker
- data-retention-archiving-planner
- data-integrity-auditor
- etl-sync-job-builder
- db-performance-watchlist

#### Testing (10 skills)
- unit-test-generator
- integration-test-builder
- e2e-test-builder
- mocking-assistant
- flaky-test-detective
- coverage-strategist
- test-data-factory-builder
- snapshot-test-refactorer
- contract-testing-builder
- test-reporting-triage-skill

#### Security (10 skills)
- secrets-scanner
- dependency-vulnerability-triage
- threat-model-generator
- secure-headers-csp-builder
- auth-security-reviewer
- input-validation-sanitization-auditor
- security-pr-checklist-skill
- pii-redaction-logging-policy-builder
- security-incident-playbook-generator
- rbac-policy-tester

#### Performance (10 skills)
- observability-setup
- structured-logging-standardizer
- alerting-dashboard-builder
- incident-runbook-generator
- core-web-vitals-tuner
- backend-latency-profiler-helper
- caching-cdn-strategy-planner
- load-test-scenario-builder
- capacity-planning-helper
- postmortem-writer

---

[1.1.0]: https://github.com/patriciomarroquin/skills/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/patriciomarroquin/skills/releases/tag/v1.0.0
