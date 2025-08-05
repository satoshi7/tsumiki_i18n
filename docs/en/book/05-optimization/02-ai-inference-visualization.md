# 5.2 AI Inference Visualization Technology

## Introduction

The most important aspect of quality management in AI-generated code is clearly understanding which parts the AI has "inferred." Using the traffic light system for AI inference visualization enables efficient review and high quality assurance.

## Theoretical Background of the Traffic Light System

### Problem Identification

AI-generated code has the following characteristics:

- **Wide-ranging auto-completion**: AI automatically fills in parts that aren't explicitly stated
- **The trap of plausibility**: Generated content looks reasonable but may differ from actual intent
- **Unclear inference basis**: It's unclear what information the generation was based on

### Solution Approach

The traffic light system classifies AI-generated content by the following criteria, clarifying review priorities:

```
🟢 Green Light → 🟡 Yellow Light → 🔴 Red Light
   Safe            Caution          Danger
```

## Detailed Definition of the Traffic Light System

### 🟢 Green Light (High Confidence/Safe)

**Definition:** Content that can be clearly inferred from referenced source files

**Characteristics:**
- Generation based on content explicitly stated in original instructions or specifications
- Implementation following patterns in existing code
- Implementation decisions with clear basis

**Specific Example:**
```javascript
// When specifications clearly state "User ID is required"
function validateUser(userId) {
  if (!userId) {  // 🟢 Clearly derived from specifications
    throw new Error('User ID is required');
  }
}
```

**Review Priority:** Low
**Check Points:** Implementation accuracy, performance impact

### 🟡 Yellow Light (Medium Confidence/Caution)

**Definition:** Content not in referenced source files but seems reasonable

**Characteristics:**
- Completion based on AI's rational inference
- Implementation based on general best practices
- Inference using domain knowledge

**Specific Example:**
```javascript
// Error handling when specifications lack detail
function processData(data) {
  try {
    return transform(data);
  } catch (error) {  // 🟡 Common but needs confirmation
    console.error('Data processing failed:', error);
    return null;
  }
}
```

**Review Priority:** High
**Check Points:** Validity of inference, alignment with business requirements

### 🔴 Red Light (Requires Judgment/Danger)

**Definition:** Content not in referenced source files and cannot be directly inferred

**Characteristics:**
- Generation based on AI's independent judgment
- Assumptions about organization-specific conventions or rules
- Implementation choices without clear basis

**Specific Example:**
```javascript
// When there's no information about organization's log format
function logUserAction(action) {
  // 🔴 Log format is organization-specific, needs confirmation
  logger.info(`[AUDIT] User performed: ${action} at ${new Date().toISOString()}`);
}
```

**Review Priority:** Highest
**Check Points:** Alignment with organizational rules, security impact

## Implementation Method and TODO File Format

### Standard TODO File Format

```markdown
## [Step Name] Result TODO

### 🟢 High Confidence Items
- [ ] Confirm type definitions in [utils.js](./src/utils.js) match specifications
- [ ] Check implementation of required field validation in [validation.js](./src/validation.js)

### 🟡 Medium Confidence Items
- [ ] Verify appropriateness of error response format in [error-handler.js](./src/error-handler.js)
- [ ] Check organizational policy compliance of default value settings in [config.js](./src/config.js)

### 🔴 Items Requiring Judgment
- [ ] Detailed check: Log output format in [logger.js](./src/logger.js) conforms to organizational standards
- [ ] Detailed check: Rationale for session management method selection in [auth.js](./src/auth.js)
```

### How to Instruct in Prompts

**Basic Instruction Template:**
```markdown
## AI Inference Visualization Instructions

Execute the following tasks and classify generated content using the traffic light system:

**Task Content:**
[Specific task details]

**Classification Criteria:**
- 🟢 Green Light: Can be clearly derived from reference files ([filename])
- 🟡 Yellow Light: Reasonable inference but not explicitly stated in reference files
- 🔴 Red Light: Generated from independent judgment (organization-specific content, etc.)

**Output File:** `./todos/[step-name]-inference-check.md`

**Output Format:**
Assign traffic light marks to each generated item and create check items in TODO format
```

### Management of Reference Source Files

**Recording File Relationships:**
```markdown
## Reference File Management

**Primary References:**
- [`requirements.md`](./docs/requirements.md) - Basic requirements definition
- [`api-spec.yaml`](./docs/api-spec.yaml) - API specifications

**Secondary References:**
- [`existing-code/`](./src/existing/) - Existing implementation patterns
- [`config-samples/`](./config/) - Configuration file examples

**External References:**
- Technical documentation (official framework docs)
- Industry standards (RFC, W3C, etc.)

**Inference Basis Tracking:**
- 🟢 items → Explicitly stated in primary references
- 🟡 items → Secondary references + general knowledge
- 🔴 items → Basis unclear/independent judgment
```

## Setting Check Priorities

### Priority Matrix

| Signal | High Impact | Medium Impact | Low Impact |
|--------|-------------|---------------|------------|
| 🔴 Red Light | **Top Priority** | High Priority | Medium Priority |
| 🟡 Yellow Light | High Priority | Medium Priority | Low Priority |
| 🟢 Green Light | Medium Priority | Low Priority | **Defer** |

### Impact Assessment Criteria

**High Impact:**
- Security-related implementation
- Affects data integrity
- Affects entire system operation

**Medium Impact:**
- Affects specific feature operation
- Affects user experience
- Affects performance

**Low Impact:**
- Log output and comments
- Internal variable names
- Auxiliary features

### Practical Check Order

1. **🔴×High Impact** - Check and fix immediately
2. **🔴×Medium Impact** and **🟡×High Impact** - Check before starting next work
3. **Other 🔴 items** - Must check before implementation completion
4. **🟡 items** - Check during review
5. **🟢 items** - Check during final review

## Real-World Operation Examples and Case Studies

### Case Study 1: REST API Implementation

**Scenario:** User management API implementation
**Reference Files:** `user-api-spec.yaml`, `existing-user-model.js`

**Classification of AI-Generated Results:**

```javascript
// 🟢 Endpoint specified in API documentation
app.post('/api/users', async (req, res) => {
  
  // 🟡 Common validation but no details in specifications
  if (!req.body.email || !req.body.password) {
    return res.status(400).json({ error: 'Email and password required' });
  }
  
  // 🔴 Hashing algorithm depends on organization-specific policy
  const hashedPassword = bcrypt.hashSync(req.body.password, 12);
  
  // 🟢 Implementation following existing model patterns
  const user = new User({
    email: req.body.email,
    password: hashedPassword
  });
});
```

**Generated TODO:**
```markdown
## API Implementation Result TODO

### 🟢 High Confidence Items
- [ ] Confirm endpoint definition in [user-controller.js](./src/controllers/user.js) matches specifications
- [ ] Verify adherence to existing patterns in [user-model.js](./src/models/user.js)

### 🟡 Medium Confidence Items
- [ ] Check appropriateness of error message format in [validation.js](./src/middleware/validation.js)
- [ ] Confirm status code selection in [user-controller.js](./src/controllers/user.js)

### 🔴 Items Requiring Judgment
- [ ] Detailed check: bcrypt salt rounds in [auth.js](./src/utils/auth.js) comply with organizational policy
```

### Case Study 2: Test Case Generation

**Scenario:** Creating test cases for the above API
**Reference Files:** `user-api-spec.yaml`, `existing-test-patterns.js`

**Classification Results:**
```javascript
describe('User API', () => {
  // 🟢 Test cases explicitly stated in specifications
  it('should create user with valid email and password', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: 'password123' });
    
    expect(response.status).toBe(201);  // 🟢 As per specifications
  });
  
  // 🟡 Common edge cases (not in specifications)
  it('should reject invalid email format', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'invalid-email', password: 'password123' });
    
    expect(response.status).toBe(400);  // 🟡 Based on inference
  });
  
  // 🔴 Inference based on organization-specific security requirements
  it('should enforce password complexity requirements', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', password: '123' });
    
    expect(response.status).toBe(400);  // 🔴 Depends on org policy
  });
});
```

## Effectiveness Measurement and Improvement Methods

### Effectiveness Metrics

**Quantitative Metrics:**
- Review time reduction rate
- Bug detection rate improvement
- Reduction in revision count

**Qualitative Metrics:**
- Review efficiency improvement
- Prevention of overlooking important issues
- Developer confidence improvement

### Example Operational Data

```markdown
## Traffic Light System Implementation Effects (1 Month)

**Traditional Review:**
- Average review time: 45 min/feature
- Bug detection rate: ~60%
- Review oversights: 3-4 per month

**After Traffic Light System Implementation:**
- Average review time: 25 min/feature (44% reduction)
- Bug detection rate: ~85% (25% improvement)
- Review oversights: 1 or less per month

**Typical 🔴 Item Issues:**
- Security-related: 40%
- Organizational policy violations: 35%
- Configuration/environment dependencies: 25%
```

### Continuous Improvement Approach

**1. Improving Classification Accuracy:**
```markdown
## Classification Criteria Improvement Log

**Week 1-2:**
- Problem: Ambiguous log format classification
- Improvement: Created checklist for organization-specific items

**Week 3-4:**
- Problem: Unstable error handling classification
- Improvement: Documented error handling patterns

**Month 2:**
- Problem: Difficulty classifying new tech stack
- Improvement: Created tech stack-specific guidelines
```

**2. Prompt Optimization:**
```markdown
## Prompt Improvement Cycle

**Pre-improvement Issues:**
- 🔴 item detection rate around 70%
- Inconsistent classification

**Improvements Made:**
- Provided clear list of organization-specific items
- Added abundant specific examples for judgment criteria

**Post-improvement Effects:**
- 🔴 item detection rate improved to over 90%
- Significant improvement in classification consistency
```

## Practical Implementation Steps

### Step 1: Building the Basic System

```bash
# Prepare project structure
mkdir -p todos
mkdir -p docs/inference-guides

# Create basic template
cat > docs/inference-guides/classification-template.md << 'EOF'
## AI Inference Classification Template

### 🟢 Green Light Criteria
- Content explicitly stated in reference file "[filename]"
- Implementation following established patterns in existing code

### 🟡 Yellow Light Criteria  
- Implementation based on general best practices
- Technically sound but not stated in reference files

### 🔴 Red Light Criteria
- Depends on organization-specific policies or conventions
- Independent judgment without clear basis
EOF
```

### Step 2: Documenting Organization-Specific Rules

```markdown
## Organization-Specific Checkpoints

**Security-Related:**
- [ ] Password hashing algorithm and strength
- [ ] Session management method
- [ ] API authentication method

**Logging/Audit-Related:**
- [ ] Log output format and levels
- [ ] Audit log output items
- [ ] Log retention period and rotation

**Coding Standards:**
- [ ] Naming conventions (variables, functions, classes)
- [ ] Error handling patterns
- [ ] Comment writing rules
```

### Step 3: Establishing Team Operations

```markdown
## Team Operation Rules

**Classification Work Assignment:**
- AI executor performs initial classification
- Reviewer confirms classification validity

**Check Work Distribution:**
- 🔴 items: Senior engineer reviews
- 🟡 items: Pair review within team  
- 🟢 items: Automated tests + minor checks

**Knowledge Accumulation:**
- Weekly classification criteria review meetings
- Sharing misclassification patterns
- Documentation of improvement cases
```

## Troubleshooting

### Common Problems and Solutions

**Problem 1: Inconsistent Classification**

```markdown
**Symptoms:** Similar content gets different classifications
**Cause:** Ambiguous classification criteria, unclear prompt instructions
**Solutions:**
1. Create more specific list of organization-specific items
2. Include past classification examples in prompts
3. Record and standardize unclear judgment items
```

**Problem 2: Missing 🔴 Items**

```markdown
**Symptoms:** Important organization-specific items classified as 🟡 or 🟢
**Cause:** AI doesn't understand organizational context
**Solutions:**
1. Provide explicit list of organization-specific items
2. Set principle of "when in doubt, classify as 🔴"
3. Reviewer checks classification validity
```

**Problem 3: Too Many TODO Items**

```markdown
**Symptoms:** Generated TODO items exceed manageable range
**Cause:** Classification too granular, impact assessment too lenient
**Solutions:**
1. Group similar items
2. Stricter impact assessment
3. Introduce "importance × urgency" matrix
```

## Practical Exercises

### Exercise 1: Creating Classification Criteria

Classify the following code snippet using the traffic light system:

```javascript
function authenticateUser(username, password) {
  // Case 1: User existence check
  const user = await User.findOne({ username });
  if (!user) {
    return { success: false, message: 'User not found' };
  }
  
  // Case 2: Password verification
  const isValid = await bcrypt.compare(password, user.hashedPassword);
  if (!isValid) {
    return { success: false, message: 'Invalid password' };
  }
  
  // Case 3: JWT token generation
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
  
  // Case 4: Log output
  console.log(`User ${username} authenticated successfully at ${new Date().toISOString()}`);
  
  return { success: true, token };
}
```

**Reference File:** `auth-spec.md` (only basic auth flow documented)

### Exercise 2: Setting TODO Item Priorities

Based on the above classification results, arrange TODO items in priority order.

## Summary

AI inference visualization technology achieves the following effects:

1. **Efficient Review**: Quality checks focused on important areas
2. **Risk Mitigation**: Reliable discovery of high-risk inference parts
3. **Knowledge Accumulation**: Documentation and sharing of organization-specific judgment criteria
4. **Continuous Improvement**: Data-based optimization of classification criteria and prompts

The traffic light system is a crucial technology for balancing human factors and AI assistance in AITDD. In the next section, we'll learn about continuous improvement and prompt optimization using these technologies.