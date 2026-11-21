# Evaluation Protocol and Reproducibility Notes

This document summarizes the evaluation procedure **reported in the manuscript** (Sections 4.2–4.4, 5 and 6.4). It explains the task data, evaluation criteria, and metrics. It is not an executable evaluation harness.

## 1. Benchmark and input specifications

The benchmark is stored in [`PLC-Diagram-Eval/benchmark.json`](PLC-Diagram-Eval/benchmark.json). It contains **100 tasks** and **771 test cases**:

| Category | Easy | Medium | Hard | Total |
|---|---:|---:|---:|---:|
| Mathematical Algorithm | 10 | 10 | 10 | 30 |
| PLC Control | 15 | 20 | 35 | 70 |
| **Total** | **25** | **30** | **45** | **100** |

Each task includes `id`, `category`, `difficulty`, `title`, `requirement`, `inputs`, `outputs`, and `test_cases`. Some tasks additionally specify `scan_cycle_ms`, `input_constraints`, or `tolerance`. Each test case provides inputs and expected outputs; the appropriate treatment of scan cycles and tolerances depends on the task definition and simulation implementation.

## 2. Generation and compilation

1. Select a benchmark task and a model from [`MODEL_CONFIGURATIONS.md`](MODEL_CONFIGURATIONS.md).
2. Apply the same three-stage prompting workflow: requirement refinement, core logic generation, and LDText/FBDText generation. Use the corresponding fixed few-shot examples for the chosen experimental configuration.
3. For **Pass@1**, generate **one initial output** per task, per model, and per experimental configuration; do not manually edit incorrect generated code before evaluation.
4. Process the generated LDText/FBDText with `ldmaker`/`fbdmaker`, using the lexical, syntactic, and structural checks implemented by that compiler.
5. For compilation-valid programs, evaluate the simulated outputs against **all test cases** associated with that task.

A program can pass despite having a different internal topology from a reference diagram; the correctness decision is based on compiler validity and test-case behavior rather than structural identity.

## 3. Correctness criteria

- **Compilation validity:** the generated program is accepted by the corresponding compiler's implemented checks.
- **Functional correctness:** the compiled program's simulated output matches expected output for all provided test cases.
- **Overall correctness:** both compilation validity and functional correctness hold.

The manuscript uses “logical correctness” for the test-based functional criterion. Passing a finite suite does not prove correctness for every possible input or operating condition.

## 4. Reported metrics

**Pass@1:** percentage of tasks that are overall correct on the first attempt. The manuscript reports a **95% Wilson score confidence interval** for each Pass@1 estimate.

**Compiler-Guided Success Rate (CGSR):** percentage of tasks that achieve overall correctness at initial generation or after **up to three additional compiler-guided revision rounds**. Compiler diagnostics are returned to the model for revision. CGSR must not be described as first-attempt Pass@1.

**Error incidence rate:** proportion of tasks in a specified task group exhibiting a given error type. A task may have multiple error types, so incidence rates need not sum to 100%. The manuscript separately discusses counts of topology-error subtypes.

**Paired ablation analysis:** the four LD configurations are T-S, NL-S, T-NS, and NL-NS. The planned comparisons are T-S versus NL-S and T-S versus T-NS **within each of the two models**, for four planned comparisons. For each, the manuscript reports the Pass@1 difference (percentage points), a paired 95% bootstrap confidence interval, a two-sided exact McNemar p-value, and a Holm-adjusted p-value. Paired bootstrap resampling preserves the within-task outcomes; Holm correction accounts for four planned tests.

## 5. Availability and limitations

This repository contains the task specifications and test cases, the generation prompt templates, the fixed few-shot examples, the model settings documented in the manuscript, and [representative generated outputs](generated_outputs/README.md).

The **proprietary `ldmaker` and `fbdmaker` compilers are not publicly included**, and the uploaded repository snapshot does **not** include an executable evaluation harness, per-task run records for all experimental configurations, or the full set of original generated experimental outputs. Consequently, these public materials document the intended workflow and support partial replication and inspection, **but do not by themselves permit exact independent rerunning or verification of all aggregate results**.

Refer to the paper for the reported numerical results and hardware-validation evidence. The generated-output examples are illustrative and are not a substitute for the underlying full evaluation records.
