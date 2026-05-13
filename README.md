# Quantization-aware Adaptive Memory Orchestration for Edge Vision-Language Model Inference

## Overview

본 연구는 Embedded Edge 환경에서 Vision-Language Model(VLM)의 실시간 추론 성능 향상을 목표로 한다.  
기존 연구들이 Quantization을 통한 메모리 절감 자체에 집중한 것과 달리, 본 연구는 Quantization으로 확보된 VRAM을 runtime에서 병목 모듈에 동적으로 재배치하여 전체 inference latency를 최소화하는 Adaptive Memory Orchestration Framework를 제안한다.

특히 제한된 메모리와 bandwidth 환경을 가지는 Jetson Orin Nano 기반 On-Device VLM 환경에서 다음 문제를 해결하고자 한다.

- KV Cache bottleneck
- Memory bandwidth saturation
- Activation memory pressure
- Visual token explosion
- Long-context degradation

---

# Research Motivation

최근 VLM은:
- 멀티모달 QA
- Robotics
- Video understanding
- Edge AI Agent

등 다양한 분야에 적용되고 있다.

하지만 VLM은:
- 높은 VRAM 사용량
- 급격한 KV Cache 증가
- Memory-bound attention 연산
- 실시간 추론 latency 증가

등의 문제를 가진다.

특히 Edge 환경에서는:
- 제한된 VRAM
- 낮은 memory bandwidth
- thermal constraint

로 인해 datacenter 환경보다 병목이 더욱 심각하다.

기존 Quantization 연구는 대부분:

```text
Quantization
    ↓
Memory Reduction
```

에 집중한다.

반면 본 연구는:

```text
Quantization
    ↓
Free VRAM 확보
    ↓
Runtime Adaptive Memory Reallocation
    ↓
Bottleneck Mitigation
    ↓
Latency Reduction
```

구조를 목표로 한다.

---

# Core Idea

본 연구의 핵심 아이디어는 다음과 같다.

> “양자화를 단순 compression 기법으로 사용하지 않고,
> 확보된 메모리를 workload-aware하게 재분배하여
> Edge VLM inference 성능을 최적화한다.”

즉:

- Long-context 상황에서는 KV Cache 확장
- Video VLM에서는 temporal buffer 증가
- Memory-bandwidth bottleneck 상황에서는 activation buffering 증가
- Compute-bound 상황에서는 batch scaling

등 상황에 따라 다른 메모리 정책을 적용한다.

---

# Research Objectives

## 1. VLM Bottleneck Analysis

다음 요소를 정량적으로 분석한다.

- Weight Memory
- KV Cache Growth
- Activation Memory
- Visual Token Memory
- Memory Bandwidth
- GPU Utilization
- Arithmetic Intensity

---

## 2. Quantization-based VRAM Reduction

다음 Quantization 기법을 적용한다.

- AWQ
- GPTQ
- SmoothQuant
- INT4 / INT8 Mixed Precision

---

## 3. Runtime Adaptive Memory Scheduler

현재 workload 상태를 기반으로:
- context length
- visual token count
- bandwidth pressure
- free VRAM

을 분석하여 memory allocation policy를 동적으로 결정한다.

---

## 4. Dynamic Memory Reallocation

확보된 VRAM을 다음 요소에 동적으로 재배치한다.

| Target | Purpose |
|---|---|
| KV Cache | Context retention |
| Activation Buffer | Bandwidth reduction |
| Visual Token Retention | Image reasoning improvement |
| Batch Buffer | Throughput increase |
| Temporal Buffer | Video understanding |

---

# Research Questions

## RQ1

Quantization으로 확보한 VRAM을 어디에 재할당하는 것이 가장 효과적인가?

---

## RQ2

Workload에 따라 병목 유형은 어떻게 변화하는가?

---

## RQ3

Edge 환경에서 runtime adaptive allocation은 static allocation 대비 어떤 성능 향상을 제공하는가?

---

## RQ4

Weight precision 감소와 KV Cache capacity 증가 중 어떤 요소가 reasoning 성능에 더 큰 영향을 미치는가?

---

# System Architecture

```text
Input
   ↓
Quantized VLM
   ↓
Runtime Profiler
   ↓
Bottleneck Analyzer
   ↓
Adaptive Memory Scheduler
   ↓
Dynamic Memory Reallocation
   ↓
Optimized Inference
```

---

# Experimental Scenarios

## Scenario 1 — Long-context VLM

### 목적
KV Cache expansion 효과 분석

### 병목
- KV Cache capacity
- Attention memory pressure

### 평가
- Long-context QA
- OCR reasoning
- Multi-turn conversation

---

## Scenario 2 — Multi-image Reasoning

### 목적
Visual token retention 효과 분석

### 병목
- Visual token explosion
- Activation memory growth

### 평가
- Multi-image QA
- Chart reasoning
- Robotics scene understanding

---

## Scenario 3 — Video VLM

### 목적
Temporal memory allocation 효과 분석

### 병목
- Temporal KV growth
- Memory bandwidth saturation

### 평가
- Video QA
- Action recognition
- Temporal reasoning

---

## Scenario 4 — Streaming Agent

### 목적
Dynamic runtime scheduling 효과 분석

### 병목
- Continuous KV accumulation
- Runtime memory fragmentation

### 평가
- Streaming VLM assistant
- Interactive multimodal agent

---

## Scenario 5 — Compute-bound Workload

### 목적
Batch scaling 및 compute utilization 분석

### 병목
- Vision encoder compute bottleneck

### 평가
- High-resolution image inference
- Encoder-heavy workload

---

# Evaluation Metrics

## Performance

- End-to-End Latency
- Tokens/sec
- TTFT (Time To First Token)
- Throughput

---

## Memory

- VRAM Usage
- Memory Bandwidth
- Cache Hit Rate
- KV Eviction Frequency

---

## Accuracy

- VQA Accuracy
- Long-context QA Accuracy
- Video QA Accuracy
- Hallucination Rate

---

# Target Platform

## Hardware

- NVIDIA Jetson Orin Nano

---

## Software

- CUDA
- TensorRT-LLM
- PyTorch
- Nsight Systems
- Nsight Compute

---

## Target Models

- Qwen2-VL
- Phi-3-Vision
- Gemma-based VLM
- LLaVA series

---

# Expected Contributions

## 1. Quantization-aware Runtime Memory Orchestration

기존 compression 중심 접근과 달리:
- runtime memory redistribution
- bottleneck-aware scheduling

을 제안한다.

---

## 2. Edge-specific VLM Optimization

Jetson 환경에서:
- VRAM
- bandwidth
- thermal constraint

를 고려한 실질적 최적화를 수행한다.

---

## 3. Workload-aware Memory Policy

상황별 최적 memory allocation 전략을 분석한다.

| Workload | Best Policy |
|---|---|
| Long Context | KV Expansion |
| Multi-image | Visual Retention |
| Video VLM | Temporal Buffer |
| Compute-heavy | Batch Scaling |
| Bandwidth-heavy | Activation Buffering |

---

# Research Keywords

- On-Device AI
- Edge AI
- Vision-Language Model
- Runtime Optimization
- KV Cache
- Quantization
- Adaptive Scheduling
- Memory Orchestration
- Embedded AI Systems
- Multimodal AI Systems

---

# Future Work

- Dynamic precision scheduling
- Hierarchical KV memory
- Edge-cloud collaborative inference
- Energy-aware runtime scheduling
- Thermal-aware memory orchestration

---

# Author

Embedded Edge / On-Device Multimodal AI Systems Research
