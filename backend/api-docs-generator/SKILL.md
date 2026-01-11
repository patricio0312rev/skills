---
name: api-docs-generator
description: Generates API documentation using OpenAPI/Swagger specifications with interactive documentation, code examples, and SDK generation. Use when users request "API documentation", "OpenAPI spec", "Swagger docs", "document API endpoints", or "generate API reference".
---

# API Docs Generator

Create comprehensive API documentation with OpenAPI specifications and interactive documentation.

## Core Workflow

1. **Analyze API endpoints**: Review routes, methods, parameters
2. **Define OpenAPI spec**: Create specification in YAML/JSON
3. **Add schemas**: Define request/response models
4. **Include examples**: Add realistic example values
5. **Generate documentation**: Deploy interactive docs
6. **Create SDK**: Optional client library generation

## OpenAPI Specification Structure

```yaml
# openapi.yaml
openapi: 3.1.0

info:
  title: My API
  version: 1.0.0
  description: |
    API description with **Markdown** support.

    ## Authentication
    All endpoints require Bearer token authentication.
  contact:
    name: API Support
    email: api@example.com
    url: https://docs.example.com
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: https://staging-api.example.com/v1
    description: Staging
  - url: http://localhost:3000/v1
    description: Development

tags:
  - name: Users
    description: User management endpoints
  - name: Products
    description: Product catalog endpoints
  - name: Orders
    description: Order processing endpoints

paths:
  # Endpoints defined here

components:
  # Reusable schemas, security, etc.
```

## Path Definitions

### Basic CRUD Endpoints

```yaml
paths:
  /users:
    get:
      tags:
        - Users
      summary: List all users
      description: Retrieve a paginated list of users
      operationId: listUsers
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/LimitParam'
        - name: role
          in: query
          description: Filter by user role
          schema:
            type: string
            enum: [admin, user, guest]
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'
              example:
                data:
                  - id: "usr_123"
                    email: "john@example.com"
                    name: "John Doe"
                    role: "admin"
                    createdAt: "2024-01-15T10:30:00Z"
                pagination:
                  page: 1
                  limit: 20
                  total: 150
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/InternalError'

    post:
      tags:
        - Users
      summary: Create a new user
      description: Create a new user account
      operationId: createUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
            example:
              email: "newuser@example.com"
              name: "New User"
              password: "securePassword123"
              role: "user"
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          $ref: '#/components/responses/BadRequest'
        '409':
          description: User already exists
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
              example:
                code: "USER_EXISTS"
                message: "A user with this email already exists"
        '422':
          $ref: '#/components/responses/ValidationError'

  /users/{userId}:
    parameters:
      - $ref: '#/components/parameters/UserId'

    get:
      tags:
        - Users
      summary: Get user by ID
      description: Retrieve a specific user by their ID
      operationId: getUserById
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          $ref: '#/components/responses/NotFound'

    patch:
      tags:
        - Users
      summary: Update user
      description: Update an existing user's information
      operationId: updateUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateUserRequest'
      responses:
        '200':
          description: User updated successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          $ref: '#/components/responses/NotFound'
        '422':
          $ref: '#/components/responses/ValidationError'

    delete:
      tags:
        - Users
      summary: Delete user
      description: Permanently delete a user
      operationId: deleteUser
      responses:
        '204':
          description: User deleted successfully
        '404':
          $ref: '#/components/responses/NotFound'
```

## Component Schemas

### Data Models

```yaml
components:
  schemas:
    # Base User Schema
    User:
      type: object
      properties:
        id:
          type: string
          format: uuid
          description: Unique user identifier
          example: "usr_123abc"
          readOnly: true
        email:
          type: string
          format: email
          description: User's email address
          example: "john@example.com"
        name:
          type: string
          minLength: 1
          maxLength: 100
          description: User's full name
          example: "John Doe"
        role:
          $ref: '#/components/schemas/UserRole'
        avatar:
          type: string
          format: uri
          nullable: true
          description: URL to user's avatar image
          example: "https://cdn.example.com/avatars/123.jpg"
        createdAt:
          type: string
          format: date-time
          description: Account creation timestamp
          readOnly: true
        updatedAt:
          type: string
          format: date-time
          description: Last update timestamp
          readOnly: true
      required:
        - id
        - email
        - name
        - role
        - createdAt

    UserRole:
      type: string
      enum:
        - admin
        - user
        - guest
      description: User's role in the system
      example: "user"

    # Request Schemas
    CreateUserRequest:
      type: object
      properties:
        email:
          type: string
          format: email
        name:
          type: string
          minLength: 1
          maxLength: 100
        password:
          type: string
          format: password
          minLength: 8
          description: Must contain at least one uppercase, one lowercase, and one number
        role:
          $ref: '#/components/schemas/UserRole'
      required:
        - email
        - name
        - password

    UpdateUserRequest:
      type: object
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 100
        role:
          $ref: '#/components/schemas/UserRole'
        avatar:
          type: string
          format: uri
          nullable: true
      minProperties: 1

    # List Response
    UserList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/User'
        pagination:
          $ref: '#/components/schemas/Pagination'

    Pagination:
      type: object
      properties:
        page:
          type: integer
          minimum: 1
          example: 1
        limit:
          type: integer
          minimum: 1
          maximum: 100
          example: 20
        total:
          type: integer
          minimum: 0
          example: 150
        hasMore:
          type: boolean
          example: true

    # Error Schemas
    Error:
      type: object
      properties:
        code:
          type: string
          description: Machine-readable error code
          example: "VALIDATION_ERROR"
        message:
          type: string
          description: Human-readable error message
          example: "The request body is invalid"
        details:
          type: array
          items:
            $ref: '#/components/schemas/ErrorDetail'
      required:
        - code
        - message

    ErrorDetail:
      type: object
      properties:
        field:
          type: string
          description: The field that caused the error
          example: "email"
        message:
          type: string
          description: Description of the validation error
          example: "Must be a valid email address"
```

## Parameters and Responses

```yaml
components:
  parameters:
    UserId:
      name: userId
      in: path
      required: true
      description: Unique user identifier
      schema:
        type: string
        format: uuid
      example: "usr_123abc"

    PageParam:
      name: page
      in: query
      description: Page number for pagination
      schema:
        type: integer
        minimum: 1
        default: 1
      example: 1

    LimitParam:
      name: limit
      in: query
      description: Number of items per page
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20
      example: 20

    SortParam:
      name: sort
      in: query
      description: Sort field and direction
      schema:
        type: string
        pattern: '^[a-zA-Z]+:(asc|desc)$'
      example: "createdAt:desc"

  responses:
    BadRequest:
      description: Bad request - invalid input
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "BAD_REQUEST"
            message: "Invalid request format"

    Unauthorized:
      description: Authentication required
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "UNAUTHORIZED"
            message: "Authentication token is missing or invalid"

    Forbidden:
      description: Permission denied
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "FORBIDDEN"
            message: "You don't have permission to access this resource"

    NotFound:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "NOT_FOUND"
            message: "The requested resource was not found"

    ValidationError:
      description: Validation error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "VALIDATION_ERROR"
            message: "Request validation failed"
            details:
              - field: "email"
                message: "Must be a valid email address"
              - field: "password"
                message: "Must be at least 8 characters"

    InternalError:
      description: Internal server error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: "INTERNAL_ERROR"
            message: "An unexpected error occurred"
```

## Security Definitions

```yaml
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: |
        JWT token obtained from the /auth/login endpoint.

        Example: `Authorization: Bearer eyJhbGciOiJIUzI1...`

    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key
      description: API key for server-to-server communication

    OAuth2:
      type: oauth2
      description: OAuth 2.0 authentication
      flows:
        authorizationCode:
          authorizationUrl: https://auth.example.com/oauth/authorize
          tokenUrl: https://auth.example.com/oauth/token
          scopes:
            read:users: Read user information
            write:users: Create and modify users
            admin: Full administrative access

# Apply security globally
security:
  - BearerAuth: []

# Or per-endpoint
paths:
  /public/health:
    get:
      security: []  # No auth required
      summary: Health check
      responses:
        '200':
          description: Service is healthy
```

## Express/Node.js Integration

### Generate from Code with express-openapi

```typescript
// src/docs/openapi.ts
import { OpenAPIV3_1 } from 'openapi-types';

export const openApiDocument: OpenAPIV3_1.Document = {
  openapi: '3.1.0',
  info: {
    title: 'My API',
    version: '1.0.0',
    description: 'API documentation',
  },
  servers: [
    { url: 'http://localhost:3000', description: 'Development' },
  ],
  paths: {},
  components: {
    schemas: {},
    securitySchemes: {
      BearerAuth: {
        type: 'http',
        scheme: 'bearer',
        bearerFormat: 'JWT',
      },
    },
  },
};
```

### Swagger UI Express

```typescript
// src/docs/swagger.ts
import swaggerUi from 'swagger-ui-express';
import YAML from 'yamljs';
import path from 'path';
import { Express } from 'express';

export function setupSwagger(app: Express) {
  const swaggerDocument = YAML.load(
    path.join(__dirname, '../../openapi.yaml')
  );

  const options: swaggerUi.SwaggerUiOptions = {
    explorer: true,
    customSiteTitle: 'API Documentation',
    customCss: '.swagger-ui .topbar { display: none }',
    swaggerOptions: {
      persistAuthorization: true,
      displayRequestDuration: true,
      filter: true,
      showExtensions: true,
    },
  };

  app.use('/docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument, options));
  app.get('/openapi.json', (req, res) => res.json(swaggerDocument));
}
```

### Zod to OpenAPI

```typescript
// src/schemas/user.ts
import { z } from 'zod';
import { extendZodWithOpenApi } from '@asteasolutions/zod-to-openapi';

extendZodWithOpenApi(z);

export const UserSchema = z.object({
  id: z.string().uuid().openapi({ example: 'usr_123abc' }),
  email: z.string().email().openapi({ example: 'john@example.com' }),
  name: z.string().min(1).max(100).openapi({ example: 'John Doe' }),
  role: z.enum(['admin', 'user', 'guest']).openapi({ example: 'user' }),
  createdAt: z.string().datetime(),
}).openapi('User');

export const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  password: z.string().min(8),
  role: z.enum(['admin', 'user', 'guest']).optional().default('user'),
}).openapi('CreateUserRequest');
```

```typescript
// src/docs/generator.ts
import {
  OpenAPIRegistry,
  OpenApiGeneratorV31,
} from '@asteasolutions/zod-to-openapi';
import { UserSchema, CreateUserSchema } from '../schemas/user';

const registry = new OpenAPIRegistry();

// Register schemas
registry.register('User', UserSchema);
registry.register('CreateUserRequest', CreateUserSchema);

// Register endpoints
registry.registerPath({
  method: 'get',
  path: '/users',
  tags: ['Users'],
  summary: 'List all users',
  responses: {
    200: {
      description: 'List of users',
      content: {
        'application/json': {
          schema: z.array(UserSchema),
        },
      },
    },
  },
});

registry.registerPath({
  method: 'post',
  path: '/users',
  tags: ['Users'],
  summary: 'Create a user',
  request: {
    body: {
      content: {
        'application/json': {
          schema: CreateUserSchema,
        },
      },
    },
  },
  responses: {
    201: {
      description: 'User created',
      content: {
        'application/json': {
          schema: UserSchema,
        },
      },
    },
  },
});

// Generate OpenAPI document
const generator = new OpenApiGeneratorV31(registry.definitions);
export const openApiDocument = generator.generateDocument({
  openapi: '3.1.0',
  info: {
    title: 'My API',
    version: '1.0.0',
  },
});
```

## FastAPI Integration

```python
# main.py
from fastapi import FastAPI, HTTPException, Query
from fastapi.openapi.utils import get_openapi
from pydantic import BaseModel, EmailStr, Field
from typing import Optional
from datetime import datetime
from enum import Enum

app = FastAPI(
    title="My API",
    description="API documentation with FastAPI",
    version="1.0.0",
    docs_url="/docs",
    redoc_url="/redoc",
)


class UserRole(str, Enum):
    admin = "admin"
    user = "user"
    guest = "guest"


class UserBase(BaseModel):
    email: EmailStr = Field(..., example="john@example.com")
    name: str = Field(..., min_length=1, max_length=100, example="John Doe")
    role: UserRole = Field(default=UserRole.user, example="user")


class UserCreate(UserBase):
    password: str = Field(..., min_length=8, example="securePassword123")


class User(UserBase):
    id: str = Field(..., example="usr_123abc")
    created_at: datetime
    updated_at: Optional[datetime] = None

    class Config:
        from_attributes = True


class UserList(BaseModel):
    data: list[User]
    total: int
    page: int
    limit: int


@app.get(
    "/users",
    response_model=UserList,
    tags=["Users"],
    summary="List all users",
    description="Retrieve a paginated list of users",
)
async def list_users(
    page: int = Query(1, ge=1, description="Page number"),
    limit: int = Query(20, ge=1, le=100, description="Items per page"),
    role: Optional[UserRole] = Query(None, description="Filter by role"),
):
    # Implementation
    pass


@app.post(
    "/users",
    response_model=User,
    status_code=201,
    tags=["Users"],
    summary="Create a new user",
    responses={
        409: {"description": "User already exists"},
        422: {"description": "Validation error"},
    },
)
async def create_user(user: UserCreate):
    # Implementation
    pass


# Custom OpenAPI schema
def custom_openapi():
    if app.openapi_schema:
        return app.openapi_schema

    openapi_schema = get_openapi(
        title="My API",
        version="1.0.0",
        description="API documentation",
        routes=app.routes,
    )

    # Add security scheme
    openapi_schema["components"]["securitySchemes"] = {
        "BearerAuth": {
            "type": "http",
            "scheme": "bearer",
            "bearerFormat": "JWT",
        }
    }
    openapi_schema["security"] = [{"BearerAuth": []}]

    app.openapi_schema = openapi_schema
    return app.openapi_schema


app.openapi = custom_openapi
```

## Documentation Generators

### Redoc

```html
<!-- docs/index.html -->
<!DOCTYPE html>
<html>
  <head>
    <title>API Documentation</title>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://fonts.googleapis.com/css?family=Montserrat:300,400,700|Roboto:300,400,700" rel="stylesheet">
    <style>
      body { margin: 0; padding: 0; }
    </style>
  </head>
  <body>
    <redoc spec-url='/openapi.yaml' expand-responses="200,201"></redoc>
    <script src="https://cdn.redoc.ly/redoc/latest/bundles/redoc.standalone.js"></script>
  </body>
</html>
```

### Stoplight Elements

```html
<!DOCTYPE html>
<html>
  <head>
    <title>API Documentation</title>
    <script src="https://unpkg.com/@stoplight/elements/web-components.min.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/@stoplight/elements/styles.min.css">
  </head>
  <body>
    <elements-api
      apiDescriptionUrl="/openapi.yaml"
      router="hash"
      layout="sidebar"
    />
  </body>
</html>
```

## SDK Generation

### OpenAPI Generator

```bash
# Install OpenAPI Generator
npm install -g @openapitools/openapi-generator-cli

# Generate TypeScript client
openapi-generator-cli generate \
  -i openapi.yaml \
  -g typescript-fetch \
  -o ./sdk/typescript \
  --additional-properties=supportsES6=true,npmName=@myorg/api-client

# Generate Python client
openapi-generator-cli generate \
  -i openapi.yaml \
  -g python \
  -o ./sdk/python \
  --additional-properties=packageName=myapi_client
```

### Configuration

```yaml
# openapitools.json
{
  "$schema": "https://raw.githubusercontent.com/OpenAPITools/openapi-generator/master/modules/openapi-generator-gradle-plugin/src/main/resources/openapitools.json",
  "spaces": 2,
  "generator-cli": {
    "version": "7.0.0",
    "generators": {
      "typescript-client": {
        "generatorName": "typescript-fetch",
        "inputSpec": "./openapi.yaml",
        "output": "./sdk/typescript",
        "additionalProperties": {
          "supportsES6": true,
          "npmName": "@myorg/api-client",
          "npmVersion": "1.0.0"
        }
      }
    }
  }
}
```

## Validation

### Spectral Linting

```yaml
# .spectral.yaml
extends: ["spectral:oas", "spectral:asyncapi"]

rules:
  operation-operationId: error
  operation-description: warn
  operation-tags: error
  info-contact: warn
  info-license: warn
  oas3-schema: error
  oas3-valid-media-example: warn

  # Custom rules
  path-must-have-tag:
    given: "$.paths[*][*]"
    severity: error
    then:
      field: tags
      function: truthy
```

```bash
# Run linting
npx @stoplight/spectral-cli lint openapi.yaml
```

## Best Practices

1. **Use $ref for reusability**: Define schemas once, reference everywhere
2. **Include examples**: Add realistic examples for all schemas
3. **Document errors**: Describe all possible error responses
4. **Version your API**: Use URL or header versioning
5. **Group with tags**: Organize endpoints logically
6. **Add descriptions**: Explain every parameter and field
7. **Use security schemes**: Document authentication clearly
8. **Validate spec**: Use Spectral or similar tools
9. **Generate SDKs**: Automate client library creation
10. **Keep spec in sync**: Generate from code or validate against it

## Output Checklist

Every API documentation should include:

- [ ] Complete OpenAPI 3.x specification
- [ ] All endpoints documented with examples
- [ ] Request/response schemas with types
- [ ] Error responses documented
- [ ] Authentication scheme defined
- [ ] Parameters described with examples
- [ ] Interactive documentation deployed (Swagger UI/Redoc)
- [ ] Specification validated with linter
- [ ] SDK generation configured
- [ ] Versioning strategy documented
