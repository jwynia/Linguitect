# Meta

## Purpose

The Meta directory contains documentation about the context network itself, including usage guides, update protocols, and evolution logs. This meta-documentation helps LLM agents understand how to effectively use and maintain the context network.

## Structure

```
meta/
├── network-guide.md            # Guide to using the context network
├── update-protocols.md         # Procedures for maintaining the network
├── evolution-log.md            # History of network changes
├── consistency-improvement-plan.md  # Plan for addressing inconsistencies
├── consistency-improvement-summary.md  # Summary of consistency improvements
├── version-concordance.md      # Tracking specification and implementation alignment
├── feature-coverage-matrix.md  # Feature support across language adapters
├── versioning-strategy.md      # Version numbering and compatibility requirements
├── mode-prompt-updates.md      # Recommendations for mode prompt improvements
└── agent-workflows/            # Workflows for specific agent tasks
    ├── translation-agent.md    # Workflows for translation agents
    ├── adapter-creation.md     # Workflows for creating new adapters
    └── quality-assessment.md   # Workflows for evaluating translations
```

## Document Structure

Each meta document follows this structure:

```markdown
# [Document Title]

## Purpose Statement
[Concise explanation of this document's function]

## Information Classification
- **Domain:** [Primary knowledge domain]
- **Stability:** [Static/Semi-stable/Dynamic]
- **Abstraction:** [Conceptual/Procedural/Detailed]
- **Confidence:** [Established/Evolving/Speculative]
- **Relevance:** [Primary use contexts]

## Core Content
[Primary information organized in a structured format]

## Relationship Network
[Relationships to other documents in the context network]

## Navigation Guidance
[Guidance for using this information effectively]
```

## Key Meta Documents

1. **network-guide.md**: Comprehensive guide to using the context network
   - Explains the overall structure and purpose
   - Provides navigation strategies for different tasks
   - Offers guidance for efficient information retrieval

2. **update-protocols.md**: Procedures for maintaining the context network
   - Defines processes for updating documents
   - Establishes consistency requirements
   - Provides templates for different document types

3. **evolution-log.md**: History of significant changes to the context network
   - Documents major structural changes
   - Tracks the addition of new languages and features
   - Provides context for understanding the network's development

4. **consistency-improvement-plan.md**: Plan for addressing inconsistencies in the system
   - Identifies potential issues and contradictions
   - Outlines specific improvement actions
   - Defines implementation phases and success criteria

5. **consistency-improvement-summary.md**: Summary of consistency improvement efforts
   - Summarizes identified issues and solutions
   - Lists created and updated documents
   - Tracks implementation status and future work

6. **version-concordance.md**: Tracking alignment between specifications and implementations
   - Maps version relationships between components
   - Provides feature coverage analysis
   - Identifies implementation gaps and priorities

7. **feature-coverage-matrix.md**: Detailed matrix of feature support across language adapters
   - Documents implementation status for each feature
   - Highlights compatibility issues
   - Prioritizes implementation efforts

8. **versioning-strategy.md**: Guidelines for version management
   - Defines version numbering scheme
   - Establishes compatibility requirements
   - Outlines update propagation rules

9. **mode-prompt-updates.md**: Recommendations for mode prompt improvements
   - Identifies current limitations in context network maintenance
   - Provides specific prompt update recommendations
   - Outlines implementation guidelines and expected benefits

10. **agent-workflows/**: Task-specific workflows for LLM agents
   - Provides step-by-step guides for common tasks
   - Defines input and output expectations
   - Offers optimization strategies for different agent roles

## Usage Guidelines for LLM Agents

1. Start with network-guide.md to understand how to use the context network
2. Consult consistency-improvement-plan.md and consistency-improvement-summary.md to understand current system improvement efforts
3. Use version-concordance.md and feature-coverage-matrix.md when working with language adapters
4. Follow update-protocols.md and versioning-strategy.md when making changes to the network
5. Review mode-prompt-updates.md for guidance on maintaining the context network during task completion
6. Consult evolution-log.md to understand the historical context
7. Use agent-workflows/ documents for guidance on specific tasks
8. Contribute to the meta documentation as the network evolves