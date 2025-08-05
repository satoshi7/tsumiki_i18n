# kairo-design

## Purpose

Generate technical design documents based on approved requirement definition documents. Perform comprehensive design including data flow diagrams, TypeScript interfaces, database schemas, and API endpoints.

## Prerequisites

- Requirement definition documents exist in `docs/spec/`
- Requirements have been approved by users

## Execution Details

**【Reliability Level Instructions】**:
For each item, comment on the verification status with source materials (EARS requirement definition documents and design documents) using the following signals:

- 🟢 **Green Signal**: When referencing EARS requirement definition documents and design documents with almost no speculation
- 🟡 **Yellow Signal**: When making reasonable speculation from EARS requirement definition documents and design documents
- 🔴 **Red Signal**: When speculation is not found in EARS requirement definition documents and design documents

1. **Requirements Analysis**
   - Read requirement definition documents
   - Organize functional and non-functional requirements
   - Clarify system boundaries

2. **Architecture Design**
   - Determine overall system architecture
   - Frontend/backend separation
   - Consider necessity of microservices

3. **Data Flow Diagram Creation**
   - Visualize data flow using Mermaid notation
   - User interaction flow
   - Data flow between systems

4. **TypeScript Interface Definition**
   - Entity type definitions
   - API request/response type definitions
   - Common type definitions

5. **Database Schema Design**
   - Table definitions
   - Relationships
   - Index strategy
   - Normalization level determination

6. **API Endpoint Design**
   - RESTful API design
   - Endpoint naming conventions
   - Appropriate use of HTTP methods
   - Request/response structure

7. **File Creation**
   - Create in `docs/design/{requirement-name}/` directory:
     - `architecture.md` - Architecture overview
     - `dataflow.md` - Data flow diagrams
     - `interfaces.ts` - TypeScript type definitions
     - `database-schema.sql` - DB schema
     - `api-endpoints.md` - API specifications

## Output Format Examples

### architecture.md

```markdown
# {Requirement Name} Architecture Design

## System Overview

{System overview description}

## Architecture Pattern

- Pattern: {Selected pattern}
- Reason: {Selection reason}

## Component Configuration

### Frontend

- Framework: {Used framework}
- State Management: {State management method}

### Backend

- Framework: {Used framework}
- Authentication Method: {Authentication method}

### Database

- DBMS: {Used DBMS}
- Cache: {Cache strategy}
```

### dataflow.md

```markdown
# Data Flow Diagrams

## User Interaction Flow

\`\`\`mermaid
flowchart TD
A[User] --> B[Frontend]
B --> C[API Gateway]
C --> D[Backend]
D --> E[Database]
\`\`\`

## Data Processing Flow

\`\`\`mermaid
sequenceDiagram
participant U as User
participant F as Frontend
participant B as Backend
participant D as Database

    U->>F: Action
    F->>B: API Request
    B->>D: Execute Query
    D-->>B: Return Result
    B-->>F: Response
    F-->>U: Update Screen

\`\`\`
```

### interfaces.ts

```typescript
// Entity definitions
export interface User {
  id: string;
  email: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}

// API request/response
export interface CreateUserRequest {
  email: string;
  name: string;
  password: string;
}

export interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: {
    code: string;
    message: string;
  };
}
```

### database-schema.sql

```sql
-- Users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Indexes
CREATE INDEX idx_users_email ON users(email);
```

### api-endpoints.md

```markdown
# API Endpoint Specifications

## Authentication

### POST /auth/login

Request:
\`\`\`json
{
"email": "user@example.com",
"password": "password"
}
\`\`\`

Response:
\`\`\`json
{
"success": true,
"data": {
"token": "jwt-token",
"user": { ... }
}
}
\`\`\`

## User Management

### GET /users/:id

### POST /users

### PUT /users/:id

### DELETE /users/:id
```

## Post-Execution Verification

- Display list of created files
- Display summary of main design points
- Display message prompting user confirmation