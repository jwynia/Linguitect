# Metadata Schema

## Purpose Statement
This document defines the documentation structure and metadata standards for the Linguitect context network, ensuring consistency and facilitating efficient navigation by LLM agents.

## Information Classification
- **Domain:** Foundation
- **Stability:** High
- **Abstraction:** Structural
- **Confidence:** Established
- **Relevance:** Documentation creation and maintenance

## Core Content

### Document Structure

All documents in the Linguitect context network should follow this standard structure:

```markdown
# Document Title

## Purpose Statement
[Concise explanation of this document's function within the context network]

## Information Classification
- **Domain:** [Primary knowledge domain]
- **Stability:** [Static/Semi-stable/Dynamic]
- **Abstraction:** [Conceptual/Procedural/Detailed]
- **Confidence:** [Established/Evolving/Speculative]
- **Relevance:** [Primary use contexts]

## Core Content
[Primary information organized in a structured format appropriate to the content type]

## Relationship Network
- **Prerequisite Information:** [Documents that should be understood first]
- **Related Information:** [Documents with associative connections]
- **Dependent Information:** [Documents that build on this information]
- **Alternative Perspectives:** [Documents with different viewpoints]
- **Implementation Details:** [Documents with more specific information]

## Navigation Guidance
- **Access Context:** [When to use this information]
- **Common Next Steps:** [Typical navigation paths from here]
- **Related Tasks:** [Activities where this information is relevant]
- **Update Patterns:** [How and when this information changes]
```

### Metadata Fields

#### Document Title
A clear, concise title that identifies the document's subject. Should be unique within its directory.

#### Purpose Statement
A brief (1-3 sentences) explanation of the document's function within the context network. Should clearly state what information the document provides and why it exists.

#### Information Classification

**Domain**:
The primary knowledge domain this document belongs to. Common values include:
- Foundation
- Semantic Constructs
- Language-Specific
- Cross-Language
- Translation Process
- Meta-Documentation

**Stability**:
How frequently the information is expected to change:
- **Static**: Rarely changes (e.g., core concepts, architecture)
- **Semi-stable**: Occasionally updated (e.g., language adapters)
- **Dynamic**: Frequently updated (e.g., evolution logs, current status)

**Abstraction**:
The level of abstraction of the information:
- **Conceptual**: High-level concepts and principles
- **Procedural**: Processes, workflows, and methods
- **Detailed**: Specific implementations and examples

**Confidence**:
The level of certainty about the information:
- **Established**: Well-understood, verified information
- **Evolving**: Information that is still being refined
- **Speculative**: Experimental or theoretical information

**Relevance**:
The primary contexts in which this information is useful:
- List specific use cases, tasks, or roles

#### Core Content
The main body of the document, organized in a structured format appropriate to the content type. Should use consistent headings, code blocks, and other formatting.

#### Relationship Network

**Prerequisite Information**:
Documents that should be understood before this one:
- List documents that provide necessary background
- Include relative paths to the documents

**Related Information**:
Documents with associative connections to this one:
- List documents that cover related topics
- Include relative paths to the documents

**Dependent Information**:
Documents that build on this information:
- List documents that depend on this one
- Include relative paths to the documents

**Alternative Perspectives**:
Documents that provide different viewpoints on the same topic:
- List documents with alternative approaches
- Include relative paths to the documents

**Implementation Details**:
Documents with more specific implementation information:
- List documents with more detailed information
- Include relative paths to the documents

#### Navigation Guidance

**Access Context**:
When to use this information:
- Describe the situations or tasks when this document is most relevant

**Common Next Steps**:
Typical navigation paths from this document:
- List documents commonly accessed after this one
- Include relative paths to the documents

**Related Tasks**:
Activities where this information is relevant:
- List specific tasks or workflows that use this information

**Update Patterns**:
How and when this information changes:
- Describe the circumstances that would trigger updates
- Note any dependencies that would be affected by changes

### Document Types

Different types of documents may have additional specialized sections:

#### Semantic Construct Documents

```markdown
## Semantic Intent
[Concise explanation of what this programming construct accomplishes]

## Linguitect Representation
```
[Linguitect IR representation]
```

## Classification
- **Paradigms:** [Applicable programming paradigms]
- **Abstraction Level:** [Low/Medium/High]
- **Mutability:** [Mutable/Immutable]
- **Side Effects:** [Pure/Impure]
- **Execution Model:** [Eager/Lazy]

## Language Implementations
[Examples of how this construct is implemented in different languages]

## Translation Guidance
[Guidance for translating this construct across languages and paradigms]

## Related Constructs
[Links to related programming constructs]

## Evolution Notes
[Notes on how this construct is evolving in programming languages]
```

#### Language Adapter Documents

```markdown
## Language Characteristics
- **Paradigms:** [Supported programming paradigms]
- **Typing:** [Static/Dynamic, Strong/Weak]
- **Memory Management:** [GC/Manual/Reference Counting/Ownership]
- **Execution Model:** [Compiled/Interpreted/JIT]

## Adapter Capabilities
[Description of what the adapter can and cannot handle]

## Implementation Status
[Current status and version compatibility]

## Mapping Rules
[Rules for mapping between the language and Linguitect]

## Idiom Catalog
[Catalog of language-specific idioms and their Linguitect representations]

## Standard Library Mapping
[Mapping of standard library functions and types]
```

#### Translation Map Documents

```markdown
## Translation Strategies
[Detailed strategies for translation, with examples]

## Decision Points
[Key decision points and criteria for making choices]

## Trade-offs
[Trade-offs involved in different translation approaches]

## Examples
[Examples of applying the translation strategies]
```

#### Protocol Documents

```markdown
## Protocol Steps
[Step-by-step process for accomplishing a task]

## Decision Points
[Key decision points and criteria for making choices]

## Examples
[Examples of applying the protocol to specific scenarios]
```

### File Naming Conventions

- Use lowercase kebab-case for all filenames (e.g., `semantic-intent.md`)
- Use descriptive names that clearly indicate the content
- Avoid abbreviations unless they are widely understood
- Include the primary subject in the filename

### Directory Structure

Maintain the established directory structure:

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

## Relationship Network
- **Prerequisite Information:** None
- **Related Information:** [linguitect-core.md](./linguitect-core.md)
- **Dependent Information:** All documentation in the context network
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When creating or updating documentation in the context network
- **Common Next Steps:** Apply this schema to create or update specific documents
- **Related Tasks:** Documentation creation, maintenance, and review
- **Update Patterns:** Updates when documentation standards evolve or new document types are introduced