# LD Core Logic Generation Prompt — NL-S

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
1. Each block has exactly one current-flow input pin named 'EN' and exactly one current-flow output pin named 'ENO'. Each block may have zero or more data input pins and zero or more data output pins.
2. A block is executed when its 'EN' pin is TRUE and is not executed when its 'EN' pin is FALSE.
3. Current-flow starts at 'Power_Positive' and ends at 'Power_Negative'. It enters a block through its 'EN' pin and exits the block through its 'ENO' pin.
4. An 'EN' pin can only receive a current connection from an 'ENO' pin or 'Power_Positive', and it cannot receive a connection from any other type of pin. An 'ENO' pin can only connect to an 'EN' pin or 'Power_Negative', and it cannot connect to any other type of pin.
5. Each parallel branch must contain at least one block. Closed current-flow loops are prohibited.
6. Each current-flow path must form a complete path from 'Power_Positive' to 'Power_Negative', and all blocks must be included in a current-flow path. A circuit consists of ladder rungs, and each block can only be contained in a single ladder rung.
7. A contact can block current-flow. Blocks other than contacts do not block current-flow. If the 'EN' pin of a block is FALSE, its 'ENO' pin is set to FALSE.
8. A connection is either a current-flow connection or a data connection. Current-flow connections are allowed only between current-flow pins, and data connections are allowed only between data pins. Current-flow pins and data pins cannot be connected to each other.
9. An output data pin can only write to an intermediate variable, and an intermediate variable can only be read by an input data pin. Therefore, an output data pin of one block must not be connected directly to an input data pin of another block.
10. Contacts and coils are blocks. A contact has exactly one 'EN' pin, exactly one 'ENO' pin, and exactly one input data pin named 'VAR'. A coil has exactly one 'EN' pin, exactly one 'ENO' pin, and exactly one output data pin named 'VAR'.

# Circuit BNF
circuit:
    blocks_section current_connections_section data_connections_section
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
current_connections_section:
    '# current connections' current_connection_list
;
current_connection_list:
    current_connection
|   current_connection_list current_connection
;
current_connection:
    start_flow
|   flow
|   end_flow
;
start_flow:
    'Power_Positive ->' flow_dest ';'
;
flow:
    flow_src '->' flow_dest ';'
;
end_flow:
    flow_src '-> Power_Negative;'
;
flow_src:
    block_id '.ENO'
;
flow_dest:
    block_id '.EN'
;
data_connections_section:
    '# data connections' connection_list
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

# Refined Requirement
{refined_requirement}

# Examples
{examples}
```
