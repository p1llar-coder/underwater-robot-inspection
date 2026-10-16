# Integrated Robotic Operation System for Intelligent O&M of Underwater Infrastructure
# 面向水下基础设施智能运维的一体化机器人作业系统

**[English](README.en.md) · [中文](README.md)**

An integrated underwater robot operation system that unifies **environmental perception, object detection, path planning, LLM-based decision-making and sim-to-real verification** for low-illumination, turbid-scattering water with complex terrain, sudden obstacles and current disturbance.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Sim: UE5/OceanGym](https://img.shields.io/badge/Sim-UE5%2FOceanGym-blue.svg)](https://arxiv.org/abs/2509.26536)
[![Gateway: FastAPI](https://img.shields.io/badge/Gateway-FastAPI%2BOllama-009688.svg)](https://fastapi.tiangolo.com/)

> This repository is the **showcase edition** of a competition/research project: it contains the system design, key innovations, experimental results and showcase images — **no runnable source code**. The LLM decision-chain code (`oceangym-agent/`), the local model service gateway (`llm-gateway/`), detection-model training code, the UE5 project and hardware design documents are maintained in a **private repository** and shared with authorized collaborators alongside the paper/patent progress — see "Source Code Access" below.

## Results at a Glance

| Metric | Result |
|---|---|
| Underwater detection | Dataset-calibrated degradation augmentation + DINOv3 dual-view distillation (DUET-DETR): URPC mAP 50.21→**50.89 (+0.68)**; all six COCO metrics positive across multi-seed runs; zero extra inference cost (10.4M params / 24.8 GFLOPs) |
| Real-time perception | **85%** continuous-frame stable-lock rate at **4.20 ms** single-frame latency |
| Path planning | HG-ALA global + GAS-3DVFH local + DSD dynamic safety domain: **100% collision-free** across three simulated terrains |
| Field validation | Wharf piles / canal caisson / reservoir dam: detection accuracy **+36.9%**, coverage **×11**, operation time **−17 days** (vs. manual inspection) |

## Showcase

| Overview (1) | Overview (2) |
|---|---|
| ![overview1](assets/overview_1.png) | ![overview2](assets/overview_2.png) |

| Path Planning Simulation | Optical-Flow Temporal Features | Detection under Degradation |
|---|---|---|
| ![planning](assets/planning_simulation.png) | ![flow](assets/optical_flow_features.png) | ![det](assets/detection_results.png) |

## Key Innovations

1. **Integrated multimodal robot**: pose, depth, vision and scanning-sonar sensors fused with edge computing, thruster control, gimbal lighting and a custom host-computer stack.
2. **Perception algorithms for underwater degradation**: lightweight image enhancement (YUV luminance-chrominance dual branch + DFA module, 0.044M params / 27.79 dB PSNR) + DUET-DETR real-time detection (training-time-only vision-foundation-model distillation; the inference graph stays identical to RT-DETRv4-S) + optical-flow temporal priors for cross-frame locking.
3. **Safety-consistent hybrid global–local path planning**: HG-ALA global reference + GAS-3DVFH local avoidance + DSD dynamic safety domain + DSD-GDVS continuous coordination, eliminating global/local planner inconsistency.
4. **LLM-driven task decision-making with sim-to-real synergy**: a local LLM gateway manages multimodal model calls; visual results are compressed into structured states to constrain model output; UE5/OceanGym high-fidelity simulation closes the "data generation – algorithm training – control verification" loop.

## Module Overview (source not public)

This repository presents the system design and results only; module source code is maintained in a private repository:

- **Local LLM service gateway** (FastAPI + Ollama): exposes locally hosted models as an OpenAI-compatible API to devices on the LAN, with Bearer-Key authentication and streaming output — built for weak-network, data-on-premise field sites.
- **OceanGym LLM decision & perception code** (secondary development on the [OceanGym](https://arxiv.org/abs/2509.26536) UE5.3 embodied underwater benchmark; credits to the original team): multimodal LLM decision-making (vision-language image reading + memory module + structured action output), a decision prompt system (atomic action set, observation fields, fallback policies), 8 simulation task scripts, and environment-mapping/perception scripts.
- **Detection-model training code, the UE5 project and hardware design documents**: shared with authorized collaborators alongside the paper/patent progress.

## Source Code Access

Because the related paper is under review and patents are pending, the source code is not publicly downloadable. For academic exchange, collaboration, or to request source-code access, please contact **pillar_dl@163.com** with your affiliation and intended use. Approved requests receive collaborator access to the private repository.

## Privacy Notice

The repository contains no site coordinates, customer information or secrets (keys in the configs are placeholders), and no unpublished paper drafts; simulation data and images come from public scenes or have been de-identified.

## License

[MIT](LICENSE) (the original OceanGym code remains under its original license and acknowledgement requirements)
