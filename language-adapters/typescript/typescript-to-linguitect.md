# TypeScript to Linguitect Adapter Specification

## 1. Adapter Metadata

```
@LANGUAGE_ADAPTER "TypeScript_to_Linguitect"
  @VERSION "1.0"
  @LANGUAGE_VERSION "TypeScript 5.x+"
  @TYPE "import"
  
  @METADATA
    @PARADIGMS ["object_oriented", "functional", "procedural"]
    @TYPING type="static" strength="strong"
    @MEMORY_MANAGEMENT type="gc"
    @EXECUTION_MODEL "transpiled"
    @STANDARD_LIBRARY_VERSION "TypeScript Standard Library 5.x"
  @END
```

## 2. Syntax Mapping Rules

### 2.1 Module Structure

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    // filename: {module_name}.ts
    {module_content}
  @END
  
  @TARGET
    @PROGRAM {module_name}
      @MODULE {module_name}
        {transformed_module_content}
      @ENDMODULE
    @END
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    namespace {namespace_name} {
      {namespace_content}
    }
  @END
  
  @TARGET
    @MODULE {namespace_name}
      {transformed_namespace_content}
    @ENDMODULE
  @END
@END
```

### 2.2 Imports and Exports

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    import {module} from '{path}';
  @END
  
  @TARGET
    @IMPORT {module} FROM {path}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    export {name1}, {name2}, ...;
  @END
  
  @TARGET
    @EXPORT {name1}, {name2}, ...
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    export default {expression};
  @END
  
  @TARGET
    @EXPORT_DEFAULT {transformed_expression}
  @END
@END
```

### 2.3 Variable Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    let {var}: {type} = {value};
  @END
  
  @TARGET
    @VAR {var} {transform_type_annotation(type)} = {transformed_value}
    @NOTE "mutable=true"
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    const {var}: {type} = {value};
  @END
  
  @TARGET
    @CONST {var} {transform_type_annotation(type)} = {transformed_value}
  @END
@END
```

### 2.4 Function Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    function {name}({params}): {return_type} {
      {body}
    }
  @END
  
  @TARGET
    @FUNC {name}({transform_params(params)}) -> {transform_type_annotation(return_type)}
      @BODY
        {transformed_body}
      @END
    @ENDFUNC
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    async function {name}({params}): Promise<{return_type}> {
      {body}
    }
  @END
  
  @TARGET
    @ASYNC {name}({transform_params(params)}) -> {transform_type_annotation(return_type)}
      @BODY
        {transformed_body}
      @END
    @ENDASYNC
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    ({params}): {return_type} => {expression}
  @END
  
  @TARGET
    @LAMBDA ({transform_params(params)}) -> {transform_type_annotation(return_type)}
      @BODY
        @RETURN {transformed_expression}
      @END
    @ENDLAMBDA
  @END
@END
```

### 2.5 Class Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    class {name} extends {parent_class} implements {interfaces} {
      {body}
    }
  @END
  
  @TARGET
    @CLASS {name} EXTENDS {parent_class} IMPLEMENTS {interfaces}
      {transformed_body}
    @ENDCLASS
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    constructor({params}) {
      {body}
    }
  @END
  
  @TARGET
    @CONSTRUCTOR({transform_params(params)})
      @BODY
        {transformed_body}
      @END
    @ENDCONSTRUCTOR
  @END
  
  @CONTEXT
    @REQUIRES in_class_context
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {access_modifier} {method_name}({params}): {return_type} {
      {body}
    }
  @END
  
  @TARGET
    @METHOD {method_name}({transform_params(params)}) -> {transform_type_annotation(return_type)} access={access_modifier}
      @BODY
        {transformed_body}
      @END
    @ENDMETHOD
  @END
  
  @CONTEXT
    @REQUIRES in_class_context
  @END
@END
```

### 2.6 Interface Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    interface {name} extends {parent_interfaces} {
      {body}
    }
  @END
  
  @TARGET
    @INTERFACE {name} EXTENDS {parent_interfaces}
      {transformed_body}
    @ENDINTERFACE
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {property}: {type};
  @END
  
  @TARGET
    @PROPERTY {property} {transform_type_annotation(type)}
  @END
  
  @CONTEXT
    @REQUIRES in_interface_context
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {method_name}({params}): {return_type};
  @END
  
  @TARGET
    @METHOD_SIGNATURE {method_name}({transform_params(params)}) -> {transform_type_annotation(return_type)}
  @END
  
  @CONTEXT
    @REQUIRES in_interface_context
  @END
@END
```

### 2.7 Type Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    type {name} = {type};
  @END
  
  @TARGET
    @TYPE_ALIAS {name} = {transform_type_annotation(type)}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    type {name}<{type_params}> = {type};
  @END
  
  @TARGET
    @GENERIC {name}<{transform_type_params(type_params)}>
      @TYPE_ALIAS {name} = {transform_type_annotation(type)}
    @ENDGENERIC
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    enum {name} {
      {members}
    }
  @END
  
  @TARGET
    @ENUM {name}
      {transform_enum_members(members)}
    @ENDENUM
  @END
@END
```

### 2.8 Control Flow

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    if ({condition}) {
      {true_body}
    } else if ({elif_condition}) {
      {elif_body}
    } else {
      {else_body}
    }
  @END
  
  @TARGET
    @IF {transformed_condition}
      {transformed_true_body}
    @ELIF {transformed_elif_condition}
      {transformed_elif_body}
    @ELSE
      {transformed_else_body}
    @ENDIF
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    for (const {var} of {iterable}) {
      {body}
    }
  @END
  
  @TARGET
    @LOOP type="for" var={var} in={transformed_iterable}
      {transformed_body}
    @ENDLOOP
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    try {
      {try_body}
    } catch ({error}) {
      {catch_body}
    } finally {
      {finally_body}
    }
  @END
  
  @TARGET
    @TRY
      {transformed_try_body}
    @CATCH {error_type} AS {error_var}
      {transformed_catch_body}
    @FINALLY
      {transformed_finally_body}
    @ENDTRY
  @END
@END
```

## 3. Type System Mapping

### 3.1 Type Annotation Mapping

```
@TYPE_MAPPING
  @SOURCE_TYPE
    number
  @END
  
  @TARGET_TYPE
    @FLOAT
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    string
  @END
  
  @TARGET_TYPE
    @STRING
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    boolean
  @END
  
  @TARGET_TYPE
    @BOOLEAN
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    any
  @END
  
  @TARGET_TYPE
    @ANY
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    {type}[]
  @END
  
  @TARGET_TYPE
    @ARRAY of={transform_type(type)}
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    {type1} | {type2}
  @END
  
  @TARGET_TYPE
    @UNION types=({transform_type(type1)}, {transform_type(type2)})
  @END
@END

@TYPE_MAPPING
  @SOURCE_TYPE
    {type1} & {type2}
  @END
  
  @TARGET_TYPE
    @INTERSECTION types=({transform_type(type1)}, {transform_type(type2)})
  @END
@END
```

## 4. Idiom Recognition

### 4.1 TypeScript-Specific Idioms

```
@IDIOM_RECOGNITION
  @NAME "optional_chaining"
  
  @PATTERN
    {obj}?.{prop}
  @END
  
  @LINGUITECT
    @OPTIONAL_ACCESS object={transformed_obj} property={prop}
  @END
  
  @INTENT
    "Access a property that may be undefined or null without causing an error"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "nullish_coalescing"
  
  @PATTERN
    {expr1} ?? {expr2}
  @END
  
  @LINGUITECT
    @NULLISH_COALESCE primary={transformed_expr1} fallback={transformed_expr2}
  @END
  
  @INTENT
    "Provide a fallback value when a value is null or undefined"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "type_guard"
  
  @PATTERN
    function is{Type}(obj: any): obj is {Type} {
      return {condition};
    }
  @END
  
  @LINGUITECT
    @TYPE_GUARD name=is{Type} param=obj target_type={Type} condition={transformed_condition}
  @END
  
  @INTENT
    "Define a function that narrows a type based on a runtime check"
  @END
@END
```

## 5. Import-Specific Hints

```
@IMPORT_HINTS
  @ERROR_HANDLING type="exceptions"
  @DEFAULT_PARAMETER_MECHANISM type="named"
  @TYPE_SYSTEM type="structural"
  
  @STANDARD_LIBRARY
    @CATEGORY "Core Types"
      Array, Map, Set, Promise, Date, RegExp, Error
    @CATEGORY "Utility Types"
      Partial, Required, Readonly, Record, Pick, Omit, Exclude, Extract, NonNullable, ReturnType
  @END
@END
```

Note: This is a simplified version of the TypeScript to Linguitect adapter specification. A complete implementation would include more detailed syntax rules, type mappings, and idiom recognition patterns.
