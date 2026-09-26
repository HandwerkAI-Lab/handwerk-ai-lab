# Hardware

My setup compared with the validated reference setup from [MCDMA](https://github.com/ashhart/MCDMA) by Ash Hart ([@ashxhart](https://x.com/ashxhart)).
Reference data taken from the [MCDMA README → "Hardware used"](https://github.com/ashhart/MCDMA#hardware-used), checked 2026-09-26.

## Bill of materials

| Part | My setup | Where / price | Status | MCDMA reference setup |
| --- | --- | --- | --- | --- |
| Mac | Mac Studio M4 Max, 64 GB | — | ✅ in use | Mac Studio M3 Ultra, 256 GB |
| macOS | macOS 27, build 26A428 + Command Line Tools 27.0 | — | ✅ | macOS 27, build 26A428 (required exact build) |
| Enclosure | OWC Mercury Helios 5S (Thunderbolt 5) | — | ✅ in use | same |
| NIC | Mellanox ConnectX-5 Ex | — | ✅ installed in Helios 5S | ConnectX-5 Ex MCX516A-CDAT, dual QSFP28 |
| Peer | 2× NVIDIA DGX Spark (ConnectX-7) | — | ✅ Spark #1 serving via vLLM | 1× DGX Spark (2 Sparks tested for transfers) |
| RDMA cable | QSFP28-to-QSFP28 DAC, MCP1600-C001 compatible (FS.com) | — | ⏳ ordered | Mellanox MCP1600-C001E30N, 100G passive copper DAC, 1 m, **one per Studio port** |
| Management network | existing LAN (Mac + Sparks) | — | ✅ | separate Wi-Fi or Ethernet required |
| ~~Cat 8 patch cable~~ | Deleycon Cat 8 S-FTP RJ45 | — | ❌ bought, not used | not part of reference setup |
| ~~SFP+ transceiver~~ | FS.com SFP+ 10GBase-T (SFP-10G-T30L / MFM1T02A-T-I) | — | ❌ bought, not used | not part of reference setup |

Prices will be added with my actual order links.

## Differences from the reference setup (read this if you rebuild it)

- **Mac Studio M4 Max 64 GB instead of M3 Ultra 256 GB.** MCDMA's published numbers are from the M3 Ultra. My results will show whether the driver and the numbers transfer to an M4 Max. Nothing measured yet.
- **Compatible DAC instead of the original Mellanox cable.** The reference link negotiates 100GBASE-CR4 with RS-FEC on the Mellanox cable. Whether a compatible cable does the same will be checked and logged.
- **Two Sparks = two cables.** The reference uses one DAC per Studio port. To link both Sparks, both CX5 ports need a cable. MCDMA reports that both links share the enclosure bandwidth.
- **The ceiling is Thunderbolt, not the 100 Gb/s port.** Per MCDMA the Thunderbolt 5 tunnel runs as PCIe Gen4 x4; sustained ~50.6 Gbit/s into and ~29.4 Gbit/s out of Studio memory were measured on MCDMA 0.1.18. Don't expect 100 Gb/s.
- **Label your cables.** MCDMA documents a case where two crossed cables showed "port active" on both ends but transfers failed. Swapping the cables fixed it.

## What I got wrong: the RJ45 / SFP+ route

My first plan was a 10 Gb/s copper link: SFP+ 10GBase-T transceiver plus a Cat 8 cable.
I switched to the QSFP28 DAC to match the validated MCDMA setup instead of debugging an untested one.
Both parts are now spare. Lesson: **buy exactly the tested parts first, experiment later.**

## Wiring

<!-- photo + short description once the DAC cable is connected -->

## Sources

- Reference hardware, link rate, throughput ceiling and cabling notes: [MCDMA README](https://github.com/ashhart/MCDMA) by Ash Hart, Apache-2.0. The product links in his README are his Amazon affiliate links; if you want to support his work, buy through [his README](https://github.com/ashhart/MCDMA#hardware-used).
