# Construct Translation Protocol

## Purpose Statement
This document provides a structured protocol for translating specific programming constructs between languages using the Linguitect intermediate representation, ensuring semantic equivalence and idiomatic output.

## Information Classification
- **Domain:** Translation Process
- **Stability:** High
- **Abstraction:** Procedural
- **Confidence:** Established
- **Relevance:** All translation tasks

## Protocol Steps

### 1. Identify the Construct

1. **Analyze the source code** to identify the specific programming construct
   - Determine the construct's type (variable declaration, function, loop, etc.)
   - Identify any language-specific features or syntax
   - Note any comments or documentation that explain intent

2. **Determine the semantic intent**
   - What is the construct trying to accomplish?
   - What are the expected behaviors and side effects?
   - What are the implicit assumptions or guarantees?

3. **Identify related constructs**
   - Are there dependent or supporting constructs?
   - Is this part of a larger pattern or idiom?
   - Are there language-specific optimizations in use?

### 2. Map to Linguitect IR

1. **Locate the appropriate semantic construct documentation**
   - Find the matching construct in the semantic-constructs directory
   - Review the Linguitect representation and classification

2. **Apply the import adapter rules**
   - Consult the source language's import adapter documentation
   - Follow the syntax mapping rules for the specific construct
   - Apply any language-specific transformations

3. **Enhance with semantic information**
   - Add type information (inferred or explicit)
   - Preserve comments and documentation
   - Include any relevant annotations or hints

### 3. Transform to Target Language

1. **Apply the export adapter rules**
   - Consult the target language's export adapter documentation
   - Follow the code generation rules for the specific construct
   - Apply any language-specific optimizations

2. **Adapt to target language idioms**
   - Consult the target language's idioms documentation
   - Replace with idiomatic equivalents where appropriate
   - Follow target language conventions and best practices

3. **Handle language-specific constraints**
   - Address type system differences
   - Manage memory model differences
   - Handle error propagation differences

### 4. Verify and Refine

1. **Check semantic equivalence**
   - Ensure the translated construct preserves the original intent
   - Verify that all behaviors and side effects are maintained
   - Confirm that all edge cases are handled appropriately

2. **Evaluate idiomaticity**
   - Does the translation feel natural in the target language?
   - Does it follow target language conventions?
   - Would a native developer recognize and understand it?

3. **Optimize if necessary**
   - Consider performance implications
   - Apply target language-specific optimizations
   - Balance readability and performance

## Decision Points

### Type Handling Decision

When translating between languages with different type systems:

- **If source is dynamically typed and target is statically typed**:
  - Use type inference to determine the most likely types
  - Add explicit type annotations in the target code
  - Consider using more general types when inference is uncertain

- **If source is statically typed and target is dynamically typed**:
  - Preserve type information in comments if valuable
  - Remove unnecessary type constraints
  - Consider adding runtime type checks for critical operations

- **If both are statically typed but with different type systems**:
  - Map to the closest equivalent types
  - Add type conversions where necessary
  - Document any semantic differences in the type guarantees

### Memory Management Decision

When translating between languages with different memory management approaches:

- **If source uses garbage collection and target uses manual management**:
  - Identify object lifetimes and ownership patterns
  - Add explicit deallocation at appropriate points
  - Consider using smart pointers or RAII patterns if available

- **If source uses manual management and target uses garbage collection**:
  - Remove explicit deallocation
  - Preserve any resource cleanup in appropriate finalization methods
  - Be aware of non-memory resources that still need explicit cleanup

- **If source uses ownership model (like Rust) and target doesn't**:
  - Translate ownership semantics to appropriate patterns
  - Add explicit copying where move semantics aren't available
  - Consider reference counting for complex ownership scenarios

### Error Handling Decision

When translating between languages with different error handling mechanisms:

- **If source uses exceptions and target uses return codes**:
  - Transform try-catch blocks to if-error-return patterns
  - Propagate errors up the call stack explicitly
  - Preserve error information in return structures

- **If source uses return codes and target uses exceptions**:
  - Transform error checks to try-catch blocks
  - Create appropriate exception types for different error codes
  - Ensure exceptions include equivalent error information

- **If source uses Result/Either types and target doesn't**:
  - Transform to the target language's preferred error handling mechanism
  - Preserve the success/failure distinction
  - Maintain error context and information

## Examples

### Example: Translating a Python For Loop to Rust

**Source (Python):**
```python
for item in items:
    if item > 10:
        process(item)
```

**Step 1: Identify the Construct**
- This is a for-each loop iterating over a collection
- It includes a conditional that filters items
- The semantic intent is to process only items greater than 10

**Step 2: Map to Linguitect IR**
```
@LOOP type="for" var=item in=items
  @IF item > 10
    @CALL process WITH (item)
  @ENDIF
@ENDLOOP
```

**Step 3: Transform to Target Language**
```rust
for item in items {
    if item > 10 {
        process(item);
    }
}
```

**Step 4: Verify and Refine**
- The semantic equivalence is preserved
- The code is idiomatic Rust
- No further optimization is needed for this simple case

### Example: Translating a Java Class to Python

**Source (Java):**
```java
public class Counter {
    private int count = 0;
    
    public void increment() {
        count++;
    }
    
    public int getCount() {
        return count;
    }
}
```

**Step 1: Identify the Construct**
- This is a class definition with a private field and public methods
- It implements a simple counter with encapsulation
- The semantic intent is to provide controlled access to a counter value

**Step 2: Map to Linguitect IR**
```
@CLASS Counter
  @PROPERTY count int access=private = 0
  
  @METHOD increment() -> void access=public
    @BODY
      @ASSIGN this.count <- this.count + 1
    @END
  @ENDMETHOD
  
  @METHOD getCount() -> int access=public
    @BODY
      @RETURN this.count
    @END
  @ENDMETHOD
@ENDCLASS
```

**Step 3: Transform to Target Language**
```python
class Counter:
    def __init__(self):
        self._count = 0
    
    def increment(self):
        self._count += 1
    
    def get_count(self):
        return self._count
```

**Step 4: Verify and Refine**
- The semantic equivalence is preserved
- The code follows Python conventions (underscore for private, snake_case for methods)
- The encapsulation is maintained through convention rather than enforcement

## Relationship Network
- **Prerequisite Information:** [../semantic-constructs/README.md](../semantic-constructs/README.md), [../language-adapters/README.md](../language-adapters/README.md)
- **Related Information:** [idiom-translation.md](./idiom-translation.md), [paradigm-bridging.md](./paradigm-bridging.md)
- **Dependent Information:** None
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When translating specific programming constructs between languages
- **Common Next Steps:** Consult [idiom-translation.md](./idiom-translation.md) for idiomatic patterns, or [performance-optimization.md](./performance-optimization.md) for optimizing the translation
- **Related Tasks:** Code translation, language adapter development
- **Update Patterns:** Updates when new construct translation strategies are developed or existing ones are refined