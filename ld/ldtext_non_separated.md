# LDText Code Generation Prompt — Non-separated (T-NS / NL-NS)

## System Prompt

```text
You are an expert in PLC programming and BNF-constrained code generation. Your task is to generate LDText code from the given requirement and Core Logic while strictly following the provided LDText BNF rules and their annotations. Preserve the logic modules and connection relationships specified in the Core Logic, and do not introduce additional logic or modify the given connections.
```

## User Prompt Template

```text
# Read all the text in the 'requirement', 'core logic', and 'BNF rules' together with their annotations.
1. Determine the program structure according to the requirement and BNF rules. Analyze the POU type, POU name, POU return type (if necessary), user-defined data type name (if necessary), and user-defined data type (if necessary). The POU type must be one of 'FUNCTION', 'FUNCTION_BLOCK', and 'PROGRAM'. The user-defined data type must be one of 'STRUCT', 'ARRAY', and 'ENUM'.
2. Generate the required program structure, including the applicable POU header and tail, global variable header and tail, and user-defined data type header and tail, strictly according to the BNF rules and their annotations.
3. Analyze the input variables, output variables, and local variables according to the requirement and connection relationships in the core logic, and fill the corresponding variable fields according to the BNF rules. Variable names and block names must not be identical.
4. Fill the 'BLOCKS' field according to the BNF rules. If a block has already been declared in the variable field, do not output it again in the 'BLOCKS' field.
5. Copy the connections in the core logic into the 'CONNECTIONS' field according to the BNF rules without changing the connection relationships.
6. Output the complete LDText code in Markdown format. Do not output explanations or any other information.

# Requirement
{requirement}

# Core Logic
{core_logic}

# LDText BNF
program:
    'PROGRAM' identifier 'END_PROGRAM'
|   'PROGRAM' identifier program_content 'END_PROGRAM'
;
program_content:
    program_var_lists
|   block_instance_lists ladder_list
|   program_var_lists block_instance_lists ladder_list
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
|   block_instance_lists ladder_list
|   function_var_lists block_instance_lists ladder_list
;
function_block:
    'FUNCTION_BLOCK' identifier 'END_FUNCTION_BLOCK'
|   'FUNCTION_BLOCK' identifier function_block_content 'END_FUNCTION_BLOCK'
;
function_block_content:
    function_var_lists
|   block_instance_lists ladder_list
|   function_var_lists block_instance_lists ladder_list
;
block_instance_lists:
    'BLOCKS' block_instance_list 'END_BLOCKS'
;
block_instance_list:
    block_instance
|   block_instance_list block_instance
;
block_instance:
    block_id ':' block_name ';'
;
block_id:
    identifier
;
block_name:
    identifier
;
ladder_list:
    ladder
|   ladder_list ladder
;
ladder:
    'LADDER' ladder_name connection_lists 'END_LADDER'
;
ladder_name:
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
    'Power_Positive ->' block_pin ';'
|   block_pin '->' block_pin ';'
|   block_pin '-> Power_Negative;'
|   block_pin 'TO' identifier ';'
|   src 'TO' block_pin ';'
;
block_pin:
    block_id '.' pin_name
;
pin_name:
    'EN'
|   'ENO'
|   block_variable
;
block_variable:
    identifier
;
src:
    identifier
|   immediate
;

# Examples
{non_separated_examples}
```
