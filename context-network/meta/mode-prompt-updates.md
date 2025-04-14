# Mode Prompt Updates for Context Network Maintenance

## Purpose Statement
This document outlines recommended updates to the Architect mode prompt to ensure consistent and proper maintenance of the context-network, particularly when completing tasks that generate valuable information.

## Information Classification
- **Domain:** Meta-Documentation
- **Stability:** Semi-stable
- **Abstraction:** Procedural
- **Confidence:** Evolving
- **Relevance:** LLM agent configuration, context network maintenance

## Core Content

### 1. Current Limitations

The current Architect mode prompt has the following limitations regarding context network maintenance:

1. **Inconsistent Documentation**: Task completion summaries and analysis results are not consistently documented in the context network
2. **Missing Preservation Step**: There's no explicit step for preserving insights in the context network
3. **Incomplete Task Closure**: Tasks are considered complete without ensuring knowledge preservation
4. **Reactive Documentation**: Documentation happens reactively (when prompted) rather than proactively

### 2. Recommended Prompt Updates

#### 2.1 Add Context-Network Update Phase

Add a specific phase to the Architect mode prompt that focuses on context network updates:

```
# Context-Network Update Phase
8. Before switching to another mode:
   - Identify any new insights or information generated during planning
   - Determine if these insights should be added to the context-network
   - Suggest specific updates to the context-network based on these insights
   - Ask the user if they would like these updates to be implemented
   - If yes, follow the appropriate update protocol from update-protocols.md:
     * Minor Content Update: For small corrections or clarifications
     * Major Content Update: For significant revisions or expansions
     * Document Addition: For creating new documents
     * Relationship Update: For changing connections between documents
   - Document changes according to the protocol requirements
   - Verify consistency of the updates (structural, relationship, terminology, navigation)
```

#### 2.2 Enhance Task Completion Process

Modify the task completion process to include context network updates:

```
# Task Completion Process
7. When completing a task:
   - Summarize the key findings, decisions, and outcomes
   - Identify which parts of this information should be preserved in the context network
   - Create or update appropriate context network documents
   - Reference these documents in the completion summary
   - Ensure all new documents follow the metadata schema
   - Update relationship references in affected documents
```

#### 2.3 Add Context Gathering Phase

Enhance the context gathering phase to leverage the context network more effectively:

```
# Context Gathering Phase
1. Before planning, gather relevant context from the context-network by:
   - Identifying which parts of the network are most relevant to the task
   - Reading foundation documents for core concepts if needed
   - Exploring semantic constructs for programming concepts relevant to the task
   - Reviewing language adapters for language-specific information

2. When starting a new task:
   - Assess Context Needs: Determine what information from the context-network is relevant to the task
   - For translation tasks: Begin with navigation-protocols/construct-translation.md
   - For language-specific tasks: Begin with the relevant language adapter documentation
   - For understanding constructs: Begin with the relevant document in semantic-constructs/
   - Use progressive loading: Start with high-level documents and load more detailed information as needed
   - Focus on one aspect of the task at a time, loading related documents in logical groups
   - Keep document paths and relationship information in context for later updates
```

### 3. Implementation Guidelines

When implementing these prompt updates:

1. **Maintain Consistency**: Ensure the updated prompt maintains consistency with other mode prompts
2. **Preserve Core Functionality**: Don't remove or disrupt existing functionality
3. **Test Effectiveness**: Validate that the updates lead to more consistent context network maintenance
4. **Provide Examples**: Include examples of proper context network updates
5. **Reference Documentation**: Point to relevant documentation like update-protocols.md and metadata-schema.md

### 4. Expected Benefits

These prompt updates should result in:

1. **Consistent Documentation**: Task results consistently preserved in the context network
2. **Proactive Knowledge Management**: Insights automatically considered for preservation
3. **Better Knowledge Reuse**: More information available for future tasks
4. **Reduced Redundancy**: Less need to recreate analysis or plans
5. **Improved Context Awareness**: Better leveraging of existing context network information

### 5. Example Workflow

#### Before:
1. User asks a question about inconsistencies
2. Architect analyzes and provides an answer
3. Task is considered complete
4. Knowledge remains only in the conversation

#### After:
1. User asks a question about inconsistencies
2. Architect gathers context from relevant context network documents
3. Architect analyzes and provides an answer
4. Architect identifies valuable insights from the analysis
5. Architect creates/updates context network documents to preserve these insights
6. Architect references these documents in the completion summary
7. Task is considered complete with knowledge preserved

## Relationship Network
- **Prerequisite Information:** [update-protocols.md](./update-protocols.md), [metadata-schema.md](../foundation/metadata-schema.md)
- **Related Information:** [network-guide.md](./network-guide.md)
- **Dependent Information:** Architect mode prompt configuration
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When configuring or updating the Architect mode prompt
- **Common Next Steps:** Implement the suggested prompt updates, then test with sample tasks
- **Related Tasks:** Mode prompt configuration, context network maintenance
- **Update Patterns:** Updates when context network structure or maintenance protocols change