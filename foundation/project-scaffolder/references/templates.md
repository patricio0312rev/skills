# Project Templates Reference

## Next.js App Router

```
project-name/
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── api/
│   │       └── hello/
│   │           └── route.ts
│   ├── components/
│   ├── lib/
│   └── types/
├── public/
├── .env.example
├── next.config.js
├── tsconfig.json
├── package.json
└── README.md
```

## Vite + React + TypeScript

```
project-name/
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   ├── hooks/
│   ├── lib/
│   ├── types/
│   └── assets/
├── public/
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
└── README.md
```

## NestJS

```
project-name/
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   ├── app.controller.ts
│   ├── app.service.ts
│   └── modules/
├── test/
├── nest-cli.json
├── tsconfig.json
├── package.json
└── README.md
```

## FastAPI

```
project-name/
├── app/
│   ├── main.py
│   ├── api/
│   │   └── v1/
│   │       └── endpoints/
│   ├── core/
│   │   └── config.py
│   ├── models/
│   ├── schemas/
│   └── services/
├── tests/
├── requirements.txt
├── .env.example
├── pyproject.toml
└── README.md
```

## Express + TypeScript

```
project-name/
├── src/
│   ├── index.ts
│   ├── app.ts
│   ├── routes/
│   ├── controllers/
│   ├── middleware/
│   ├── services/
│   └── types/
├── dist/
├── tsconfig.json
├── package.json
└── README.md
```

## Common Package Scripts

### Node.js Projects

```json
{
  "scripts": {
    "dev": "...",
    "build": "...",
    "start": "...",
    "test": "...",
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}
```

### Python Projects

```toml
[tool.poetry.scripts]
dev = "uvicorn app.main:app --reload"
test = "pytest"
format = "black . && isort ."
lint = "flake8 . && mypy ."
```
