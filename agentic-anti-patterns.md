---
tags: [agents, anti-patterns, mistakes, pitfalls]
created: 2026-02-15
difficulty: intermediate
status: draft
---

# Agentic Anti-Patterns

Common mistakes teams make when adopting AI agents for software development. These patterns emerge from traditional engineering culture that doesn't translate to agentic workflows.

## Process Anti-Patterns

### Coding Before Alignment

**Anti-pattern**: Engineers jump straight to coding without defining clear objectives.

**Why it fails**: Agents generate code quickly, but without clear objectives you get:
- Code that works but solves the wrong problem
- Multiple implementations that need reconciliation
- Wasted agent utilization on throwaway work
- Context switches as requirements clarify mid-implementation

**Solution**: [[agentic-patterns#Pair Prompting]] - spend first hours aligning on objectives before any coding

**Red flags**:
- "Let's just start coding and see what we need"
- Skipping objective definition to "move fast"
- Different team members prompting agents with conflicting goals

### Specifying Implementation Instead of Outcomes

**Anti-pattern**: Writing detailed implementation instructions for agents like they're junior developers.

**Example**:
```markdown
❌ Bad: Implementation specification
1. Create a React component called UserList
2. Import useState and useEffect from react
3. Fetch users from /api/users endpoint using fetch()
4. Map over the results and render each user in a <div>
5. Add error handling with try/catch
```

**Why it fails**: You're doing the agent's job (figuring out implementation) instead of your job (defining success).

**Solution**: [[agentic-patterns#Spec Outcomes, Not Process]]

**What to write instead**:
```markdown
✅ Good: Outcome specification
Objective: Display list of users from backend
Constraints:
- Load time <500ms
- Handle errors gracefully
- Mobile responsive
Success criteria:
- All users visible
- Filters work correctly
- No console errors
```

### Line-by-Line Code Review

**Anti-pattern**: Reading every line of agent-generated code like you would human-written code.

**Why it fails**:
- Agent-generated code is often verbose but correct
- Agents use different patterns than humans
- Time spent reviewing is better spent defining next objectives
- Creates bottleneck in agent utilization (idle while waiting for review)

**When this is appropriate**:
- Security-critical code paths
- Novel architectural decisions
- Debugging repeated failures
- Learning what the agent is doing

**Solution**: [[agentic-patterns#Review Output, Not Code]] - test against objectives, ship if it passes

### Keeping Dead Code Paths

**Anti-pattern**: Leaving old implementations alongside new ones "just in case."

**Examples**:
```python
# Old implementation - TODO: remove after migration
def old_process_user(user):
    ...

# New implementation
def process_user(user):
    ...
```

**Why it fails**: "The codebase is agent context. Every dead path is noise that degrades agent performance."

- Agents get confused about which implementation to use
- Dead code appears in context, wastes tokens
- Agents might call deprecated functions
- Future prompts need to specify "use the new one"

**Solution**: [[agentic-patterns#Kill Old Ways Immediately]] - delete old code the moment new code ships

### Maximizing Human Coding Time

**Anti-pattern**: Preserving the old culture of "minimize meetings, maximize coding time."

**Why it fails**: You're optimizing for the wrong bottleneck.

- Old bottleneck: Engineer coding time
- New bottleneck: Clear objective definition + agent utilization

**Symptoms**:
- Engineers coding in isolation without alignment
- No time for pair prompting
- Agents idle during work hours while engineers code manually
- "I'll just write it myself, it's faster"

**Solution**: Invert your priorities—spend more time on alignment, less on implementation

### Manual Repetition

**Anti-pattern**: Doing the same task manually multiple times instead of systematizing it.

**Examples**:
- Manually testing the same workflow after each change
- Copy-pasting deployment commands
- Manually updating documentation after code changes
- Repeatedly prompting agents with similar requests

**Why it fails**: "If a human is repeating a task, the system isn't set up right."

**Solution**: [[agentic-patterns#Think in Systems]] - automate after the second repetition

## Code Anti-Patterns

### Optimizing for Human Cleverness

**Anti-pattern**: Writing clever, terse code that's hard for agents to understand.

**Examples**:
```python
# Too clever - agent might misunderstand
process = lambda x: [i for i in x if i%2] if isinstance(x, list) else []

# Clear for agents
def extract_odd_numbers(numbers):
    """Returns odd numbers from input list, or empty list if input is not a list."""
    if not isinstance(numbers, list):
        return []
    return [num for num in numbers if num % 2 != 0]
```

**Why it fails**: Agents read code to understand intent before regenerating. Clever code obscures intent.

**Solution**: [[agentic-patterns#Code as Context, Not Library]] - optimize for comprehension

### Building Reusable Abstractions Too Early

**Anti-pattern**: Creating generic, reusable functions/classes before you have multiple use cases.

**Traditional wisdom**: DRY (Don't Repeat Yourself), build abstractions early
**Agentic reality**: Agents regenerate code anyway, premature abstraction adds cognitive load

**Why it fails**:
- Abstractions are harder for agents to understand than concrete code
- Agents often regenerate rather than reuse
- Premature abstraction optimizes for the wrong thing

**When abstractions matter**:
- Shared data structures (agents need to agree on format)
- System boundaries and interfaces
- Third-party API wrappers (centralized for changes)

**Solution**: Wait until you have 3+ similar implementations, then extract commonality

### Over-Specified Schemas

**Anti-pattern**: Defining rigid, detailed schemas upfront with every field locked down.

**Why it fails**: Agents can adapt to variation better than rigid schemas. Over-specification:
- Creates friction when requirements change
- Prevents agents from making reasonable implementation choices
- Requires schema updates for small changes

**Solution**: [[agentic-patterns#Define Rules, Not Structure]] - set constraints and conventions, let agents determine details

### Function-Oriented Interfaces

**Anti-pattern**: Designing component interfaces as function signatures instead of data contracts.

**Why it fails**: "Clean data lets agents compose systems without being told how."

- Agents need to know exact function names and parameters
- Changes require updating all callers
- Harder for agents to discover capabilities

**Solution**: [[agentic-patterns#Data-Driven Interfaces]] - define data structures with clear semantics

## Team Anti-Patterns

### Enforcing Tool Uniformity

**Anti-pattern**: Requiring everyone to use the same IDE, agent, or prompting workflow.

**Why it fails**:
- Engineers have different preferences for agent interaction
- New tools emerge monthly
- Enforced uniformity creates friction
- Productivity varies wildly by tool/person fit

**What to standardize**: Data patterns, objective formats, component responsibilities
**What to leave flexible**: IDE choice, prompting style, local workflows

**Solution**: [[agentic-patterns#Individual Autonomy, Shared Interfaces]]

### Treating Agents Like Junior Developers

**Anti-pattern**: Onboarding agents like you'd onboard a junior human engineer.

**Examples**:
- "Let's start you on small bug fixes to learn the codebase"
- Explaining architectural decisions in detail
- Gradually increasing complexity
- Pairing with the agent for mentorship

**Why it fails**: Agents aren't junior developers—they're tools with different capabilities.

- Agents can handle complex tasks immediately with proper objectives
- Agents don't need gradual learning curves
- Agents don't build institutional knowledge
- Time spent "training" agents is wasted

**What agents need**: Clear objectives, good context, well-structured data

### Token Optimization Over Time Optimization

**Anti-pattern**: Spending human time to reduce token usage.

**Examples**:
- Carefully trimming context to minimize tokens
- Writing scripts to send only changed lines
- Complex prompt engineering to reduce output length
- Choosing smaller models to save cost

**Why it fails**: "The bottleneck is human decision-making time, not compute cost."

- Engineer hours cost $50-200/hour
- 10,000 tokens cost ~$0.01-$0.10
- Optimizing tokens wastes expensive human time to save pennies

**When token cost matters**: At massive scale (millions of requests/day), not during development

**Solution**: [[agentic-patterns#Optimize for Time, Not Tokens]]

### Building for Future-Proofing

**Anti-pattern**: Spending time on "future-proof architecture" to prevent rewrites.

**Why it fails**: "Technology shifts monthly now. Every decision you make today will soon be wrong."

- You can't predict what will change
- Over-engineering for unknowns adds complexity
- Rewrites are normal in fast-moving AI landscape
- 3-6 month lifespan for architectural decisions

**Solution**: [[agentic-patterns#Assume 3-Month Expiration]] - build modular, optimize for changeability

### Silent Anti-Pattern Accumulation

**Anti-pattern**: Letting anti-patterns accumulate without calling them out.

**Why it fails**: "Old habits compound fast."

- Team members fall into traditional patterns unconsciously
- Anti-patterns spread through imitation
- Technical debt accumulates quickly
- Hard to unwind after becoming established

**Solution**: [[agentic-patterns#Immediate Anti-Pattern Flagging]] - create culture of immediate feedback

## Organizational Anti-Patterns

### Measuring Engineer Output by Code Written

**Anti-pattern**: Using lines of code, commits, or PRs as productivity metrics.

**Why it fails**: In agentic workflows, engineers write less code but deliver more value.

**Old metrics**: Lines of code, commits/day, PRs merged
**New metrics**: Objectives achieved, agent utilization, time-to-delivery

### Preserving Old Engineering Rituals

**Anti-pattern**: Keeping practices designed for human-written code.

**Examples**:
- Sprint planning with story points based on implementation complexity
- Detailed technical design docs before implementation
- Code review checklist for style/patterns
- Estimation based on coding time

**Why it fails**: These practices assume humans implement code. With agents, implementation is fast—the bottleneck shifts to objective clarity.

**What to keep**: Objective alignment, outcome validation, system thinking
**What to drop**: Implementation estimation, detailed design docs, coding time optimization

### Hiring for Coding Skills Over System Thinking

**Anti-pattern**: Optimizing hiring for implementation skills (LeetCode, algorithm knowledge).

**Why it fails**: Agent's job is implementation. Engineer's job is:
- Defining clear objectives
- Designing system boundaries
- Choosing right constraints
- Validating outcomes

**Skills that matter more now**:
- System design thinking
- Problem decomposition
- Communication and alignment
- Product sense
- Prompt engineering

## Warning Signs

Your team might be falling into anti-patterns if:

- Agents sit idle overnight while nothing runs
- Engineers spend more time coding than defining objectives
- Codebase has multiple implementations of same thing
- Dead code accumulates
- Team reviews every line of agent-generated code
- Engineers say "it's faster to write it myself"
- Prompts include detailed implementation steps
- Token optimization is a regular discussion
- Planning focuses on implementation details
- Hiring emphasizes coding interview performance

## Related

- [[agentic-patterns]]
- [[agentic-tooling]]

## References

- [No Coding Before 10am - Michael Bloch](https://x.com/michaelxbloch/status/2022678437362598163)
