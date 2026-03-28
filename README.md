# 🔥 Blackwell-llama.cpp Inference Container  
# [STATUS]: STABLE_RELEASE | [TARGET]: NVIDIA_RTX_50_SERIES

[**English**](#philosophy) | [**中文**](#简介)

---

## 🏛️ Philosophy

*This is not “another Docker image with llama.cpp installed”.  
It is a **ready-to-run inference environment** that eliminates the infamous “environment hell” for NVIDIA Blackwell GPU owners.*

- **No compilation, no pain**  
  Pre‑compiled `llama.cpp` with `sm_120` support (`-DCMAKE_CUDA_ARCHITECTURES=120`).  
  All CUDA 12.8 dependencies and system libraries are baked in – you never need to touch a compiler.

- **Hardware‑native performance**  
  NVFP4 hardware acceleration ready (compiled with `-DGGML_CUDA_NVFP4=ON`).  
  Aggressive memory management (cold/hot separation, KV cache snapshot) squeezes every last token out of your 12GB GPU.

- **Just run**  
  One command to load, another to chat.  
  Built‑in `llama-chat` script with readline support – no more “> spam” on paste.

> **CRITICAL**: This image is for **inference only**. You do not need to know what `LD_LIBRARY_PATH` is.  
> Just download, load, and talk to your model.

---

## 🏮 简介

*这不是又一个装了 llama.cpp 的 Docker 镜像，而是一个专为 NVIDIA Blackwell GPU 深度优化的即用推理环境，目的就是彻底消灭卡住 99% 用户的“环境地狱”。*

- **无需编译，无需折腾**  
  预编译 `llama.cpp`，已启用 `sm_120` 指令集。  
  CUDA 12.8 工具链及所有动态库已内置并固化。

- **硬件级性能**  
  预留 NVFP4 硬件加速支持（编译参数已开，等上游代码合入）。  
  显存冷热分离调度 + KV cache 内存快照，榨干 12GB 显存极限。

- **开箱即用**  
  一条命令加载镜像，一条命令启动对话。  
  内置 `llama-chat` 脚本，解决原生 `>` 界面粘贴换行乱跳问题。

> **警告**：本镜像专为推理设计，你不需要理解 `LD_LIBRARY_PATH`，不需要重新编译。

---

## 🕊️ Acknowledgments / 特别致敬

- [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) – 核心推理引擎  
- [NVIDIA CUDA](https://developer.nvidia.com/cuda-toolkit) – 底层工具链  
- [Ubuntu](https://ubuntu.com) – 基础系统

---

## 🚀 Quick Start / 快速启动指南

1. Download & Merge / 下载并合并

从 [Releases](https://github.com/LianHe-BI/Blackwell-optimized-llama.cpp-Docker-image/releases/tag/content) 下载所有分卷（如 `blackwell-v4.1-final.tar.gz.part-aa` …），然后合并：

```bash
cat blackwell-v4.1-final.tar.gz.part-* > blackwell-v4.1-final.tar.gz
2. Load Image / 加载镜像

```bash
docker load < blackwell-v4.1-final.tar.gz
```

3. Run Container / 启动容器

```bash
docker run -it --rm --gpus all -v /your/model/dir:/data blackwell-cu128-workspace:v4.1-final bash
```

将 /your/model/dir 替换为存放 .gguf 模型的实际路径。

4. Start Chatting / 开始对话

```bash
llama-chat /data/your-model.gguf
```

---

📊 Performance / 实测性能

测试平台：RTX 5070 Ti Laptop 12GB，Ubuntu 24.04，驱动 590.48.01

Model Quantization Prompt Processing Generation
4B F16 250+ tok/s 70+ tok/s
14B Q4_K_M – 30+ tok/s

---

⚙️ Advanced Usage / 高级用法

Start an HTTP server / 启动 HTTP 服务

```bash
llama-server -m /data/your-model.gguf -ngl 99 --host 0.0.0.0 --port 8080
```

Quantize a model / 量化模型

```bash
llama-quantize /data/model.gguf /data/model-q4.gguf Q4_K_M
```

Convert HuggingFace model to GGUF / 转换 HuggingFace 模型

```bash
python /data/workspace/llama.cpp/convert_hf_to_gguf.py /path/to/hf_model --outfile /data/output.gguf --outtype f16
```

---

📦 Image Contents / 镜像内容

Component Version / Description
Base OS Ubuntu 24.04.4 LTS (OCI 纯净构建)
CUDA 12.8.93 (nvcc) + 590.48.01 driver support
llama.cpp b8275 (6c770d16c) – sm_120 optimized
Python 3.12.3 + PyTorch 2.10.0+cu128 + Transformers 5.3.0
Tools rlwrap, git, cmake, build-essential
Scripts /usr/local/bin/llama-chat (友好交互脚本)
Work Dir /data/workspace/llama.cpp/build/bin

---

💻 Requirements / 环境要求

Item Requirement
GPU RTX 5070 Ti / 5080 / 5090 (向下兼容 30/40 系)
Host OS Ubuntu 24.04 / 26.04 (其他发行版需驱动 ≥570)
Docker installed & supports --gpus all
NVIDIA Driver ≥570 (590 recommended)

⚠️ WSL2 未测试，建议使用原生 Ubuntu 环境。

---

📜 License & Disclaimer / 协议与免责

· Open Source：MIT License（见 LICENSE）
· Usage：自由使用、修改、再分发，但请保留版权声明。
· Liability：镜像内包含 NVIDIA 专有驱动组件，请遵守相关法律。
我们不承担因误用造成的任何损失。

🚦 Release Notes / 版本历史

· v4.1-final (2026-03-28)
  · 首次正式发布，支持 Blackwell 和 NVFP4 预备支持
  · 包含友好交互脚本 llama-chat
  · 固化 LD_LIBRARY_PATH，开箱即用

---

Now you can stop fighting the environment and start building. 🚀
别再和环境较劲了，开始创造吧。
