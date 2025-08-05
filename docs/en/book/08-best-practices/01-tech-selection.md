# 8.1 Technology Selection Guidelines

This provides practical guidelines for engineers to make appropriate decisions when selecting technologies for AITDD implementation.

## AI Tool Selection Criteria

### Organizational Policy Takes Priority

#### Using Company-Designated AI Tools
Consider organizational governance and compliance as the highest priority over technical superiority.

- **Use Only Approved Tools**: Use only AI tools directed by the company for business operations
- **Compliance with Organizational Policies**: Prioritize organizational rules over technical advantages
- **Regular Policy Review**: Regularly check for changes in AI tool usage guidelines

#### Recommended Tool: Claude Sonnet 4
Recommended tools based on current practical experience and their reasons:

- **High-Quality Implementation Generation**: Code generation capabilities suitable for the TDD process
- **Claude Code Integration**: Efficiency through integration with development environments
- **Japanese Language Support**: Quality of specification descriptions and comment generation
- **Token Capacity**: Stable operation with large contexts

### Technical Selection Criteria

#### Project Application Conditions

**Cases Suitable for Application**
- **New Development Projects**: Very suitable
- **Modern Technology Stack**: AI assistance is effective
- **Clear Requirements Definition**: When specifications are clearly defined
- **Iterative Development**: Agile and incremental development

**Cases Not Suitable for Application**
- **Life-Critical Projects**: Not applicable
- **Legacy Systems**: Existing systems with complex constraints
- **Extremely High Quality Requirements**: Projects with low risk tolerance
- **Short-Term Small Modifications**: When AIDD introduction costs are not justified

## Security and Compliance

### Confidential Information Management

#### Criteria for Transmittable Information
Practical criteria to ensure the safety of information sent to AI tools:

- **Exclude Personal Information**: Information that can identify individuals is excluded from transmission
- **Exclude Confidential Data**: Information classified as company confidential is excluded from transmission
- **General Technical Information**: Only publicly available technical information is permitted for transmission

#### Practical Data Protection Methods
```
Pre-transmission Checklist:
□ Check for presence of personal information
□ Confirm removal of confidential data
□ Evaluate necessity of transmission
□ Consider alternative methods
```

### Intellectual Property Considerations

#### Organizational Policy Compliance
- **Application of Intellectual Property Policy**: Operation according to organizational intellectual property policies
- **Coordination with Legal Department**: Consultation with legal department as necessary
- **Contract Terms Verification**: Confirm AI tool usage contract provisions

#### Risk Management
- **Recognition of Copyright Risks**: Understanding copyright issues with AI-generated code
- **License Terms Verification**: Compliance with license terms of AI tools used
- **Commercial Use Appropriateness**: Confirmation of usability in commercial products

## Team Composition and Skill Requirements

### Required Roles and Skills

#### Design Lead
**Required Level**: Experienced professionals should primarily handle design

Required Skills:
- **API Design**: Experience in designing RESTful APIs or GraphQL
- **Database Design**: Ability to design appropriate schemas
- **Architecture Design**: Experience in overall system structure design
- **AI-Compatible Design**: Ability to create systems that AI can easily work with

#### Development Members
**Basic Requirements**: Programming fundamentals and adaptability to AI utilization

- **Understanding of TDD**: Basic concepts of test-driven development
- **Prompt Skills**: Effective dialogue ability with AI (learnable)
- **Code Review Ability**: Appropriate evaluation of AI-generated code
- **Continuous Learning Motivation**: Adaptability to new methods

### Addressing Prompt Skill Gaps

#### Realistic Challenge Recognition
**"The difference between those who can imagine AI's response and those who cannot"**

Main Differences:
- Familiarity differences from AI dialogue experience
- Degree of understanding AI characteristics and quirks
- Skills to predict prompt effectiveness in advance
- Accumulated experience from trial and error

#### Countermeasure Approaches
- **Continuous Practice**: Realistic recognition that "you just have to keep using it"
- **Gradual Proficiency**: Start with small tasks and accumulate experience
- **Knowledge Sharing**: Share success cases and best practices
- **Pair Programming**: Learning promotion through pairing experienced and novice users

## Cost-Effectiveness Judgment

### Efficiency Evaluation Criteria

#### Development Speed Improvement
Effects based on practical data:
- **Implementation Speed**: Achieving 20-48x improvement over conventional methods
- **Development Cycle**: 1-2 day tasks reduced to about 1 hour
- **Ease of Trial and Error**: Many approaches can be tried through rapid iteration

#### New Cost Elements
- **Quality Management Cost**: Load of checking and reviewing AI-generated code
- **Learning Cost**: Time for team members to learn AIDD
- **Tool Usage Cost**: Claude Code API fees, etc.

### Return on Investment Judgment

#### Basic Policy
- **Criteria**: No problem if overwhelmingly faster than manual work
- **Effect**: Significant reduction in developer time (reduced to 1/4 to 1/8)
- **Overall Evaluation**: Balance between implementation efficiency vs quality management cost

#### Practical Introduction Decision
```
Introduction Consideration Checklist:
□ Sufficient project duration (considering learning costs)
□ Team has understanding of AI utilization
□ Conforms to organizational AI usage policy
□ Quality requirements are within appropriate range
□ Security risks are manageable
```

## Technology Stack Selection Guidelines

### AI-Support Suitable Technologies

#### Recommended Technology Stack
- **Modern Frameworks**: React, Vue.js, Next.js, etc.
- **Type-Safe Languages**: TypeScript, Rust, Go, etc.
- **Standard Architectures**: RESTful API, MVC, Microservices
- **Testable Design**: Dependency injection, unit-testable structures

#### Technologies to Avoid
- **Complex Legacy Technologies**: Old technologies with insufficient documentation
- **Non-Standard Architectures**: Proprietary designs difficult for AI to understand
- **Test-Difficult Structures**: Designs not suitable for TDD

### Gradual Technology Introduction

#### Introduction Phases
1. **Pilot Project**: Trial with small-scale new features
2. **Partial Application**: Apply to some features of existing projects
3. **Full Deployment**: AITDD practice across the entire team

#### Technology Migration Strategy
- **Parallel with Existing Technologies**: Risk reduction through gradual migration
- **Knowledge Transfer**: Utilize insights from conventional methods in AITDD
- **Continuous Improvement**: Optimization of methods through practice

---

Please refer to these technology selection guidelines to implement AITDD practices suitable for your organization and project characteristics.