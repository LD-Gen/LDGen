# LDGen / FBD Prompt Templates

This repository contains the prompt templates and few-shot examples for LDGen and the FBD extension described in the paper.

## Directory structure

```text
common/
  requirement_refinement.md

ld/
  core_logic_T-S.md
  core_logic_NL-S.md
  core_logic_T-NS.md
  core_logic_NL-NS.md
  ldtext_separated.md
  ldtext_non_separated.md

fbd/
  core_logic.md
  fbdtext.md

examples/
  ld_core_logic_separated.md
  ld_core_logic_non_separated.md
  ldtext_separated.md
  ldtext_non_separated.md
  fbd_core_logic.md
  fbdtext.md
```

## Configuration mapping

| Configuration | Core-logic prompt | Code-generation prompt | Core-logic examples | Code-generation examples |
|---|---|---|---|---|
| T-S | `ld/core_logic_T-S.md` | `ld/ldtext_separated.md` | `examples/ld_core_logic_separated.md` | `examples/ldtext_separated.md` |
| NL-S | `ld/core_logic_NL-S.md` | `ld/ldtext_separated.md` | `examples/ld_core_logic_separated.md` | `examples/ldtext_separated.md` |
| T-NS | `ld/core_logic_T-NS.md` | `ld/ldtext_non_separated.md` | `examples/ld_core_logic_non_separated.md` | `examples/ldtext_non_separated.md` |
| NL-NS | `ld/core_logic_NL-NS.md` | `ld/ldtext_non_separated.md` | `examples/ld_core_logic_non_separated.md` | `examples/ldtext_non_separated.md` |
| FBD | `fbd/core_logic.md` | `fbd/fbdtext.md` | `examples/fbd_core_logic.md` | `examples/fbdtext.md` |

Both LD and FBD use `common/requirement_refinement.md` for requirement refinement.

## Few-shot examples

T-S and NL-S share four separated-connection examples. T-NS and NL-NS share four non-separated examples. The separated and non-separated configurations use different example sets consistent with their respective connection representations.

For separated LD Core Logic, current-flow and data-flow connections are placed in separate sections. For non-separated LD Core Logic, both appear in one `# connections` section: `->` is used for current-flow and `TO` is used for data connections. Contacts and coils retain explicit `EN`/`ENO` pins. For Functions and Function Blocks, an appropriate BOOL input or output pin may serve as the current-flow input or output in place of `EN` or `ENO`.

The code-generation example files are represented as `<Requirement, LDText>` or `<Requirement, FBDText>` pairs and do not repeat the BNF supplied by the corresponding prompt template.

## Runtime placeholders

The prompt templates contain placeholders such as `{requirement}`, `{refined_requirement}`, `{core_logic}`, `{examples}`, and `{non_separated_examples}`. These placeholders are replaced with the corresponding task input or fixed few-shot examples at inference time.
