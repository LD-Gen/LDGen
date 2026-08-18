# LD Core Logic Generation Prompt — T-S

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
The LD rules are represented as triplets in the form (A)-[B]->(C), where A represents the subject, B represents the relation, and C represents the object.
(Block)-[HAS_CURRENT_INPUT]->(EN)
(Block)-[HAS_CURRENT_OUTPUT]->(ENO)
(Block)-[HAS_DATA_INPUT]->(DataInputPin)
(Block)-[HAS_DATA_OUTPUT]->(DataOutputPin)
(Block)-[HAS_EXACTLY_ONE]->(EN)
(Block)-[HAS_EXACTLY_ONE]->(ENO)
(Block)-[HAS_ZERO_OR_MORE]->(DataInputPin)
(Block)-[HAS_ZERO_OR_MORE]->(DataOutputPin)
(Block)-[EXECUTES_WHEN]->(EN_IsTrue)
(Block)-[SKIPS_WHEN]->(EN_IsFalse)
(CurrentFlow)-[STARTS_AT]->(Power_Positive)
(CurrentFlow)-[ENDS_AT]->(Power_Negative)
(CurrentFlow)-[ENTERS_BLOCK_AT]->(EN)
(CurrentFlow)-[EXITS_BLOCK_AT]->(ENO)
(EN)-[ACCEPTS_CONNECTION_FROM]->(ENO)
(EN)-[ACCEPTS_CONNECTION_FROM]->(Power_Positive)
(EN)-[REJECTS_CONNECTION_FROM]->(OtherPins)
(ENO)-[ACCEPTS_CONNECTION_TO]->(EN)
(ENO)-[ACCEPTS_CONNECTION_TO]->(Power_Negative)
(ENO)-[REJECTS_CONNECTION_TO]->(OtherPins)
(ParallelBranch)-[MUST_CONTAIN]->(Block)
(Circuit)-[PROHIBITS]->(ClosedCurrentPath)
(CurrentFlow)-[MUST_BE_CLOSED]->(True)
(CurrentFlow)-[CONTAINS_ALL]->(Blocks)
(Circuit)-[CONSISTS_OF]->(LadderRung)
(Block)-[MUST_BE_CONTAINED_IN]->(SingleLadderRung)
(CurrentFlow)-[CAN_BE_BLOCKED_BY]->(Contact)
(CurrentFlow)-[ALWAYS_ALLOWED_BY]->(Blocks_Except_Contact)
(EN_IsFalse)-[SETS]->(ENO_IsFalse)
(Connection)-[HAS_TYPE]->(CurrentFlow)
(Connection)-[HAS_TYPE]->(DataConnection)
(CurrentFlow)-[ALLOWS_ONLY]->(CurrentPin_To_CurrentPin)
(DataConnection)-[ALLOWS_ONLY]->(DataPin_To_DataPin)
(OutputDataPin)-[CAN_ONLY_WRITE_TO]->(IntermediateVariable)
(IntermediateVariable)-[CAN_ONLY_READ_BY]->(InputDataPin)
(InputDataPin)-[MUST_NOT_CONNECT_DIRECTLY_TO]->(OutputDataPin)
(Contact)-[IS_A]->(Block)
(Coil)-[IS_A]->(Block)
(Contact)-[HAS_EXACTLY_ONE]->(EN)
(Contact)-[HAS_EXACTLY_ONE]->(ENO)
(Contact)-[HAS_EXACTLY_ONE_INPUT_DATAPIN]->(VAR)
(Coil)-[HAS_EXACTLY_ONE]->(EN)
(Coil)-[HAS_EXACTLY_ONE]->(ENO)
(Coil)-[HAS_EXACTLY_ONE_OUTPUT_DATAPIN]->(VAR)

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
