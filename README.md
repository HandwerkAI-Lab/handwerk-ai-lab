# Handwerk AI Lab

**English** · [Deutsch](README.de.md)

**A German craftsman running his solar business on local AI.**
Two NVIDIA DGX Sparks, one Mac Studio, real customers. Everything reproducible, every source credited.

I'm Julian, I run a solar installation company in Freiburg, Germany (8 people, 1,000+ installs).
I'm not an ML engineer. This repo is my lab notebook: what I run, what I measure, what breaks.

→ Follow along on X: [@HandwerkAiLab](https://x.com/HandwerkAiLab)

## Hardware

| Component | Role |
| --- | --- |
| 2× NVIDIA DGX Spark (128 GB unified memory each) | Inference servers (vLLM / SGLang) |
| Mac Studio | Local agent host (HERMES), MLX experiments |
| Mellanox ConnectX NIC in OWC Mercury Helios 5S (Thunderbolt) – model being verified, see [`hardware/`](hardware/) | RDMA link Mac ↔ Spark via [MCDMA](https://github.com/ashhart/MCDMA) |
| DAC cable (type depends on NIC ports) | Physical link Mac ↔ Spark |

Details: [`hardware/`](hardware/)

## What's in here

| Folder | Content |
| --- | --- |
| [`logs/`](logs/) | Lab log, one entry per day/week, each linked to its X post |
| [`recipes/`](recipes/) | Model setups per Spark configuration |
| [`benchmarks/`](benchmarks/) | Measurements with date, method and raw data |
| [`agent/`](agent/) | Agent config templates (no keys, no customer data) |
| [`mcdma/`](mcdma/) | Notes on the Mac ↔ Spark RDMA setup |
| [`notes/`](notes/) | Field notes: DGX Spark networking, known issues, what makes it fast |
| [`SOURCES.md`](SOURCES.md) | Every repo, post and paper I built on |

## Principles

1. **Local first.** Customer data never leaves the building.
2. **Human in the loop.** The AI drafts, a human sends. Nothing is published or sent automatically.
3. **Measured vs. estimated.** Every number states hardware, model, quantization and date.
4. **Credit.** If I used your work, you're in [`SOURCES.md`](SOURCES.md) and tagged in the post.

## License

Scripts: MIT · Texts: CC BY 4.0 · Third-party recipes keep their original license.
