# LD Core Logic Generation Prompt — NL-NS

## System Prompt

```text
You are an expert in IEC61131-3 Ladder Diagram (LD) logic design. Your task is to design the core logic of an LD program according to the given requirement, LD rules, and output grammar.
```

## User Prompt Template

```text
# Task Instruction
First, analyze the 'Refined Requirement'.
Second, determine the logic modules required for each refined state or condition and generate their energy-flow and data-flow connections according to the following LD rules.
The generated connections must satisfy the specified LD rules and use only the supported functionality defined for LDGen.

# LD Rules
1. Contacts and coils retain explicit current-flow pins. A contact has exactly one 'EN' pin, one 'ENO' pin, and one input data pin named 'VAR'. A coil has exactly one 'EN' pin, one 'ENO' pin, and one output data pin named 'VAR'.
2. A Function or Function Block may use 'EN' as its current-flow input. If it has a BOOL data input pin, one appropriate BOOL input pin may instead serve as the current-flow input. If no appropriate BOOL input exists, 'EN' is used.
3. A Function or Function Block may use 'ENO' as its current-flow output. If it has a BOOL data output pin, one appropriate BOOL output pin may instead serve as the current-flow output. If no appropriate BOOL output exists, 'ENO' is used.
4. Current-flow starts at 'Power_Positive' and ends at 'Power_Negative'. It enters each block through the selected current-flow input pin and exits through the selected current-flow output pin. Every block must belong to a complete current-flow path.
5. The operator '->' represents current-flow. It may connect 'Power_Positive' to a valid current-flow input pin, a valid current-flow output pin to a valid current-flow input pin, or a valid current-flow output pin to 'Power_Negative'. Only BOOL data pins of Functions or Function Blocks may substitute for 'EN' or 'ENO' in such connections.
6. The operator 'TO' represents a data connection. An identifier or immediate value may connect to a block data input pin, and a block data output pin may connect to an identifier.
7. A BOOL data pin used as a current-flow input or output participates in the current-flow path of that Function or Function Block. Non-BOOL data pins must not carry current-flow.
8. A contact may block current-flow according to its 'VAR' input. When a BOOL output pin of a Function or Function Block is used as its current-flow output, that BOOL result determines whether current-flow continues through the subsequent connection.
9. Each parallel branch must contain at least one block. Closed current-flow loops are prohibited. Each block can only be contained in a single ladder rung.

# Circuit BNF
circuit:
    blocks_section connections_section
;
blocks_section:
    '# blocks' block_instance_list
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
connections_section:
    '# connections' connection_list
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

# Refined Requirement
{refined_requirement}

# Examples
{examples}
```
