# TypeScript Language Adapter

## Purpose

This directory contains documentation for the TypeScript language adapter in the Linguitect ecosystem. These documents describe the mapping between TypeScript and the Linguitect intermediate representation, enabling accurate translation between TypeScript and other supported languages.

## Contents

- **README.md**: Overview of TypeScript adapter (this file)
- **typescript-to-linguitect.md**: Documentation for converting from TypeScript to Linguitect
- **linguitect-to-typescript.md**: Documentation for converting from Linguitect to TypeScript
- **typescript-specific-idioms.md**: Catalog of TypeScript-specific idioms and patterns

## Language Characteristics

- **Paradigms**: Multi-paradigm (object-oriented, functional, procedural)
- **Typing**: Static, strong, with gradual typing capabilities
- **Memory Management**: Garbage collection
- **Execution Model**: Transpiled to JavaScript
- **Version Focus**: TypeScript 5.x+

## Usage Guidelines for LLM Agents

1. Use typescript-to-linguitect.md when translating from TypeScript to other languages
2. Use linguitect-to-typescript.md when translating from other languages to TypeScript
3. Consult typescript-specific-idioms.md for TypeScript-specific patterns and their translations
4. Reference the standard library mappings for TypeScript equivalents in other languages

The TypeScript adapter supports most TypeScript language features, with particular strength in handling TypeScript's rich type system, including interfaces, generics, union and intersection types, and type guards. The adapter is designed to work with TypeScript code targeting both browser and Node.js environments.

## TypeScript-Specific Features

TypeScript has several unique features that require special handling:

1. **Structural Type System**: TypeScript uses structural typing (duck typing) rather than nominal typing
2. **Advanced Type Features**: Union types, intersection types, conditional types, mapped types
3. **Type Guards and Narrowing**: Flow-based type analysis and user-defined type guards
4. **Declaration Merging**: Ability to merge declarations across multiple files
5. **Ambient Declarations**: Type definitions for existing JavaScript libraries

These features are carefully mapped to and from the Linguitect intermediate representation to ensure accurate translations.