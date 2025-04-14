# 🦀 Rust-Specific Emoji Linguitect Extension

## 1. Introduction

This document defines the official Rust extension for the Emoji Linguitect Specification (ELS). It provides emoji representations for Rust's unique language features while maintaining compatibility with the core ELS.

### 1.1 Extension Principles

This extension follows these principles:
- Uses the Rust language identifier 🦀 to mark Rust-specific contexts
- Reuses core symbols where semantics align
- Clearly documents contextual overrides
- Registers all new symbols to avoid conflicts

### 1.2 Symbol Registration Status

All symbols defined in this extension have been registered in the ELS Extension Registry and have been checked for conflicts with the core specification.

## 2. Rust-Specific Type System Features

### 2.1 Ownership and Borrowing

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🔏v` | `@OWNERSHIP type="own" var` | Owned value |
| `🦀👁️v` | `@OWNERSHIP type="ref" var` | Immutable reference |
| `🦀✏️v` | `@OWNERSHIP type="mut_ref" var` | Mutable reference |
| `🦀📖v` | `@OWNERSHIP type="deref" var` | Dereference operation |

*Note: 👁️ is used with a different meaning in core ELS (startsWith). The 🦀 prefix disambiguates the Rust-specific usage.*

### 2.2 Lifetimes

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀⏳'a=🔗(p1,p2)` | `@LIFETIME 'a = shared_between(param1, param2)` | Lifetime connecting parameters |
| `🦀⏳'a=🏷️f` | `@LIFETIME 'a = applies_to(field)` | Lifetime applies to field |
| `🦀⏳'a=🏆` | `@LIFETIME 'a = applies_to_class` | Lifetime applies to class |
| `🦀⏳∞` | `@LIFETIME 'static` | Static lifetime |

### 2.3 Result and Option Types

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀✅v` | `@RESULT @SUCCESS value @ENDRESULT` | Result success/Ok value |
| `🦀❌e` | `@RESULT @ERROR error @ENDRESULT` | Result error/Err value |
| `🦀💫v` | `@OPTIONAL @VALUE value @ENDOPTIONAL` | Some variant of Option |
| `🦀💫❓` | `@OPTIONAL @NONE @ENDOPTIONAL` | None variant of Option |
| `🦀❓e` | `@EXPR_ERROR_PROPAGATION expr` | Error propagation (? operator) |

*Note: ✅ has a different meaning in core ELS (boolean type or postcondition). The 🦀 prefix indicates the Rust-specific meaning.*

## 3. Rust Pattern Matching Extensions

### 3.1 Match Expressions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🧩e{👓p1{b1}👓p2{b2}💯{b3}}` | `@PATTERN_MATCH expr @PATTERN p1 body1 @PATTERN p2 body2 @DEFAULT body3 @ENDPATTERN_MATCH` | Match expression |
| `🦀👓✅(x){b}` | `@PATTERN Ok(x) body @ENDPATTERN` | Match on Result::Ok |
| `🦀👓❌(e){b}` | `@PATTERN Err(e) body @ENDPATTERN` | Match on Result::Err |
| `🦀👓💫(v){b}` | `@PATTERN Some(v) body @ENDPATTERN` | Match on Option::Some |
| `🦀👓💫❓{b}` | `@PATTERN None body @ENDPATTERN` | Match on Option::None |

### 3.2 If Let Expressions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀👓✅(x)=e{b}` | `@MATCH Ok(x) = expr body @ENDMATCH` | If let Ok(x) = expr |
| `🦀👓💫(v)=e{b}` | `@MATCH Some(v) = expr body @ENDMATCH` | If let Some(v) = expr |

## 4. Rust-Specific Traits and Implementations

### 4.1 Trait and Implementation Extensions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🎭n:b{...}` | `@INTERFACE name EXTENDS bounds ... @ENDINTERFACE` | Trait with bounds |
| `🦀🏛️n🔄🎭{...}` | `@CLASS name IMPLEMENTS trait ... @ENDCLASS` | Struct implementing trait |
| `🦀🎲t=c` | `@ASSOCIATED_TYPE type = concrete_type` | Associated type definition |

### 4.2 Where Clauses and Bounds

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🔍T:b` | `@GENERIC T extends bounds` | Generic type with bounds |
| `🦀🔍T↪️{c}` | `@GENERIC T where constraints` | Where clause constraints |

## 5. Rust Error Handling Extensions

### 5.1 Try Operator and Error Types

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🏆v⚠️e` | `@RESULT_TYPE ok=ok_type error=error_type` | Result<T, E> type |
| `🦀💫t` | `@OPTIONAL of=base_type` | Option<T> type |

### 5.2 From Implementation

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🏛️t🔄From<s>{🔑f(v:s)→t{b}}` | `@CLASS target IMPLEMENTS From<source> @METHOD from(var: source) -> target body @ENDMETHOD @ENDCLASS` | From trait implementation |

## 6. Unsafe Rust Extensions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀⚠️{...}` | `@UNSAFE body @ENDUNSAFE` | Unsafe block |
| `🦀⚠️🔧n(p)→r{b}` | `@FUNC name(params) -> return_type @DECORATOR unsafe body @END @ENDFUNC` | Unsafe function |

## 7. Rust Module System Extensions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀📂n{...}` | `@MODULE name module_content @ENDMODULE` | Private module |
| `🦀📂+n{...}` | `@MODULE name @NOTE "This module is public" module_content @ENDMODULE` | Public module |
| `🦀🏺n{...}` | `@CRATE name ... @ENDCRATE` | Rust crate |
| `🦀📤...` | `@EXPORT items` | Re-export items (pub use) |

## 8. Rust-Specific Macro Extensions

| ELS Syntax | Standard Linguitect | Description |
|------------|---------------------|-------------|
| `🦀🧠m!(a)` | `@EXPAND macro WITH (args)` | Macro invocation |
| `🦀🧠📜!n{p1⇒{e1},p2⇒{e2}}` | `@MACRO name(pattern1) @EXPANSION expansion1 @END @ENDMACRO @MACRO name(pattern2) @EXPANSION expansion2 @END @ENDMACRO` | Declarative macro definition |
| `🦀🏅d(s)` | `@DECORATOR derive(traits)` | Derive attribute |
| `🦀🏅c(s)` | `@CONDITIONAL cfg=condition` | #[cfg] attribute |

## 9. Example Translation

### 9.1 Simple Rust Function with Result

**Rust Code:**
```rust
fn divide(a: i32, b: i32) -> Result<f64, String> {
    if b == 0 {
        return Err("Division by zero".to_string());
    }
    Ok(a as f64 / b as f64)
}
```

**Standard Linguitect:**
```
@FUNC divide(@PARAM a @INT, @PARAM b @INT) -> @RESULT_TYPE ok=@FLOAT error=@STRING
  @BODY
    @IF @EXPR_COMPARE type="==" operands=(b, 0)
      @RETURN @RESULT
        @ERROR @CALL_METHOD "Division by zero".to_string WITH ()
      @ENDRESULT
    @ENDIF
    @RETURN @RESULT
      @SUCCESS @EXPR_ARITHMETIC type="/" operands=(
        @EXPR_CAST a @FLOAT,
        @EXPR_CAST b @FLOAT
      )
    @ENDRESULT
  @END
@ENDFUNC
```

**Emoji Linguitect with Rust Extensions:**
```
🦀🔧divide(a:🔢,b:🔢)→🏆💲⚠️📝{
  ❔⚖️(b,0){
    🔙🦀❌(📝"Division by zero".📞to_string())
  }
  🔙🦀✅(➗(🧬(a,💲),🧬(b,💲)))
}
```

### 9.2 Struct with Lifetime and Implementation

**Rust Code:**
```rust
struct Borrowed<'a> {
    x: &'a i32,
}

impl<'a> Borrowed<'a> {
    fn new(x: &'a i32) -> Self {
        Borrowed { x }
    }
    
    fn get(&self) -> &i32 {
        self.x
    }
}
```

**Emoji Linguitect with Rust Extensions:**
```
🦀🧱Borrowed{
  x:🦀👁️🔢,
  🦀⏳'a=🏷️x
}

🦀🏛️Borrowed{
  🦀⏳'a=🏆
  
  🔑new(x:🦀👁️🔢)→Borrowed{
    📦{
      🆕Borrowed{x:x}
    }
  }
  
  🔑get()→🦀👁️🔢{
    📦{
      🔙📝g👇.x
    }
  }
}
```

## 10. Implementation Notes

### 10.1 Context-Sensitive Parsing

When implementing a parser for this extension:

1. First identify language context (look for 🦀 prefix)
2. Apply Rust-specific symbol interpretations in Rust contexts
3. Fall back to core ELS interpretations elsewhere
4. Handle ambiguous cases by inspecting surrounding syntax

### 10.2 Symbol Conflict Resolution

The following core ELS symbols have contextual overrides in Rust contexts:

| Symbol | Core ELS Meaning | Rust Context Meaning |
|--------|-----------------|---------------------|
| `👁️` | startsWith | immutable reference |
| `✅` | boolean type/postcondition | Result::Ok |
| `❓` | undefined type | try operator or None check |

These contextual overrides are resolved by looking for the 🦀 prefix or examining the surrounding syntax pattern.

## 11. Compatibility with Future Extensions

This extension has been designed to:

1. Minimize conflicts with other language extensions
2. Use the language prefix for disambiguation
3. Follow the ELS extension guidelines
4. Register all symbols in the central registry

Future extensions to this specification should follow the same principles to maintain compatibility with the core ELS and other language extensions.