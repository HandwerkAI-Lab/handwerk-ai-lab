# DGX Spark – Field Notes

What I learned while setting up two DGX Sparks and a Mac Studio, collected from docs, forums and people who know more than I do.
**Stand / as of: 2026-09-26.** Forum knowledge changes fast, especially around firmware. Every claim links to its source. Numbers here are **other people's measurements**, not mine – mine go to [`benchmarks/`](../benchmarks/).

## 1. The one-sentence model of the Spark

A lot of memory (128 GB) with modest memory bandwidth (273 GB/s): it holds big models, reads prompts fast, and writes tokens slowly – unless you use MoE models, speculative decoding, several parallel requests, or link several Sparks.
Sources: [NVIDIA hardware docs](https://docs.nvidia.com/dgx/dgx-spark/hardware.html), [LMSYS review](https://www.lmsys.org/blog/2025-10-13-nvidia-dgx-spark/), [EXO Labs: DGX Spark Handbook](https://x.com/exolabs/status/2103617535765573959) by [@0xSero](https://x.com/0xSero).

## 2. Networking: what connects to a Spark and what doesn't

| Connection | Status | Source |
| --- | --- | --- |
| Spark ↔ Spark, QSFP112 DAC (Amphenol NJAAKK-N911, Luxshare LMTQF022-SD-R) | ✅ validated by NVIDIA | [NVIDIA Spark Stacking](https://docs.nvidia.com/dgx/dgx-spark/spark-clustering.html) |
| Spark ↔ Spark, NVIDIA MCP1650-V00AE30 (200G) | ✅ reported working, cheaper option | [EXO Handbook](https://x.com/exolabs/status/2103617535765573959) |
| Spark ↔ Mac via ConnectX-5 Ex (QSFP28 DAC, 100GBASE-CR4) | ✅ byte-verified RDMA | [MCDMA](https://github.com/ashhart/MCDMA) by [@ashxhart](https://x.com/ashxhart) |
| QSFP breakout cable (→ 4× SFP28) | ❌ ports report "not splittable", only one lane links; NVIDIA: never validated | [NVIDIA forum](https://forums.developer.nvidia.com/t/mellanox-qsfp-breakout/349775) |
| QSFP-to-SFP adapter (QSA) / SFP28 NICs | ⚠️ "not officially tested"; one report of no link detected at all | [NVIDIA forum](https://forums.developer.nvidia.com/t/can-a-dgx-spark-and-agx-thor-connect-using-qsfp-cables/359450/6) |
| USB-C or the 10 GbE RJ45 port for clustering | ❌ far too slow | [EXO Handbook](https://x.com/exolabs/status/2103617535765573959) |

**Performance trap:** each QSFP port shows up as two ~100 Gb/s interfaces on separate PCIe Gen5 x4 links. Wrong IP/interface mapping gives 80–100 instead of 180–200 Gb/s. Follow NVIDIA's playbook. Source: [ServeTheHome](https://www.servethehome.com/the-nvidia-gb10-connectx-7-200gbe-networking-is-really-different/).

**My lesson:** check the **port type** (SFP28 / QSFP28 / QSFP112) of NIC, cable and Spark before ordering. See [`hardware/`](../hardware/).

## 3. Known problems and community workarounds

| Problem | Workaround | Official? | Source |
| --- | --- | --- | --- |
| Hard power-off under sustained load, no log | Cap GPU clock: `sudo nvidia-smi -lgc 300,2200` (~5 % throughput cost reported) | ❌ community | [dgx-spark-hard-poweroff-fix](https://github.com/tonyd2wild/dgx-spark-hard-poweroff-fix) |
| Throttling after EC `0x02000005` / UEFI `0x03000006` (fans ramp too late) | Some users rolled back firmware; no official fix as of Aug 2026 | ❌ | [NVIDIA forum](https://forums.developer.nvidia.com/t/dgx-spark-gb10-thermal-throttling-after-ec-uefi-updates-acpi-zones-96-97c-fans-not-ramping/377044) |
| Runs hot | Stand it on its side, space around the mesh | ❌ experience | [EXO Handbook](https://x.com/exolabs/status/2103617535765573959) |
| Model won't start | Almost always memory: one big model per Spark at a time | ❌ experience | [EXO Handbook](https://x.com/exolabs/status/2103617535765573959) |
| Plyisty 2×25 Gbit/s adapter overheats and shuts down, even with a large heatsink | Add active cooling (a fan) | ❌ experience | [@petruspennanen](https://x.com/petruspennanen/status/2103838514324377802) |
| QsfpTek 100G cables not recognized by a MikroTik switch | Naddod cables worked. Third-party "compatible" cables are not interchangeable everywhere – check reports for your exact device | ❌ experience | [@petruspennanen](https://x.com/petruspennanen/status/2103838514324377802) |
| sparkDash has no auth on its API | Only run it on a trusted network | – | [noze.it](https://www.noze.it/en/insights/dgx-spark-local-models-august-2026/) |

## 4. What makes a Spark fast

- **MoE models** (few active parameters per token) fit the Spark's bandwidth far better than dense models.
- **Speculative decoding** (MTP, DSpark, DFlash, EAGLE3): a small helper drafts tokens, the big model checks them in one pass. Costs 1–2 GB memory. Speed depends on the task – code/JSON is easier to guess than prose, so always state the task with a speed number.
- **Parallel requests:** the model is read once per step for all requests, so total throughput grows much faster than single-user speed. Relevant for agent setups.
- **NVFP4:** the GB10 reads NVIDIA's 4-bit format natively.
- **Linking Sparks** with tensor parallelism adds up memory bandwidth: NVIDIA measured ~2.0× decode speed with two and ~3.7× with four Sparks.

Sources: [EXO Handbook](https://x.com/exolabs/status/2103617535765573959), [LMSYS](https://www.lmsys.org/blog/2025-10-13-nvidia-dgx-spark/), [NVIDIA playbooks](https://github.com/NVIDIA/dgx-spark-playbooks).

## 5. Why a Mac Studio next to a Spark

The Spark is strong at **prefill** (reading the prompt, compute-bound), a Mac with higher memory bandwidth is strong at **decode** (writing, bandwidth-bound). In one llama.cpp comparison an M4 Max generated faster than a Spark while the Spark processed prompts faster ([llama.cpp #16578](https://github.com/ggml-org/llama.cpp/discussions/16578)).
MCDMA demonstrated a first prefill-on-Spark / decode-on-Mac handoff with Qwen3-4B ([MCDMA](https://github.com/ashhart/MCDMA)). Testing this on my setup is one of the main goals of this lab.

**Two MCDMA link types – don't mix them up:**

| Link | Published by Ash Hart | Measured (his numbers) | In the public repo? |
| --- | --- | --- | --- |
| USB-C, Spark ⇄ Mac directly, no extra NIC | [X post, 2026-08-18](https://x.com/ashxhart/status/2089749434087227672) | 939 MB/s single link; 1.80 GB/s Mac → both Sparks; 1.25 GB/s both Sparks → Mac; 24 µs round trip | Not documented in the current README (checked 2026-09-26) |
| ConnectX-5 Ex in a Thunderbolt 5 enclosure, QSFP28 DAC to the Spark | [MCDMA README](https://github.com/ashhart/MCDMA) | ~50.6 Gbit/s into / ~29.4 Gbit/s out of Studio memory (0.1.18) | ✅ the documented, validated path |

In the August post Ash also describes his target setup: two Sparks linked over ConnectX-7 for prompt processing, each Spark linked to the Mac Studio for decode.

**Independent confirmation of the ConnectX-5 route:** [@petruspennanen](https://x.com/petruspennanen/status/2103837517208289760) (2026-09-26) linked a Bosgame M5 (AMD Strix Halo) to an ASUS GX10 (GB10) with the same card and enclosure (Mellanox MCX516A-CDAT in an OWC Helios 5S) and measured **29.7 Gbit/s with zero errors** using Ash's bandwidth tool. The 100G link was fine; the host's Thunderbolt/USB4 port capped the throughput. Takeaway: with this setup, **the host's Thunderbolt port sets the speed limit**, not the network card.

## 6. Where to learn more

- [NVIDIA DGX Spark playbooks](https://github.com/NVIDIA/dgx-spark-playbooks) – official guides (vLLM, SGLang, NVFP4, linking Sparks, NCCL)
- [MiaAI Lab](https://github.com/MiaAI-Lab) – tested recipes for 1–4 Sparks
- [EXO Labs: DGX Spark Handbook](https://x.com/exolabs/status/2103617535765573959) – best single overview, step-by-step 1 → 4 Sparks
- [MCDMA](https://github.com/ashhart/MCDMA) – RDMA between Mac and Spark
- [How To Spark](https://howtospark.com/) – crowdsourced benchmarks with recipes
- [NVIDIA Developer Forum – DGX Spark / GB10](https://forums.developer.nvidia.com/t/connectx-7-nic-in-dgx-spark/350417)

---

🇩🇪 **Kurz gesagt:** Der Spark hat viel Speicher, aber wenig Speicherbandbreite. Schnell wird er mit MoE-Modellen, Speculative Decoding, parallelen Anfragen und mehreren verbundenen Sparks. Beim Netzwerk unbedingt die Port-Typen prüfen – SFP28-Karten und Adapter sind am Spark nicht getestet.
