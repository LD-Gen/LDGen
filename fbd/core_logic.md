# FBD Core Logic Generation Prompt

## System Prompt

```text
You are an expert in IEC 61131-3 Function Block Diagram (FBD) logic design. Your task is to design the core logic of an FBD program according to the given requirement, FBD rules, and output grammar. Preserve the semantics of the refined requirement and use only the supported FBD functions and function blocks.
```

## User Prompt Template

```text
# Task Instruction
1. analyze the 'Refined Requirement'.
2. determine the symbols required for each refined state or condition and generate their data-flow connections according to the following FBD rules. The generated symbols and connections must satisfy the specified FBD rules and use only the supported functionality defined for the FBD implementation.

# FBD Rules
The FBD rules are represented as triplets in the form (A)-[B]->(C), where A denotes the source entity or concept, B denotes the relation, and C denotes the target entity or concept.
(Function)-[IS_A]->(Symbol)
(FunctionBlock)-[IS_A]->(Symbol)
(Symbol)-[HAS_ZERO_OR_MORE]->(DataInputPin)
(Symbol)-[HAS_ZERO_OR_MORE]->(DataOutputPin)
(DataInputPin)-[BELONGS_TO]->(Symbol)
(DataOutputPin)-[BELONGS_TO]->(Symbol)
(Connection)-[REPRESENTS]->(DataFlow)
(Connection)-[HAS_SOURCE]->(Source)
(Connection)-[HAS_DESTINATION]->(Destination)
(DataFlow)-[FLOWS_FROM]->(Source)
(DataFlow)-[FLOWS_TO]->(Destination)
(Source)-[CAN_BE]->(Variable)
(Source)-[CAN_BE]->(DataOutputPin)
(DataInputPin)-[MUST_NOT_BE_SOURCE]->(True)
(Destination)-[CAN_BE]->(Variable)
(Destination)-[CAN_BE]->(DataInputPin)
(DataOutputPin)-[MUST_NOT_BE_DESTINATION]->(True)
(DataOutputPin)-[CAN_CONNECT_TO]->(DataInputPin)
(DataOutputPin)-[CAN_CONNECT_TO]->(Variable)
(Variable)-[CAN_CONNECT_TO]->(DataInputPin)
(Variable)-[CAN_CONNECT_TO]->(Variable)
(DataInputPin)-[ACCEPTS_AT_MOST_ONE]->(IncomingConnection)
(DataOutputPin)-[MAY_CONNECT_TO]->(OneOrMoreDestinations)
(SymbolInstance)-[HAS_UNIQUE]->(SymbolId)
(SymbolInstance)-[HAS_TYPE]->(SymbolName)
(Circuit)-[CONTAINS]->(SymbolInstance)
(Circuit)-[CONTAINS]->(Connection)
(SymbolReference)-[MUST_REFER_TO]->(DeclaredSymbolInstance)
(Connection)-[MUST_REFERENCE]->(DeclaredSymbolOrVariable)

# Circuit BNF
circuit:
    symbols_section connections_section
;
symbols_section:
    '# symbols' symbol_instance_list
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
connections_section:
    '# connections' connection_list
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
|   symbol_pin
;
dest:
    identifier
|   symbol_pin
;
symbol_pin:
    symbol_id '.' pin_name
;
pin_name:
    identifier
;

# Refined Requirement
{refined_requirement}

# Examples
{examples}
```
