# Awesome RDNA [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Tools, builds, and guides for running LLMs on AMD RDNA GPUs - RDNA 3 (RX 7000), RDNA 3.5 (Strix Halo), RDNA 4 (RX 9000 / R9700).

Consumer and workstation cards only. `gfx1100` through `gfx1201`. CDNA (MI-series) doesn't belong here.

Want to add something? Read the [contribution guidelines](CONTRIBUTING.md) first.

## Contents

- [llama.cpp](#llamacpp)
- [vLLM](#vllm)
- [Containers and tooling](#containers-and-tooling)
- [Community](#community)

## llama.cpp

- [llama-cpp-rdna-boosts](https://github.com/stew675/llama-cpp-rdna-boosts) - Performance patches for llama.cpp on RDNA 3/3.5/4. MTP spec decode, WMMA flash attention, BF16 KV, fused MoE, k-quant decode boosts, hybrid all-reduce for multi-GPU tensor split. Ships as 16 `git am` blocks - take all of them or just the ones you want.

## vLLM

- [Qwen3.6 / Qwen3.8 vLLM launchers for gfx1201](https://github.com/zzpanic/qwen3.6-vllm-gfx1201-launchers) - Launch scripts for serving Qwen 27B dense and 35B-A3B on a single R9700. Every knob is measured, not guessed - KV pinning, spec-decode depth sweeps, MXFP4 vs int4 quality numbers. The README tells you what breaks and why.
- [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4) - MXFP4 vLLM for the R9700, in a container. One command gets you a server - no image build, no host ROCm install. Custom gfx1201 GEMM/attention/all-reduce kernels, DFlash2 and MTP spec decode. Fastest vLLM stack on this card.
- [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) - Codeberg fork of StillDeadcode's vllm-radiance. Pinned vLLM 0.30 plus libr4d's hand-written RDNA4 kernels - R4D attention, gated-delta-net, MXFP4, all-reduce. Qualified on dual R9700s, benchmark runs published.

## Containers and tooling

- [amd-r9700-vllm-toolboxes](https://github.com/kyuz0/amd-r9700-vllm-toolboxes) - Toolbx/Distrobox container for vLLM on the R9700. Interactive launcher for picking models and backends, AITER unified attention for long context, benchmarks on a GitHub Pages site.
- [amd-strix-halo-toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) - Prebuilt llama.cpp containers for Strix Halo (gfx1151). Vulkan and ROCm stable channels plus experimental fork builds - strix-llama, EngramHalo, ROCmFPX. Also ships a unified-memory VRAM estimator and cluster distributed inference.

## Community

- [Launch80 Discord](https://discord.gg/launch80) - Where the R9700 radiance and MXFP4 work actually gets coordinated. GGZ14 and friends hang out here.
