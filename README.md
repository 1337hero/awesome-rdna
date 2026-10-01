# Awesome RDNA

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Curated tools, builds, and guides for running AI/LLM inference on AMD RDNA GPUs — RDNA 3 (RX 7000), RDNA 3.5 (Strix Point / Strix Halo), and RDNA 4 (RX 9000 / Radeon AI PRO R9700).

Scope is the consumer/workstation RDNA lineage (`gfx1100`, `gfx1150`, `gfx1151`, `gfx1200`, `gfx1201`). Datacenter CDNA (MI-series) is out of scope.

Contributions welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Contents

- [llama.cpp](#llamacpp)
- [vLLM](#vllm)
- [Containers and tooling](#containers-and-tooling)
- [Community](#community)

## llama.cpp

- [llama-cpp-rdna-boosts](https://github.com/stew675/llama-cpp-rdna-boosts) — Patch set for llama.cpp bringing RDNA-specific performance work: adaptive MTP speculative decoding, WMMA flash attention, BF16 KV, fused MoE and k-quant decode paths, and a hybrid all-reduce for multi-GPU tensor split. Runtime arch selection across RDNA 3/3.5/4, shipped as self-contained `git am` blocks.

## vLLM

- [Qwen3.6 / Qwen3.8 vLLM launchers for gfx1201](https://github.com/zzpanic/qwen3.6-vllm-gfx1201-launchers) — Tuned standalone launch scripts for serving Qwen 27B dense and 35B-A3B MoE models on a single Radeon AI PRO R9700, with deep measured documentation: KV cache pinning, speculative-decode depth sweeps, and MXFP4-vs-int4 quality benchmarks.
- [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4) — Native MXFP4 W4A8 vLLM stack for the Radeon AI PRO R9700 (gfx1201), packaged as a container with a one-command quickstart, RDNA4-tuned GEMM/attention/all-reduce kernels, and DFlash2/MTP speculative drafting.
- [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) — Fork of StillDeadcode's vllm-radiance combining a pinned vLLM 0.30 ROCm stack with libr4d's hand-written RDNA4 kernels (R4D attention, gated-delta-net, MXFP4, all-reduce), plus reproducible benchmarks and deployment qualification on dual R9700s.

## Containers and tooling

- [amd-r9700-vllm-toolboxes](https://github.com/kyuz0/amd-r9700-vllm-toolboxes) — Toolbx/Podman/Distrobox container for serving LLMs with vLLM on R9700 GPUs, with an interactive model launcher, AITER unified-attention integration, and published benchmarks.
- [amd-strix-halo-toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) — Pre-built llama.cpp containers for Strix Halo (gfx1151) across stable Vulkan and ROCm channels plus experimental forks, with a unified-memory VRAM estimator, distributed-inference tooling, and a cooling/power watchdog.

## Community

- [Launch80 Discord](https://discord.gg/launch80) — Where much of the R9700/RDNA4 vLLM work (radiance, MXFP4 kernels) gets coordinated.

## Contribute

Contributions welcome! Read the [contribution guidelines](CONTRIBUTING.md) first.
