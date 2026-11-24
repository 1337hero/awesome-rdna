# Awesome RDNA [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Local AI on AMD GPUs - Radeon AI PRO R9700, RX 7900 XTX, RX 9000 series, Strix Halo / Gorgon Halo APUs.

New to local AI on AMD? Start here. Most guides assume you're running NVIDIA - this list is for everyone else. Everything below is built by people actually running and tuning on this silicon: patch sets, containers, launch scripts, benchmarks. If it's on this list, someone measured it.

Consumer and workstation RDNA only - `gfx1100` through `gfx1201`. CDNA (MI-series datacenter cards) doesn't belong here.

Want to add something? Read the [contribution guidelines](CONTRIBUTING.md) first.

## Contents

- [llama.cpp](#llamacpp)
- [vLLM](#vllm)
- [SGLang](#sglang)
- [Standalone engines](#standalone-engines)
- [Containers and tooling](#containers-and-tooling)
- [Benchmarks and monitoring](#benchmarks-and-monitoring)
- [Community](#community)

## llama.cpp

- [llama-cpp-rdna-boosts](https://github.com/stew675/llama-cpp-rdna-boosts) - Performance patches for llama.cpp on RDNA 3/3.5/4. Ships as 16 `git am` blocks - take all of them or just the ones you want.

- [ROCmFP4](https://github.com/charlie12345/rocmfp4-llama) - New GGUF formats for llama.cpp on AMD (`Q4_0_ROCMFP4` dual-scale, `Q4_0_ROCMFP4_FAST` single-scale). Developed on Strix Halo (`gfx1151`). Experimental research build: numbers are specific to the hardware, driver and prompt.

- [ROCmFPX](https://github.com/charlie12345/ROCmFPX) - FPX quantization formats for llama.cpp on RDNA4 - ROCmFP4/FP6/FP8, ROCmI4 W4A4. Upstream refuses the tensor layouts, so these weights only run here.

## vLLM

- [qwen3.6-vllm-gfx1201-launchers](https://github.com/zzpanic/qwen3.6-vllm-gfx1201-launchers) - Launch scripts for serving Qwen 27B dense and 35B-A3B on a single R9700. Every knob is measured, not guessed - KV pinning, spec-decode depth sweeps, MXFP4 vs int4 quality numbers. The README tells you what breaks and why.

- [qwen3.6-vllm-gfx1201-launchers (BMorgan1296 fork)](https://github.com/BMorgan1296/qwen3.6-vllm-gfx1201-launchers) - Fork of the zzpanic launchers adding DSpark and DFlash2 speculative-decode paths on one R9700.

- [R9V](https://github.com/Dyluhn/R9V) - Pinned vLLM fork with `gfx1201` kernels, GGUF loading, MoE expert offload and 4-token MTP. Serves Qwen3.8 Flash-Next across two R9700s. The IQ4_XS and Q4_K_XL profiles are both experimental.

- [vllm-gfx1201-launchers](https://github.com/malicz/vllm-gfx1201-launchers) - Four `docker run` launchers for Qwen3.8-27B on one R9700: NVFP4 requantised to MXFP4 at load, PARO MXFP4, PARO int5 and AWQ, each behind a DFlash2 drafter. Sized for two long-context coding agents at once, one model resident.

- [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4) - MXFP4 vLLM for the R9700, in a container. One command gets you a server - no image build, no host ROCm install.

- [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) - Codeberg fork of StillDeadcode's vllm-radiance. Pinned vLLM 0.30 plus libr4d's hand-written RDNA4 kernels. Qualified on dual R9700s, benchmark runs published.

- [vllm-radiance-p2p](https://github.com/mkadrlik/vllm-radiance-p2p) - Prebuilt Docker image and compose file for 2× RX 7900 XTX (`gfx1100`). Tensor-parallel 2 over PCIe P2P, built on StillDeadcode's Radiance container. Profiles for Qwen3.8-27B AWQ with MTP, an Ornith vision model, and any AWQ model on one GPU.

- [vllm-radlight](https://codeberg.org/hifi/vllm-radlight) - Rootless-podman setup for Qwen3.8-27B MXFP4 at 256K context on one R9700. Runs AMD's own ROCm vLLM image patched at start, no custom build. Contains no original kernels - the value is the arrangement, and the README says so.

- [vllm-rocm-rdna4](https://github.com/Capicua25x/vllm-rocm-rdna4) - vLLM 0.28 ported to `gfx1200`/`gfx1201`. Two serving profiles: MTP-3 for concurrency, DFlash2-FP8 for single-stream. MXFP4 model recipes with accuracy grids. Measured on 2× RX 9070 XT at 200K context.

- [vllm-sly-radiance](https://github.com/SlyBase/vllm-sly-radiance) - vLLM image for `gfx1201`. Hand-written MXFP4 kernels, DFlash2 spec decode and up to 262K context on one 32 GB card. Author reports ~154 tok/s single-stream decode, ~3,900 tok/s prefill and 480 tok/s at 8 concurrent requests. Runs published.

## SGLang

- [2x-R9700-RDNA4-GFX1201-sglang-inference](https://github.com/mattbucci/2x-R9700-RDNA4-GFX1201-sglang-inference) - SGLang v0.5.23 with 103 RDNA4 patches. Tuned for single-user FP8 inference at 256K context on two R9700s (TP=2 over PCIe).

## Standalone engines

- [hipEngine](https://github.com/shisa-ai/hipEngine) - HIP-native inference engine for RDNA3 (`gfx1100`) and Strix Halo (`gfx1151`), written from scratch rather than ported from CUDA. No PyTorch dependency. OpenAI-compatible server with continuous batching, prefix caching, shared KV pool. Custom DMS INT8 KV compression squeezes Qwen3.8-27B Q4_K_M up to 262K context into a 24GB 7900 XTX.

- [radiance](https://codeberg.org/StillDeadcode/radiance) - C++/HIP inference server. Device kernels for RDNA4 (`gfx1200`/`gfx1201`), RDNA3 (`gfx11xx`) and Strix Halo (`gfx1151`), plus an AVX host backend. Serves the OpenAI, Anthropic and llama-server APIs - text, image, video, speech and music. Gemma 4 31B w4a8 decodes 103 tok/s over two R9700s, GLM-5.3-Flash MXFP4 166 tok/s a stream over eight. Its libr4d RDNA4 kernels are the same ones [vllm-radiance](https://github.com/magiccodingman/vllm-radiance) carries.

- [saber-engine](https://github.com/GGZ14/saber-engine) - Rust-based inference engine for the Qwen3.8 family on the AMD R9700 (`gfx1201`), by the author of [vllm-mxfp4](https://github.com/GGZ14/vllm-mxfp4). OpenAI-compatible server. Qwen3.8-27B (with DFlash2 drafter model) decodes at 229 tok/s a stream at 47 ms TTFT. Prefills 5,000+ tok/s on 2 R9700s (TP=2). All while capping each R9700 at 210W! Also serves the Flash-Next MoE.

## Containers and tooling

- [amd-r9700-vllm-toolboxes](https://github.com/kyuz0/amd-r9700-vllm-toolboxes) - Toolbx/Distrobox container for vLLM on the R9700. Interactive launcher for picking models and backends, benchmarks on a GitHub Pages site.

- [amd-strix-halo-toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) - Prebuilt llama.cpp containers for Strix Halo (gfx1151). Vulkan and ROCm stable channels plus experimental fork builds - strix-llama, EngramHalo, ROCmFPX. Also ships a unified-memory VRAM estimator and cluster distributed inference.

- [dual-r9700-vllm-proxmox](https://github.com/bkvargyas/dual-r9700-vllm-proxmox) - Guide to running 2× R9700 in a Proxmox VM. Includes the four fixes needed to make GPU↔GPU P2P work under passthrough. 2 profiles: FP8 + DFlash2 and MXFP4-W4A8 + DFlash2-FP8. 922k token KV cache, 160 combined tok/s decode.

## Benchmarks and monitoring

- [BetterBench](https://github.com/GGZ14/BetterBench) - Percentile-based benchmark for any OpenAI-compatible `/v1` endpoint (vLLM, llama.cpp, SGLang, Ollama, LM Studio). Includes a paired A/B mode built to resolve ~1% differences. Writes a self-contained HTML report beside its JSON.

## Community

- [Launch80 Discord](https://discord.gg/launch80) - Where the R9700 radiance and MXFP4 work actually gets coordinated. GGZ14 and friends hang out here. The [Launch80 AwesomeList](https://github.com/launch80/AwesomeList) indexes the same projects with hardware gates, status labels and benchmark records.
