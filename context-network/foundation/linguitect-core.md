# Linguitect Core Concepts

## Purpose Statement
This document defines the fundamental concepts of the Linguitect intermediate representation language, serving as the foundation for understanding the entire Linguitect ecosystem.

## Information Classification
- **Domain:** Foundation
- **Stability:** High
- **Abstraction:** Conceptual
- **Confidence:** Established
- **Relevance:** All translation tasks

## Core Content

### What is Linguitect?

Linguitect is an intermediate representation (IR) language designed specifically for Large Language Models (LLMs) to translate code between programming languages. It focuses on capturing semantic intent rather than syntax, enabling more accurate translations across programming paradigms.

### Key Design Principles

1. **Self-documenting**: Linguitect uses verbose, explicit tags that make structure obvious, enhancing readability for both humans and LLMs.

2. **Semantic-focused**: Linguitect represents what code does rather than how it's written, capturing the programmer's intent rather than syntactic details.

3. **Paradigm-neutral**: Linguitect supports multiple programming paradigms (procedural, object-oriented, functional) without bias, enabling cross-paradigm translation.

4. **Intent-preserving**: Linguitect captures programmer intent through annotations, preserving the purpose of code beyond its literal implementation.

### Core Architecture

Linguitect operates as a hub-and-spoke translation system:

```
                 ┌─────────┐
                 │ Python  │
                 └────┬────┘
                      │
┌─────────┐      ┌────┴────┐      ┌─────────┐
│  Rust   │◄────►│Linguitect│◄────►│   Java  │
└─────────┘      └────┬────┘      └─────────┘
                      │
                 ┌────┴────┐
                 │JavaScript│
                 └─────────┘
```

This architecture enables:
- **N×1 Translation Efficiency**: Instead of creating N×(N-1) direct translators for N languages, only 2×N adapters are needed (N import adapters and N export adapters).
- **Semantic Consistency**: All translations share a common semantic representation, ensuring consistent interpretation.
- **Incremental Expansion**: New languages can be added by creating just 2 new adapters (import and export), instantly enabling translation to/from all existing languages.

### Fundamental Components

1. **Language Adapters**: Bridge between specific programming languages and Linguitect IR
   - **Import Adapters**: Convert source language code to Linguitect IR
   - **Export Adapters**: Convert Linguitect IR to target language code

2. **Semantic Constructs**: Core programming concepts represented in Linguitect
   - Variables and constants
   - Functions and methods
   - Control flow structures
   - Data structures
   - Object-oriented constructs
   - Functional programming constructs
   - Error handling mechanisms
   - Concurrency primitives

3. **Translation Maps**: Guidance for mapping between languages and paradigms
   - Cross-paradigm translation strategies
   - Idiomatic pattern equivalents
   - Domain-specific translation approaches

### Representation Format

Linguitect uses a tag-based format with explicit opening and closing tags:

```
@TAG [parameters]
  [content]
@ENDTAG
```

This format:
- Makes structure explicit and unambiguous
- Supports nested constructs with clear boundaries
- Allows for rich metadata and annotations
- Is easily parseable by both machines and humans

### Translation Process

The translation process follows these general steps:

1. **Parse** source code into an abstract syntax tree (AST)
2. **Analyze** the AST to infer types, resolve names, and identify idioms
3. **Transform** the analyzed AST into Linguitect IR
4. **Optimize** the Linguitect IR for the target language
5. **Generate** target language code from the optimized IR

## Relationship Network
- **Prerequisite Information:** None
- **Related Information:** [architecture.md](./architecture.md), [design-principles.md](./design-principles.md)
- **Dependent Information:** [../semantic-constructs/README.md](../semantic-constructs/README.md), [../language-adapters/README.md](../language-adapters/README.md)
- **Alternative Perspectives:** None
- **Implementation Details:** External: [spec/linguitect-spec.md]

## Navigation Guidance
- **Access Context:** Initial orientation, fundamental understanding
- **Common Next Steps:** Explore [architecture.md](./architecture.md) for system structure, [../semantic-constructs/](../semantic-constructs/) for programming constructs, or [../language-adapters/](../language-adapters/) for language-specific implementations
- **Related Tasks:** Creating new language adapters, understanding translation processes
- **Update Patterns:** Rare updates, only with major conceptual changes