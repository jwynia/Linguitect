# Update Protocols

## Purpose Statement
This document defines procedures for maintaining and updating the Linguitect context network, ensuring consistency, quality, and proper documentation of changes.

## Information Classification
- **Domain:** Meta-Documentation
- **Stability:** High
- **Abstraction:** Procedural
- **Confidence:** Established
- **Relevance:** All contributors to the context network, especially LLM agents

## Core Content

### Update Principles

All updates to the context network should adhere to these principles:

1. **Consistency**: Maintain consistent structure, terminology, and relationships
2. **Traceability**: Document the rationale and impact of changes
3. **Minimalism**: Make the smallest change necessary to achieve the goal
4. **Relationship Preservation**: Update all affected relationships
5. **Backward Compatibility**: Avoid breaking existing navigation paths when possible

### Update Types

#### 1. Content Updates

Changes to the information within existing documents:

- **Minor Content Updates**: Clarifications, corrections, or small additions
- **Major Content Updates**: Significant revisions or expansions
- **Restructuring**: Reorganization of content within a document

#### 2. Relationship Updates

Changes to the relationships between documents:

- **Relationship Addition**: Adding new connections between documents
- **Relationship Modification**: Changing the nature of existing connections
- **Relationship Removal**: Removing obsolete connections

#### 3. Structural Updates

Changes to the structure of the context network:

- **Document Addition**: Creating new documents
- **Document Relocation**: Moving documents to different locations
- **Document Removal**: Removing obsolete documents
- **Directory Restructuring**: Reorganizing the directory structure

### Update Protocols by Type

#### Minor Content Update Protocol

For small corrections, clarifications, or additions:

1. **Assess Impact**:
   - Determine if the change affects only the document itself
   - Check if it impacts any semantic understanding

2. **Make Changes**:
   - Update the content while maintaining the document structure
   - Preserve existing headings and organization
   - Ensure terminology consistency

3. **Update Metadata**:
   - Revise the "Information Classification" if necessary
   - Update "Navigation Guidance" if the change affects usage

4. **Document Change**:
   - Add an entry to the document's change log (if present)
   - No need to update the evolution-log.md for minor changes

#### Major Content Update Protocol

For significant revisions or expansions:

1. **Assess Impact**:
   - Identify all documents that reference this content
   - Determine if the change affects semantic understanding
   - Evaluate impact on navigation paths

2. **Plan Changes**:
   - Create a change plan documenting the updates
   - Identify all affected documents
   - Determine the sequence of updates

3. **Make Changes**:
   - Update the primary document
   - Revise all affected documents for consistency
   - Ensure terminology and concept alignment

4. **Update Metadata**:
   - Revise "Information Classification" as needed
   - Update "Relationship Network" to reflect any changes
   - Revise "Navigation Guidance" to account for new content

5. **Document Changes**:
   - Add an entry to the document's change log
   - Update evolution-log.md with a summary of the changes
   - Document rationale and impact

#### Document Addition Protocol

For creating new documents:

1. **Assess Need**:
   - Determine if the information fits within existing documents
   - Verify that the new document serves a distinct purpose
   - Identify where it fits in the directory structure

2. **Create Document**:
   - Use the appropriate template from metadata-schema.md
   - Follow the standard document structure
   - Include all required metadata

3. **Establish Relationships**:
   - Identify prerequisite, related, and dependent documents
   - Add the new document to the relationship networks of related documents
   - Ensure bidirectional relationships are consistent

4. **Update Navigation**:
   - Add the document to relevant navigation paths
   - Update README files if necessary
   - Ensure the document is discoverable

5. **Document Addition**:
   - Update evolution-log.md with the new document
   - Document rationale and intended use

#### Document Removal Protocol

For removing obsolete documents:

1. **Assess Impact**:
   - Identify all documents that reference this document
   - Determine if the information should be preserved elsewhere
   - Evaluate impact on navigation paths

2. **Preserve Information**:
   - If valuable, migrate content to other documents
   - Update references to point to new locations
   - Ensure no critical information is lost

3. **Update Relationships**:
   - Remove references from other documents' relationship networks
   - Redirect navigation paths to alternative documents
   - Update README files if necessary

4. **Document Removal**:
   - Update evolution-log.md with the removal
   - Document rationale and alternative sources for the information

#### Directory Restructuring Protocol

For reorganizing the directory structure:

1. **Assess Impact**:
   - Identify all affected documents
   - Evaluate impact on navigation paths
   - Determine if the restructuring improves organization

2. **Plan Restructuring**:
   - Create a detailed plan documenting the new structure
   - Map old locations to new locations
   - Identify all references that need updating

3. **Implement Changes**:
   - Move documents to new locations
   - Update all internal references to use new paths
   - Update README files to reflect the new structure

4. **Update Navigation**:
   - Revise navigation paths in all affected documents
   - Update relationship networks to use new paths
   - Ensure all documents remain discoverable

5. **Document Restructuring**:
   - Update evolution-log.md with the restructuring
   - Document rationale and benefits
   - Provide a mapping from old to new locations

### Special Update Scenarios

#### Language Addition

When adding a new programming language to the Linguitect ecosystem:

1. **Create Language Adapter Documentation**:
   - Follow the adapter-template structure
   - Document import and export rules
   - Catalog idioms and standard library mappings

2. **Update Semantic Constructs**:
   - Add language-specific implementations to relevant constructs
   - Document any unique features or limitations

3. **Create Translation Maps**:
   - Add language-specific entries to pattern maps
   - Create paradigm bridging guidance if needed
   - Document domain-specific considerations

4. **Update Navigation**:
   - Add the language to relevant navigation paths
   - Update README files to include the new language
   - Ensure the language is discoverable

5. **Document Addition**:
   - Update evolution-log.md with the new language
   - Document rationale and capabilities

#### Paradigm Addition

When adding support for a new programming paradigm:

1. **Update Foundation**:
   - Add the paradigm to relevant foundation documents
   - Document the paradigm's core concepts and principles

2. **Create Semantic Constructs**:
   - Add a new directory for paradigm-specific constructs
   - Document the constructs' semantic intent and implementation

3. **Update Translation Maps**:
   - Create paradigm bridging maps for the new paradigm
   - Update existing maps to include the new paradigm

4. **Update Language Adapters**:
   - Add paradigm support to relevant language adapters
   - Document paradigm-specific idioms and patterns

5. **Document Addition**:
   - Update evolution-log.md with the new paradigm
   - Document rationale and implications

### Consistency Verification

After making updates, verify consistency using these checks:

1. **Structural Consistency**:
   - All documents follow the standard structure
   - All required metadata is present
   - File naming conventions are followed

2. **Relationship Consistency**:
   - All relationships are bidirectional
   - All referenced documents exist
   - Navigation paths are valid

3. **Terminology Consistency**:
   - Terms are used consistently across documents
   - New terms are added to terminology.md if necessary
   - Abbreviations are defined on first use

4. **Navigation Consistency**:
   - All documents are reachable through navigation paths
   - README files accurately reflect directory contents
   - Common tasks have clear navigation guidance

## Relationship Network
- **Prerequisite Information:** [../foundation/metadata-schema.md](../foundation/metadata-schema.md)
- **Related Information:** [network-guide.md](./network-guide.md), [evolution-log.md](./evolution-log.md)
- **Dependent Information:** All documents in the context network
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When making changes to the context network
- **Common Next Steps:** Apply these protocols to make specific updates, or consult [evolution-log.md](./evolution-log.md) to understand previous changes
- **Related Tasks:** Documentation maintenance, content updates, structural changes
- **Update Patterns:** Updates when new update scenarios are identified or existing protocols need refinement