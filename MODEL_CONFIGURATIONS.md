# Model Configurations

This document summarizes the model identifiers, decoding settings, and prompt configurations **reported in the paper** (Section 4.2, Table 3). It records the published experimental setup; it is not an executable API configuration file and contains no credentials.

## Base models and decoding parameters

| Model | Model identifier reported in the paper | Temperature | Top-p |
|---|---|---:|---|
| GPT-4.1 | `gpt-4.1-2025-04-14` | 0.2 | API default |
| DeepSeek-V3 | `deepseek-chat` | 0.2 | API default |

Both models were accessed through cloud-based APIs. “API default” means that a fixed numeric `top_p` value was not specified in the reported configuration. The `deepseek-chat` name is the reported API identifier, not an immutable checkpoint identifier; API model aliases may change over time.

## Generation workflow and prompts

1. **Requirement refinement**: use [`common/requirement_refinement.md`](common/requirement_refinement.md).
2. **Core logic generation**: use the LD rule/BNF prompt corresponding to the chosen configuration, or [`fbd/core_logic.md`](fbd/core_logic.md) for FBD.
3. **LDText/FBDText code generation**: use the appropriate code-generation prompt and generated core logic.
4. **Compilation and visualization**: process generated code using the corresponding proprietary compiler (`ldmaker` for LD or `fbdmaker` for FBD).

The system/user prompt templates are also described in Appendices A and B of the paper. At inference time, placeholders such as `{requirement}`, `{refined_requirement}`, `{core_logic}`, and `{examples}` are filled with task-specific content or the corresponding fixed examples.

## LD ablation configurations

| Configuration | Rule representation | Connection representation | Core-logic prompt | Code-generation prompt |
|---|---|---|---|---|
| T-S | Triplets | Separated | [`core_logic_T-S.md`](ld/core_logic_T-S.md) | [`ldtext_separated.md`](ld/ldtext_separated.md) |
| NL-S | Natural-language rules | Separated | [`core_logic_NL-S.md`](ld/core_logic_NL-S.md) | [`ldtext_separated.md`](ld/ldtext_separated.md) |
| T-NS | Triplets | Non-separated | [`core_logic_T-NS.md`](ld/core_logic_T-NS.md) | [`ldtext_non_separated.md`](ld/ldtext_non_separated.md) |
| NL-NS | Natural-language rules | Non-separated | [`core_logic_NL-NS.md`](ld/core_logic_NL-NS.md) | [`ldtext_non_separated.md`](ld/ldtext_non_separated.md) |

- Each LD core-logic prompt uses **four fixed** `<Refined Requirement, Core Logic>` examples.
- Each LDText code-generation prompt uses **four fixed** code-generation examples.
- T-S and NL-S share the separated-connection few-shot sets; T-NS and NL-NS share the non-separated-connection few-shot sets. Within a configuration, examples were held fixed across both models.
- The separated and non-separated configurations do **not** use identical example tasks. As noted in the paper's limitations, two non-separated examples resemble benchmark tasks `PLC-E-001` and `PLC-E-003`; this may confound the estimate of the connection-separation effect.

The corresponding files are listed in [`examples/README.md`](examples/README.md).

## FBD extension

FBDGen uses [`fbd/core_logic.md`](fbd/core_logic.md) and [`fbd/fbdtext.md`](fbd/fbdtext.md) with the fixed FBD example files described in [`examples/README.md`](examples/README.md). The available FBD example files each contain one example for the corresponding generation stage.

For evaluation criteria and compiler availability, see [`EVALUATION_PROTOCOL.md`](EVALUATION_PROTOCOL.md).
