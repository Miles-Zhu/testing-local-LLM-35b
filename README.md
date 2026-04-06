# Local LLM Test Report - Snake Game Code Generation Task

![](https://cdn.jsdelivr.net/gh/Miles-Zhu/bucket@main/imgs/20260406224042787.png)

*Test date: 2026-04-06*

---

## 1. Local Environment

### 1.1 Hardware Configuration

| Item | Specification |
|------|------|
| Device Model | MacBook |
| CPU | M2 Max |
| Memory (RAM) | 96GB unified memory |
| Storage | 2TB |
| GPU / NPU | 12 + 38 |

### 1.2 Software Configuration

| Item | Version |
|------|------|
| Operating System | macOS 25.4.0 (Darwin) |
| LLM Inference Framework | oMLX v0.3.4 |
| Model Name | MLX-Qwen3.5-35B-A3B-Claude-4.6-Opus-Reasoning-Distilled-8bit |
| Model Quantization | Q8_0 |
| Context Window | 40K tokens |
| Runtime Environment | Claude CLI |
| Skills | frontend-design |

---

## 2. Test Results

![](https://cdn.jsdelivr.net/gh/Miles-Zhu/bucket@main/imgs/20260406220251775.png)


---

## 3. Main Bottlenecks

### 3.1 Inference Speed

- **Generation speed is limited**: On consumer hardware, the 35B model generates at roughly 45-60 tok/s, and the wait time per run is still within an acceptable range.

### 3.2 Memory Pressure

![](https://cdn.jsdelivr.net/gh/Miles-Zhu/bucket@main/imgs/20260406222035473.png)

- The 35B model (Q8_K_M quantization) requires about 60GB of memory, and the overall runtime remains stable.

### 3.3 Context Length

- A 32k context is clearly insufficient. After only 2-3 interaction rounds, it exceeds the limit and throws an error. Compared with the native 1M context in `claude code`, this is 31.25 times smaller and is the main bottleneck.
- Supporting a 1M context would require roughly `1TB-class memory`, which is not feasible for local hardware.

### 3.4 Code Quality Limitations

- It can complete a simple game and produce a visually appealing interface.

---

## 4. Summary and Recommendations

| Recommendation | Notes |
|------|------|
| Quantization Choice | 35B Q8_K_M is usable |
| Programming Bottlenecks | 32K context is not enough, and the M2 Max 400GB/s bandwidth ceiling is still insufficient |

---
