# LDText Code Generation Prompt — Separated (T-S / NL-S)

## System Prompt

```text
You are an expert in PLC programming and BNF-constrained code generation. Your task is to generate LDText code from the given requirement and Core Logic while strictly following the provided LDText BNF rules and their annotations. Preserve the logic modules and connection relationships specified in the Core Logic, and do not introduce additional logic or modify the given connections.
```

## User Prompt Template

```text
# Read all the text in the 'requirement', 'core logic', and 'BNF rules' together with their annotations.
1. Determine the program structure according to the requirement and BNF rules. Analyze the POU type, POU name, POU return type (if necessary), user-defined data type name (if necessary), and user-defined data type type (if necessary). The POU type must be one of 'FUNCTION', 'FUNCTION_BLOCK', and 'PROGRAM'. The user-defined data type must be one of 'STRUCT', 'ARRAY', and 'ENUM'.
2. Generate the required program structure, including the applicable POU header and tail, global variable header and tail, and user-defined data type header and tail, strictly according to the BNF rules and their annotations.
3. Analyze the input variables, output variables, and local variables according to the requirement and data connection relationships in the core logic, and fill the corresponding variable fields according to the BNF rules. Variable names and block names must not be identical.
4. Fill the 'BLOCKS' field according to the BNF rules. If a block has already been declared in the variable field, do not output it again in the 'BLOCKS' field.
5. Copy the current connections in the core logic into the 'FLOWS' field according to the BNF rules without changing the current connection relationships. Copy the data connections in the core logic into the 'CONNECTIONS' field according to the BNF rules without changing the data connection relationships.
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
    'LADDER' ladder_name flow_lists connection_lists 'END_LADDER'
;
ladder_name:
    identifier
;
flow_lists:
    'FLOWS' start_flow end_flow 'END_FLOWS'
|   'FLOWS' start_flow flow_list end_flow 'END_FLOWS'
;
start_flow:
    'Power_Positive ->' flow_dest ';'
;
end_flow:
    flow_src '-> Power_Negative;'
;
flow_list:
    flow
|   flow_list flow
;
flow:
    flow_src '->' flow_dest ';'
;
flow_src:
    block_id '.ENO'
;
flow_dest:
    block_id '.EN'
;
connection_lists:
    'CONNECTIONS' connection_list 'END_CONNECTIONS'
;
connection_list:
    connection
|   connection_list connection
;
connection:
    block_src 'TO' dest ';'
|   src 'TO' block_dest ';'
;
block_src:
    block_id '.' block_variable
;
block_dest:
    block_id '.' block_variable
;
block_variable:
    identifier
;
src:
    identifier
|   immediate
;
dest:
    identifier
;

# Examples:
{examples}
```
