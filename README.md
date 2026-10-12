# llama.cpp SYCL Benchmark

**Benchmark date:** 2026-10-12   
**Backend:** SYCL / Level Zero  
**llama.cpp:** `0.6.0-dev` · build `11429` · commit `d81235049`

This README reports the October 12, 2026 benchmark run only. Results are listed alphabetically by model, with one row per model and quantization. No ranking or previous-run comparison is included.

## Test system

| Setting | Value |
|---|---|
| CPU | Intel Core Ultra 9 285H |
| GPU | Intel Arc 140T integrated graphics |
| System memory | 64 GB DDR5 |
| OS | Ubuntu 24.04 |
| Compiler | IntelLLVM 2026.1.1 |
| Backend | SYCL / Level Zero |
| Prompt workload | 512 tokens (`pp512`) |
| Generation workload | 128 tokens (`tg128`) |
| Repeats | 3 |
| GPU-layer offload (`-ngl`) | `999`, `40`, `30`, `20` |

## Metrics and table guide

- **PP512**: prompt-processing throughput in tokens/second for a 512-token prompt.
- **TG128**: generation throughput in tokens/second for 128 generated tokens.
- **NGL 999**: maximum GPU-layer offload requested; `40`, `30`, and `20` are partial-offload settings.
- Values are shown as **mean ± standard deviation** across three repeats.
- **Type** is a Dense/MoE architecture-family label based on model family/name; confirm against model-card or GGUF metadata if model files change.
- GGUF file size is not total runtime memory. KV cache, compute buffers, runtime allocations, and the OS require additional memory.
- Throughput does not measure reasoning quality, coding correctness, factuality, instruction following, or multilingual performance.

## Results — prompt processing (`pp512`, tokens/s)

Each row is one model + quantization. NGL settings are separate columns so the same configuration can be compared across offload levels.

| Model | Type | Quantization | GGUF size | Params | NGL 999 | NGL 40 | NGL 30 | NGL 20 |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| Gemma 4 12B | Dense | Q4_K_M | 6.62 GiB | 11.91 B | 164.50 ± 2.75 | 144.20 ± 1.19 | 130.52 ± 0.39 | 118.89 ± 0.55 |
| Gemma 4 12B | Dense | Q6_K | 9.10 GiB | 11.91 B | 161.54 ± 1.23 | 131.07 ± 0.57 | 109.06 ± 0.44 | 92.56 ± 0.68 |
| Gemma 4 26B A4B | MoE | Q4_K_XL | 15.83 GiB | 25.23 B | 171.16 ± 1.73 | 169.16 ± 0.75 | 163.64 ± 0.87 | 115.14 ± 1.05 |
| Gemma 4 E4B | MoE | Q4_K_XL | 4.76 GiB | 7.52 B | 365.53 ± 15.97 | 343.00 ± 9.90 | 300.46 ± 4.19 | 268.03 ± 2.33 |
| Gemma 4 E4B | MoE | Q6_K_XL | 6.93 GiB | 7.52 B | 369.72 ± 6.83 | 338.36 ± 4.74 | 285.03 ± 3.08 | 261.38 ± 3.00 |
| Gemma 4 E4B | MoE | Q8_K_XL | 8.10 GiB | 7.52 B | 361.01 ± 4.01 | 310.68 ± 1.84 | 256.80 ± 1.72 | 229.79 ± 3.71 |
| GLM-4.7-Flash | MoE | Q4_K_XL | 16.31 GiB | 29.94 B | 83.26 ± 1.06 | 76.48 ± 0.42 | 66.80 ± 0.36 | 59.54 ± 0.46 |
| GPT-OSS 20B | MoE | Q4_K_M | 10.81 GiB | 20.91 B | 231.66 ± 7.63 | 228.00 ± 1.50 | 220.65 ± 8.17 | 182.50 ± 2.13 |
| GPT-OSS 20B | MoE | Q4_K_XL | 11.04 GiB | 20.91 B | 233.00 ± 2.39 | 225.76 ± 0.82 | 219.98 ± 1.47 | 184.70 ± 0.89 |
| GPT-OSS 20B | MoE | Q5_K_M | 10.90 GiB | 20.91 B | 228.35 ± 1.19 | 218.67 ± 3.80 | 213.05 ± 2.49 | 183.57 ± 2.25 |
| GPT-OSS 20B | MoE | Q6_K_XL | 11.20 GiB | 20.91 B | 232.19 ± 4.77 | 219.07 ± 2.66 | 224.45 ± 4.02 | 186.74 ± 2.12 |
| NVIDIA Nemotron 3.5 Lightning 30B-A3B | MoE | Q4_K_XL | 23.75 GiB | 32.91 B | 150.41 ± 1.24 | 101.17 ± 0.72 | 83.42 ± 1.79 | 72.84 ± 0.62 |
| NVIDIA Nemotron 3.5 Lightning 30B-A3B | MoE | Q5_K_M | 28.13 GiB | 32.91 B | 158.25 ± 2.42 | 100.08 ± 0.53 | 83.01 ± 0.84 | 70.19 ± 1.25 |
| Qwen3-Coder 30B-A3B Instruct | MoE | Q4_K_XL | 16.45 GiB | 30.53 B | 160.92 ± 1.75 | 129.73 ± 0.93 | 106.82 ± 0.41 | 92.19 ± 0.43 |
| Qwen3.5 4B | Dense | Q6_K_XL | 3.85 GiB | 4.21 B | 511.00 ± 4.08 | 509.37 ± 6.73 | 447.34 ± 12.64 | 368.14 ± 1.49 |
| Qwen3.5 9B | Dense | Q6_K_XL | 8.15 GiB | 8.95 B | 352.74 ± 5.90 | 352.13 ± 6.06 | 287.30 ± 7.14 | 187.77 ± 0.87 |
| Qwen3.6 35B-A3B | MoE | Q4_K_M | 20.60 GiB | 34.66 B | 180.48 ± 13.59 | 166.93 ± 0.74 | 119.14 ± 0.72 | 93.70 ± 0.35 |
| Qwen3.6 35B-A3B | MoE | Q5_K_M | 24.63 GiB | 34.66 B | 175.40 ± 1.31 | 157.33 ± 0.55 | 109.17 ± 0.63 | 85.24 ± 0.55 |
| Qwen3.6 35B-A3B | MoE | Q6_K | 27.29 GiB | 34.66 B | 165.34 ± 0.80 | 149.41 ± 0.35 | 99.10 ± 1.22 | 75.21 ± 2.39 |
| Qwen3.6 35B-A3B MTP-named | MoE | Q4_K_XL | 21.27 GiB | 35.51 B | 185.98 ± 0.24 | 170.52 ± 0.81 | 116.32 ± 1.83 | 92.89 ± 1.02 |
| Qwen3.6 35B-A3B MTP-named | MoE | Q6_K | 27.94 GiB | 35.51 B | 163.89 ± 0.53 | 141.90 ± 0.43 | 97.98 ± 1.02 | 75.70 ± 0.28 |
| Qwen3.8 27B | Dense | Q4_K_XL | 16.34 GiB | 27.32 B | 101.68 ± 1.58 | 66.83 ± 1.45 | 57.47 ± 0.39 | 51.76 ± 0.23 |

## Results — generation (`tg128`, tokens/s)

| Model | Type | Quantization | GGUF size | Params | NGL 999 | NGL 40 | NGL 30 | NGL 20 |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| Gemma 4 12B | Dense | Q4_K_M | 6.62 GiB | 11.91 B | 6.76 ± 0.08 | 6.28 ± 0.01 | 5.80 ± 0.08 | 5.46 ± 0.13 |
| Gemma 4 12B | Dense | Q6_K | 9.10 GiB | 11.91 B | 5.81 ± 0.01 | 5.42 ± 0.00 | 4.94 ± 0.05 | 4.55 ± 0.04 |
| Gemma 4 26B A4B | MoE | Q4_K_XL | 15.83 GiB | 25.23 B | 14.26 ± 0.02 | 14.26 ± 0.05 | 13.02 ± 0.02 | 12.25 ± 0.13 |
| Gemma 4 E4B | MoE | Q4_K_XL | 4.76 GiB | 7.52 B | 11.26 ± 0.26 | 11.42 ± 0.06 | 11.73 ± 0.00 | 10.98 ± 0.05 |
| Gemma 4 E4B | MoE | Q6_K_XL | 6.93 GiB | 7.52 B | 10.36 ± 0.18 | 10.17 ± 0.01 | 9.94 ± 0.02 | 9.32 ± 0.06 |
| Gemma 4 E4B | MoE | Q8_K_XL | 8.10 GiB | 7.52 B | 8.42 ± 0.00 | 8.21 ± 0.02 | 7.98 ± 0.08 | 7.38 ± 0.08 |
| GLM-4.7-Flash | MoE | Q4_K_XL | 16.31 GiB | 29.94 B | 9.78 ± 0.14 | 7.70 ± 0.03 | 8.79 ± 0.02 | 10.01 ± 0.03 |
| GPT-OSS 20B | MoE | Q4_K_M | 10.81 GiB | 20.91 B | 11.23 ± 0.01 | 11.28 ± 0.01 | 11.19 ± 0.11 | 10.10 ± 0.01 |
| GPT-OSS 20B | MoE | Q4_K_XL | 11.04 GiB | 20.91 B | 11.06 ± 0.02 | 11.14 ± 0.02 | 11.11 ± 0.02 | 10.01 ± 0.04 |
| GPT-OSS 20B | MoE | Q5_K_M | 10.90 GiB | 20.91 B | 11.07 ± 0.01 | 11.20 ± 0.07 | 11.17 ± 0.02 | 10.13 ± 0.08 |
| GPT-OSS 20B | MoE | Q6_K_XL | 11.20 GiB | 20.91 B | 11.18 ± 0.01 | 11.41 ± 0.14 | 11.55 ± 0.01 | 10.41 ± 0.06 |
| NVIDIA Nemotron 3.5 Lightning 30B-A3B | MoE | Q4_K_XL | 23.75 GiB | 32.91 B | 14.51 ± 0.01 | 12.49 ± 0.02 | 11.43 ± 0.08 | 10.84 ± 0.03 |
| NVIDIA Nemotron 3.5 Lightning 30B-A3B | MoE | Q5_K_M | 28.13 GiB | 32.91 B | 13.62 ± 0.01 | 12.04 ± 0.02 | 11.33 ± 0.02 | 10.59 ± 0.06 |
| Qwen3-Coder 30B-A3B Instruct | MoE | Q4_K_XL | 16.45 GiB | 30.53 B | 21.82 ± 0.06 | 17.21 ± 0.06 | 15.93 ± 0.08 | 15.01 ± 0.05 |
| Qwen3.5 4B | Dense | Q6_K_XL | 3.85 GiB | 4.21 B | 12.47 ± 0.01 | 12.56 ± 0.02 | 11.24 ± 0.03 | 10.05 ± 0.06 |
| Qwen3.5 9B | Dense | Q6_K_XL | 8.15 GiB | 8.95 B | 7.94 ± 0.09 | 8.01 ± 0.01 | 7.24 ± 0.01 | 6.26 ± 0.02 |
| Qwen3.6 35B-A3B | MoE | Q4_K_M | 20.60 GiB | 34.66 B | 15.90 ± 0.05 | 14.29 ± 0.09 | 13.46 ± 0.09 | 12.78 ± 0.05 |
| Qwen3.6 35B-A3B | MoE | Q5_K_M | 24.63 GiB | 34.66 B | 15.13 ± 0.05 | 13.79 ± 0.07 | 12.84 ± 0.07 | 12.22 ± 0.00 |
| Qwen3.6 35B-A3B | MoE | Q6_K | 27.29 GiB | 34.66 B | 14.79 ± 0.05 | 13.71 ± 0.04 | 12.23 ± 0.06 | 11.89 ± 0.04 |
| Qwen3.6 35B-A3B MTP-named | MoE | Q4_K_XL | 21.27 GiB | 35.51 B | 15.52 ± 0.12 | 14.23 ± 0.04 | 13.12 ± 0.05 | 12.39 ± 0.02 |
| Qwen3.6 35B-A3B MTP-named | MoE | Q6_K | 27.94 GiB | 35.51 B | 10.52 ± 0.01 | 13.57 ± 0.02 | 12.55 ± 0.05 | 11.86 ± 0.04 |
| Qwen3.8 27B | Dense | Q4_K_XL | 16.34 GiB | 27.32 B | 3.39 ± 0.00 | 2.51 ± 0.02 | 2.27 ± 0.02 | 2.07 ± 0.01 |

## Reproducibility

**Source log:** [View raw benchmark results](source/llama-sycl-bench-20261012-094640.txt)

```text
llama.cpp : 0.6.0-dev (build 11429, commit d81235049)
Backend   : SYCL / Level Zero
GPU       : Intel Arc 140T
Prompt    : 512 tokens
Generate  : 128 tokens
Repeats   : 3
NGL       : 999, 40, 30, 20
timestamp : 2026-10-12
```

## Notes

- A filename containing `MTP` does not, by itself, prove that multi-token prediction or speculative decoding was active. Verify the command-line configuration and runtime logs before attributing performance to MTP.
- These results cover one prompt/generation workload and three repeats per setting; long-context or sustained-load performance may differ.
- Benchmark outcomes can depend on drivers, oneAPI/toolchain versions, power state, background workload, context length, and runtime settings.
