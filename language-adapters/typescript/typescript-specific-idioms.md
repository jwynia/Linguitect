# TypeScript-Specific Idioms

This document catalogs TypeScript-specific idioms and patterns, their Linguitect representations, and their translations to and from other languages. These idioms represent common coding patterns in TypeScript that may require special handling during translation.

## 1. Type System Idioms

### 1.1 Type Guards

Type guards are functions that perform runtime checks to narrow down the type of a value within a conditional block.

#### User-Defined Type Guards

```typescript
// TypeScript
function isString(value: any): value is string {
  return typeof value === 'string';
}

if (isString(value)) {
  // value is treated as string here
  console.log(value.toUpperCase());
}
```

```
// Linguitect
@TYPE_GUARD name=isString param=value target_type=@STRING condition=@EXPR_COMPARE type="==" operands=(@CALL typeof WITH (value), "string")

@IF @CALL isString WITH (value)
  @CALL_METHOD console.log WITH (@CALL_METHOD value.toUpperCase WITH ())
@ENDIF
```

#### Using `instanceof` for Type Guards

```typescript
// TypeScript
if (error instanceof Error) {
  console.log(error.message);
}
```

```
// Linguitect
@IF @EXPR_INSTANCEOF value=error type=Error
  @CALL_METHOD console.log WITH (@PROPERTY_GET error.message)
@ENDIF
```

### 1.2 Discriminated Unions

Discriminated unions use a common property (the discriminant) to differentiate between union members.

```typescript
// TypeScript
type Shape = 
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "rectangle":
      return shape.width * shape.height;
  }
}
```

```
// Linguitect
@TYPE_ALIAS Shape = @DISCRIMINATED_UNION
  discriminator="kind"
  variants=(
    @VARIANT name="circle" properties=(@PROPERTY radius @FLOAT),
    @VARIANT name="rectangle" properties=(@PROPERTY width @FLOAT, @PROPERTY height @FLOAT)
  )
@END

@FUNC area(@PARAM shape Shape) -> @FLOAT
  @BODY
    @PATTERN_MATCH shape.kind
      @PATTERN "circle"
        @RETURN @EXPR_ARITHMETIC type="*" operands=(
          @PROPERTY_GET Math.PI,
          @EXPR_ARITHMETIC type="**" operands=(
            @PROPERTY_GET shape.radius,
            2
          )
        )
      @PATTERN "rectangle"
        @RETURN @EXPR_ARITHMETIC type="*" operands=(
          @PROPERTY_GET shape.width,
          @PROPERTY_GET shape.height
        )
    @ENDPATTERN_MATCH
  @END
@ENDFUNC
```

### 1.3 Intersection Types

Intersection types combine multiple types into one.

```typescript
// TypeScript
interface Named {
  name: string;
}

interface Aged {
  age: number;
}

type Person = Named & Aged;

const person: Person = {
  name: "Alice",
  age: 30
};
```

```
// Linguitect
@INTERFACE Named
  @PROPERTY name @STRING
@ENDINTERFACE

@INTERFACE Aged
  @PROPERTY age @INT
@ENDINTERFACE

@TYPE_ALIAS Person = @INTERSECTION types=(Named, Aged)

@CONST person Person = @OBJECT {
  name: "Alice",
  age: 30
}
```

### 1.4 Mapped Types

Mapped types create new types by transforming properties of existing types.

```typescript
// TypeScript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

interface User {
  name: string;
  age: number;
}

const readonlyUser: Readonly<User> = {
  name: "Bob",
  age: 25
};
```

```
// Linguitect
@TYPE_ALIAS Readonly<T> = @MAPPED_TYPE
  source=T
  key_transform="identity"
  value_transform="@READONLY of=T[K]"
@END

@INTERFACE User
  @PROPERTY name @STRING
  @PROPERTY age @INT
@ENDINTERFACE

@CONST readonlyUser @APPLY_TYPE Readonly<User> = @OBJECT {
  name: "Bob",
  age: 25
}
```

## 2. Syntax Idioms

### 2.1 Optional Chaining

Optional chaining allows for safely accessing nested properties of an object that might be null or undefined.

```typescript
// TypeScript
const name = user?.profile?.name;
```

```
// Linguitect
@VAR name {inferred_type} = @OPTIONAL_ACCESS
  object=@OPTIONAL_ACCESS
    object=user
    property=profile
  property=name
```

### 2.2 Nullish Coalescing

Nullish coalescing provides a default value when a value is null or undefined.

```typescript
// TypeScript
const name = user?.name ?? "Anonymous";
```

```
// Linguitect
@VAR name {inferred_type} = @NULLISH_COALESCE
  primary=@OPTIONAL_ACCESS
    object=user
    property=name
  fallback="Anonymous"
```

### 2.3 Destructuring Assignment

Destructuring assignment extracts values from objects or arrays into individual variables.

```typescript
// TypeScript
const { name, age } = user;
const [first, second] = array;
```

```
// Linguitect
@DESTRUCTURE
  source=user
  pattern=@OBJECT_PATTERN
    properties=(name, age)
@END

@DESTRUCTURE
  source=array
  pattern=@ARRAY_PATTERN
    variables=(first, second)
@END
```

### 2.4 Parameter Destructuring

Parameter destructuring extracts properties from an object parameter directly in the function signature.

```typescript
// TypeScript
function greet({ name, age }: { name: string; age: number }) {
  console.log(`Hello, ${name}! You are ${age} years old.`);
}
```

```
// Linguitect
@FUNC greet(@PARAM_DESTRUCTURE @OBJECT { name: @STRING, age: @INT } properties=(name, age)) -> @VOID
  @BODY
    @CALL_METHOD console.log WITH (
      @EXPR_STRING_INTERPOLATE "Hello, ${name}! You are ${age} years old." vars=(name, age)
    )
  @END
@ENDFUNC
```

## 3. Functional Programming Idioms

### 3.1 Higher-Order Functions with Type Parameters

```typescript
// TypeScript
function map<T, U>(array: T[], fn: (item: T) => U): U[] {
  return array.map(fn);
}
```

```
// Linguitect
@FUNC map<T, U>(@PARAM array @ARRAY of=T, @PARAM fn @FUNCTION params=(@PARAM item T) return=U) -> @ARRAY of=U
  @BODY
    @RETURN @CALL_METHOD array.map WITH (fn)
  @END
@ENDFUNC
```

### 3.2 Function Composition

```typescript
// TypeScript
const compose = <T, U, V>(f: (x: U) => V, g: (x: T) => U) => (x: T): V => f(g(x));
```

```
// Linguitect
@CONST compose = @LAMBDA <T, U, V>(@PARAM f @FUNCTION params=(@PARAM x U) return=V, @PARAM g @FUNCTION params=(@PARAM x T) return=U) -> @FUNCTION params=(@PARAM x T) return=V
  @BODY
    @RETURN @LAMBDA (@PARAM x T) -> V
      @BODY
        @RETURN @CALL f WITH (@CALL g WITH (x))
      @END
    @ENDLAMBDA
  @END
@ENDLAMBDA
```

### 3.3 Immutable Data Patterns

```typescript
// TypeScript
interface User {
  readonly name: string;
  readonly age: number;
}

function updateAge(user: User, newAge: number): User {
  return { ...user, age: newAge };
}
```

```
// Linguitect
@INTERFACE User
  @PROPERTY name @STRING mutable=false
  @PROPERTY age @INT mutable=false
@ENDINTERFACE

@FUNC updateAge(@PARAM user User, @PARAM newAge @INT) -> User
  @BODY
    @RETURN @OBJECT_SPREAD base=user overrides=(@PROPERTY age newAge)
  @END
@ENDFUNC
```

## 4. Asynchronous Programming Idioms

### 4.1 Async/Await

```typescript
// TypeScript
async function fetchUserData(userId: string): Promise<User> {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error('Failed to fetch user data');
  }
  return await response.json();
}
```

```
// Linguitect
@ASYNC fetchUserData(@PARAM userId @STRING) -> User
  @BODY
    @VAR response {inferred_type} = @AWAIT @CALL fetch WITH (@EXPR_STRING_INTERPOLATE "/api/users/${userId}" vars=(userId))
    @IF @EXPR_LOGICAL type="NOT" operands=(@PROPERTY_GET response.ok)
      @THROW @NEW Error WITH ("Failed to fetch user data")
    @ENDIF
    @RETURN @AWAIT @CALL_METHOD response.json WITH ()
  @END
@ENDASYNC
```

### 4.2 Promise Chaining

```typescript
// TypeScript
function fetchUserData(userId: string): Promise<User> {
  return fetch(`/api/users/${userId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error('Failed to fetch user data');
      }
      return response.json();
    })
    .then(data => data as User);
}
```

```
// Linguitect
@FUNC fetchUserData(@PARAM userId @STRING) -> @PROMISE of=User
  @BODY
    @RETURN @PROMISE_CHAIN
      @PROMISE_START @CALL fetch WITH (@EXPR_STRING_INTERPOLATE "/api/users/${userId}" vars=(userId))
      @THEN @LAMBDA (@PARAM response {inferred_type}) -> {inferred_type}
        @BODY
          @IF @EXPR_LOGICAL type="NOT" operands=(@PROPERTY_GET response.ok)
            @THROW @NEW Error WITH ("Failed to fetch user data")
          @ENDIF
          @RETURN @CALL_METHOD response.json WITH ()
        @END
      @ENDLAMBDA
      @THEN @LAMBDA (@PARAM data {inferred_type}) -> User
        @BODY
          @RETURN @EXPR_CAST data User
        @END
      @ENDLAMBDA
    @END
  @END
@ENDFUNC
```

## 5. Object-Oriented Programming Idioms

### 5.1 Class with Private Fields

```typescript
// TypeScript
class Counter {
  #count = 0;
  
  increment(): void {
    this.#count++;
  }
  
  get value(): number {
    return this.#count;
  }
}
```

```
// Linguitect
@CLASS Counter
  @PROPERTY count @INT = 0 access=private
  
  @METHOD increment() -> @VOID
    @BODY
      @ASSIGN_OP @THIS.count "+" 1
    @END
  @ENDMETHOD
  
  @METHOD value() -> @INT
    @DECORATOR property
    @BODY
      @RETURN @THIS.count
    @END
  @ENDMETHOD
@ENDCLASS
```

### 5.2 Abstract Classes

```typescript
// TypeScript
abstract class Shape {
  abstract getArea(): number;
  
  getDescription(): string {
    return `A shape with area ${this.getArea()}`;
  }
}
```

```
// Linguitect
@ABSTRACT_CLASS Shape
  @ABSTRACT_METHOD getArea() -> @FLOAT
  
  @METHOD getDescription() -> @STRING
    @BODY
      @RETURN @EXPR_STRING_INTERPOLATE "A shape with area ${this.getArea()}" vars=(@CALL_METHOD @THIS.getArea WITH ())
    @END
  @ENDMETHOD
@ENDABSTRACT_CLASS
```

### 5.3 Method Overloading

```typescript
// TypeScript
class Calculator {
  add(a: number, b: number): number;
  add(a: string, b: string): string;
  add(a: any, b: any): any {
    if (typeof a === 'number' && typeof b === 'number') {
      return a + b;
    }
    return String(a) + String(b);
  }
}
```

```
// Linguitect
@CLASS Calculator
  @METHOD_OVERLOAD add
    @SIGNATURE (@PARAM a @FLOAT, @PARAM b @FLOAT) -> @FLOAT
    @SIGNATURE (@PARAM a @STRING, @PARAM b @STRING) -> @STRING
    @IMPLEMENTATION (@PARAM a @ANY, @PARAM b @ANY) -> @ANY
      @BODY
        @IF @EXPR_LOGICAL type="AND" operands=(
          @EXPR_COMPARE type="==" operands=(@CALL typeof WITH (a), "number"),
          @EXPR_COMPARE type="==" operands=(@CALL typeof WITH (b), "number")
        )
          @RETURN @EXPR_ARITHMETIC type="+" operands=(a, b)
        @ENDIF
        @RETURN @EXPR_ARITHMETIC type="+" operands=(@CALL String WITH (a), @CALL String WITH (b))
      @END
    @END
  @END
@ENDCLASS
```

## 6. Translation Considerations

When translating TypeScript idioms to other languages, consider the following:

1. **Type System Differences**: Languages with nominal type systems (like Java or C#) will need different approaches for structural typing features.

2. **Optional Chaining and Nullish Coalescing**: These may need to be translated to explicit null checks in languages that don't support these operators.

3. **Discriminated Unions**: These may translate to class hierarchies or pattern matching in other languages.

4. **Mapped Types**: These may require code generation or reflection in languages without similar metaprogramming capabilities.

5. **Private Fields**: The privacy model varies across languages, so `#privateField` syntax may translate to different access modifiers.

6. **Async/Await**: This may translate to different concurrency models in other languages (e.g., coroutines, futures, etc.).

When translating from other languages to TypeScript, leverage TypeScript's rich type system to enhance type safety and expressiveness.