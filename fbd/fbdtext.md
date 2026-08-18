# FBDText Code Generation Prompt

## System Prompt

```text
You are an expert in PLC programming and BNF-constrained code generation. Your task is to generate FBDText code from the given requirement and Core Logic while strictly following the provided FBDText BNF rules and their annotations. Preserve the symbols and connection relationships specified in the Core Logic, and do not introduce additional logic or modify the given connections.
```

## User Prompt Template

```text
# Read all the text in the 'requirement', 'core logic', and 'BNF rules' together with their annotations.
1. Determine the program structure according to the requirement and BNF rules. Analyze the POU type, POU name, POU return type (if necessary), user-defined data type name (if necessary), and user-defined data type (if necessary). The POU type must be one of 'FUNCTION', 'FUNCTION_BLOCK', and 'PROGRAM'. The user-defined data type must be one of 'STRUCT', 'ARRAY', and 'ENUM'.
2. Generate the required program structure, including the applicable POU header and tail, global variable header and tail, and user-defined data type header and tail, strictly according to the BNF rules and their annotations.
3. Analyze the input variables, output variables, and local variables according to the requirement and connection relationships in the Core Logic, and fill the corresponding variable fields according to the BNF rules. Variable names and symbol instance names must not be identical.
4. Fill the 'SYMBOLS' field according to the symbols specified in the Core Logic. Preserve the symbol instance names and symbol types without adding, deleting, or modifying any symbol.
5. Copy the connections specified in the Core Logic into the 'CONNECTIONS' field according to the BNF rules. Preserve all source and destination relationships without adding, deleting, or modifying any connection.
6. Output the complete FBDText code in Markdown format. Do not output explanations or any other information.

# Requirement
{requirement}

# Core Logic
{core_logic}

# FBDText BNF
program:
    'PROGRAM' identifier 'END_PROGRAM'
|   'PROGRAM' identifier program_content 'END_PROGRAM'
;
program_content:
    program_var_lists
|   symbol_instance_lists
|   symbol_instance_lists connection_lists
|   program_var_lists symbol_instance_lists
|   program_var_lists symbol_instance_lists connection_lists
;
function_block:
    'FUNCTION_BLOCK' identifier 'END_FUNCTION_BLOCK'
|   'FUNCTION_BLOCK' identifier function_block_content 'END_FUNCTION_BLOCK'
;
function_block_content:
    function_var_lists
|   symbol_instance_lists
|   symbol_instance_lists connection_lists
|   function_var_lists symbol_instance_lists
|   function_var_lists symbol_instance_lists connection_lists
;
function:
    'FUNCTION' identifier 'END_FUNCTION'
|   'FUNCTION' identifier ':' data_type 'END_FUNCTION'
|   'FUNCTION' identifier ':' 'VOID' 'END_FUNCTION'
|   'FUNCTION' identifier function_content 'END_FUNCTION'
|   'FUNCTION' identifier ':' data_type function_content 'END_FUNCTION'
|   'FUNCTION' identifier ':' 'VOID' function_content 'END_FUNCTION'
;
function_content:
    function_var_lists
|   symbol_instance_lists
|   symbol_instance_lists connection_lists
|   function_var_lists symbol_instance_lists
|   function_var_lists symbol_instance_lists connection_lists
;
symbol_instance_lists:
    'SYMBOLS' symbol_instance_list 'END_SYMBOLS'
;
symbol_instance_list:
    symbol_instance
|   symbol_instance_list symbol_instance
;
symbol_instance:
    symbol_id ':' symbol_name ';'
;
symbol_id:
    identifier
;
symbol_name:
    identifier
;
connection_lists:
    'CONNECTIONS' connection_list 'END_CONNECTIONS'
;
connection_list:
    connection
|   connection_list connection
;
connection:
    src 'TO' dest ';'
;
src:
    identifier
|   identifier '.' identifier
;
dest:
    identifier
|   identifier '.' identifier
;

# Examples
{examples}
```
