# Context Network Guide

## Purpose Statement
This document provides a comprehensive guide to using the Linguitect context network, explaining how to navigate, understand, and utilize the information effectively, particularly for LLM agents.

## Information Classification
- **Domain:** Meta-Documentation
- **Stability:** High
- **Abstraction:** Procedural
- **Confidence:** Established
- **Relevance:** All users of the context network, especially LLM agents

## Core Content

### Introduction to the Context Network

The Linguitect Context Network is an agentic memory system designed primarily for LLM agents to efficiently work with and expand the Linguitect project. It provides a structured framework for organizing, navigating, and evolving the complex knowledge space of code translation across multiple programming languages using Linguitect as the intermediate layer.

### Network Structure

The context network is organized into a hierarchical structure with seven main directories:

```
context-network/
├── foundation/             # Core concepts, architecture, and principles
├── semantic-constructs/    # Programming constructs and their Linguitect representations
├── language-adapters/      # Language-specific adapter documentation
├── translation-maps/       # Cross-language and cross-paradigm translation guidance
├── navigation-protocols/   # Processes for navigating the translation space
├── evolution-mechanisms/   # Processes for adapting and expanding the network
└── meta/                   # Documentation about the context network itself
```

Each directory contains documents that follow a consistent structure with metadata, core content, relationship information, and navigation guidance.

### Navigation Strategies

#### 1. Task-Based Navigation

When approaching a specific task, follow these steps:

1. **Identify the task type**:
   - Code translation between specific languages
   - Adding a new language adapter
   - Improving translation quality
   - Understanding a specific programming construct

2. **Start at the appropriate entry point**:
   - For translation tasks: Begin with `navigation-protocols/construct-translation.md`
   - For new adapters: Begin with `language-adapters/adapter-template/adapter-structure.md`
   - For understanding constructs: Begin with the relevant document in `semantic-constructs/`

3. **Follow the relationship network**:
   - Use the "Related Information" and "Dependent Information" sections to navigate to relevant documents
   - Consult "Prerequisite Information" if you need more background

4. **Apply navigation protocols**:
   - Follow the step-by-step processes in the navigation-protocols directory
   - Make decisions based on the criteria provided at decision points

#### 2. Exploration-Based Navigation

For broader understanding or exploration:

1. **Start with foundation documents**:
   - Begin with `foundation/linguitect-core.md` for core concepts
   - Explore `foundation/architecture.md` for system structure
   - Review `foundation/design-principles.md` for guiding philosophy

2. **Explore by domain**:
   - Browse the semantic-constructs directory for programming concepts
   - Explore language-adapters for language-specific information
   - Review translation-maps for cross-language strategies

3. **Build a mental model**:
   - Connect related concepts across directories
   - Understand the relationships between different components
   - Identify patterns and principles that apply across domains

#### 3. Problem-Solving Navigation

When addressing a specific problem or challenge:

1. **Define the problem scope**:
   - Identify the specific languages, constructs, or paradigms involved
   - Determine the desired outcome or success criteria

2. **Gather relevant information**:
   - Consult semantic-constructs for the programming concepts involved
   - Review language-adapters for language-specific considerations
   - Explore translation-maps for relevant translation strategies

3. **Apply navigation protocols**:
   - Follow the appropriate protocol for your problem type
   - Make decisions based on the criteria provided
   - Document your reasoning and decisions

4. **Evaluate and refine**:
   - Use evaluation metrics from evolution-mechanisms
   - Refine your approach based on feedback
   - Document lessons learned for future reference

### Information Retrieval Patterns

#### Metadata-Based Retrieval

Use document metadata to quickly assess relevance:

1. **Domain**: Identifies the primary knowledge domain
2. **Stability**: Indicates how frequently the information changes
3. **Abstraction**: Shows the level of detail (conceptual, procedural, detailed)
4. **Confidence**: Reflects the certainty of the information
5. **Relevance**: Specifies the primary use contexts

#### Relationship-Based Retrieval

Use the relationship network to find connected information:

1. **Prerequisite Information**: Documents you should understand first
2. **Related Information**: Documents with associative connections
3. **Dependent Information**: Documents that build on this information
4. **Alternative Perspectives**: Documents with different viewpoints
5. **Implementation Details**: Documents with more specific information

#### Navigation Guidance Retrieval

Use navigation guidance to understand context and next steps:

1. **Access Context**: When to use this information
2. **Common Next Steps**: Typical navigation paths from here
3. **Related Tasks**: Activities where this information is relevant
4. **Update Patterns**: How and when this information changes

### Context Window Optimization

For LLM agents with limited context windows:

1. **Progressive Loading**:
   - Start with high-level documents (foundation, README files)
   - Load more detailed information as needed
   - Prioritize documents based on task relevance

2. **Information Chunking**:
   - Focus on one aspect of the task at a time
   - Load related documents in logical groups
   - Maintain core context while rotating detailed information

3. **Reference Preservation**:
   - Keep document paths and relationship information in context
   - Maintain a "navigation stack" of documents consulted
   - Preserve key metadata across context window refreshes

### Common Workflows

#### Translation Workflow

1. **Understand the source code**:
   - Identify the programming constructs used
   - Recognize language-specific idioms
   - Determine the semantic intent

2. **Map to Linguitect IR**:
   - Consult semantic-constructs for appropriate representations
   - Apply import adapter rules from language-adapters
   - Preserve semantic intent and programmer comments

3. **Transform to target language**:
   - Apply export adapter rules from language-adapters
   - Consult translation-maps for paradigm bridging if needed
   - Generate idiomatic code in the target language

4. **Evaluate and refine**:
   - Assess translation quality using metrics from evolution-mechanisms
   - Refine the translation based on feedback
   - Document any challenges or improvements

#### Adapter Creation Workflow

1. **Analyze the language**:
   - Understand the language's features and paradigms
   - Identify language-specific idioms and patterns
   - Determine type system and memory management approach

2. **Map to Linguitect concepts**:
   - Match language constructs to semantic-constructs
   - Identify gaps or unique features
   - Develop mapping strategies

3. **Create adapter documentation**:
   - Follow the adapter-template structure
   - Document import and export rules
   - Catalog idioms and standard library mappings

4. **Test and refine**:
   - Validate with example translations
   - Refine based on evaluation metrics
   - Document lessons learned

### Contribution Guidelines

When contributing to the context network:

1. **Follow the metadata schema**:
   - Use the standard document structure
   - Include all required metadata
   - Document relationships explicitly

2. **Maintain consistency**:
   - Use consistent terminology (see foundation/terminology.md)
   - Follow file naming conventions
   - Adhere to the established directory structure

3. **Document relationships**:
   - Update relationship networks in affected documents
   - Ensure bidirectional relationships are consistent
   - Document dependencies and prerequisites

4. **Follow evolution processes**:
   - Use the appropriate process from evolution-mechanisms
   - Document changes in the evolution-log
   - Update affected documents

## Relationship Network
- **Prerequisite Information:** [../README.md](../README.md), [../foundation/metadata-schema.md](../foundation/metadata-schema.md)
- **Related Information:** [update-protocols.md](./update-protocols.md), [evolution-log.md](./evolution-log.md)
- **Dependent Information:** [agent-workflows/translation-agent.md](./agent-workflows/translation-agent.md), [agent-workflows/adapter-creation.md](./agent-workflows/adapter-creation.md)
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** Initial orientation to the context network, reference for navigation strategies
- **Common Next Steps:** Explore [../foundation/linguitect-core.md](../foundation/linguitect-core.md) for core concepts, or begin a specific task using the appropriate workflow
- **Related Tasks:** All tasks involving the context network
- **Update Patterns:** Updates when navigation strategies evolve or new components are added to the network