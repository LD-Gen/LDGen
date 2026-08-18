# LD Core Logic Generation Prompt — T-NS

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
(Contact)-[IS_A]->(Block)
(Coil)-[IS_A]->(Block)
(Function)-[IS_A]->(Block)
(FunctionBlock)-[IS_A]->(Block)
(Contact)-[HAS_EXACTLY_ONE_CURRENT_INPUT]->(EN)
(Contact)-[HAS_EXACTLY_ONE_CURRENT_OUTPUT]->(ENO)
(Contact)-[HAS_EXACTLY_ONE_INPUT_DATAPIN]->(VAR)
(Coil)-[HAS_EXACTLY_ONE_CURRENT_INPUT]->(EN)
(Coil)-[HAS_EXACTLY_ONE_CURRENT_OUTPUT]->(ENO)
(Coil)-[HAS_EXACTLY_ONE_OUTPUT_DATAPIN]->(VAR)
(Function)-[MAY_USE_CURRENT_INPUT]->(EN)
(Function)-[MAY_USE_CURRENT_INPUT]->(BOOL_DataInputPin)
(Function)-[MAY_USE_CURRENT_OUTPUT]->(ENO)
(Function)-[MAY_USE_CURRENT_OUTPUT]->(BOOL_DataOutputPin)
(FunctionBlock)-[MAY_USE_CURRENT_INPUT]->(EN)
(FunctionBlock)-[MAY_USE_CURRENT_INPUT]->(BOOL_DataInputPin)
(FunctionBlock)-[MAY_USE_CURRENT_OUTPUT]->(ENO)
(FunctionBlock)-[MAY_USE_CURRENT_OUTPUT]->(BOOL_DataOutputPin)
(FunctionOrFunctionBlockWithoutBOOLInput)-[USES_CURRENT_INPUT]->(EN)
(FunctionOrFunctionBlockWithoutBOOLOutput)-[USES_CURRENT_OUTPUT]->(ENO)
(CurrentInputPin)-[CAN_BE]->(EN)
(CurrentInputPin)-[CAN_BE]->(BOOL_DataInputPin)
(CurrentOutputPin)-[CAN_BE]->(ENO)
(CurrentOutputPin)-[CAN_BE]->(BOOL_DataOutputPin)
(CurrentFlow)-[STARTS_AT]->(Power_Positive)
(CurrentFlow)-[ENDS_AT]->(Power_Negative)
(CurrentFlow)-[ENTERS_BLOCK_AT]->(CurrentInputPin)
(CurrentFlow)-[EXITS_BLOCK_AT]->(CurrentOutputPin)
(CurrentFlowConnection)-[USES_OPERATOR]->(->)
(DataConnection)-[USES_OPERATOR]->(TO)
(CurrentFlowConnection)-[ALLOWS]->(Power_Positive_To_CurrentInputPin)
(CurrentFlowConnection)-[ALLOWS]->(CurrentOutputPin_To_CurrentInputPin)
(CurrentFlowConnection)-[ALLOWS]->(CurrentOutputPin_To_Power_Negative)
(DataConnection)-[ALLOWS]->(Source_To_DataInputPin)
(DataConnection)-[ALLOWS]->(DataOutputPin_To_Identifier)
(DataConnectionSource)-[CAN_BE]->(Identifier)
(DataConnectionSource)-[CAN_BE]->(Immediate)
(BOOL_DataInputPin)-[MAY_CARRY]->(CurrentFlow)
(BOOL_DataOutputPin)-[MAY_CARRY]->(CurrentFlow)
(NonBOOL_DataPin)-[MUST_NOT_CARRY]->(CurrentFlow)
(ParallelBranch)-[MUST_CONTAIN]->(Block)
(Circuit)-[PROHIBITS]->(ClosedCurrentPath)
(CurrentFlow)-[MUST_BE_CLOSED]->(True)
(CurrentFlow)-[CONTAINS_ALL]->(Blocks)
(Circuit)-[CONSISTS_OF]->(LadderRung)
(Block)-[MUST_BE_CONTAINED_IN]->(SingleLadderRung)

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
