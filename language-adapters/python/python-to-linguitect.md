# Python to Linguitect Adapter Specification

## 1. Adapter Metadata

```
@LANGUAGE_ADAPTER "Python_to_Linguitect"
  @VERSION "1.0"
  @LANGUAGE_VERSION "Python 3.6-3.11"
  @TYPE "import"
  
  @METADATA
    @PARADIGMS ["procedural", "object_oriented", "functional"]
    @TYPING type="dynamic" strength="strong"
    @MEMORY_MANAGEMENT type="gc"
    @EXECUTION_MODEL "interpreted"
    @STANDARD_LIBRARY_VERSION "Python Standard Library 3.11"
  @END
```

## 2. Syntax Mapping Rules

### 2.1 Module Structure

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    # filename: {module_name}.py
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
```

### 2.2 Imports

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    import {module}
  @END
  
  @TARGET
    @IMPORT {module}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    from {package} import {module}
  @END
  
  @TARGET
    @IMPORT {module} FROM {package}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    import {module} as {alias}
  @END
  
  @TARGET
    @IMPORT {module} AS {alias}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    from {module} import {name1}, {name2}, ...
  @END
  
  @TARGET
    @IMPORT {name1}, {name2}, ... FROM {module}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    from {module} import *
  @END
  
  @TARGET
    @IMPORT * FROM {module}
    @NOTE "Wildcard imports are generally discouraged in Python"
  @END
@END
```

### 2.3 Variable Declarations

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    {var} = {value}
  @END
  
  @TARGET
    @VAR {var} {inferred_type} = {transformed_value}
  @END
  
  @CONTEXT
    @REQUIRES type_inference
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {var}: {type_annotation} = {value}
  @END
  
  @TARGET
    @VAR {var} {transform_type_annotation(type_annotation)} = {transformed_value}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {var}: {type_annotation}
  @END
  
  @TARGET
    @VAR {var} {transform_type_annotation(type_annotation)}
  @END
@END
```

### 2.4 Function Definitions

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    def {name}({params}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @FUNC {name}({transform_params(params)}) -> {inferred_return_type}
      @DOCS {docstring}
      @BODY
        {transformed_body}
      @END
    @ENDFUNC
  @END
  
  @CONTEXT
    @REQUIRES return_type_inference
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    def {name}({params}) -> {return_type}:
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @FUNC {name}({transform_params(params)}) -> {transform_type_annotation(return_type)}
      @DOCS {docstring}
      @BODY
        {transformed_body}
      @END
    @ENDFUNC
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    lambda {params}: {expression}
  @END
  
  @TARGET
    @LAMBDA ({transform_params(params)}) -> {inferred_return_type}
      @BODY
        @RETURN {transformed_expression}
      @END
    @ENDLAMBDA
  @END
  
  @CONTEXT
    @REQUIRES return_type_inference
  @END
@END
```

### 2.5 Class Definitions

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    class {name}:
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @CLASS {name}
      @DOCS {docstring}
      {transformed_body}
    @ENDCLASS
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    class {name}({bases}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @CLASS {name} EXTENDS {transform_bases(bases)}
      @DOCS {docstring}
      {transformed_body}
    @ENDCLASS
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    class {name}(metaclass={metaclass}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @CLASS {name}
      @META {metaclass}
      @DOCS {docstring}
      {transformed_body}
    @ENDCLASS
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    def __init__(self, {params}):
        {body}
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
    def {method_name}(self, {params}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @METHOD {method_name}({transform_params(params)}) -> {inferred_return_type}
      @DOCS {docstring}
      @BODY
        {transformed_body}
      @END
    @ENDMETHOD
  @END
  
  @CONTEXT
    @REQUIRES in_class_context && is_instance_method
    @REQUIRES return_type_inference
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    @classmethod
    def {method_name}(cls, {params}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @METHOD {method_name}({transform_params(params)}) -> {inferred_return_type}
      @DECORATOR classmethod
      @DOCS {docstring}
      @BODY
        {transformed_body}
      @END
    @ENDMETHOD
  @END
  
  @CONTEXT
    @REQUIRES in_class_context
    @REQUIRES return_type_inference
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    @staticmethod
    def {method_name}({params}):
        {"""docstring"""}
        {body}
  @END
  
  @TARGET
    @METHOD {method_name}({transform_params(params)}) -> {inferred_return_type}
      @DECORATOR staticmethod
      @DOCS {docstring}
      @BODY
        {transformed_body}
      @END
    @ENDMETHOD
  @END
  
  @CONTEXT
    @REQUIRES in_class_context
    @REQUIRES return_type_inference
  @END
@END
```

### 2.6 Control Flow

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    if {condition}:
        {true_body}
    elif {elif_condition}:
        {elif_body}
    else:
        {else_body}
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
    for {var} in {iterable}:
        {body}
  @END
  
  @TARGET
    @LOOP type="for" var={var} in={transformed_iterable}
      {transformed_body}
    @ENDLOOP
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    for {var} in range({start}, {end}, {step}):
        {body}
  @END
  
  @TARGET
    @LOOP type="for-range" var={var} from={start} to={end} step={step}
      {transformed_body}
    @ENDLOOP
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    while {condition}:
        {body}
  @END
  
  @TARGET
    @LOOP type="while" {transformed_condition}
      {transformed_body}
    @ENDLOOP
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    try:
        {try_body}
    except {exception} as {var}:
        {handler_body}
    finally:
        {finally_body}
  @END
  
  @TARGET
    @TRY
      {transformed_try_body}
    @CATCH {transformed_exception} AS {var}
      {transformed_handler_body}
    @FINALLY
      {transformed_finally_body}
    @ENDTRY
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    with {context_expr} as {var}:
        {body}
  @END
  
  @TARGET
    @WITH {transformed_context_expr} AS {var}
      {transformed_body}
    @ENDWITH
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    match {subject}:
        case {pattern1}:
            {body1}
        case {pattern2}:
            {body2}
        case _:
            {default_body}
  @END
  
  @TARGET
    @PATTERN_MATCH {transformed_subject}
      @PATTERN {transformed_pattern1}
        {transformed_body1}
      @PATTERN {transformed_pattern2}
        {transformed_body2}
      @DEFAULT
        {transformed_default_body}
    @ENDPATTERN_MATCH
  @END
  
  @CONTEXT
    @REQUIRES python_version >= 3.10
  @END
@END
```

### 2.7 Expressions

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} + {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="+" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} - {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="-" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} * {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="*" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} / {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="/" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} // {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="//" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} % {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="%" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} ** {expr2}
  @END
  
  @TARGET
    @EXPR_ARITHMETIC type="**" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} == {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type="==" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} != {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type="!=" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} < {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type="<" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} <= {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type="<=" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} > {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type=">" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} >= {expr2}
  @END
  
  @TARGET
    @EXPR_COMPARE type=">=" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} and {expr2}
  @END
  
  @TARGET
    @EXPR_LOGICAL type="AND" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} or {expr2}
  @END
  
  @TARGET
    @EXPR_LOGICAL type="OR" operands=({transformed_expr1}, {transformed_expr2})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    not {expr}
  @END
  
  @TARGET
    @EXPR_LOGICAL type="NOT" operands=({transformed_expr})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {value_if_true} if {condition} else {value_if_false}
  @END
  
  @TARGET
    @EXPR_TERNARY {transformed_condition} {transformed_value_if_true} {transformed_value_if_false}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {obj}.{attr}
  @END
  
  @TARGET
    @PROPERTY_GET {transformed_obj}.{attr}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {obj}.{attr} = {value}
  @END
  
  @TARGET
    @PROPERTY_SET {transformed_obj}.{attr} <- {transformed_value}
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {func}({args})
  @END
  
  @TARGET
    @CALL {func} WITH ({transformed_args})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {obj}.{method}({args})
  @END
  
  @TARGET
    @CALL_METHOD {transformed_obj}.{method} WITH ({transformed_args})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    f"{string_with_expressions}"
  @END
  
  @TARGET
    @EXPR_STRING_INTERPOLATE "{string_template}" vars=({extracted_vars})
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {expr1} + {expr2} + ... # String concatenation
  @END
  
  @TARGET
    @EXPR_STRING_CONCAT operands=({transformed_expr1}, {transformed_expr2}, ...)
  @END
  
  @CONTEXT
    @REQUIRES is_string_type(expr1) && is_string_type(expr2)
  @END
@END
```

### 2.8 Comprehensions

```
@SYNTAX_RULE
  @SOURCE_PATTERN
    [{expr} for {var} in {iterable}]
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
    @ENDCOMPREHENSION
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    [{expr} for {var} in {iterable} if {condition}]
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
      @WHERE {transformed_condition}
    @ENDCOMPREHENSION
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {{{key}: {value} for {var} in {iterable}}}
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_key}: {transformed_value}
      @FROM {var} in {transformed_iterable}
      @TYPE map
    @ENDCOMPREHENSION
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {{{key}: {value} for {var} in {iterable} if {condition}}}
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_key}: {transformed_value}
      @FROM {var} in {transformed_iterable}
      @WHERE {transformed_condition}
      @TYPE map
    @ENDCOMPREHENSION
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    {{{expr} for {var} in {iterable}}}
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
      @TYPE set
    @ENDCOMPREHENSION
  @END
@END

@SYNTAX_RULE
  @SOURCE_PATTERN
    ({expr} for {var} in {iterable})
  @END
  
  @TARGET
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
      @TYPE generator
    @ENDCOMPREHENSION
  @END
@END
```

## 3. Semantic Analysis Rules

### 3.1 Type Inference

```
@TYPE_INFERENCE
  @PATTERN
    {var} = {numeric_literal}
  @END
  
  @STRATEGY
    if is_integer(numeric_literal):
      return "@INT"
    elif is_float(numeric_literal):
      return "@FLOAT"
  @END
  
  @CONFIDENCE level="high"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = "{string_literal}"
  @END
  
  @STRATEGY
    return "@STRING"
  @END
  
  @CONFIDENCE level="high"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = True|False
  @END
  
  @STRATEGY
    return "@BOOLEAN"
  @END
  
  @CONFIDENCE level="high"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = None
  @END
  
  @STRATEGY
    return "@NULL"
  @END
  
  @CONFIDENCE level="high"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = [{items}]
  @END
  
  @STRATEGY
    common_type = find_common_type(items)
    if common_type:
      return "@LIST of=" + common_type
    else:
      return "@LIST of=@ANY"
  @END
  
  @CONFIDENCE level="medium"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = {{{items}}}
  @END
  
  @STRATEGY
    if is_dict_literal(items):
      key_type = find_common_type(dict_keys(items))
      value_type = find_common_type(dict_values(items))
      return "@MAP key=" + (key_type or "@ANY") + " value=" + (value_type or "@ANY")
    else:
      return "@SET of=" + find_common_type(items) or "@ANY"
  @END
  
  @CONFIDENCE level="medium"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = ({items})
  @END
  
  @STRATEGY
    if is_single_item_tuple(items):
      return "@TUPLE types=(" + infer_type(items[0]) + ")"
    else:
      return "@TUPLE types=(" + ", ".join(infer_type(item) for item in items) + ")"
  @END
  
  @CONFIDENCE level="medium"
@END

@TYPE_INFERENCE
  @PATTERN
    {var} = {func}({args})
  @END
  
  @STRATEGY
    if is_known_function(func):
      return get_return_type_of_known_function(func, args)
    elif is_constructor(func):
      return class_name_to_type(func)
    else:
      return "@ANY"
  @END
  
  @CONFIDENCE level="low"
@END

@TYPE_INFERENCE
  @PATTERN
    def {name}({params}):
        {body}
  @END
  
  @STRATEGY
    # Analyze return statements in the function body
    return_types = collect_return_statement_types(body)
    if not return_types:
      return "@VOID"
    elif is_homogeneous(return_types):
      return return_types[0]
    else:
      return "@UNION types=(" + ", ".join(return_types) + ")"
  @END
  
  @CONFIDENCE level="low"
@END

@TYPE_INFERENCE
  @PATTERN
    {var}: {type_annotation}
  @END
  
  @STRATEGY
    return transform_type_annotation(type_annotation)
  @END
  
  @CONFIDENCE level="high"
@END
```

### 3.2 Type Annotation Mapping

```
@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "int"
  @LINGUITECT_TYPE "@INT"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "float"
  @LINGUITECT_TYPE "@FLOAT"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "bool"
  @LINGUITECT_TYPE "@BOOLEAN"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "str"
  @LINGUITECT_TYPE "@STRING"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "bytes"
  @LINGUITECT_TYPE "@TYPE_PRIMITIVE @CHAR encoding=\"bytes\" @END"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "List[{T}]"
  @LINGUITECT_TYPE "@LIST of={transform_type_annotation(T)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Dict[{K}, {V}]"
  @LINGUITECT_TYPE "@MAP key={transform_type_annotation(K)} value={transform_type_annotation(V)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Set[{T}]"
  @LINGUITECT_TYPE "@SET of={transform_type_annotation(T)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Tuple[{T1}, {T2}, ...]"
  @LINGUITECT_TYPE "@TUPLE types=({transform_type_annotation(T1)}, {transform_type_annotation(T2)}, ...)"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Optional[{T}]"
  @LINGUITECT_TYPE "@OPTIONAL of={transform_type_annotation(T)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Union[{T1}, {T2}, ...]"
  @LINGUITECT_TYPE "@UNION types=({transform_type_annotation(T1)}, {transform_type_annotation(T2)}, ...)"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Any"
  @LINGUITECT_TYPE "@ANY"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Callable[[{Args}], {Return}]"
  @LINGUITECT_TYPE "@FUNCTION<{transform_args(Args)}, {transform_type_annotation(Return)}>"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Generator[{Yield}, {Send}, {Return}]"
  @LINGUITECT_TYPE "@GENERATOR of={transform_type_annotation(Yield)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Iterable[{T}]"
  @LINGUITECT_TYPE "@ITERABLE of={transform_type_annotation(T)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "Sequence[{T}]"
  @LINGUITECT_TYPE "@SEQUENCE of={transform_type_annotation(T)}"
@END

@TYPE_ANNOTATION_MAPPING
  @PYTHON_TYPE "TypeVar('{name}')"
  @LINGUITECT_TYPE "@GENERIC {name}"
@END
```

## 4. Idiom Catalog

### 4.1 Common Python Idioms

```
@IDIOM_RECOGNITION
  @NAME "list_comprehension"
  
  @PATTERN
    [{expr} for {var} in {iterable} if {condition}]
  @END
  
  @LINGUITECT
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
      @WHERE {transformed_condition}
    @ENDCOMPREHENSION
  @END
  
  @INTENT
    "Create a new list by transforming elements of an iterable that satisfy a condition"
  @END
  
  @PERFORMANCE
    "More efficient than equivalent explicit loops in Python"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "resource_context_manager"
  
  @PATTERN
    with open({filename}, {mode}) as {file_var}:
        {body}
  @END
  
  @LINGUITECT
    @WITH @CALL open WITH ({transformed_filename}, {transformed_mode}) AS {file_var}
      {transformed_body}
    @ENDWITH
  @END
  
  @INTENT
    "Safely open a file with automatic cleanup"
  @END
  
  @PERFORMANCE
    "Guarantees proper resource cleanup even in the case of exceptions"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "tuple_unpacking"
  
  @PATTERN
    {var1}, {var2}, ... = {iterable}
  @END
  
  @LINGUITECT
    @MATCH ({var1}, {var2}, ...) = {transformed_iterable}
  @END
  
  @INTENT
    "Extract multiple values from an iterable in a single statement"
  @END
  
  @PERFORMANCE
    "More efficient and readable than indexing"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "extended_unpacking"
  
  @PATTERN
    {var1}, *{rest_var}, {var2} = {iterable}
  @END
  
  @LINGUITECT
    @MATCH ({var1}, *{rest_var}, {var2}) = {transformed_iterable}
  @END
  
  @INTENT
    "Extract some values and collect the remaining ones in a separate variable"
  @END
  
  @PERFORMANCE
    "Avoids explicit slicing operations"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "dict_get_with_default"
  
  @PATTERN
    {dict}.get({key}, {default})
  @END
  
  @LINGUITECT
    @CALL_METHOD {transformed_dict}.get WITH ({transformed_key}, {transformed_default})
  @END
  
  @INTENT
    "Safely access a dictionary value with a fallback default"
  @END
  
  @PERFORMANCE
    "Avoids exception handling for missing keys"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "defaultdict_usage"
  
  @PATTERN
    from collections import defaultdict
    {var} = defaultdict({factory})
  @END
  
  @LINGUITECT
    @IMPORT defaultdict FROM collections
    @NEW defaultdict WITH ({transformed_factory}) -> {var}
  @END
  
  @INTENT
    "Create a dictionary that automatically initializes missing keys"
  @END
  
  @PERFORMANCE
    "More efficient than explicitly checking for missing keys"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "counter_usage"
  
  @PATTERN
    from collections import Counter
    {var} = Counter({iterable})
  @END
  
  @LINGUITECT
    @IMPORT Counter FROM collections
    @NEW Counter WITH ({transformed_iterable}) -> {var}
  @END
  
  @INTENT
    "Count occurrences of elements in an iterable"
  @END
  
  @PERFORMANCE
    "More efficient than manual counting with dictionaries"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "generator_expression"
  
  @PATTERN
    ({expr} for {var} in {iterable} if {condition})
  @END
  
  @LINGUITECT
    @COMPREHENSION
      @OUTPUT {transformed_expr}
      @FROM {var} in {transformed_iterable}
      @WHERE {transformed_condition}
      @TYPE generator
    @ENDCOMPREHENSION
  @END
  
  @INTENT
    "Lazily generate values on-demand"
  @END
  
  @PERFORMANCE
    "Memory-efficient for large datasets as it evaluates lazily"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "enumerate_usage"
  
  @PATTERN
    for {index}, {value} in enumerate({iterable}):
        {body}
  @END
  
  @LINGUITECT
    @CALL enumerate WITH ({transformed_iterable}) -> enumerated
    @LOOP type="for" var=(index, value) in=enumerated
      {transformed_body}
    @ENDLOOP
  @END
  
  @INTENT
    "Iterate over elements of a sequence while keeping track of their indices"
  @END
  
  @PERFORMANCE
    "More readable and less error-prone than manual index tracking"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "dict_comprehension"
  
  @PATTERN
    {{{key}: {value} for {var} in {iterable} if {condition}}}
  @END
  
  @LINGUITECT
    @COMPREHENSION
      @OUTPUT {transformed_key}: {transformed_value}
      @FROM {var} in {transformed_iterable}
      @WHERE {transformed_condition}
      @TYPE map
    @ENDCOMPREHENSION
  @END
  
  @INTENT
    "Create a dictionary by transforming elements of an iterable"
  @END
  
  @PERFORMANCE
    "More concise and potentially more efficient than manual dictionary building"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "zip_usage"
  
  @PATTERN
    for {var1}, {var2}, ... in zip({iterable1}, {iterable2}, ...):
        {body}
  @END
  
  @LINGUITECT
    @CALL zip WITH ({transformed_iterable1}, {transformed_iterable2}, ...) -> zipped
    @LOOP type="for" var=({var1}, {var2}, ...) in=zipped
      {transformed_body}
    @ENDLOOP
  @END
  
  @INTENT
    "Process multiple iterables in parallel"
  @END
  
  @PERFORMANCE
    "Provides synchronized iteration over multiple sequences"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "string_join"
  
  @PATTERN
    "{delimiter}".join({iterable})
  @END
  
  @LINGUITECT
    @CALL_METHOD "".join WITH ({
      @PROPERTY_GET {transformed_delimiter},
      @CALL_METHOD {transformed_iterable}.map WITH ((item) -> @CALL str WITH (item))
    })
  @END
  
  @INTENT
    "Efficiently concatenate strings with a delimiter"
  @END
  
  @PERFORMANCE
    "More efficient than string concatenation in a loop"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "function_decorator"
  
  @PATTERN
    @{decorator}
    def {func_name}({params}):
        {body}
  @END
  
  @LINGUITECT
    @FUNC {func_name}({transformed_params}) -> {inferred_return_type}
      @DECORATOR {decorator}
      @BODY
        {transformed_body}
      @END
    @ENDFUNC
  @END
  
  @INTENT
    "Modify or enhance function behavior without changing its code"
  @END
  
  @PERFORMANCE
    "Enables clean separation of concerns and code reuse"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "property_decorator"
  
  @PATTERN
    @property
    def {name}(self):
        {body}
  @END
  
  @LINGUITECT
    @METHOD {name}() -> {inferred_return_type}
      @DECORATOR property
      @BODY
        {transformed_body}
      @END
    @ENDMETHOD
  @END
  
  @INTENT
    "Define a method that can be accessed like an attribute"
  @END
  
  @PERFORMANCE
    "Provides controlled access to private attributes with getter/setter semantics"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "generator_function"
  
  @PATTERN
    def {name}({params}):
        {body with yield statements}
  @END
  
  @LINGUITECT
    @FUNC {name}({transformed_params}) -> @GENERATOR of={inferred_yield_type}
      @BODY
        {transformed_body}
      @END
    @ENDFUNC
  @END
  
  @INTENT
    "Define a function that generates values on-demand"
  @END
  
  @PERFORMANCE
    "Memory-efficient alternative to returning full collections"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "list_to_dict"
  
  @PATTERN
    dict(zip({keys}, {values}))
  @END
  
  @LINGUITECT
    @CALL dict WITH (@CALL zip WITH ({transformed_keys}, {transformed_values}))
  @END
  
  @INTENT
    "Create a dictionary from parallel lists of keys and values"
  @END
  
  @PERFORMANCE
    "Cleaner than manual dictionary construction"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "conditional_assignment"
  
  @PATTERN
    {var} = {value_if_true} if {condition} else {value_if_false}
  @END
  
  @LINGUITECT
    @EXPR_TERNARY {transformed_condition} {transformed_value_if_true} {transformed_value_if_false} -> {var}
  @END
  
  @INTENT
    "Conditionally assign a value in a single expression"
  @END
  
  @PERFORMANCE
    "More concise than if-else blocks for simple assignments"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "context_manager_class"
  
  @PATTERN
    class {name}:
        def __enter__(self):
            {enter_body}
        def __exit__(self, exc_type, exc_val, exc_tb):
            {exit_body}
  @END
  
  @LINGUITECT
    @CLASS {name}
      @METHOD __enter__(self) -> {inferred_return_type}
        @SPECIAL method_type="context_entry"
        @BODY
          {transformed_enter_body}
        @END
      @ENDMETHOD
      
      @METHOD __exit__(self, exc_type, exc_val, exc_tb) -> boolean
        @SPECIAL method_type="context_exit"
        @BODY
          {transformed_exit_body}
        @END
      @ENDMETHOD
    @ENDCLASS
  @END
  
  @INTENT
    "Define a class that can be used with the 'with' statement"
  @END
  
  @PERFORMANCE
    "Ensures proper resource management and cleanup"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "namedtuple_usage"
  
  @PATTERN
    from collections import namedtuple
    {Type} = namedtuple('{Type}', [{fields}])
  @END
  
  @LINGUITECT
    @IMPORT namedtuple FROM collections
    @STRUCT {Type}
      @FIELD {field1} {inferred_type1}
      @FIELD {field2} {inferred_type2}
      ...
    @ENDSTRUCT
  @END
  
  @INTENT
    "Create a lightweight immutable record type"
  @END
  
  @PERFORMANCE
    "More memory-efficient than full classes for simple data containers"
  @END
@END

@IDIOM_RECOGNITION
  @NAME "dataclass_usage"
  
  @PATTERN
    @dataclass
    class {Name}:
        {field1}: {type1}
        {field2}: {type2} = {default2}
        ...
  @END
  
  @LINGUITECT
    @CLASS {Name}
      @DECORATOR dataclass
      @PROPERTY {field1} {transformed_type1}
      @PROPERTY {field2} {transformed_type2} = {transformed_default2}
      ...
    @ENDCLASS
  @END
  
  @INTENT
    "Create a class optimized for storing data"
  @END
  
  @PERFORMANCE
    "Reduces boilerplate for data container classes"
  @END
@END

## 5. Optimization Hints

### 5.1 Performance Patterns

```
@OPTIMIZATION_HINT
  @PATTERN
    for {var} in range({start}, {end}, {step}):
      {body}
  @END
  
  @HINT
    "Can be implemented as C-style for loop in languages with that construct"
  @END
  
  @TRANSLATION_NOTE
    "This loop has known bounds and can be efficiently implemented"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    {str1} + {str2} + {str3} + ...  # String concatenation in a loop
  @END
  
  @HINT
    "Use string builder pattern in target languages for efficiency"
  @END
  
  @TRANSLATION_NOTE
    "String concatenation in loops is inefficient in most languages"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    if {key} in {dict}:
      {value} = {dict}[{key}]
      {use_value}
  @END
  
  @HINT
    "Can be optimized to a single dictionary lookup in most languages"
  @END
  
  @TRANSLATION_NOTE
    "Avoid redundant key lookup by getting the value directly with get() or similar"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    [... for ... in ... if ...]  # List comprehension with filter
  @END
  
  @HINT
    "May require separate filter and map operations in languages without comprehensions"
  @END
  
  @TRANSLATION_NOTE
    "Consider performance implications when translating to languages without native comprehensions"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    def memoized_function({params}):
        if {params} in cache:
            return cache[{params}]
        result = ...  # Expensive computation
        cache[{params}] = result
        return result
  @END
  
  @HINT
    "Memoization pattern for expensive computations"
  @END
  
  @TRANSLATION_NOTE
    "Consider language-specific memoization mechanisms (e.g., @lru_cache or similar)"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    with open({file}, {mode}) as {handle}:
        {body}
  @END
  
  @HINT
    "Resource management pattern that needs proper cleanup in target language"
  @END
  
  @TRANSLATION_NOTE
    "Ensure proper resource cleanup in languages without context managers (e.g., try-finally)"
  @END
@END

@OPTIMIZATION_HINT
  @PATTERN
    {value} = next((x for x in {iterable} if {condition}), {default})
  @END
  
  @HINT
    "Short-circuit search pattern for first matching item"
  @END
  
  @TRANSLATION_NOTE
    "Translate to efficient find-first operation in target language"
  @END
@END
```

### 5.2 Memory Management Hints

```
@MEMORY_HINT
  @PATTERN
    {var} = [{large_expression} for {item} in {huge_iterable}]
  @END
  
  @HINT
    "List comprehension on large iterables can consume significant memory"
  @END
  
  @TRANSLATION_NOTE
    "Consider generator expressions or streaming approaches in memory-constrained environments"
  @END
@END

@MEMORY_HINT
  @PATTERN
    def process_large_file({file_path}):
        with open({file_path}) as f:
            {content} = f.read()
        # Process content
  @END
  
  @HINT
    "Loading entire file contents into memory can be problematic for large files"
  @END
  
  @TRANSLATION_NOTE
    "Consider line-by-line or chunked processing in languages with manual memory management"
  @END
@END

@MEMORY_HINT
  @PATTERN
    {var} = {collection}[:]  # Slice copy
  @END
  
  @HINT
    "Creates a shallow copy of the entire collection"
  @END
  
  @TRANSLATION_NOTE
    "Ensure target language uses efficient copy mechanisms; consider if copy is truly needed"
  @END
@END

@MEMORY_HINT
  @PATTERN
    def {generator_func}({params}):
        for {var} in {huge_range}:
            yield {expression}
  @END
  
  @HINT
    "Generator function for memory-efficient processing"
  @END
  
  @TRANSLATION_NOTE
    "Preserve lazy evaluation semantics in target language if possible"
  @END
@END
```

### 5.3 Parallelism Hints

```
@PARALLELISM_HINT
  @PATTERN
    from concurrent.futures import ThreadPoolExecutor
    with ThreadPoolExecutor(max_workers={workers}) as executor:
        results = list(executor.map({func}, {iterable}))
  @END
  
  @HINT
    "Parallel execution pattern using thread pool"
  @END
  
  @TRANSLATION_NOTE
    "Thread pools are suitable for I/O-bound tasks; use equivalent concurrency primitive in target language"
  @END
@END

@PARALLELISM_HINT
  @PATTERN
    from concurrent.futures import ProcessPoolExecutor
    with ProcessPoolExecutor(max_workers={workers}) as executor:
        results = list(executor.map({func}, {iterable}))
  @END
  
  @HINT
    "Parallel execution pattern using process pool"
  @END
  
  @TRANSLATION_NOTE
    "Process pools are suitable for CPU-bound tasks; handle serialization requirements in target language"
  @END
@END

@PARALLELISM_HINT
  @PATTERN
    import asyncio
    async def {async_func}({params}):
        {async_body}
    asyncio.run({async_func}({args}))
  @END
  
  @HINT
    "Asynchronous execution pattern using asyncio"
  @END
  
  @TRANSLATION_NOTE
    "Map to appropriate asynchronous primitives in target language (e.g., promises, futures)"
  @END
@END
```

## 6. Import-Specific Hints

```
@IMPORT_HINTS
  @ERROR_HANDLING type="exceptions"
  @DEFAULT_PARAMETER_MECHANISM type="named_and_positional"
  
  @STANDARD_LIBRARY
    # Note: This is a high-level overview of standard library modules.
    # Detailed function mappings should be provided in separate specialized documents.
    @MODULE_GROUP "collections" description="Specialized container datatypes"
    @MODULE_GROUP "itertools" description="Functions for efficient iteration"
    @MODULE_GROUP "functools" description="Higher-order functions and operations on callable objects"
    @MODULE_GROUP "os" description="Operating system interfaces"
    @MODULE_GROUP "re" description="Regular expression operations"
    @MODULE_GROUP "json" description="JSON encoder and decoder"
    @MODULE_GROUP "datetime" description="Date and time handling"
    @MODULE_GROUP "math" description="Mathematical functions"
    @MODULE_GROUP "random" description="Generate pseudo-random numbers"
  @END
  
  @DYNAMIC_FEATURES
    @FEATURE "duck_typing" level="high"
    @FEATURE "dynamic_attribute_access" level="high"
    @FEATURE "runtime_type_checking" level="medium"
    @FEATURE "metaprogramming" level="high"
    @FEATURE "reflection" level="medium"
  @END
  
  @GOTCHAS
    @ISSUE "mutable_default_arguments"
      description="Default arguments are evaluated only once at function definition"
      detection="Look for mutable defaults like [], {}, or set()"
      resolution="Use None as default and initialize in function body"
    @END
    
    @ISSUE "late_binding_closures"
      description="Variables in loops captured by lambdas/closures use their final value"
      detection="Look for lambda/function definition in loops that use loop variables"
      resolution="Use default arguments to capture current value"
    @END
    
    @ISSUE "global_interpreter_lock"
      description="Python's GIL limits true parallel execution in threads"
      detection="Look for CPU-bound multithreaded code"
      resolution="Use multiprocessing instead of threading for CPU-bound tasks"
    @END
  @END
@END
```

## 7. Testing and Validation

### 7.1 Unit Tests

```
@ADAPTER_TEST
  @NAME "basic_variable_declaration"
  
  @SOURCE_CODE
    x = 42
    y = "hello"
    z = True
  @END
  
  @EXPECTED_LINGUITECT
    @VAR x int = 42
    @VAR y string = "hello"
    @VAR z boolean = true
  @END
@END

@ADAPTER_TEST
  @NAME "simple_function_definition"
  
  @SOURCE_CODE
    def greet(name):
        """Greet a person."""
        return "Hello, " + name + "!"
  @END
  
  @EXPECTED_LINGUITECT
    @FUNC greet(name: string) -> string
      @DOCS "Greet a person."
      @BODY
        @EXPR_STRING_CONCAT operands=("Hello, ", name, "!") -> result
        @RETURN result
      @END
    @ENDFUNC
  @END
@END

@ADAPTER_TEST
  @NAME "class_with_methods"
  
  @SOURCE_CODE
    class Counter:
        def __init__(self, initial=0):
            self.value = initial
            
        def increment(self):
            self.value += 1
            return self.value
            
        def reset(self):
            self.value = 0
  @END
  
  @EXPECTED_LINGUITECT
    @CLASS Counter
      @CONSTRUCTOR(initial: int = 0)
        @BODY
          @PROPERTY_SET this.value <- initial
        @END
      @ENDCONSTRUCTOR
      
      @METHOD increment() -> int
        @BODY
          @ASSIGN_OP this.value + 1
          @RETURN this.value
        @END
      @ENDMETHOD
      
      @METHOD reset() -> void
        @BODY
          @ASSIGN this.value <- 0
        @END
      @ENDMETHOD
    @ENDCLASS
  @END
@END

@ADAPTER_TEST
  @NAME "list_comprehension"
  
  @SOURCE_CODE
    squares = [x*x for x in range(10) if x % 2 == 0]
  @END
  
  @EXPECTED_LINGUITECT
    @COMPREHENSION
      @OUTPUT @EXPR_ARITHMETIC type="*" operands=(x, x)
      @FROM x in @CALL range WITH (10)
      @WHERE @EXPR_COMPARE type="==" operands=(
        @EXPR_ARITHMETIC type="%" operands=(x, 2),
        0
      )
    @ENDCOMPREHENSION -> squares
  @END
@END
```

## 8. Conclusion

This Python to Linguitect adapter specification provides a framework for translating Python code to the Linguitect intermediate representation. By focusing on Python's syntax and semantics, including its idioms and type system, this adapter enables accurate translation of Python code for further processing or translation to other languages.

Key features of this adapter include:
- Comprehensive syntax mapping for Python's core constructs
- Type inference and annotation mapping
- Recognition of common Python idioms 
- Performance and memory optimization hints
- Testing framework for validation

For detailed mappings of Python's standard library functions, separate specialized documents should be created, organized by library modules and functionality domains. This modular approach allows for more maintainable and flexible translation while keeping the core adapter specification focused on Python's fundamental language features.