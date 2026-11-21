# LDGen / FBD Prompt Templates

This repository contains the PLC-Diagram-Eval benchmark, prompt templates, few-shot examples, and representative generated outputs for LDGen and the FBD extension described in the paper.

## Directory structure

```text
PLC-Diagram-Eval/
  benchmark.json

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

generated_outputs/
  README.md
  MA-E-007_LD/
  MA-E-007_FBD/
  PLC-M-027_LD/
  PLC-H-044_LD/

MODEL_CONFIGURATIONS.md
EVALUATION_PROTOCOL.md
```

## PLC-Diagram-Eval benchmark

The [benchmark](PLC-Diagram-Eval/benchmark.json) contains **100 programming tasks**: 30 mathematical algorithm tasks and 70 PLC control tasks, spanning Easy, Medium, and Hard difficulty levels. Each task includes a natural-language requirement, input/output specifications, and associated functional test cases (771 test cases in total). Some tasks also include timing/scan-cycle information, input constraints, or numeric tolerances.

See [Evaluation protocol](EVALUATION_PROTOCOL.md) for the evaluation criteria and metrics reported in the paper.

## Generated outputs

The [generated outputs](generated_outputs/README.md) contain representative LDText/FBDText results for four examples under GPT-4.1 and DeepSeek-V3, with rendered LD/FBD images where provided. The examples are intended to illustrate the output formats and workflow, not to serve as the complete per-task experimental output archive.

The [generated outputs README](generated_outputs/README.md) lists the available text files and image status for every example/model pair.

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

## Model settings and evaluation notes

- [Model configurations](MODEL_CONFIGURATIONS.md): reported model identifiers, decoding settings, and prompt/example configuration.
- [Evaluation protocol](EVALUATION_PROTOCOL.md): task inputs, correctness criteria, Pass@1, CGSR, and paired ablation analysis.

The `ldmaker` and `fbdmaker` compilers are proprietary and are not included in this repository. This limits independent end-to-end reproduction; the available materials support inspection and partial replication of the reported setup.
