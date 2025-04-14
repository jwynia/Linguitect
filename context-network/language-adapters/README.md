# Language Adapters

## Purpose

The Language Adapters directory contains structured documentation for language-specific adapters in the Linguitect ecosystem. These adapters serve as bridges between specific programming languages and the Linguitect intermediate representation (IR), enabling accurate and efficient multi-language code translation.

## Types of Adapters

There are two types of adapters:
- **Import Adapters**: Convert source language code to Linguitect IR
- **Export Adapters**: Convert Linguitect IR to target language code

## Structure

```
language-adapters/
├── adapter-template/           # Templates for new language adapters
│   ├── adapter-structure.md    # Template for overall adapter structure
│   ├── import-adapter.md       # Template for import adapters
│   └── export-adapter.md       # Template for export adapters
├── python/                     # Python language adapter documentation
│   ├── overview.md             # Python adapter summary
│   ├── import-adapter.md       # Python to Linguitect mapping
│   ├── export-adapter.md       # Linguitect to Python mapping
│   ├── idioms.md               # Python-specific idioms
│   └── stdlib-mapping.md       # Standard library equivalents
├── rust/                       # Rust language adapter documentation
│   ├── overview.md             # Rust adapter summary
│   ├── import-adapter.md       # Rust to Linguitect mapping
│   ├── export-adapter.md       # Linguitect to Rust mapping
│   ├── idioms.md               # Rust-specific idioms
│   └── stdlib-mapping.md       # Standard library equivalents
└── typescript/                 # TypeScript language adapter documentation
    ├── README.md               # TypeScript adapter summary
    ├── typescript-to-linguitect.md # TypeScript to Linguitect mapping
    ├── linguitect-to-typescript.md # Linguitect to TypeScript mapping
    └── typescript-specific-idioms.md # TypeScript-specific idioms
```

Additional language directories will be added as the project expands.

## Document Structure

Each language adapter directory contains these key documents:

1. **overview.md**: General information about the language and its adapter
   - Language characteristics (paradigms, typing, memory management)
   - Adapter capabilities and limitations
   - Implementation status and version compatibility

2. **import-adapter.md**: Documentation for converting from the language to Linguitect
   - Syntax mapping rules
   - Type inference and resolution strategies
   - Idiom recognition patterns

3. **export-adapter.md**: Documentation for converting from Linguitect to the language
   - Code generation rules
   - Type mapping strategies
   - Idiom implementation patterns

4. **idioms.md**: Catalog of language-specific idioms and patterns
   - Common idioms and their Linguitect representations
   - Performance characteristics
   - Usage guidance

5. **stdlib-mapping.md**: Mapping of standard library functions and types
   - Core library functions and their equivalents
   - Data structure mappings
   - Common API patterns

## Usage Guidelines for LLM Agents

1. Start with the overview.md to understand the language's characteristics
2. Consult import-adapter.md when translating from the language to Linguitect
3. Consult export-adapter.md when translating from Linguitect to the language
4. Reference idioms.md for language-specific patterns and optimizations
5. Use stdlib-mapping.md for standard library function translations
6. Refer to adapter-template/ when creating adapters for new languages