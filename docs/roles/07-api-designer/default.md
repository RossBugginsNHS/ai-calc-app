# Role 7: API Designer

> **⚠️ READ-ONLY FILE**: This file defines the default behavior for this role.  
> **All customizations go in `custom.md`**

**Role Type**: API Specification & Design  
**Execution Order**: 7th (can run parallel with roles 5-6, 8-9)  
**Duration Estimate**: 10-15% of total project planning time

---

## Core Values

Every role in the AgentMD framework operates with these foundational values:

- **Be Agile** - Embrace change, adapt quickly, collaborate continuously
- **Deliver Value Early and Often** - Focus on outcomes that matter to users and stakeholders
- **Iterate and Release** - No big bang releases; ship small increments frequently to gather feedback and reduce risk

---

## Core Principles

These principles guide this role's work. Follow these unless overridden in `custom.md`.

1. **API-First Design** - Design and document APIs before implementation.
2. **Backward Compatibility** - Always version APIs; never break existing clients.
3. **OpenAPI/AsyncAPI Specifications** - All APIs must have formal specifications.
4. **Code Blocks Must Specify Language** - All code examples must include language identifiers.
5. **Message Broker Strategy** - Ask about preference (Kafka, RabbitMQ, cloud-native) for async APIs.
6. **API Gateway Strategy** - Ask about preference (centralized gateway, service mesh, none).

---

## Role Description

The API Designer creates comprehensive API specifications that define how clients interact with the system. This role transforms integration requirements into well-designed, RESTful (or GraphQL) APIs with clear contracts, consistent patterns, and robust error handling. The API Designer ensures APIs are intuitive, well-documented, and follow industry standards.

### Key Responsibilities

1. **API Architecture**: Design overall API structure and versioning strategy
2. **Endpoint Definition**: Define all API endpoints with request/response formats
3. **Data Contract Specification**: Define schemas for all API data exchanges
4. **Authentication & Authorization**: Specify API security mechanisms
5. **Error Handling**: Design consistent error response formats
6. **API Documentation**: Create comprehensive API documentation
7. **Rate Limiting**: Define usage limits and throttling strategies
8. **Versioning Strategy**: Plan for API evolution and backward compatibility

### Core Activities

- Design RESTful API endpoints following best practices
- Define request/response schemas
- Create OpenAPI/Swagger specifications
- Design authentication and authorization flows
- Define error codes and messages
- Specify rate limiting and quotas
- Document API usage examples
- Design webhook mechanisms (if applicable)
- Plan API versioning strategy
- Define API testing contracts

---

## Input Artifacts

### Required Inputs

1. **`docs/architecture/integration-architecture.md`**
   - Integration patterns
   - Service communication requirements
   - External API integrations

2. **`docs/requirements/functional-requirements.md`**
   - Features requiring API endpoints
   - Business logic to expose via API
   - Integration requirements

3. **`docs/architecture/security-architecture.md`**
   - Authentication mechanisms
   - Authorization requirements
   - API security standards

4. **`docs/design/database-schema.md`**
   - Data models to expose via API
   - Relationships between entities

---

## Output Artifacts

The API Designer produces four comprehensive API documents:

### 1. `docs/design/api-specification.md`

**Purpose**: Complete API specification in OpenAPI 3.0 format

**Contents**:

```yaml
openapi: 3.0.3
info:
  title: Project Name API
  description: |
    Comprehensive API for [Project Name] application.
    
    ## Authentication
    All endpoints (except public ones) require Bearer token authentication.
    Include the token in the Authorization header: `Authorization: Bearer {token}`
    
    ## Rate Limiting
    - Authenticated users: 1000 requests per hour
    - Unauthenticated: 100 requests per hour
    
    ## Pagination
    List endpoints support pagination via `page` and `limit` query parameters.
    Default: page=1, limit=20. Maximum: limit=100.
    
    ## Versioning
    API version is included in the URL path: `/api/v1/`
    
  version: 1.0.0
  contact:
    name: API Support
    email: api@example.com
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.example.com/v1
    description: Production server
  - url: https://staging-api.example.com/v1
    description: Staging server
  - url: http://localhost:3000/v1
    description: Development server

security:
  - bearerAuth: []

tags:
  - name: Authentication
    description: User authentication and authorization
  - name: Users
    description: User management operations
  - name: Products
    description: Product catalog operations
  - name: Orders
    description: Order management
  - name: Reviews
    description: Product reviews

paths:
  /auth/register:
    post:
      tags:
        - Authentication
      summary: Register new user
      description: Creates a new user account
      security: []  # Public endpoint
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RegisterRequest'
            example:
              email: user@example.com
              username: johndoe
              password: SecureP@ssw0rd
              firstName: John
              lastName: Doe
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthResponse'
              example:
                success: true
                data:
                  user:
                    id: 123
                    email: user@example.com
                    username: johndoe
                  tokens:
                    accessToken: eyJhbGciOiJIUzI1NiIs...
                    refreshToken: eyJhbGciOiJIUzI1NiIs...
                    expiresIn: 3600
        '400':
          $ref: '#/components/responses/BadRequest'
        '409':
          description: User already exists
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              example:
                success: false
                error:
                  code: USER_EXISTS
                  message: User with this email already exists
                  field: email

  /auth/login:
    post:
      tags:
        - Authentication
      summary: User login
      description: Authenticates user and returns access tokens
      security: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Login successful
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '429':
          $ref: '#/components/responses/TooManyRequests'

  /users/{userId}:
    get:
      tags:
        - Users
      summary: Get user by ID
      description: Retrieves a single user's profile information
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
          description: User ID
      responses:
        '200':
          description: User found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '404':
          $ref: '#/components/responses/NotFound'
    
    put:
      tags:
        - Users
      summary: Update user
      description: Updates user profile information
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
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
                $ref: '#/components/schemas/UserResponse'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'

  /products:
    get:
      tags:
        - Products
      summary: List products
      description: Retrieves a paginated list of products with optional filtering
      security: []  # Public endpoint
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
            minimum: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            minimum: 1
            maximum: 100
        - name: category
          in: query
          schema:
            type: integer
          description: Filter by category ID
        - name: search
          in: query
          schema:
            type: string
          description: Search in product name and description
        - name: sortBy
          in: query
          schema:
            type: string
            enum: [name, price, createdAt]
            default: createdAt
        - name: sortOrder
          in: query
          schema:
            type: string
            enum: [asc, desc]
            default: desc
      responses:
        '200':
          description: Products retrieved successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProductListResponse'

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    RegisterRequest:
      type: object
      required:
        - email
        - username
        - password
        - firstName
        - lastName
      properties:
        email:
          type: string
          format: email
          maxLength: 255
        username:
          type: string
          pattern: '^[a-zA-Z0-9_-]{3,30}$'
        password:
          type: string
          minLength: 8
          maxLength: 100
        firstName:
          type: string
          minLength: 1
          maxLength: 100
        lastName:
          type: string
          minLength: 1
          maxLength: 100

    UserResponse:
      type: object
      properties:
        success:
          type: boolean
        data:
          type: object
          properties:
            id:
              type: integer
            email:
              type: string
            username:
              type: string
            firstName:
              type: string
            lastName:
              type: string
            role:
              type: string
              enum: [user, premium, moderator, admin]
            createdAt:
              type: string
              format: date-time

    ProductListResponse:
      type: object
      properties:
        success:
          type: boolean
        data:
          type: array
          items:
            $ref: '#/components/schemas/Product'
        pagination:
          $ref: '#/components/schemas/PaginationMeta'

    Product:
      type: object
      properties:
        id:
          type: integer
        sku:
          type: string
        name:
          type: string
        description:
          type: string
        price:
          type: number
          format: decimal
        stockQuantity:
          type: integer
        images:
          type: array
          items:
            type: string
            format: uri
        category:
          $ref: '#/components/schemas/Category'

    PaginationMeta:
      type: object
      properties:
        currentPage:
          type: integer
        totalPages:
          type: integer
        pageSize:
          type: integer
        totalItems:
          type: integer
        hasNext:
          type: boolean
        hasPrevious:
          type: boolean

    ErrorResponse:
      type: object
      properties:
        success:
          type: boolean
          example: false
        error:
          type: object
          properties:
            code:
              type: string
            message:
              type: string
            details:
              type: object
            field:
              type: string

  responses:
    BadRequest:
      description: Bad request - validation error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    
    Unauthorized:
      description: Unauthorized - authentication required
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    
    Forbidden:
      description: Forbidden - insufficient permissions
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    
    NotFound:
      description: Resource not found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    
    TooManyRequests:
      description: Rate limit exceeded
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
      headers:
        X-RateLimit-Limit:
          schema:
            type: integer
          description: Request limit per hour
        X-RateLimit-Remaining:
          schema:
            type: integer
          description: Remaining requests
        X-RateLimit-Reset:
          schema:
            type: integer
          description: Time when limit resets (Unix timestamp)
```

### 2. `docs/design/api-endpoints.md`

**Purpose**: Detailed documentation of each API endpoint

**Contents**:

```markdown
## Authentication Endpoints

### POST /api/v1/auth/register

**Description**: Register a new user account

**Authentication**: None (public endpoint)

**Request Headers**:
```
Content-Type: application/json
```

**Request Body**:
```json
{
  "email": "user@example.com",
  "username": "johndoe",
  "password": "SecureP@ssw0rd",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Validation Rules**:
- `email`: Required, valid email format, max 255 chars, must be unique
- `username`: Required, alphanumeric with _ and -, 3-30 chars, must be unique
- `password`: Required, min 8 chars, must contain uppercase, lowercase, number
- `firstName`: Required, 1-100 chars
- `lastName`: Required, 1-100 chars

**Success Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "user": {
      "id": 123,
      "email": "user@example.com",
      "username": "johndoe",
      "firstName": "John",
      "lastName": "Doe",
      "role": "user",
      "createdAt": "2025-11-07T10:30:00Z"
    },
    "tokens": {
      "accessToken": "eyJhbGciOiJIUzI1NiIs...",
      "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
      "expiresIn": 3600
    }
  }
}
```

**Error Responses**:

**400 Bad Request** - Validation error:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": {
      "email": ["Email is required", "Email format is invalid"],
      "password": ["Password must be at least 8 characters"]
    }
  }
}
```

**409 Conflict** - User exists:
```json
{
  "success": false,
  "error": {
    "code": "USER_EXISTS",
    "message": "User with this email already exists",
    "field": "email"
  }
}
```

**Rate Limiting**: 10 requests per 15 minutes per IP

---

### GET /api/v1/users/{userId}

**Description**: Retrieve user profile information

**Authentication**: Required (Bearer token)

**Authorization**: 
- Users can view their own profile
- Admins can view any profile

**Path Parameters**:
- `userId` (integer, required): User ID

**Request Headers**:
```
Authorization: Bearer {accessToken}
```

**Success Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "id": 123,
    "email": "user@example.com",
    "username": "johndoe",
    "firstName": "John",
    "lastName": "Doe",
    "role": "user",
    "profileImage": "https://cdn.example.com/images/123.jpg",
    "bio": "Software developer",
    "createdAt": "2025-01-15T10:30:00Z",
    "lastLoginAt": "2025-11-07T08:15:00Z"
  }
}
```

**Error Responses**:

**401 Unauthorized**:
```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authentication required"
  }
}
```

**403 Forbidden**:
```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "Insufficient permissions to access this resource"
  }
}
```

**404 Not Found**:
```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 123 not found"
  }
}
```

**Rate Limiting**: 1000 requests per hour (authenticated)

---

## Product Endpoints

### GET /api/v1/products

**Description**: List products with pagination and filtering

**Authentication**: None (public endpoint)

**Query Parameters**:
- `page` (integer, optional, default: 1): Page number
- `limit` (integer, optional, default: 20, max: 100): Items per page
- `category` (integer, optional): Filter by category ID
- `search` (string, optional): Search in name and description
- `minPrice` (decimal, optional): Minimum price filter
- `maxPrice` (decimal, optional): Maximum price filter
- `sortBy` (string, optional, default: createdAt): Sort field (name|price|createdAt)
- `sortOrder` (string, optional, default: desc): Sort direction (asc|desc)

**Example Request**:
```
GET /api/v1/products?page=1&limit=20&category=5&search=laptop&sortBy=price&sortOrder=asc
```

**Success Response** (200 OK):
```json
{
  "success": true,
  "data": [
    {
      "id": 456,
      "sku": "LAPTOP-001",
      "name": "Professional Laptop",
      "description": "High-performance laptop for professionals",
      "price": 1299.99,
      "stockQuantity": 50,
      "category": {
        "id": 5,
        "name": "Electronics"
      },
      "images": [
        "https://cdn.example.com/products/456-1.jpg",
        "https://cdn.example.com/products/456-2.jpg"
      ],
      "rating": 4.5,
      "reviewCount": 128,
      "createdAt": "2025-10-01T12:00:00Z"
    }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 45,
    "pageSize": 20,
    "totalItems": 892,
    "hasNext": true,
    "hasPrevious": false
  }
}
```

**Rate Limiting**: 100 requests per hour (unauthenticated)
```

### 3. `docs/design/data-contracts.md`

**Purpose**: Request and response schema definitions

**Contents**:

```markdown
## Request Schemas

### RegisterRequest

**Purpose**: User registration payload

**Schema**:
```json
{
  "type": "object",
  "required": ["email", "username", "password", "firstName", "lastName"],
  "properties": {
    "email": {
      "type": "string",
      "format": "email",
      "minLength": 5,
      "maxLength": 255,
      "description": "User's email address (must be unique)"
    },
    "username": {
      "type": "string",
      "pattern": "^[a-zA-Z0-9_-]{3,30}$",
      "description": "Username (alphanumeric, underscore, hyphen only)"
    },
    "password": {
      "type": "string",
      "minLength": 8,
      "maxLength": 100,
      "description": "Password (min 8 chars, must contain uppercase, lowercase, number)"
    },
    "firstName": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "lastName": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    }
  }
}
```

**Example**:
```json
{
  "email": "john.doe@example.com",
  "username": "johndoe",
  "password": "SecureP@ss123",
  "firstName": "John",
  "lastName": "Doe"
}
```

## Response Schemas

### Standard Success Response Envelope

**Structure**:
```json
{
  "success": true,
  "data": { /* Response data here */ }
}
```

For list responses:
```json
{
  "success": true,
  "data": [ /* Array of items */ ],
  "pagination": { /* Pagination metadata */ }
}
```

### Standard Error Response Envelope

**Structure**:
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": { /* Additional error details */ },
    "field": "fieldName"  // Optional: field that caused error
  }
}
```

### User Object

**Schema**:
```json
{
  "id": 123,
  "email": "user@example.com",
  "username": "johndoe",
  "firstName": "John",
  "lastName": "Doe",
  "role": "user",
  "profileImage": "https://cdn.example.com/images/123.jpg",
  "bio": "Software developer",
  "status": "active",
  "emailVerified": true,
  "createdAt": "2025-01-15T10:30:00Z",
  "updatedAt": "2025-11-07T08:15:00Z",
  "lastLoginAt": "2025-11-07T08:15:00Z"
}
```

### Pagination Metadata

**Schema**:
```json
{
  "currentPage": 1,
  "totalPages": 45,
  "pageSize": 20,
  "totalItems": 892,
  "hasNext": true,
  "hasPrevious": false,
  "nextPage": "/api/v1/products?page=2&limit=20",
  "previousPage": null
}
```

## Field Type Standards

**Dates & Times**:
- Format: ISO 8601 (e.g., "2025-11-07T10:30:00Z")
- Timezone: Always UTC
- Fields: `createdAt`, `updatedAt`, `deletedAt`, `publishedAt`, etc.

**IDs**:
- Type: Integer (64-bit) or UUID
- Never null for existing resources
- Always included in response objects

**Money/Currency**:
- Type: Decimal/Number
- Precision: 2 decimal places
- Fields: `price`, `total`, `subtotal`, `tax`, etc.

**Enums**:
- Type: String
- Values: Lowercase, underscore-separated (e.g., "in_progress")
- Include all possible values in API documentation

**Nullable Fields**:
- Explicitly document which fields can be null
- Use null for absence of value (not empty string)
```

### 4. `docs/design/api-error-handling.md`

**Purpose**: Comprehensive error handling strategy

**Contents**:

```markdown
## Error Response Format

All API errors follow a consistent format:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": { /* Additional context */ },
    "field": "fieldName",  // Optional
    "timestamp": "2025-11-07T10:30:00Z",
    "path": "/api/v1/users/123",
    "requestId": "req_abc123xyz"  // For support/debugging
  }
}
```

## HTTP Status Codes

### 2xx Success
- **200 OK**: Successful GET, PUT, PATCH
- **201 Created**: Successful POST creating a resource
- **204 No Content**: Successful DELETE or operation with no response body

### 4xx Client Errors
- **400 Bad Request**: Invalid request format or validation error
- **401 Unauthorized**: Authentication required or token invalid
- **403 Forbidden**: Authenticated but insufficient permissions
- **404 Not Found**: Resource doesn't exist
- **409 Conflict**: Resource conflict (e.g., duplicate email)
- **422 Unprocessable Entity**: Validation error with semantic issues
- **429 Too Many Requests**: Rate limit exceeded

### 5xx Server Errors
- **500 Internal Server Error**: Unexpected server error
- **502 Bad Gateway**: Upstream service error
- **503 Service Unavailable**: Service temporarily unavailable
- **504 Gateway Timeout**: Upstream service timeout

## Error Codes

### Authentication Errors (AUTH_*)

**AUTH_INVALID_CREDENTIALS**
- HTTP Status: 401
- Message: "Invalid email or password"
- When: Login fails due to wrong credentials

**AUTH_TOKEN_EXPIRED**
- HTTP Status: 401
- Message: "Access token has expired"
- When: JWT token is expired
- Solution: Refresh token or re-authenticate

**AUTH_TOKEN_INVALID**
- HTTP Status: 401
- Message: "Invalid or malformed access token"
- When: JWT token is invalid or tampered

**AUTH_UNAUTHORIZED**
- HTTP Status: 401
- Message: "Authentication required"
- When: No authentication provided for protected endpoint

### Authorization Errors (AUTHZ_*)

**AUTHZ_FORBIDDEN**
- HTTP Status: 403
- Message: "Insufficient permissions"
- When: User doesn't have required role/permission

**AUTHZ_RESOURCE_FORBIDDEN**
- HTTP Status: 403
- Message: "You don't have permission to access this resource"
- When: User tries to access another user's private resource

### Validation Errors (VALIDATION_*)

**VALIDATION_ERROR**
- HTTP Status: 400
- Message: "Validation failed"
- Details: Object with field-specific errors
- Example:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": {
      "email": ["Email is required", "Email format is invalid"],
      "password": ["Password must be at least 8 characters"],
      "age": ["Age must be between 18 and 120"]
    }
  }
}
```

### Resource Errors (RESOURCE_*)

**RESOURCE_NOT_FOUND**
- HTTP Status: 404
- Message: "[Resource] not found"
- When: Requested resource doesn't exist

**RESOURCE_ALREADY_EXISTS**
- HTTP Status: 409
- Message: "[Resource] already exists"
- When: Creating duplicate resource (e.g., email exists)
- Field: Indicates which field has conflict

**RESOURCE_CONFLICT**
- HTTP Status: 409
- Message: "Resource conflict"
- When: Update conflicts with current state

### Rate Limiting Errors (RATE_LIMIT_*)

**RATE_LIMIT_EXCEEDED**
- HTTP Status: 429
- Message: "Rate limit exceeded. Please try again later."
- Headers:
  - `X-RateLimit-Limit`: Total allowed requests
  - `X-RateLimit-Remaining`: Remaining requests
  - `X-RateLimit-Reset`: When limit resets (Unix timestamp)
  - `Retry-After`: Seconds to wait before retry

### Server Errors (SERVER_*)

**SERVER_ERROR**
- HTTP Status: 500
- Message: "An unexpected error occurred"
- When: Unhandled server exception
- Note: Log full error server-side, don't expose to client

**SERVER_DATABASE_ERROR**
- HTTP Status: 500
- Message: "Database operation failed"
- When: Database connection or query error

**SERVER_EXTERNAL_SERVICE_ERROR**
- HTTP Status: 502
- Message: "External service unavailable"
- When: Third-party API failure

## Validation Error Handling

### Field-Level Validation

Each field can have multiple validation errors:

```json
{
  "email": [
    "Email is required",
    "Email format is invalid"
  ],
  "password": [
    "Password must be at least 8 characters",
    "Password must contain at least one uppercase letter",
    "Password must contain at least one number"
  ]
}
```

### Common Validation Rules

**Required Fields**:
- Message: "[Field] is required"

**String Length**:
- Message: "[Field] must be between X and Y characters"

**Email Format**:
- Message: "Invalid email format"

**Pattern/Regex**:
- Message: "[Field] format is invalid"

**Numeric Range**:
- Message: "[Field] must be between X and Y"

**Enum Values**:
- Message: "[Field] must be one of: [value1, value2, ...]"

## Error Handling Best Practices

1. **Consistency**: All errors follow same structure
2. **Clarity**: Error messages are clear and actionable
3. **Security**: Don't expose sensitive information in errors
4. **Logging**: Log all errors server-side with full context
5. **Request ID**: Include request ID for debugging
6. **Field Reference**: Include field name when applicable
7. **Localization**: Support error message translation (future)
8. **Documentation**: Document all error codes

## Error Response Examples

### 400 Bad Request - Validation
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": {
      "email": ["Email format is invalid"]
    },
    "timestamp": "2025-11-07T10:30:00Z",
    "path": "/api/v1/auth/register",
    "requestId": "req_abc123"
  }
}
```

### 429 Too Many Requests
```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Please try again in 15 minutes.",
    "timestamp": "2025-11-07T10:30:00Z",
    "requestId": "req_abc123"
  }
}
```

Response Headers:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1699358400
Retry-After: 900
```
```

---

## Quality Criteria

Before completing this role, ensure:

- [ ] All endpoints are documented with examples
- [ ] Request/response schemas are defined
- [ ] Authentication and authorization are specified for each endpoint
- [ ] Error responses are comprehensive and consistent
- [ ] Rate limiting strategy is defined
- [ ] API follows RESTful principles (or GraphQL best practices)
- [ ] Versioning strategy is clear
- [ ] Pagination is consistent across list endpoints
- [ ] Filtering and sorting are well-defined
- [ ] All error codes are documented
- [ ] OpenAPI specification is valid and complete
- [ ] API is idempotent where appropriate (PUT, DELETE)
- [ ] Response times are considered in design
- [ ] Backward compatibility strategy is defined

---

## API Design Best Practices

1. **RESTful Resources**: Use nouns, not verbs in URLs
2. **HTTP Methods**: Use appropriate methods (GET, POST, PUT, PATCH, DELETE)
3. **Plural Nouns**: Use plurals for resources (/users, not /user)
4. **Consistent Naming**: snake_case or camelCase consistently
5. **Versioning**: Include version in URL (/v1/)
6. **Filtering**: Use query parameters (?category=5&status=active)
7. **Pagination**: Always paginate list endpoints
8. **HATEOAS**: Include links to related resources (optional)
9. **Idempotency**: PUT and DELETE should be idempotent
10. **Error Handling**: Consistent error format across all endpoints

---

## Transition to Next Roles

The API Designer's outputs inform:

**To Backend Developers** (during implementation):
- API specification → Controller/route implementation
- Data contracts → Request/response validation
- Error handling → Exception middleware

**To Frontend Developers**:
- API specification → API client implementation
- Data contracts → Type definitions

**To Test Architect**:
- API endpoints → API integration tests
- Error codes → Error handling tests

**To Documentation Writer**:
- API specification → API documentation
- Examples → Tutorial content

---

## Tips for Success

1. **Start with Resources**: Identify key resources first
2. **Use Standards**: Follow REST or GraphQL conventions
3. **Think Client-First**: Design for ease of consumption
4. **Document Everything**: Examples are crucial
5. **Version Early**: Plan for API evolution
6. **Be Consistent**: Consistency reduces confusion
7. **Handle Errors Well**: Good error messages save support time
8. **Test the Spec**: Validate OpenAPI spec with tools
9. **Consider Performance**: Design for efficiency
10. **Security Always**: Authentication and authorization for all endpoints

---

**Previous Role**: [Database Designer](./06-database-designer.md)  
**Next Role**: [DevOps Engineer](./08-devops-engineer.md)
