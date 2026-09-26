# MCDMA: Mac Studio ↔ DGX Spark over RDMA

Based on [MCDMA](https://github.com/ashhart/MCDMA) by [@ashxhart](https://x.com/ashxhart) · [mcdma.dev](https://mcdma.dev/).
This folder only holds my notes and results. For install steps, follow the upstream README.

## Status

- [x] Mellanox NIC installed in Helios 5S, connected via Thunderbolt
- [ ] Confirm NIC model via PCI ID (invoice says ConnectX-4 Lx, see [`hardware/`](../hardware/))
- [x] macOS upgraded to required build, matching Command Line Tools installed
- [x] Portable build/test suite passes
- [ ] Matching cable chosen (SFP28 vs. QSFP28) and connected
- [ ] Kext install (Recovery / security settings) – backup done
- [ ] First cross-host latency measurement

## Results

→ `benchmarks/`
