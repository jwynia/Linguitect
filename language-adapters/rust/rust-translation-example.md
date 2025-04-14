# Rust-Linguitect Translation Example

This document demonstrates a complete translation example from Rust to Linguitect IR and back, showing how the adapter specifications work in practice.

## 1. Original Rust Code

Let's consider a complex Rust program that showcases many of Rust's distinctive features:

```rust
use std::collections::HashMap;
use std::error::Error;
use std::fmt;

// Custom error type
#[derive(Debug)]
pub enum AppError {
    FileNotFound(String),
    ParseError(String),
    NetworkError { code: u32, message: String },
}

impl fmt::Display for AppError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            AppError::FileNotFound(path) => write!(f, "File not found: {}", path),
            AppError::ParseError(details) => write!(f, "Parse error: {}", details),
            AppError::NetworkError { code, message } => write!(f, "Network error {}: {}", code, message),
        }
    }
}

impl Error for AppError {}

// A trait for entities that can be serialized
pub trait Serialize {
    fn serialize(&self) -> String;
    
    fn serialize_pretty(&self) -> String {
        format!("Pretty: {}", self.serialize())
    }
}

// A generic struct with lifetime parameters
#[derive(Debug, Clone)]
pub struct DataStore<'a, T: 'a + Clone> {
    pub name: String,
    data: Vec<&'a T>,
    metadata: HashMap<String, String>,
}

impl<'a, T: 'a + Clone + fmt::Debug> DataStore<'a, T> {
    // Constructor
    pub fn new(name: String) -> Self {
        DataStore {
            name,
            data: Vec::new(),
            metadata: HashMap::new(),
        }
    }
    
    // Method with mutable self
    pub fn add_item(&mut self, item: &'a T) -> Result<(), AppError> {
        if self.data.len() >= 100 {
            return Err(AppError::ParseError("Too many items".to_string()));
        }
        self.data.push(item);
        Ok(())
    }
    
    // Method that consumes self
    pub fn with_metadata(mut self, key: String, value: String) -> Self {
        self.metadata.insert(key, value);
        self
    }
    
    // Method with pattern matching and advanced control flow
    pub fn find_item<F>(&self, predicate: F) -> Option<&'a T>
    where
        F: Fn(&T) -> bool,
    {
        for item in &self.data {
            if predicate(item) {
                return Some(item);
            }
        }
        None
    }
}

// Implementing the Serialize trait for DataStore
impl<'a, T: 'a + Clone + fmt::Debug> Serialize for DataStore<'a, T> {
    fn serialize(&self) -> String {
        format!("DataStore '{}' with {} items", self.name, self.data.len())
    }
}

// A function that uses advanced pattern matching and the ? operator
pub fn process_data<'a, T>(data_store: &DataStore<'a, T>, search_key: &str) -> Result<String, AppError>
where
    T: Clone + fmt::Debug + 'a,
{
    let metadata = match data_store.metadata.get(search_key) {
        Some(value) => value,
        None => return Err(AppError::FileNotFound(format!("Key '{}' not found", search_key))),
    };
    
    // Use of closures
    let item_count = data_store.data.len();
    let status = if item_count > 10 {
        "Complete"
    } else if item_count > 0 {
        "In progress"
    } else {
        "Empty"
    };
    
    // Using the ? operator for error propagation
    let serialized = data_store.serialize();
    
    Ok(format!("Status: {}, Metadata: {}, Details: {}", status, metadata, serialized))
}

// Main function to demonstrate usage
fn main() -> Result<(), Box<dyn Error>> {
    // Create some sample data
    let items = vec![1, 2, 3, 4, 5];
    
    // Lifetimes in action
    let mut data_store = DataStore::new("Sample Store".to_string());
    
    // Builder pattern
    let data_store = data_store
        .with_metadata("created".to_string(), "today".to_string())
        .with_metadata("owner".to_string(), "me".to_string());
    
    // Error handling with ?
    for item in &items {
        let mut cloned_store = data_store.clone();
        cloned_store.add_item(item)?;
        
        // Closures and higher-order functions
        let found = cloned_store.find_item(|&i| i > &2);
        
        if let Some(value) = found {
            println!("Found value: {:?}", value);
        }
    }
    
    // Pattern matching
    match process_data(&data_store, "owner") {
        Ok(result) => println!("Process result: {}", result),
        Err(AppError::FileNotFound(path)) => eprintln!("Missing file: {}", path),
        Err(AppError::ParseError(details)) => eprintln!("Parse error: {}", details),
        Err(AppError::NetworkError { code, message }) => {
            eprintln!("Network error {} - {}", code, message)
        }
    }
    
    Ok(())
}
```

## 2. Rust to Linguitect IR Translation

Let's translate the above Rust code to Linguitect IR using our adapter:

```
@PROGRAM data_store_app
  @MODULE data_store_app
    @IMPORT HashMap FROM std::collections
    @IMPORT Error FROM std::error
    @IMPORT fmt FROM std

    // Custom error type
    @EXCEPTION AppError
      @DECORATOR derive(Debug)
      @NOTE "This enum is public"
      @VALUE FileNotFound with_fields=(@STRING)
      @VALUE ParseError with_fields=(@STRING)
      @VALUE NetworkError with_struct_fields=(code: @INT bits=32 signed=false, message: @STRING)
      
      @METHOD fmt(f: @OWNERSHIP type="mut_ref" fmt::Formatter) -> fmt::Result
        @BODY
          @PATTERN_MATCH @THIS
            @PATTERN AppError::FileNotFound(path)
              @CALL_METHOD f.write WITH ("File not found: {}", path)
            @PATTERN AppError::ParseError(details)
              @CALL_METHOD f.write WITH ("Parse error: {}", details)
            @PATTERN AppError::NetworkError { code, message }
              @CALL_METHOD f.write WITH ("Network error {}: {}", code, message)
          @ENDPATTERN_MATCH
        @END
      @ENDMETHOD
    @ENDEXCEPTION

    @CLASS AppError IMPLEMENTS Error
      // Empty implementation since Error trait has no required methods
    @ENDCLASS

    // A trait for entities that can be serialized
    @INTERFACE Serialize
      @NOTE "This trait is public"
      @METHOD_SIGNATURE serialize() -> @STRING
      
      @METHOD serialize_pretty() -> @STRING
        @BODY
          @EXPR_STRING_INTERPOLATE "Pretty: {}" vars=(@CALL_METHOD @THIS.serialize WITH ())
        @END
      @ENDMETHOD
    @ENDINTERFACE

    // A generic struct with lifetime parameters
    @STRUCT DataStore
      @DECORATOR derive(Debug, Clone)
      @NOTE "This struct is public"
      @FIELD name @STRING access=public
      @FIELD data @LIST of=@OWNERSHIP type="ref" T access=private
      @FIELD metadata @MAP key=@STRING value=@STRING access=private
      @LIFETIME 'a = applies_to(data)
      @GENERIC T extends @LIFETIME 'a + Clone
    @ENDSTRUCT

    @CLASS DataStore<'a, T>
      @GENERIC T extends @LIFETIME 'a + Clone + fmt::Debug
      
      // Constructor
      @METHOD new(@PARAM name @STRING) -> DataStore<'a, T>
        @NOTE "This function is public"
        @BODY
          @NEW DataStore WITH (
            name: name,
            data: @NEW Vec WITH (),
            metadata: @NEW HashMap WITH ()
          )
        @END
      @ENDMETHOD
      
      // Method with mutable self
      @METHOD add_item(@PARAM item @OWNERSHIP type="ref" T) -> @RESULT_TYPE ok=@VOID error=AppError
        @NOTE "This method modifies the struct (mutable self)"
        @NOTE "This function is public"
        @BODY
          @IF @EXPR_COMPARE type=">=" operands=(@CALL_METHOD @PROPERTY_GET @THIS.data.len WITH (), 100)
            @RESULT
              @ERROR @NEW AppError::ParseError WITH ("Too many items".to_string())
            @ENDRESULT
          @ENDIF
          
          @CALL_METHOD @PROPERTY_GET @THIS.data.push WITH (item)
          
          @RESULT
            @SUCCESS @VOID
          @ENDRESULT
        @END
      @ENDMETHOD
      
      // Method that consumes self
      @METHOD with_metadata(@PARAM key @STRING, @PARAM value @STRING) -> DataStore<'a, T>
        @NOTE "This method consumes self"
        @NOTE "This function is public"
        @BODY
          @CALL_METHOD @PROPERTY_GET @THIS.metadata.insert WITH (key, value)
          @RETURN @THIS
        @END
      @ENDMETHOD
      
      // Method with pattern matching and advanced control flow
      @METHOD find_item<F>(@PARAM predicate F) -> @OPTIONAL of=@OWNERSHIP type="ref" T
        @NOTE "This function is public"
        @GENERIC F extends Fn(&T) -> @BOOLEAN
        @BODY
          @LOOP type="for" var=item in=@PROPERTY_GET @THIS.data
            @IF @CALL predicate WITH (item)
              @RETURN @OPTIONAL
                @VALUE item
              @ENDOPTIONAL
            @ENDIF
          @ENDLOOP
          
          @RETURN @OPTIONAL
            @NONE
          @ENDOPTIONAL
        @END
      @ENDMETHOD
    @ENDCLASS

    // Implementing the Serialize trait for DataStore
    @CLASS DataStore<'a, T> IMPLEMENTS Serialize
      @GENERIC T extends @LIFETIME 'a + Clone + fmt::Debug
      
      @METHOD serialize() -> @STRING
        @BODY
          @EXPR_STRING_INTERPOLATE "DataStore '{}' with {} items" 
            vars=(@PROPERTY_GET @THIS.name, @CALL_METHOD @PROPERTY_GET @THIS.data.len WITH ())
        @END
      @ENDMETHOD
    @ENDCLASS

    // A function that uses advanced pattern matching and the ? operator
    @FUNC process_data<'a, T>(@PARAM data_store @OWNERSHIP type="ref" DataStore<'a, T>, @PARAM search_key @OWNERSHIP type="ref" @STRING) -> @RESULT_TYPE ok=@STRING error=AppError
      @GENERIC T extends Clone + fmt::Debug + @LIFETIME 'a
      @NOTE "This function is public"
      @BODY
        @VAR metadata @OWNERSHIP type="ref" @STRING = @PATTERN_MATCH @CALL_METHOD @PROPERTY_GET data_store.metadata.get WITH (search_key)
          @PATTERN Some(value)
            value
          @PATTERN None
            @RETURN @RESULT
              @ERROR @NEW AppError::FileNotFound WITH (@EXPR_STRING_INTERPOLATE "Key '{}' not found" vars=(search_key))
            @ENDRESULT
        @ENDPATTERN_MATCH
        
        // Use of closures
        @VAR item_count @INT = @CALL_METHOD @PROPERTY_GET data_store.data.len WITH ()
        @VAR status @STRING = @IF @EXPR_COMPARE type=">" operands=(item_count, 10)
          "Complete"
        @ELIF @EXPR_COMPARE type=">" operands=(item_count, 0)
          "In progress"
        @ELSE
          "Empty"
        @ENDIF
        
        // Using variable for serialization
        @VAR serialized @STRING = @CALL_METHOD data_store.serialize WITH ()
        
        @RETURN @RESULT
          @SUCCESS @EXPR_STRING_INTERPOLATE "Status: {}, Metadata: {}, Details: {}" vars=(status, metadata, serialized)
        @ENDRESULT
      @END
    @ENDFUNC

    // Main function to demonstrate usage
    @FUNC main() -> @RESULT_TYPE ok=@VOID error=@BOX of=@DYNAMIC_TRAIT Error
      @BODY
        // Create some sample data
        @VAR items @LIST of=@INT = @CALL vec WITH (1, 2, 3, 4, 5)
        
        // Lifetimes in action
        @VAR data_store DataStore<'a, @INT> mutable=true = @CALL DataStore::new WITH ("Sample Store".to_string())
        
        // Builder pattern
        @VAR data_store DataStore<'a, @INT> = @CALL_METHOD data_store.with_metadata WITH (
          "created".to_string(), 
          "today".to_string()
        )
        @ASSIGN data_store <- @CALL_METHOD data_store.with_metadata WITH (
          "owner".to_string(), 
          "me".to_string()
        )
        
        // Error handling with ?
        @LOOP type="for" var=item in=items
          @VAR cloned_store DataStore<'a, @INT> mutable=true = @CALL_METHOD data_store.clone WITH ()
          @EXPR_ERROR_PROPAGATION @CALL_METHOD cloned_store.add_item WITH (item)
          
          // Closures and higher-order functions
          @VAR found @OPTIONAL of=@OWNERSHIP type="ref" @INT = @CALL_METHOD cloned_store.find_item WITH (
            @LAMBDA (i) -> @BOOLEAN
              @BODY
                @RETURN @EXPR_COMPARE type=">" operands=(i, 2)
              @END
            @ENDLAMBDA
          )
          
          @MATCH found
            @CASE Some(value)
              @CALL println WITH ("Found value: {:?}", value)
            @DEFAULT
              // No operation for None case
          @ENDMATCH
        @ENDLOOP
        
        // Pattern matching
        @PATTERN_MATCH @CALL process_data WITH (data_store, "owner")
          @PATTERN Ok(result)
            @CALL println WITH ("Process result: {}", result)
          @PATTERN Err(AppError::FileNotFound(path))
            @CALL eprintln WITH ("Missing file: {}", path)
          @PATTERN Err(AppError::ParseError(details))
            @CALL eprintln WITH ("Parse error: {}", details)
          @PATTERN Err(AppError::NetworkError { code, message })
            @CALL eprintln WITH ("Network error {} - {}", code, message)
        @ENDPATTERN_MATCH
        
        @RETURN @RESULT
          @SUCCESS @VOID
        @ENDRESULT
      @END
    @ENDFUNC
  @ENDMODULE
@END