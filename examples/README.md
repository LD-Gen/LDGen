# Few-shot Example Files

| Stage | Configuration(s) | Example file | Number of examples |
|---|---|---|---:|
| LD Core Logic Generation | T-S, NL-S | `ld_core_logic_separated.md` | 4 |
| LDText Code Generation | T-S, NL-S | `ldtext_separated.md` | 4 |
| LD Core Logic Generation | T-NS, NL-NS | `ld_core_logic_non_separated.md` | 4 |
| LDText Code Generation | T-NS, NL-NS | `ldtext_non_separated.md` | 4 |
| FBD Core Logic Generation | FBD | `fbd_core_logic.md` | 1 |
| FBDText Code Generation | FBD | `fbdtext.md` | 1 |

Core-logic examples are stored as `<Refined Requirement, Core Logic>` pairs.

LD code-generation examples are stored as `<Requirement, LDText>` pairs, and the FBD code-generation example is stored as a `<Requirement, FBDText>` pair. BNF rules are supplied by the corresponding prompt templates and are not repeated in the example files.
