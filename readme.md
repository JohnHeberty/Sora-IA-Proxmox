# Sora-IA-Proxmox 🎬🚀  
Deploy do Open-Sora (Text2Video / Text2Image) em VM Proxmox com GPU via Docker

Este repositório entrega um **ambiente pronto em Docker** para rodar o [Open-Sora](https://github.com/hpcaitech/Open-Sora) dentro de uma **VM no Proxmox com GPU passthrough**, expondo uma interface **Gradio** na porta `7860`.

A ideia é:  
> *subir uma VM no Proxmox, passar a GPU pra ela, rodar `docker compose up` e já sair gerando vídeos/imagens com IA estilo Sora.*

---

## ✨ Funcionalidades

- 🧠 **Modelo Open-Sora v1.1** (pipeline de Text2Video / geração de imagens)  
- 🐳 **Container único** com:
  - CUDA 12.1
  - PyTorch com suporte a GPU
  - xFormers, flash-attn (otimizações de atenção)
  - Rotary Embeddings e dependências do Open-Sora
- 🖥️ **UI em Gradio** acessível via navegador (`http://IP_DA_VM:7860`)
- 💾 **Persistência**:
  - Cache do Hugging Face em volume (`hf_cache`)
  - Saída de vídeos / imagens em `./outputs` no host
- 🔧 Vários pequenos **patches de compatibilidade** já aplicados (T5, AV/FFmpeg, etc.) para evitar os erros mais chatos em ambiente real.

---

## 🧱 Arquitetura do Projeto

- **Proxmox** como hypervisor
- **VM Linux** (ex.: Ubuntu 22.04) com:
  - GPU NVIDIA em *passthrough*
  - Drivers NVIDIA instalados
  - Docker + NVIDIA Container Toolkit configurados
- Dentro da VM:
  - `Dockerfile` monta a imagem com CUDA + PyTorch + Open-Sora
  - `docker-compose.yml` sobe o serviço `sora-gradio` usando a GPU

---

## ✅ Requisitos

### No host (Proxmox)

- Proxmox 7/8 com:
  - IOMMU habilitado
  - GPU NVIDIA configurada em passthrough para a VM
- GPU recomendada:
  - **Mínimo:** 16 GB VRAM  
  - **Ideal:** 24 GB VRAM (ex.: 3090 / 4090 / similares)

### Na VM

- Linux (recomendado: Ubuntu 22.04)
- Docker + Docker Compose plugin
- NVIDIA drivers + NVIDIA Container Toolkit funcionando

Teste rápido dentro da VM:

```bash
nvidia-smi
docker run --rm --gpus all nvidia/cuda:12.1.0-base-ubuntu22.04 nvidia-smi
