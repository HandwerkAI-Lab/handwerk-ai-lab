# Sources & Credits

Everything this lab builds on. If I used your work, you're listed here **and** tagged in the X post.
Licenses were checked on the date shown. Always re-check the upstream repo before copying code.

## How I credit (rules for this repo)

1. **Link + author + license** for every repo, model, post or paper I build on.
2. **Copied code keeps its license.** Files taken from another repo stay under that repo's license, keep the original copyright header and are listed below with the exact file path.
3. **Apache-2.0:** keep the LICENSE text and any NOTICE file, mark what I changed.
4. **MIT:** keep the copyright notice.
5. **AGPL-3.0:** do **not** copy code into this MIT repo. Link to it and describe how I used it. If I ever fork it, the fork stays AGPL.
6. **"Inspired by" ≠ "copied from".** If I only followed an approach, I say "based on the approach in …".
7. **Measurements are mine** unless stated otherwise. Someone else's numbers are quoted with a link.

## Repos

| Project | Author | License (checked) | How I use it |
| --- | --- | --- | --- |
| [MCDMA](https://github.com/ashhart/MCDMA) · [mcdma.dev](https://mcdma.dev/) | Ash Hart · [@ashxhart](https://x.com/ashxhart) | Apache-2.0 (2026-09-26) | RDMA link Mac Studio ↔ DGX Spark. Installed per upstream README, no code copied |
| [Qwen3.8-Flash-Next-Single-DGX-Spark](https://github.com/MiaAI-Lab/Qwen3.8-Flash-Next-Single-DGX-Spark) | Mia's AI Lab · [@MiaAI_lab](https://x.com/MiaAI_lab) | AGPL-3.0-or-later (2026-09-26) | Reference recipe for qwen3.8-flash-next on one Spark. Linked only, not copied |
| ↳ [qwen3.8-Flash-DGX](https://github.com/lancelind/qwen3.8-Flash-DGX) | lancelind | Apache-2.0 (per Mia's README) | FP8 KV-cache approach used in Mia's recipe |
| [sparkDash](https://github.com/MiaAI-Lab/sparkDash) | Mia's AI Lab · [@MiaAI_lab](https://x.com/MiaAI_lab) | MIT, © 2026 Mia's AI Lab (2026-09-26) | Multi-Spark monitoring |
| [vLLM](https://github.com/vllm-project/vllm) | vLLM project | Apache-2.0 | Inference server on the Sparks |

## Models

The model I run is a chain of three people's work. Credit goes to all of them:

| Step | Model | By | License (checked) | Role |
| --- | --- | --- | --- | --- |
| 1. Base model | [Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) | Qwen Team (Alibaba Group) | Qwen Community License 1.0 (2026-09-26) | Original weights |
| 2. Quantization | [Qwen3.8-Flash-Next-NVFP4](https://huggingface.co/local-inference-lab/Qwen3.8-Flash-Next-NVFP4) | local-inference-lab | Qwen Community License 1.0 (2026-09-26) | NVFP4/MXFP8 via quantization-aware distillation – **source of truth** |
| 3. Mirror + recipe | [Mia-AiLab/Qwen3.8-Flash-Next-NVFP4](https://huggingface.co/Mia-AiLab/Qwen3.8-Flash-Next-NVFP4) + [recipe](https://github.com/MiaAI-Lab/Qwen3.8-Flash-Next-Single-DGX-Spark) | Mia's AI Lab · [@MiaAI_lab](https://x.com/MiaAI_lab) | Mirror page says Apache-2.0, but a mirror can't change the license – I follow the original (Qwen Community License 1.0). Recipe: AGPL-3.0-or-later | Where I downloaded it + how I run it on one Spark |

Runs on Spark #1 via vLLM as `qwen3.8-flash-next`.

**Citation requested by the Qwen Team:**

```bibtex
@techreport{qwen2026design,
    title       = {On the Design of {Qwen3.8-Next} Architecture: Evaluation, Efficiency, and Training Stability},
    author      = {{Qwen Team}},
    institution = {Alibaba Group},
    month       = {August},
    year        = {2026}
}

@misc{qwen3.8flashnext,
    title  = {{Qwen3.8-Flash-Next}: A New Architecture, Towards Ultimate Cost-Efficiency},
    author = {{Qwen Team}},
    month  = {August},
    year   = {2026},
    url    = {https://qwen.ai/blog?id=qwen3.8-flash-next}
}
```

## Posts, articles, papers

| Date | Title | Author | Link | Used for |
| --- | --- | --- | --- | --- |
| | | | | |

## Hardware references

| Topic | Source |
| --- | --- |
| MCDMA tested hardware list | [MCDMA README](https://github.com/ashhart/MCDMA) |
# Sources & Credits

Everything this lab builds on. If I used your work, you're listed here **and** tagged in the X post.
Licenses were checked on the date shown. Always re-check the upstream repo before copying code.

## How I credit (rules for this repo)

1. **Link + author + license** for every repo, model, post or paper I build on.
2. **Copied code keeps its license.** Files taken from another repo stay under that repo's license, keep the original copyright header and are listed below with the exact file path.
3. **Apache-2.0:** keep the LICENSE text and any NOTICE file, mark what I changed.
4. **MIT:** keep the copyright notice.
5. **AGPL-3.0:** do **not** copy code into this MIT repo. Link to it and describe how I used it. If I ever fork it, the fork stays AGPL.
6. **"Inspired by" ≠ "copied from".** If I only followed an approach, I say "based on the approach in …".
7. **Measurements are mine** unless stated otherwise. Someone else's numbers are quoted with a link.

## Repos

| Project | Author | License (checked) | How I use it |
| --- | --- | --- | --- |
| [MCDMA](https://github.com/ashhart/MCDMA) · [mcdma.dev](https://mcdma.dev/) | Ash Hart · [@ashxhart](https://x.com/ashxhart) | Apache-2.0 (2026-09-26) | RDMA link Mac Studio ↔ DGX Spark. Installed per upstream README, no code copied |
| [Qwen3.8-Flash-Next-Single-DGX-Spark](https://github.com/MiaAI-Lab/Qwen3.8-Flash-Next-Single-DGX-Spark) | Mia's AI Lab · [@MiaAI_lab](https://x.com/MiaAI_lab) | AGPL-3.0-or-later (2026-09-26) | Reference recipe for qwen3.8-flash-next on one Spark. Linked only, not copied |
| ↳ [qwen3.8-Flash-DGX](https://github.com/lancelind/qwen3.8-Flash-DGX) | lancelind | Apache-2.0 (per Mia's README) | FP8 KV-cache approach used in Mia's recipe |
| [sparkDash](https://github.com/MiaAI-Lab/sparkDash) | Mia's AI Lab · [@MiaAI_lab](https://x.com/MiaAI_lab) | MIT, © 2026 Mia's AI Lab (2026-09-26) | Multi-Spark monitoring |
| [vLLM](https://github.com/vllm-project/vllm) | vLLM project | Apache-2.0 | Inference server on the Sparks |

## Models

| Model | Source | License | Used for |
| --- | --- | --- | --- |
| qwen3.8-flash-next | <Hugging Face link> | <check model card> | Main model on Spark #1 |

## Posts, articles, papers

| Date | Title | Author | Link | Used for |
| --- | --- | --- | --- | --- |
| | | | | |

## Hardware references

| Topic | Source |
| --- | --- |
| MCDMA tested hardware list | [MCDMA README](https://github.com/ashhart/MCDMA) |
