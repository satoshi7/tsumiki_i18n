# 2.1 Required Tools and Setup

This section explains the tools and environment setup procedures necessary for practicing AITDD. AITDD doesn't require special tools and can be started by simply adding AI assistance tools to your existing development environment.

## Required Tools List

### Essential Tools

#### 1. Claude Sonnet 4 + Claude Code
**Role**: Main AI development assistance tool  
**Usage**: Code generation, test creation, refactoring, validation  
**Access method**: Via Claude Code

#### 2. VS Code
**Role**: Integrated development environment  
**Usage**: Code editing, debugging, project management  
**Features**: Integration with Claude Code possible

#### 3. Git + GitHub
**Role**: Version control  
**Usage**: Code management, history tracking, recovery  
**Importance**: Recovery method when AI doesn't produce expected results

### Auxiliary Tools

#### Gemini (Optional)
**Role**: Research and information gathering AI  
**Usage**: Library research, technical information collection  
**Features**: Large-scale information processing utilizing long context

## Setup Procedures

### Step 1: Claude Pro Plan Subscription

1. **Create Claude Account**
   - Access https://claude.ai
   - Create an account

2. **Upgrade to Pro Plan**
   - Subscribe to Pro plan at $20/month
   - Can set limit up to $200
   - Avoid unlimited API costs

3. **Enable Claude Code**
   - Accessible with Pro plan
   - Free usage for development purposes

### Step 2: VS Code Setup

1. **Install VS Code**
   ```bash
   # Windows
   winget install Microsoft.VisualStudioCode

   # macOS
   brew install --cask visual-studio-code

   # Linux
   sudo apt install code
   ```

2. **Install Basic Extensions**
   - Git Graph (Git operation visualization)
   - GitLens (Enhanced Git information display)
   - Language-specific extensions (JavaScript, Python, etc.)

3. **Configure Claude Code Integration**
   - VS Code plugin or Claude Code configuration
   - Project directory integration settings

### Step 3: Git Environment Setup

1. **Install and Configure Git**
   ```bash
   # Basic configuration
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   
   # Default branch setting
   git config --global init.defaultBranch main
   ```

2. **Prepare GitHub Account**
   - Create GitHub account
   - Set up SSH key or Personal Access Token
   - Prepare repository creation

3. **AITDD Branch Strategy**
   ```bash
   # Basic workflow
   git checkout -b feature/new-functionality
   # Practice AITDD
   git add .
   git commit -m "Implement feature with AITDD"
   git push origin feature/new-functionality
   ```

## AI Tool Comparison and Selection Criteria

### Reasons for Choosing Claude Sonnet 4

When practicing AITDD, we explain why Claude Sonnet 4 was chosen as the main tool among many available AI tools.

#### Selection Based on Comprehensive Evaluation

**Main Considerations:**
1. **Cost Efficiency**: Reasonable cost level is essential as AITDD involves frequent trials
2. **Coding Performance**: Prioritize stable performance over peak performance
3. **Accessibility**: Free usage with plans that have few restrictions
4. **Integration**: Collaboration with development environment and workflow consistency

**Claude Sonnet 4's Advantages:**
- **Claude Code Integration**: Close integration via VS Code plugin
- **Pro Plan**: $20/month with $200 limit setting (avoiding unlimited API costs)
- **AITDD Suitability**: Optimized for trial-focused development style
- **Overall Balance**: Optimal balance of performance, cost, and usability

#### Comparison Results with Other Tools

**Evaluated AI Tools:**
- ChatGPT: High performance but cost concerns
- GitHub Copilot: Specialized in code completion, insufficient for full AITDD
- Other AI tools: After trials, converged to Claude Sonnet 4 overall

**Convergence Reasons:**
```
Item                   Claude Sonnet 4    Other Tools
────────────────────────────────────────────────────
Cost Efficiency       ◎                 △
Coding Performance     ○                 ◎
Accessibility          ◎                 △
Integration           ◎                 ○
AITDD Suitability     ◎                 △
────────────────────────────────────────────────────
Overall Evaluation    Optimal           Issues
```

### Strategy for Using Gemini

While using Claude Sonnet 4 as the main tool, we auxiliary use Gemini for specific purposes.

#### Basic Policy for Division of Use

**Claude Sonnet 4 (Main Tool):**
- All implementation phases (design → test → implementation → refactoring → validation)
- Consistent execution of entire AITDD process
- Both code generation and quality checking

**Gemini (Auxiliary Tool):**
- Technical research and information gathering
- Large-scale information processing utilizing long context
- Required library research
- Tasks requiring extensive research

#### Specific Collaboration Patterns

```
Research Phase:
Gemini → Technical information collection → Provide information to Claude Sonnet 4

Implementation Phase:
Claude Sonnet 4 → Consistent AITDD process execution
```

**Collaboration Example:**
1. **New Library Research**: Information collection with Gemini
2. **Integration of Research Results**: Provide research results to Claude Sonnet 4
3. **Implementation Execution**: Execute AITDD process with Claude Sonnet 4

### Fallback Strategy

We explain how to handle cases when AI doesn't produce expected results.

#### Basic Fallback Procedures

**Step 1: State Recovery**
```bash
# Go back to previous state
git reset --hard HEAD~1
# Or go back to specific commit
git reset --hard <commit-hash>
```

**Step 2: Prompt Adjustment**
- Clarify and detail instructions
- Add contextual information
- Specify constraints explicitly

**Step 3: Re-execution**
- Retry with same tool (Claude Sonnet 4)
- Don't switch to different tools
- Maintain consistency

#### Fallback Decision Criteria

**When to Execute git reset:**
- When final code significantly deviates from expectations
- When rewriting is judged faster than correction requests
- When multiple correction attempts show no improvement

**Prompt Adjustment Guidelines:**
```
# Before improvement (vague)
"Fix this code"

# After improvement (specific)
"Fix the following issues in this code:
1. Validation errors are not handled properly
2. Return value type differs from specification
3. Edge case tests are insufficient"
```

#### Characteristics of Fallback Strategy

**Simplicity:**
- Avoid complex decision logic
- Prioritize quick recovery

**Consistency:**
- Basic approach of retrying with same tool
- Avoid confusion from tool switching

**Learning:**
- Improve intuition through repetition
- Accumulate experience to improve decision criteria

### Evaluation Method for New AI Tools

We also establish evaluation methods for new AI tools for continuous improvement.

#### Evaluation Process

**Information Gathering Phase:**
1. **Continuous Checking**: Constantly check information on new AI tools
2. **Trend Confirmation**: Check if there's sustained interest, not just temporary hype
3. **Community Response**: Confirm evaluation in developer community

**Trial Decision Criteria:**
- **Sustainability**: Has the topic continued for several months?
- **Practicality**: Can it be applied to AITDD workflow?
- **Cost Efficiency**: Is there advantage compared to current tool configuration?

**Cautious Approach:**
- Don't jump in immediately, conduct sufficient information gathering
- Evaluate while maintaining current stable workflow
- Consider migration only when clear advantages are confirmed

## Recommended Technology Stack

### Programming Languages

#### Recommended Languages
**JavaScript/TypeScript**
- Package management: npm/yarn
- Transparency: Dependency visualization via package.json
- AI support: Dynamic library research possible

**Python**
- Package management: pip/poetry
- Transparency: Dependency visualization via requirements.txt
- AI support: Rich library information available to AI

#### Languages Requiring Attention
**Java/C# and other compiled languages**
- Reason: Binary distribution via jar or dll makes dynamic dependency investigation difficult for AI
- Response: Usable but requires preparation and human assistance

### Project Types

#### Optimal Projects
- **CRUD-focused applications**
- **Web API development**
- **Database-integrated applications**
- **Relatively large-scale projects**

#### Effective Code Patterns
- Scenarios creating large amounts of similar code
- Easily templatable processes
- Standard design pattern implementations

## Cost Management

### Expected Costs
- **Claude Pro**: $20/month
- **GitHub**: Free for personal use, $4/user/month for team use
- **VS Code**: Free
- **Others**: Project-specific dependency costs

### Cost Optimization Points
1. **Claude Pro Limit Setting**: Set maximum at $200
2. **Efficient Usage**: Optimize AI usage with clear goal setting
3. **Git History Management**: Proper commit strategy to avoid wasteful trial and error

## Setup Completion Verification

Check the following items to confirm that setup is complete:

- [ ] Claude Pro plan is active
- [ ] Claude Code is accessible
- [ ] VS Code is installed
- [ ] Basic VS Code extensions are installed
- [ ] Git is configured
- [ ] GitHub account is prepared
- [ ] Project directory is created
- [ ] Basic environment for chosen technology stack is prepared

## Troubleshooting

### Common Problems and Solutions

**Cannot access Claude Code**
- Confirm Pro plan validity
- Clear browser cache
- Check network connection

**Problems with VS Code integration**
- Reinstall Claude Code plugin
- Restart VS Code
- Check configuration files

**Git operation errors**
- Reset authentication credentials
- Check remote repository URL
- Verify access permissions

## Next Steps

Once tool setup is complete, learn specific AI tool usage methods in the next chapter "2.2 Using Claude Sonnet 4". By mastering effective prompt design and AI collaboration techniques, you'll be able to unleash the true value of AITDD.