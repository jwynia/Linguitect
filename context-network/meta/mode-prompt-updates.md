# Mode Prompt Updates for Context-Network Integration

## Purpose Statement
This document outlines the changes needed for Roo agent mode prompts to ensure proper context gathering at the start of tasks and context-network updates at the end of tasks.

## Information Classification
- **Domain:** Meta-Documentation
- **Stability:** High
- **Abstraction:** Procedural
- **Confidence:** Established
- **Relevance:** All Roo agent modes

## Core Content

### Overview

These changes aim to enhance the Roo agent mode prompts to:
1. Ensure tasks start by gathering the right amount of context from the context-network
2. End by updating the context-network with any valuable context from the task's work

### Common Changes for All Modes

#### Context Gathering Phase (Beginning of Tasks)

```
When starting a new task:

1. **Assess Context Needs**: Determine what information from the context-network is relevant to the task.
   - For translation tasks: Begin with navigation-protocols/construct-translation.md
   - For language-specific tasks: Begin with the relevant language adapter documentation
   - For understanding constructs: Begin with the relevant document in semantic-constructs/

2. **Progressive Loading**: Start with high-level documents (foundation, README files) and load more detailed information as needed.

3. **Information Chunking**: Focus on one aspect of the task at a time, loading related documents in logical groups.

4. **Reference Preservation**: Keep document paths and relationship information in context for later updates.
```

#### Context-Network Update Phase (End of Tasks)

```
Before completing a task:

1. **Assess Update Needs**: Determine if the task has generated new information that should be added to the context-network.
   - New language features or idioms
   - Improved translation strategies
   - Clarifications of existing documentation
   - New semantic constructs or patterns

2. **Select Update Protocol**: Choose the appropriate update protocol from update-protocols.md:
   - Minor Content Update: For small corrections or clarifications
   - Major Content Update: For significant revisions or expansions
   - Document Addition: For creating new documents
   - Relationship Update: For changing connections between documents

3. **Document Changes**: Follow the documentation requirements for the selected protocol:
   - Update the document's change log if present
   - Update evolution-log.md for major changes
   - Document rationale and impact

4. **Verify Consistency**: Ensure updates maintain:
   - Structural consistency (standard structure, required metadata)
   - Relationship consistency (bidirectional relationships)
   - Terminology consistency (consistent terms across documents)
   - Navigation consistency (documents remain reachable)
```

### Mode-Specific Changes

#### Architect Mode

##### Context Gathering (Beginning)
```
Before planning, gather relevant context from the context-network by:
1. Identifying which parts of the network are most relevant to the task
2. Reading foundation documents for core concepts if needed
3. Exploring semantic constructs for programming concepts relevant to the task
4. Reviewing language adapters for language-specific information
```

##### Context-Network Update (End)
```
After the user confirms the plan and before switching to another mode:
1. Identify any new insights or information generated during planning
2. Determine if these insights should be added to the context-network
3. Suggest specific updates to the context-network based on these insights
4. Ask the user if they would like these updates to be implemented
```

#### Code Mode

##### Context Gathering (Beginning)
```
Before implementing code:
1. Gather relevant context from the context-network
2. For translation tasks, follow the translation workflow in network-guide.md
3. For language-specific tasks, consult the appropriate language adapter documentation
4. For semantic understanding, review the relevant semantic constructs
```

##### Context-Network Update (End)
```
After implementing a solution:
1. Document any new patterns, idioms, or translation strategies discovered
2. Identify improvements to existing context-network documentation
3. Suggest specific updates to the context-network
4. Ask the user if they would like these updates to be implemented
```

#### Debug Mode

##### Context Gathering (Beginning)
```
When diagnosing problems:
1. Consult the context-network for relevant semantic constructs and language-specific information
2. Use the information to inform your analysis of possible problem sources
3. Reference specific documents from the context-network that relate to the problem domain
```

##### Context-Network Update (End)
```
After resolving a problem:
1. Document the root cause and solution
2. Identify if this represents a pattern that should be added to the context-network
3. Suggest specific updates to the context-network based on the debugging insights
4. Ask the user if they would like these updates to be implemented
```

#### Ask Mode

##### Context Gathering (Beginning)
```
When answering questions:
1. Consult the context-network for authoritative information
2. Reference specific documents from the context-network in your explanations
3. Use the network's structured information to provide comprehensive answers
```

##### Context-Network Update (End)
```
After answering a question:
1. Identify if your answer contains information that should be added to the context-network
2. Suggest specific updates to the context-network based on your answer
3. Ask the user if they would like these updates to be implemented
```

## Implementation Instructions

To implement these changes:

1. Update each `.roo/system-prompt-*` file with the appropriate mode-specific instructions
2. Add the common instructions to all mode prompts
3. Ensure the instructions are added to the "USER'S CUSTOM INSTRUCTIONS" section under "Mode-specific Instructions"

## Relationship Network
- **Prerequisite Information:** [../README.md](../README.md), [update-protocols.md](./update-protocols.md), [network-guide.md](./network-guide.md)
- **Related Information:** [evolution-log.md](./evolution-log.md)
- **Dependent Information:** None
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When updating Roo agent mode prompts
- **Common Next Steps:** Implement these changes in the `.roo/system-prompt-*` files
- **Related Tasks:** Mode prompt maintenance, context-network integration
- **Update Patterns:** Updates when context-network structure or usage patterns change