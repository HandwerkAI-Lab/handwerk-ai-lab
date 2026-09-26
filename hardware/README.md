# Hardware

My setup compared with the validated reference setup from [MCDMA](https://github.com/ashhart/MCDMA) by Ash Hart ([@ashxhart](https://x.com/ashxhart)).
Reference data taken from the [MCDMA README → "Hardware used"](https://github.com/ashhart/MCDMA#hardware-used) and [hardware validation](https://github.com/ashhart/MCDMA/blob/main/docs/hardware-validation.md), checked 2026-09-26.

## Bill of materials

| Part | My setup | Price | Status | MCDMA reference setup |
| --- | --- | --- | --- | --- |
| Mac | Mac Studio M4 Max, 64 GB | — | ✅ in use | Mac Studio M3 Ultra, 256 GB |
| macOS | macOS 27, build 26A428 + Command Line Tools 27.0 | — | ✅ | macOS 27, build 26A428 (required exact build) |
| Enclosure | OWC Mercury Helios 5S (Thunderbolt 5) | — | ✅ in use | same |
| NIC | Mellanox ConnectX-4 Lx, MCX4121A-ACAT (CX4121C), dual SFP28, 25 GbE, PCIe 3.0 x8, refurbished | 71,90 € | ✅ bought · ⚠️ card in the enclosure to be confirmed via PCI ID | ConnectX-5 Ex MCX516A-CDAT, dual QSFP28, 100 GbE |
| Peer | 2× NVIDIA DGX Spark (ConnectX-7) | — | ✅ Spark #1 serving via vLLM | 1× DGX Spark (2 Sparks tested for transfers) |
| RDMA cable | QSFP28-to-QSFP28 DAC, MCP1600-C001 compatible (FS.com) | — | ⏳ ordered · ⚠️ does not fit SFP28 ports | Mellanox MCP1600-C001E30N, 100G passive copper DAC, 1 m, one per Studio port |
| Spark ↔ Spark cable | not bought yet – options: NVIDIA MCP1650-V00AE30 (reported working), Amphenol NJAAKK-N911 / Luxshare LMTQF022-SD-R (NVIDIA-validated) | — | ⏳ needed | – ([NVIDIA](https://docs.nvidia.com/dgx/dgx-spark/spark-clustering.html), [EXO Handbook](https://x.com/exolabs/status/2103617535765573959)) |
| Management network | existing LAN (Mac + Sparks) | — | ✅ | separate Wi-Fi or Ethernet required |
| Copper cable | Deleycon Cat 8 S-FTP RJ45 | — | 🅿️ parked | not part of reference setup |
| SFP+ transceiver | FS.com SFP+ 10GBase-T (SFP-10G-T30L / MFM1T02A-T-I) | — | 🅿️ parked | not part of reference setup |

Prices will be completed as I add my order details.

## ⚠️ Open issue: ConnectX-4 Lx instead of ConnectX-5 Ex

My invoice shows a **ConnectX-4 Lx**, not the ConnectX-5 Ex used for all published MCDMA results. What that means:

- **Driver support:** MCDMA's source accepts the ConnectX-4 Lx (PCI `15b3:1015`) since a [community contribution (PR #3)](https://github.com/ashhart/MCDMA/pull/3). The contributor reports bidirectional RDMA and bandwidth tests; per the [hardware validation doc](https://github.com/ashhart/MCDMA/blob/main/docs/hardware-validation.md) the maintainer has **not reproduced** them. All published MCDMA numbers are from the ConnectX-5 Ex.
- **Ports:** the ConnectX-4 Lx has **SFP28** ports (25 GbE). A QSFP28-to-QSFP28 DAC does not plug into it. The DGX Spark side is QSFP. The link needs an SFP28 ↔ QSFP solution (e.g. a QSFP-to-SFP adapter on the Spark plus an SFP28 DAC, or a breakout cable). Which option the Spark accepts is **not verified yet**.
- **Speed:** 25 GbE per port instead of 100 GbE.

Next step: check the PCI device ID on the Mac (System Information → PCI: `0x1015` = ConnectX-4 Lx, `0x1019` = ConnectX-5 Ex), then choose the cable.

## Differences from the reference setup (read this if you rebuild it)

- **Mac Studio M4 Max 64 GB instead of M3 Ultra 256 GB.** MCDMA's published numbers are from the M3 Ultra. Nothing measured on the M4 Max yet.
- **Two Sparks = two links.** The reference uses one cable per Studio port. MCDMA reports that both links share the enclosure bandwidth.
- **The ceiling is Thunderbolt, not the port.** Per MCDMA the Thunderbolt 5 tunnel runs as PCIe Gen4 x4; sustained ~50.6 Gbit/s into and ~29.4 Gbit/s out of Studio memory were measured on MCDMA 0.1.18 with the ConnectX-5 Ex.
- **Label your cables.** MCDMA documents a case where two crossed cables showed "port active" on both ends but transfers failed. Swapping the cables fixed it.

## Lesson so far

Check the **port type** (SFP28 vs. QSFP28) of card, cable and peer before ordering anything. Card names alone are not enough.

## Wiring

<!-- photo + short description once the link is connected -->

## Sources

- Reference hardware, link rate, throughput ceiling and cabling notes: [MCDMA README](https://github.com/ashhart/MCDMA) by Ash Hart, Apache-2.0. The product links in his README are his Amazon affiliate links; if you want to support his work, buy through [his README](https://github.com/ashhart/MCDMA#hardware-used).
- ConnectX-4 Lx support: [MCDMA PR #3](https://github.com/ashhart/MCDMA/pull/3) and [hardware validation](https://github.com/ashhart/MCDMA/blob/main/docs/hardware-validation.md).
