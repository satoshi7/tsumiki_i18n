# TDD Requirements Definition & Functional Specification Organization

Begin TDD development. Please organize requirements for the following functionality:

**【Feature Name】**: {{feature_name}}

## Preparation

Prepare the development context:

**Task Tool Execution**: `/tdd-load-context` to load TDD-related files and prepare context

After loading completion, start TDD requirements definition work based on the prepared context information.

## TDD Requirements Organization Format

**【Reliability Level Instructions】**:
For each item, comment on the verification status with source materials (EARS requirements definition documents and design documents) using the following signals:

- 🟢 **Green Signal**: When referencing EARS requirements and design documents with almost no speculation
- 🟡 **Yellow Signal**: When making reasonable speculation from EARS requirements and design documents  
- 🔴 **Red Signal**: When speculation is not found in EARS requirements and design documents

## 1. Feature Overview (Based on EARS Requirements & Design Documents)

- 🟢🟡🔴 Record reliability level for each item
- What this feature does (extracted from user stories)
- What problems it solves (extracted from As a / So that)
- Expected users (extracted from As a)
- Position within the system (extracted from architecture design)
- **Referenced EARS Requirements**: [specific requirement IDs]
- **Referenced Design Documents**: [relevant sections of architecture.md]

## 2. Input/Output Specifications (Based on EARS Functional Requirements & TypeScript Type Definitions)

- 🟢🟡🔴 Record reliability level for each item
- Input parameters (type, range, constraints) - extracted from interfaces.ts
- Output values (type, format, examples) - extracted from interfaces.ts
- Input/output relationships
- Data flow (extracted from dataflow.md)
- **Referenced EARS Requirements**: [specific REQ-XXX]
- **Referenced Design Documents**: [relevant interfaces in interfaces.ts]

## 3. Constraint Conditions (Based on EARS Non-Functional Requirements & Architecture Design)

- 🟢🟡🔴 Record reliability level for each item
- Performance requirements (extracted from NFR-XXX)
- Security requirements (extracted from NFR-XXX)
- Compatibility requirements (extracted from REQ-XXX MUST)
- Architecture constraints (extracted from architecture.md)
- Database constraints (extracted from database-schema.sql)
- API constraints (extracted from api-endpoints.md)
- **Referenced EARS Requirements**: [specific NFR-XXX, REQ-XXX]
- **Referenced Design Documents**: [relevant sections of architecture.md, database-schema.sql, etc.]

## 4. Expected Use Cases (Based on EARS Edge Cases & Data Flow)

- 🟢🟡🔴 Record reliability level for each item
- Basic usage patterns (extracted from normal requirements REQ-XXX)
- Data flow (extracted from dataflow.md)
- Edge cases (extracted from EDGE-XXX)
- Error cases (extracted from EDGE-XXX error handling)
- **Referenced EARS Requirements**: [specific EDGE-XXX]
- **Referenced Design Documents**: [relevant flow diagrams in dataflow.md]

## 5. Correspondence with EARS Requirements & Design Documents

When referencing existing EARS requirements definition documents and design documents, clearly specify the following correspondence:

- **Referenced User Stories**: [story names]
- **Referenced Functional Requirements**: [REQ-001, REQ-002, ...]
- **Referenced Non-Functional Requirements**: [NFR-001, NFR-101, ...]
- **Referenced Edge Cases**: [EDGE-001, EDGE-101, ...]
- **Referenced Acceptance Criteria**: [specific test items]
- **Referenced Design Documents**:
  - **Architecture**: [relevant sections of architecture.md]
  - **Data Flow**: [relevant flow diagrams in dataflow.md]
  - **Type Definitions**: [relevant interfaces in interfaces.ts]
  - **Database**: [relevant tables in database-schema.sql]
  - **API Specifications**: [relevant endpoints in api-endpoints.md]

After organization, execute the following:

1. Save requirements definition to docs/implements/{{task_id}}/{feature_name}-requirements.md (append if existing file exists)
2. Update TODO status (mark requirements definition completion)
3. **Quality Assessment**: Assess requirements quality by the following criteria
   - Requirements are clear without ambiguity
   - Input/output specifications are specifically defined
   - Constraint conditions are clear
   - Implementation feasibility is certain
4. **Next Step Display**: Regardless of assessment results, display next recommended command
   - "Next recommended step: `/tdd-testcases` to perform test case identification."

## Quality Assessment Criteria

```
✅ High Quality:
- Requirement ambiguity: None
- Input/output definition: Complete
- Constraint conditions: Clear
- Implementation feasibility: Certain

⚠️ Needs Improvement:
- Some ambiguous parts in requirements
- Input/output details unclear
- Technical constraints unknown
- User intent confirmation needed
```

## TODO Update Pattern

```
- Mark current TODO as "completed"
- Reflect completion of requirements definition phase in TODO content
- Add next phase "Test case identification" to TODO
- Record quality assessment results in TODO content
```

Next step: `/tdd-testcases` to perform test case identification.