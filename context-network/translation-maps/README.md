# Translation Maps

## Purpose

The Translation Maps directory contains documentation about cross-language and cross-paradigm translation strategies. These maps provide guidance for translating code between different programming paradigms, patterns, and domains using Linguitect as the intermediate representation.

## Structure

```
translation-maps/
├── paradigm-maps/              # Cross-paradigm translation strategies
│   ├── oop-to-functional.md    # OOP to functional translation strategies
│   ├── procedural-to-oop.md    # Procedural to OOP translation strategies
│   └── functional-to-oop.md    # Functional to OOP translation strategies
├── pattern-maps/               # Design pattern implementations across languages
│   ├── iterator-patterns.md    # Iterator implementation patterns
│   ├── builder-patterns.md     # Builder pattern implementations
│   └── observer-patterns.md    # Observer pattern implementations
└── domain-maps/                # Domain-specific translation guidance
    ├── data-processing.md      # Data processing patterns
    ├── web-development.md      # Web development patterns
    └── numerical-computing.md  # Numerical computing patterns
```

## Document Structure

Each translation map document follows this structure:

```markdown
# [Translation Map Name]

## Purpose Statement
[Concise explanation of this translation map's function]

## Information Classification
- **Domain:** [Primary knowledge domain]
- **Stability:** [Static/Semi-stable/Dynamic]
- **Abstraction:** [Conceptual/Procedural/Detailed]
- **Confidence:** [Established/Evolving/Speculative]
- **Relevance:** [Primary use contexts]

## Translation Strategies
[Detailed strategies for translation, with examples]

## Relationship Network
[Relationships to other documents in the context network]

## Navigation Guidance
[Guidance for using this translation map effectively]
```

## Types of Translation Maps

1. **Paradigm Maps**: Provide strategies for translating between different programming paradigms
   - Focus on fundamental conceptual differences
   - Address semantic gaps between paradigms
   - Provide pattern transformations for equivalent functionality

2. **Pattern Maps**: Document how common design patterns are implemented across languages
   - Map idiomatic implementations of the same pattern
   - Address language-specific optimizations
   - Provide guidance for pattern selection

3. **Domain Maps**: Offer guidance for domain-specific translation challenges
   - Address specialized libraries and frameworks
   - Provide domain-specific idioms and patterns
   - Focus on maintaining domain semantics across languages

## Usage Guidelines for LLM Agents

1. Use paradigm maps when translating between languages with different paradigms
2. Consult pattern maps when translating code that implements common design patterns
3. Reference domain maps when working with specialized domains
4. Combine multiple maps when addressing complex translation scenarios
5. Consider the trade-offs documented in each translation strategy