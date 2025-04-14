# Python Language Adapter

## Purpose

This directory contains documentation for the Python language adapter in the Linguitect ecosystem. These documents describe the mapping between Python and the Linguitect intermediate representation, enabling accurate translation between Python and other supported languages.

## Contents

- **overview.md**: General information about Python and its adapter
- **import-adapter.md**: Documentation for converting from Python to Linguitect
- **export-adapter.md**: Documentation for converting from Linguitect to Python
- **idioms.md**: Catalog of Python-specific idioms and patterns
- **stdlib-mapping.md**: Mapping of Python standard library functions and types

## Language Characteristics

- **Paradigms**: Multi-paradigm (procedural, object-oriented, functional)
- **Typing**: Dynamic, strong
- **Memory Management**: Garbage collection
- **Execution Model**: Interpreted
- **Version Focus**: Python 3.6+

## Usage Guidelines for LLM Agents

1. Use import-adapter.md when translating from Python to other languages
2. Use export-adapter.md when translating from other languages to Python
3. Consult idioms.md for Python-specific patterns and their translations
4. Reference stdlib-mapping.md for standard library equivalents in other languages

The Python adapter supports most common Python features, with particular strength in translating procedural and object-oriented code. Functional patterns are also supported, though some advanced functional features may have limitations in languages with weaker functional support.