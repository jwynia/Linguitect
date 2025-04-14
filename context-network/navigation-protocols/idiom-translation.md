# Idiom Translation Protocol

## Purpose Statement
This document provides a structured protocol for translating idiomatic code patterns between programming languages using the Linguitect intermediate representation, ensuring that code not only works correctly but feels natural in the target language.

## Information Classification
- **Domain:** Translation Process
- **Stability:** High
- **Abstraction:** Procedural
- **Confidence:** Established
- **Relevance:** All translation tasks involving idiomatic code

## Protocol Steps

### 1. Identify the Idiom

1. **Recognize idiomatic patterns** in the source code
   - Look for language-specific patterns that may not translate literally
   - Identify constructs that have a conventional usage in the source language
   - Note patterns that solve common problems in language-specific ways

2. **Determine the semantic intent**
   - What problem is the idiom solving?
   - What are the performance characteristics?
   - What are the implicit guarantees or assumptions?

3. **Consult idiom catalogs**
   - Check the source language's idioms documentation
   - Verify that the pattern matches a known idiom
   - Understand the canonical representation in Linguitect

### 2. Map to Linguitect IR

1. **Apply idiom recognition rules**
   - Consult the source language's import adapter documentation
   - Use the idiom recognition patterns to map to Linguitect
   - Preserve the semantic intent rather than the literal syntax

2. **Use canonical representation**
   - Map to the standard Linguitect representation for this concept
   - Include intent annotations to clarify the purpose
   - Add performance hints if relevant

3. **Preserve context**
   - Maintain relationships with surrounding code
   - Preserve any comments explaining the idiom
   - Note any language-specific optimizations

### 3. Transform to Target Language

1. **Identify target language idioms**
   - Consult the target language's idioms documentation
   - Find the idiomatic way to express the same concept
   - Consider multiple alternatives if available

2. **Select the most appropriate equivalent**
   - Consider the specific context and requirements
   - Evaluate performance characteristics
   - Assess readability and maintainability
   - Choose the most natural expression in the target language

3. **Apply idiomatic transformation**
   - Use the target language's export adapter
   - Apply the idiom implementation patterns
   - Ensure the code follows target language conventions

### 4. Verify and Refine

1. **Check semantic equivalence**
   - Ensure the translated idiom preserves the original intent
   - Verify that all behaviors and edge cases are handled
   - Confirm that performance characteristics are maintained or improved

2. **Evaluate idiomaticity**
   - Would a native developer of the target language recognize this as idiomatic?
   - Does it follow the target language's conventions and best practices?
   - Is it the most natural way to express this concept?

3. **Add explanatory comments if needed**
   - For complex idioms, consider adding comments explaining the intent
   - Document any subtle differences from the original
   - Note any performance implications

## Decision Points

### Idiom Selection Decision

When multiple idiomatic options exist in the target language:

- **If performance is critical**:
  - Choose the most efficient idiom
  - Consider adding comments explaining the performance benefits
  - Ensure the performance advantage is meaningful in context

- **If readability is prioritized**:
  - Choose the most widely recognized idiom
  - Prefer explicit over implicit patterns
  - Consider the familiarity level of the target audience

- **If maintainability is key**:
  - Choose idioms that are stable across language versions
  - Avoid cutting-edge features that may change
  - Prefer simpler idioms over complex ones

### Cultural Adaptation Decision

When idioms reflect programming culture differences:

- **If the source idiom has no direct equivalent**:
  - Decompose into the underlying intent
  - Reconstruct using target language patterns
  - Consider adding comments explaining the original approach

- **If the idiom involves language-specific features**:
  - Find the closest conceptual equivalent
  - Adapt to use available features in the target language
  - Preserve the core functionality and intent

- **If the idiom relates to ecosystem conventions**:
  - Adapt to the target ecosystem's conventions
  - Consider how the code will interact with libraries
  - Follow established patterns for the target environment

### Optimization Level Decision

When translating performance-oriented idioms:

- **If the target language has different performance characteristics**:
  - Reconsider the optimization strategy
  - Choose idioms that optimize for the target language's strengths
  - Be willing to use completely different approaches

- **If the optimization is no longer relevant**:
  - Simplify to a more readable version
  - Document the change in approach
  - Focus on clarity over premature optimization

- **If the optimization is critical**:
  - Research target language performance patterns
  - Consider multiple implementation options
  - Benchmark if necessary to select the best approach

## Examples

### Example: Translating Python List Comprehension to Java

**Source (Python):**
```python
squared_evens = [x*x for x in numbers if x % 2 == 0]
```

**Step 1: Identify the Idiom**
- This is a list comprehension, a Python idiom for creating lists
- It filters for even numbers and squares them
- It's concise and expressive, combining filtering and mapping

**Step 2: Map to Linguitect IR**
```
@COMPREHENSION
  @OUTPUT x*x
  @FROM x in numbers
  @WHERE x % 2 == 0
@ENDCOMPREHENSION
```

**Step 3: Transform to Target Language**
```java
List<Integer> squaredEvens = numbers.stream()
    .filter(x -> x % 2 == 0)
    .map(x -> x * x)
    .collect(Collectors.toList());
```

**Step 4: Verify and Refine**
- The semantic equivalence is preserved
- The code uses Java streams, which is the idiomatic approach for this operation
- It follows Java naming conventions (camelCase)

### Example: Translating JavaScript Promise Chain to Python Async/Await

**Source (JavaScript):**
```javascript
fetchData()
  .then(data => processData(data))
  .then(result => saveResult(result))
  .catch(error => handleError(error));
```

**Step 1: Identify the Idiom**
- This is a Promise chain, a JavaScript idiom for handling asynchronous operations
- It represents a sequence of operations that depend on the previous step
- It includes error handling for the entire chain

**Step 2: Map to Linguitect IR**
```
@ASYNC_SEQUENCE
  @STEP fetchData() -> data
  @STEP processData(data) -> result
  @STEP saveResult(result) -> final_result
  @ERROR handleError(error)
@END_ASYNC_SEQUENCE
```

**Step 3: Transform to Target Language**
```python
async def perform_operation():
    try:
        data = await fetch_data()
        result = await process_data(data)
        final_result = await save_result(result)
        return final_result
    except Exception as error:
        handle_error(error)
```

**Step 4: Verify and Refine**
- The semantic equivalence is preserved
- The code uses Python's async/await, which is the idiomatic approach for this pattern
- It follows Python naming conventions (snake_case)
- The structure is adapted to Python's approach to async programming

## Relationship Network
- **Prerequisite Information:** [../language-adapters/README.md](../language-adapters/README.md), [../semantic-constructs/README.md](../semantic-constructs/README.md)
- **Related Information:** [construct-translation.md](./construct-translation.md), [paradigm-bridging.md](./paradigm-bridging.md)
- **Dependent Information:** None
- **Alternative Perspectives:** None
- **Implementation Details:** None

## Navigation Guidance
- **Access Context:** When translating idiomatic code patterns between languages
- **Common Next Steps:** Consult [performance-optimization.md](./performance-optimization.md) for optimizing the translation, or language-specific idiom documentation
- **Related Tasks:** Code translation, language adapter development, idiom catalog creation
- **Update Patterns:** Updates when new idioms are identified or existing translations are refined