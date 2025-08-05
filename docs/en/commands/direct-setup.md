# direct-setup

## Purpose

Execute setup tasks for DIRECT tasks. Perform environment setup, configuration file creation, dependency installation, etc., based on design documents.

## Prerequisites

- Task ID is provided
- Related design documents exist
- Necessary permissions and environment are prepared

## Execution Details

1. **Design Document Verification**
   - Check `docs/design/{requirement-name}/architecture.md`
   - Check `docs/design/{requirement-name}/database-schema.sql`
   - Check other related design documents

2. **Setup Task Execution**
   - Environment variable configuration
   - Configuration file creation/update
   - Dependency installation
   - Database initialization
   - Service startup configuration
   - Permission settings

3. **Work Record Creation**
   - Record executed commands
   - Record configuration changes
   - Record encountered problems and solutions

## Output Destination

Work records are created in the `docs/implements/{TASK-ID}/` directory as:
- `setup-report.md`: Setup task execution record

## Output Format Example

````markdown
# {TASK-ID} Setup Task Execution

## Work Overview

- **Task ID**: {TASK-ID}
- **Work Content**: {Setup task overview}
- **Execution Date**: {Execution date}
- **Executor**: {Executor}

## Design Document Reference

- **Referenced Documents**: {List of referenced design documents}
- **Related Requirements**: {REQ-XXX, REQ-YYY}

## Executed Tasks

### 1. Environment Variable Configuration

```bash
# Executed commands
export NODE_ENV=development
export DATABASE_URL=postgresql://localhost:5432/mydb
```
````

**Configuration Details**:

- NODE_ENV: Set to development environment
- DATABASE_URL: PostgreSQL database URL

### 2. Configuration File Creation

**Created File**: `config/database.json`

```json
{
  "development": {
    "host": "localhost",
    "port": 5432,
    "database": "mydb"
  }
}
```

### 3. Dependency Installation

```bash
# Executed commands
npm install express pg
```

**Installed Components**:

- express: Web framework
- pg: PostgreSQL client

### 4. Database Initialization

```bash
# Executed commands
createdb mydb
psql -d mydb -f database-schema.sql
```

**Execution Details**:

- Database creation
- Schema application

## Work Results

- [ ] Environment variable configuration completed
- [ ] Configuration file creation completed
- [ ] Dependency installation completed
- [ ] Database initialization completed
- [ ] Service startup configuration completed

## Encountered Problems and Solutions

### Problem 1: {Problem overview}

- **Situation**: {Situation where problem occurred}
- **Error Message**: {Error message}
- **Solution**: {Solution method}

## Next Steps

- Execute `direct-verify.md` to verify configuration
- Perform configuration adjustments as needed

```

## Post-Execution Verification
- Verify that `docs/implements/{TASK-ID}/setup-report.md` file is created
- Verify that configuration is properly applied
- Verify that next step (direct-verify) is ready

## Directory Creation

Create necessary directories before execution:
```bash
mkdir -p docs/implements/{TASK-ID}
```
```