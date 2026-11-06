# Awesome RDNA [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Local AI on AMD GPUs - Radeon AI PRO R9700, RX 7900 XTX, RX 9000 series, and Strix Halo / Gorgon Halo APUs.

New to local AI on AMD? Start here. Most guides assume you're running NVIDIA - this list is for everyone else. Everything below is built by people actually running and tuning on this silicon: patch sets, containers, launch scripts, benchmarks. If it's on this list, someone measured it.

Consumer and workstation RDNA only - `gfx1100` through `gfx1201`. CDNA (MI-series datacenter cards) doesn't belong here.

Want to add something? Read the [contribution guidelines](CONTRIBUTING.md) first.

## Contents

- [llama.cpp](#llamacpp)
- [vLLM](#vllm)
- [Standalone engines](#standalone-engines)
- [Containers and tooling](#containers-and-tooling)
- [Community](#community)

## llama.cpp

- [llama-cpp-rdna-boosts](https://github.com/stew675/llama-cpp-rdna-boosts) - Performance patches for llama.cpp on RDNA 3/3.5/4. MTP spec decode, WMMA flash attention, BF16 KV, fused MoE, k-quant decode boosts, hybrid all-reduce for multi-GPU tensor split. Ships as 16 `git am` blocks - take all of them or just the ones you want.

- [ROCmFPX](https://github.com/charlie12345/ROCmFPX) - FPX quantization formats for llama.cpp on RDNA4 - ROCmFP4/FP6/FP8, ROCmI4 W4A4. Upstream refuses the tensor layouts, so these weights only run here.

## vLLM

- [Qwen3.6 / Qwen3.8 vLLM launchers for gfx1201](https://github.com/zzpanic/qwen3.6-vllm-gfx1201-launchers) - Launch scripts for serving Qwen 27B dense and 35B-A3B on a single R9700. Every knob is measured, not guessed - KV pinning, spec-decode depth sweeps, MXFP4 vs int4 quality numbers. The README tells you what breaks and why.
- [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4) - MXFP4 vLLM for the R9700, in a container. One command gets you a server - no image build, no host ROCm install. Custom gfx1201 GEMM/attention/all-reduce kernels, DFlash2 and MTP spec decode. Fastest vLLM stack on this card.
- [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) - Codeberg fork of StillDeadcode's vllm-radiance. Pinned vLLM 0.30 plus libr4d's hand-written RDNA4 kernels - R4D attention, gated-delta-net, MXFP4, all-reduce. Qualified on dual R9700s, benchmark runs published.

## Standalone engines

- [radiance](https://codeberg.org/StillDeadcode/radiance) - C++/HIP inference server; architectures, kernel libraries and quantisers load as `.so` plugins. Device kernels for RDNA4 (`gfx1200`/`gfx1201`), RDNA3 (`gfx11xx`) and Strix Halo (`gfx1151`), plus an AVX host backend. Serves the OpenAI, Anthropic and llama-server APIs - text, image, video, speech and music. Gemma 4 31B w4a8 decodes 103 tok/s over two R9700s, GLM-5.3-Flash MXFP4 166 tok/s a stream over eight. Its libr4d RDNA4 kernels are the same ones [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) carries.
- [saber-engine](https://github.com/GGZ14/saber-engine) - Rust inference engine for the Qwen3.8 family on the R9700 (`gfx1201`), from the author of [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4). Boots from pre-arranged weight packs; HIP code objects with build manifests, host-owned launch plans, OpenAI-compatible server, TP=1 or TP=2 only. Qwen3.8-27B behind the DFlash2 drafter decodes 229 tok/s a stream at 47 ms TTFT and prefills 5,000+ tok/s over two R9700s under a 210 W per-card cap. Also serves Flash-Next MoE.

## Containers and tooling

- [amd-r9700-vllm-toolboxes](https://github.com/kyuz0/amd-r9700-vllm-toolboxes) - Toolbx/Distrobox container for vLLM on the R9700. Interactive launcher for picking models and backends, AITER unified attention for long context, benchmarks on a GitHub Pages site.
- [amd-strix-halo-toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) - Prebuilt llama.cpp containers for Strix Halo (gfx1151). Vulkan and ROCm stable channels plus experimental fork builds - strix-llama, EngramHalo, ROCmFPX. Also ships a unified-memory VRAM estimator and cluster distributed inference.

## Community

- [Launch80 Discord](https://discord.gg/launch80) - Where the R9700 radiance and MXFP4 work actually gets coordinated. GGZ14 and friends hang out here.
