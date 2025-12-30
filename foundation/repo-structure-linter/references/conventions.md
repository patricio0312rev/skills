# Repository Structure Conventions

## Next.js App Router

**Required Structure:**

```
src/
├── app/          # Routes and pages
├── components/   # React components
├── lib/          # Utility functions
├── types/        # TypeScript types
└── hooks/        # Custom React hooks
```

**Naming:**

- Components: PascalCase (Button.tsx)
- Utilities: camelCase (formatDate.ts)
- Types: PascalCase (User.ts)
- Pages: lowercase (page.tsx, layout.tsx)

**Anti-patterns:**

- Business logic in app/ directory
- Helper functions outside lib/
- Mixed component and utility files

## NestJS

**Required Structure:**

```
src/
├── main.ts
├── app.module.ts
├── common/       # Shared utilities
├── config/       # Configuration
└── modules/      # Feature modules
    └── users/
        ├── users.module.ts
        ├── users.controller.ts
        ├── users.service.ts
        ├── dto/
        └── entities/
```

**Naming:**

- Controllers: kebab-case.controller.ts
- Services: kebab-case.service.ts
- Modules: kebab-case.module.ts
- DTOs: kebab-case.dto.ts

## Python Package

**Required Structure:**

```
project_name/
├── __init__.py
├── main.py
├── models/
├── services/
├── utils/
└── tests/
```

**Naming:**

- Packages: snake_case
- Modules: snake_case.py
- Classes: PascalCase
- Functions: snake_case

## React Component Library

**Required Structure:**

```
src/
├── components/
│   └── Button/
│       ├── Button.tsx
│       ├── Button.test.tsx
│       ├── Button.stories.tsx
│       └── index.ts
├── hooks/
├── utils/
└── types/
```

**Naming:**

- Components: PascalCase directory and file
- Hooks: camelCase starting with 'use'
- Test files: \*.test.tsx
- Story files: \*.stories.tsx
