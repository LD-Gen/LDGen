# Representative LDGen / FBDGen Generated Outputs

This directory provides **representative generated results** for four benchmark examples, organized by task and model. Each example/model directory contains the generated LDText or FBDText in a `.txt` file. A `.png` file is included where a rendered diagram is available.

These examples illustrate output format and generation behavior. They are not the complete archive of per-task outputs used to calculate the aggregate results in the paper; consult the paper for the experimental statistics and evaluation method.

## Files by task and model

| Benchmark task | Model | Generated text | Rendered diagram |
|---|---|---|---|
| MA-E-007 (LD; point inside a circle) | GPT-4.1 | [LDText](MA-E-007_LD/GPT-4.1/MA-E-007_LD.txt) | [PNG](MA-E-007_LD/GPT-4.1/MA-E-007_LD.png) |
| MA-E-007 (LD; point inside a circle) | DeepSeek-V3 | [LDText](MA-E-007_LD/DeepSeek-V3/MA-E-007-LD.txt) | [PNG](MA-E-007_LD/DeepSeek-V3/MA-E-007-LD.png) |
| MA-E-007 (FBD; point inside a circle) | GPT-4.1 | [FBDText](MA-E-007_FBD/GPT-4.1/MA-E-007-FBD.txt) | [PNG](MA-E-007_FBD/GPT-4.1/MA-E-007-FBD.png) |
| MA-E-007 (FBD; point inside a circle) | DeepSeek-V3 | [FBDText](MA-E-007_FBD/DeepSeek-V3/MA-E-007-FBD.txt) | [PNG](MA-E-007_FBD/DeepSeek-V3/MA-E-007_FBD.png) |
| PLC-M-027 (LD; temperature-controlled fan with manual override) | GPT-4.1 | [LDText](PLC-M-027_LD/GPT-4.1/PLC-M-027_LD.txt) | [PNG](PLC-M-027_LD/GPT-4.1/PLC-M-027_LD.png) |
| PLC-M-027 (LD; temperature-controlled fan with manual override) | DeepSeek-V3 | [LDText](PLC-M-027_LD/DeepSeek-V3/PLC-M-027_LD.txt) | Compilation failed; no PNG |
| PLC-H-044 (LD; dual-contactor safe switching) | GPT-4.1 | [LDText](PLC-H-044_LD/GPT-4.1/PLC-H-044_LD.txt) | Compilation failed; no PNG |
| PLC-H-044 (LD; dual-contactor safe switching) | DeepSeek-V3 | [LDText](PLC-H-044_LD/DeepSeek-V3/PLC-H-044_LD.txt) | Compilation failed; no PNG |

Task definitions and test cases are available in [PLC-Diagram-Eval/benchmark.json](../PLC-Diagram-Eval/benchmark.json).

## Compilation status and diagram availability

There are **eight generated text files and five diagram PNG files**. Compilation was unsuccessful for the following three task/model combinations; consequently, no LD diagram was generated and no PNG is included:

- `PLC-M-027_LD / DeepSeek-V3`
- `PLC-H-044_LD / GPT-4.1`
- `PLC-H-044_LD / DeepSeek-V3`

For these three cases, the generated LDText did not meet the compiler's required **syntax and/or connection rules**. The original generated text files are retained to document these unsuccessful outputs. Individual compiler diagnostic logs and a per-case breakdown of the violated rules are not included in this repository.

The presence of a rendered PNG only indicates that a diagram was produced; **it does not establish functional correctness**. Functional correctness must be assessed against the benchmark test cases using the compiler and simulation procedure described in the [evaluation protocol](../EVALUATION_PROTOCOL.md).
