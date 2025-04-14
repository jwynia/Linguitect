# Rust Language Adapter

## Purpose

This directory contains documentation for the Rust language adapter in the Linguitect ecosystem. These documents describe the mapping between Rust and the Linguitect intermediate representation, enabling accurate translation between Rust and other supported languages.

## Contents

- **overview.md**: General information about Rust and its adapter
- **import-adapter.md**: Documentation for converting from Rust to Linguitect
- **export-adapter.md**: Documentation for converting from Linguitect to Rust
- **idioms.md**: Catalog of Rust-specific idioms and patterns
- **stdlib-mapping.md**: Mapping of Rust standard library functions and types

## Language Characteristics

- **Paradigms**: Multi-paradigm (procedural, functional, with object-oriented features)
- **Typing**: Static, strong
- **Memory Management**: Ownership model with borrowing
- **Execution Model**: Compiled
- **Version Focus**: Rust 1.54+

## Usage Guidelines for LLM Agents

1. Use import-adapter.md when translating from Rust to other languages
2. Use export-adapter.md when translating from other languages to Rust
3. Consult idioms.md for Rust-specific patterns and their translations
4. Reference stdlib-mapping.md for standard library equivalents in other languages

The Rust adapter has particular strengths in handling Rust's ownership system, translating it to appropriate memory management patterns in other languages. Special attention is given to lifetime annotations, borrowing patterns, and ensuring memory safety when translating to languages with different memory models.