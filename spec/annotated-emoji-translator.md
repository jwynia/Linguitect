# Program declaration with metadata
📦🔄📝1.0👤🧑📜🌐  # Program ELSTranslator with version 1.0, author "Translator", license "MIT"

# Main translation function: takes ELS code and returns standard Linguitect
🔧🔍(📜)→📝{  # Function translate(text: string) -> string
  # Process each line and join results
  📋👆↔️:📜.↪️("\n")→📚  # Map process() over each line after splitting by newlines
  🔙📚.🔗("\n")  # Return lines joined with newlines
}

# Process a single line of ELS code
🔧↔️(📜)→📝{  # Function process(line: string) -> string
  📝⚡=""  # Variable result: string = "" - will hold the translated line
  
  # Determine construct type and call appropriate translator
  ❔👆👁️(📜,"📦"){  # If line startsWith("📦") - Program declaration
    📝⚡←👆📦(📜)  # result = translateProgram(line)
  }↪️👆👁️(📜,"🗃️"){  # Else if line startsWith("🗃️") - Module declaration
    📝⚡←👆🗃(📜)  # result = translateModule(line)
  }↪️👆👁️(📜,"🔧"){  # Else if line startsWith("🔧") - Function declaration
    📝⚡←👆⚙️(📜)  # result = translateFunc(line)
  }↪️👆👁️(📜,"🏛️"){  # Else if line startsWith("🏛️") - Class declaration
    📝⚡←👆🏢(📜)  # result = translateClass(line)
  }↪️👆👁️(📜,"📝")||👆👁️(📜,"📌"){  # Else if line startsWith("📝") or startsWith("📌") - Variable/constant
    📝⚡←👆📄(📜)  # result = translateVar(line)
  }↪️👆✨(📜){  # Else if isExpression(line)
    📝⚡←👆💭(📜)  # result = translateExpr(line)
  }🔄{  # Else
    📝⚡←📜  # result = line (pass through unchanged)
  }
  
  🔙⚡  # Return the translated result
}

# Check if line starts with given prefix
🔧👁️(📜,🪄)→✅{  # Function startsWith(line: string, prefix: string) -> boolean
  🔙📜.↩️(0)==🪄  # Return line.charAt(0) == prefix
}

# Translate program declaration
🔧📦(📜)→📝{  # Function translateProgram(line: string) -> string
  # Extract program components from the emoji syntax
  📝⚡←👆🔎(📜,"📦",["📝","👤","📜"])  # name = extractText between 📦 and 📝/👤/📜
  📝📱←❓👆🪞(📜,"📝")?👆🔎(📜,"📝",["👤","📜"]):""  # version (if present)
  📝👤←❓👆🪞(📜,"👤")?👆🔎(📜,"👤",["📜"]):""  # author (if present)
  📝📃←❓👆🪞(📜,"📜")?👆🔎(📜,"📜",[]):""  # license (if present)
  
  # Build standard Linguitect program declaration
  📝⭐←"@PROGRAM "+⚡+"\n"  # Start with @PROGRAM tag
  ❔📱≠""{  # If version exists
    📝⭐←⭐+"  @VERSION \""+📱+"\"\n"  # Add VERSION tag
  }
  ❔👤≠""{  # If author exists
    📝⭐←⭐+"  @AUTHOR \""+👤+"\"\n"  # Add AUTHOR tag
  }
  ❔📃≠""{  # If license exists
    📝⭐←⭐+"  @LICENSE \""+📃+"\"\n"  # Add LICENSE tag
  }
  📝⭐←⭐+"@END"  # Add closing @END tag
  
  🔙⭐  # Return the translated program declaration
}

# Translate module declaration
🔧🗃(📜)→📝{  # Function translateModule(line: string) -> string
  # Extract module components
  📝⚡←👆🔎(📜,"🗃️",["⬆️","{"])  # Extract text after 🗃️ before ⬆️ or {
  📝🏆←⚡  # Default: assume extracted text is just the name
  📝🆙←""  # Default: no parent namespace
  
  # Handle parent namespace if present (indicated by ⬆️)
  ❔👆🪞(📜,"⬆️"){  # If the line contains ⬆️ (extends/parent emoji)
    📝🏆←👆🔎(📜,"🗃️",["⬆️"])  # name = text between 🗃️ and ⬆️
    📝🆙←👆🔎(📜,"⬆️",["{"])  # parent = text between ⬆️ and {
  }
  
  # Extract and translate module body
  📝📂←👆📁(📜)  # Extract content between { }
  📝📊←👆🔍(📂)  # Recursively translate the body
  
  # Build standard Linguitect module declaration
  📝🎪←"@MODULE "+🏆  # Start with @MODULE tag and name
  ❔🆙≠""{  # If parent namespace exists
    📝🎪←🎪+" "+🆙  # Add parent namespace
  }
  📝🎪←🎪+"\n"+📊+"\n@ENDMODULE"  # Add body and closing tag
  
  🔙🎪  # Return the translated module
}

# Translate function declaration
🔧⚙️(📜)→📝{  # Function translateFunc(line: string) -> string
  # Extract function components
  📝🚩←👆🔎(📜,"🔧","→")  # Extract name and parameters before →
  📝📛←🚩.↪️("(")[0]  # name = text before (
  📝🔣←🚩.⏩(🚩.📍("(")+1,🚩.🔙(")"))  # params between ()
  📝🔙←👆🔎(📜,"→","{")  # Return type = text between → and {
  
  # Extract and prepare function body
  📝📂←👆📁(📜)  # Extract content between { }
  
  # Build standard Linguitect function declaration
  📝🎫←"@FUNC "+📛+"("+👆🎲(🔣)+") -> "+👆🎨(🔙)+"\n"
  
  # Handle docstring if present
  ❔📂.📍("📝")>=0&&!👆👁️(📂,"📦"){  # If body has 📝 (documentation) not at beginning of body block
    📝📗←👆🔎(📂,"📝",["⚠️","✅","💥","📦"])  # Extract docs before other tags
    ❔📗≠""{  # If docs were found
      📝🎫←🎫+"  @DOCS "+📗+"\n"  # Add DOCS tag
    }
  }
  
  # Handle preconditions if present
  ❔📂.📍("⚠️")>=0{  # If body has ⚠️ (requires/preconditions)
    📝⚠️←👆🔎(📂,"⚠️",["✅","💥","📦"])  # Extract preconditions
    ❔⚠️≠""{  # If preconditions were found
      📝🎫←🎫+"  @REQUIRES "+⚠️+"\n"  # Add REQUIRES tag
    }
  }
  
  # Handle postconditions if present
  ❔📂.📍("✅")>=0&&!👆👁️(📂,"✅"){  # If body has ✅ (ensures/postconditions) not at beginning
    📝🏁←👆🔎(📂,"✅",["💥","📦"])  # Extract postconditions
    ❔🏁≠""{  # If postconditions were found
      📝🎫←🎫+"  @ENSURES "+🏁+"\n"  # Add ENSURES tag
    }
  }
  
  # Handle throws if present
  ❔📂.📍("💥")>=0&&!👆👁️(📂,"💥"){  # If body has 💥 (throws) not at beginning
    📝💣←👆🔎(📂,"💥",["📦"])  # Extract throws
    ❔💣≠""{  # If throws were found
      📝🎫←🎫+"  @THROWS "+💣+"\n"  # Add THROWS tag
    }
  }
  
  # Handle main function body
  ❔📂.📍("📦{")>=0{  # If body has 📦{ (body block)
    📝📦←👆📁(📂.⏩(📂.📍("📦{")))  # Extract content of body block
    📝📱←👆🔍(📦)  # Translate the body content
    📝🎫←🎫+"  @BODY\n"+📱+"\n  @END"  # Add BODY tags and content
  }
  
  📝🎫←🎫+"\n@ENDFUNC"  # Add closing ENDFUNC tag
  
  🔙🎫  # Return the translated function
}

# Translate class declaration
🔧🏢(📜)→📝{  # Function translateClass(line: string) -> string
  # Extract class components
  📝🚩←👆🔎(📜,"🏛️",["{"])  # Extract name and inheritance info
  📝🏠←🚩  # Default: assume extracted text is just the name
  📝🏗️←""  # Default: no parent classes
  📝🏭←""  # Default: no implemented interfaces
  
  # Parse inheritance (extends, implements)
  ❔🚩.📍("⬆️")>=0{  # If includes ⬆️ (extends emoji)
    📝🏠←👆🔎(🚩,"","⬆️")  # name = text before ⬆️
    📝🏢←🚩.⏩(🚩.📍("⬆️")+1)  # Get text after ⬆️
    
    # Handle extends
    ❔👆👁️(🏢,"("){  # If remaining starts with ( - list of parent classes
      📝🏗️←🏢.⏩(1,🏢.📍(")"))  # extends = text between ( )
      📝🏢←🏢.⏩(🏢.📍(")")+1)  # Update remaining text
    }
    
    # Handle implements
    ❔🏢.📍("🔄")>=0{  # If includes 🔄 (implements emoji)
      📝🏭←🏢.⏩(🏢.📍("🔄")+3)  # Get text after 🔄
      ❔👆👁️(🏭,"("){  # If implements list is in parentheses
        📝🏭←🏭.⏩(1,🏭.📍(")"))  # Extract interfaces list
      }
    }
  }
  
  # Extract and translate class body
  📝📂←👆📁(📜)  # Extract content between { }
  📝🏮←👆🎇(📂)  # Process class members
  
  # Build standard Linguitect class declaration
  📝🏫←"@CLASS "+🏠  # Start with @CLASS tag and name
  ❔🏗️≠""{  # If parent classes exist
    📝🏫←🏫+" EXTENDS "+🏗️  # Add EXTENDS clause
  }
  ❔🏭≠""{  # If interfaces exist
    📝🏫←🏫+" IMPLEMENTS "+🏭  # Add IMPLEMENTS clause
  }
  
  📝🏫←🏫+"\n"+🏮+"\n@ENDCLASS"  # Add body and closing tag
  
  🔙🏫  # Return the translated class
}

# Translate variable/constant declaration
🔧📄(📜)→📝{  # Function translateVar(line: string) -> string
  📝🔒←👆👁️(📜,"📌")  # Check if this is a constant (📌) vs variable (📝)
  📝🔑←❓🔒?"@CONST":"@VAR"  # Choose appropriate tag
  
  # Parse components
  📝🔏←📜.⏩(1).↪️(",")  # Split into parts after removing first char
  📝🔐←🔏[0]  # First part is the name
  📝🔓←❓🔏.📏>1?🔏[1]:""  # Second part (if exists) is type
  📝🔎←❓!🔒&&🔏.📏>2&&🔏[2]==="✓"  # Check if mutable flag (✓) is present
  
  # Handle value if present
  📝🔍←📜.📍("=")  # Find = for value assignment
  📝🔆←❓🔍>0?📜.⏩(🔍+1):""  # Extract value after =
  
  # Build standard Linguitect variable/constant declaration
  📝🔅←🔑+" "+🔐+" "+👆🎨(🔓)  # Start with tag, name, type
  ❔🔎{  # If mutable flag is present
    📝🔅←🔅+" mutable=true"  # Add mutable attribute
  }
  ❔🔆≠""{  # If value exists
    📝🔅←🔅+" = "+🔆  # Add value assignment
  }
  
  🔙🔅  # Return the translated variable/constant
}

# Check if line is an expression
🔧✨(📜)→✅{  # Function isExpr(line: string) -> boolean
  # Check if line starts with any expression operator emoji
  🔙👆👁️(📜,"➕")||👆👁️(📜,"➖")||👆👁️(📜,"✖️")||
     👆👁️(📜,"➗")||👆👁️(📜,"📏")||👆👁️(📜,"💪")||
     👆👁️(📜,"⚖️")||👆👁️(📜,"🚫⚖️")||👆👁️(📜,"◀️")||
     👆👁️(📜,"▶️")||👆👁️(📜,"🔗")||👆👁️(📜,"🔀")||
     👆👁️(📜,"❗")||👆👁️(📜,"❓")||👆👁️(📜,"🧬")
}

# Translate expressions (arithmetic, comparison, logical, etc.)
🔧💭(📜)→📝{  # Function translateExpr(line: string) -> string
  📝🚁←""  # Expression category (arithmetic, compare, logical)
  📝🚂←""  # Operator type (+, -, ==, etc.)
  
  # Determine expression type and operator based on starting emoji
  ❔👆👁️(📜,"➕"){  # Addition
    📝🚁←"arithmetic"
    📝🚂←"+"
  }↪️👆👁️(📜,"➖"){  # Subtraction
    📝🚁←"arithmetic"
    📝🚂←"-"
  }↪️👆👁️(📜,"✖️"){  # Multiplication
    📝🚁←"arithmetic"
    📝🚂←"*"
  }↪️👆👁️(📜,"➗"){  # Division
    📝🚁←"arithmetic"
    📝🚂←"/"
  }↪️👆👁️(📜,"📏"){  # Modulo
    📝🚁←"arithmetic"
    📝🚂←"%"
  }↪️👆👁️(📜,"💪"){  # Power
    📝🚁←"arithmetic"
    📝🚂←"**"
  }↪️👆👁️(📜,"⚖️"){  # Equal
    📝🚁←"compare"
    📝🚂←"=="
  }↪️👆👁️(📜,"🚫⚖️"){  # Not equal
    📝🚁←"compare"
    📝🚂←"!="
  }↪️👆👁️(📜,"◀️⚖️"){  # Less than or equal
    📝🚁←"compare"
    📝🚂←"<="
  }↪️👆👁️(📜,"▶️⚖️"){  # Greater than or equal
    📝🚁←"compare"
    📝🚂←">="
  }↪️👆👁️(📜,"◀️"){  # Less than
    📝🚁←"compare"
    📝🚂←"<"
  }↪️👆👁️(📜,"▶️"){  # Greater than
    📝🚁←"compare"
    📝🚂←">"
  }↪️👆👁️(📜,"🔗"){  # Logical AND
    📝🚁←"logical"
    📝🚂←"AND"
  }↪️👆👁️(📜,"🔀"){  # Logical OR
    📝🚁←"logical"
    📝🚂←"OR"
  }↪️👆👁️(📜,"❗"){  # Logical NOT
    📝🚁←"logical"
    📝🚂←"NOT"
  }
  
  # Handle standard expression types (arithmetic, comparison, logical)
  ❔🚁≠""{  # If a standard expression type was identified
    📝🚁←📜.📍("(")  # Find opening parenthesis position
    📝🚂←📜.🔙(")")  # Find closing parenthesis position
    📝🚃←📜.⏩(🚁+1,🚂)  # Extract operands between parentheses
    
    # Build appropriate standard Linguitect expression based on type
    ❔🚁==="arithmetic"{
      🔙"@EXPR_ARITHMETIC type=\""+🚂+"\" operands=("+🚃+")"
    }↪️🚁==="compare"{
      🔙"@EXPR_COMPARE type=\""+🚂+"\" operands=("+🚃+")"
    }↪️🚁==="logical"{
      🔙"@EXPR_LOGICAL type=\""+🚂+"\" operands=("+🚃+")"
    }
  }↪️👆👁️(📜,"❓("){  # Handle ternary expression
    📝🚁←📜.⏩(2,📜.📏-1).↪️(",")  # Split into condition, then, else parts
    🔙"@EXPR_TERNARY "+🚁[0]+" "+🚁[1]+" "+🚁[2]  # Build ternary expression
  }↪️👆👁️(📜,"🧬"){  # Handle cast expression
    📝🚁←📜.⏩(📜.📍("(")+1,📜.🔙(")")).↪️(",")  # Split into expr, type, explicit parts
    📝🚂←🚁.📏>2&&🚁[2]==="✓"  # Check if explicit flag is present
    🔙"@EXPR_CAST "+🚁[0]+" "+🚁[1]+" explicit="+🚂  # Build cast expression
  }
  
  🔙📜  # If not recognized, return unchanged
}

# Translate function parameters
🔧🎲(📝🎯)→📝{  # Function translateParams(params: string) -> string
  ❔🎯===""{  # If no parameters
    🔙""  # Return empty string
  }
  
  # Map over each parameter and join with commas
  📋👆🎮:🎯.↪️(",")→🎰  # Map translateParam over each param after splitting
  🔙🎰.🔗(", ")  # Join with comma+space
}

# Translate a single parameter
🔧🎮(📝🎯)→📝{  # Function translateParam(param: string) -> string
  📝🎲←🎯.↪️(",")  # Split parameter by commas
  📝🎪←🎲[0]  # First part is the name
  📝🎭←❓🎲.📏>1?👆🎨(🎲[1]):""  # Second part (if exists) is type
  📝🎟️←🎯.📍("=")>=0  # Check if parameter has default value
  📝🎪←❓🎟️?🎯.⏩(🎯.📍("=")+1):""  # Extract default value if present
  
  # Build standard Linguitect parameter
  📝🎭←🎪  # Start with parameter name
  ❔🎭≠""{  # If type exists
    📝🎭←🎭+": "+🎭  # Add type annotation
  }
  ❔🎟️{  # If default value exists
    📝🎭←🎭+" = "+🎪  # Add default value
  }
  
  🔙🎭  # Return the translated parameter
}

# Translate type representations
🔧🎨(📝🎨)→📝{  # Function translateType(type: string) -> string
  # Handle primitive types
  ❔🎨==="🔢"{  # Integer
    🔙"int"
  }↪️🎨==="💲"{  # Float
    🔙"float"
  }↪️🎨==="✅"{  # Boolean
    🔙"boolean"
  }↪️🎨==="📝"{  # String
    🔙"string"
  }↪️🎨==="⚫"{  # Void
    🔙"void"
  }↪️🎨==="⛔"{  # Null
    🔙"null"
  }↪️🎨==="❓"{  # Undefined
    🔙"undefined"
  }↪️🎨==="🃏"{  # Any
    🔙"any"
  }
  
  # Handle compound types
  ❔👆👁️(🎨,"📋"){  # List
    📝🎭←🎨.⏩(1)  # Extract element type after 📋
    🔙"@LIST of="+👆🎨(🎭)  # Translate to list type
  }↪️👆👁️(🎨,"🗺️"){  # Map
    📝🎪←🎨.⏩(2).↪️(",")  # Extract key and value types
    🔙"@MAP key="+👆🎨(🎪[0])+" value="+👆🎨(🎪[1])  # Translate to map type
  }
  
  🔙🎨  # If not recognized, return unchanged
}

# Extract text between markers
🔧🔎(📝🔎,📝🔦,📝🔭)→📝{  # Function extractText(text,start,endMarkers) -> string
  📝🔮←❓🔦===""?0:🔎.📍(🔦)+🔦.📏  # Find start position (after prefix)
  📝🔯←🔎.📏  # Default end is end of text
  
  # Find the closest end marker after start position
  🔁i:0→🔭.📏{  # Loop through each end marker
    ❔🔎.📍(🔭[i])>=0{  # If this marker exists in text
      📝🕯️←🔎.📍(🔭[i])  # Find position of marker
      ❔🕯️>🔮&&🕯️<🔯{  # If marker is after start and closer than current end
        📝🔯←🕯️  # Update end position to this marker
      }
    }
  }
  
  🔙🔎.⏩(🔮,🔯)  # Return text between start and end positions
}

# Extract content between braces { }
🔧📁(📝🔎)→📝{  # Function extractBody(text: string) -> string
  📝🔦←🔎.📍("{")  # Find opening brace
  📝🔭←👆📟(🔎,🔦)  # Find matching closing brace
  
  ❔🔦>=0&&🔭>🔦{  # If valid braces were found
    🔙🔎.⏩(🔦+1,🔭)  # Return content between them
  }
  
  🔙""  # If no valid braces, return empty string
}

# Find matching closing brace for an opening brace
🔧📟(📝🔎,📝🔦)→🔢{  # Function findMatchingBrace(text,openIndex) -> int
  📝🔭←1  # Brace nesting counter (1 for the opening brace we start with)
  📝i←🔦+1  # Start searching after the opening brace
  
  🔁?i<🔎.📏&&🔭>0{  # While we haven't reached end and haven't found match
    ❔🔎.↩️(i)==="{"{  # If found another opening brace
      📝🔭←🔭+1  # Increment nesting counter
    }↪️🔎.↩️(i)==="}"{  # If found closing brace
      📝🔭←🔭-1  # Decrement nesting counter
    }
    
    ❔🔭===0{  # If nesting counter reached zero (found match)
      🔙i  # Return current position
    }
    
    📝i←i+1  # Move to next character
  }
  
  🔙-1  # If no match found, return -1
}

# Check if text contains a symbol
🔧🪞(📝🔎,📝🔦)→✅{  # Function containsSymbol(text,symbol) -> boolean
  🔙🔎.📍(🔦)>=0  # Return true if text includes symbol (indexOf >= 0)
}

# Process class body content
🔧🎇(📝🔎)→📝{  # Function processClassBody(body: string) -> string
  📝🔭←🔎.↪️("\n")  # Split body into lines
  📝🔮←""  # Initialize result
  
  # Process each line in class body
  🔁i:0→🔭.📏{  # For each line
    📝🔯←🔭[i].⭐()  # Trim whitespace
    ❔🔯≠""{  # If line is not empty
      📝📟←👆↔️(🔯)  # Translate the line
      🔮←🔮+"  "+📟+"\n"  # Add to result with indentation
    }
  }
  
  🔙🔮  # Return processed class body
}