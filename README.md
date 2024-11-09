# ComfyUI Docker Implementation

This repository contains Docker configurations for running ComfyUI on different GPU platforms: NVIDIA, AMD, and Apple Silicon (M1/M2).

## Overview

ComfyUI is a powerful and modular UI for Stable Diffusion. This repository provides Dockerfiles for easy deployment across different GPU platforms, with automated releases and multi-architecture support.

## Pre-built Images

Pre-built images are available on GitHub Container Registry. New versions are automatically released with each commit to the main branch.

### Latest Release
```bash
# NVIDIA (AMD64, ARM64)
docker pull ghcr.io/yourusername/comfyui-docker:latest-nvidia

# AMD (AMD64, ARM64)
docker pull ghcr.io/yourusername/comfyui-docker:latest-amd

# Mac ARM (ARM64)
docker pull ghcr.io/yourusername/comfyui-docker:latest-mac-arm
```

### Specific Version
```bash
# Replace vX.Y.Z with the desired version
docker pull ghcr.io/yourusername/comfyui-docker:vX.Y.Z-nvidia
docker pull ghcr.io/yourusername/comfyui-docker:vX.Y.Z-amd
docker pull ghcr.io/yourusername/comfyui-docker:vX.Y.Z-mac-arm
```

## Architecture Support

| Platform | Architectures |
|----------|--------------|
| NVIDIA | linux/amd64, linux/arm64 |
| AMD | linux/amd64, linux/arm64 |
| Mac ARM | linux/arm64 |

## Requirements

### General Requirements
- Docker installed on your system
- Internet connection for pulling images and models

### Platform-Specific Requirements

#### NVIDIA
- NVIDIA GPU
- NVIDIA Driver installed
- NVIDIA Container Toolkit installed

#### AMD
- AMD GPU
- ROCm installed
- ROCm-compatible Linux distribution

#### Mac ARM
- Apple Silicon (M1/M2) Mac
- Docker Desktop for Apple Silicon

## Usage

### Using Pre-built Images

#### For NVIDIA:
```bash
docker run --gpus all -p 8188:8188 \
  -v $(pwd)/models:/app/models \
  -v $(pwd)/output:/app/output \
  ghcr.io/yourusername/comfyui-docker:latest-nvidia
```

#### For AMD:
```bash
docker run --device=/dev/kfd --device=/dev/dri \
  -p 8188:8188 \
  -v $(pwd)/models:/app/models \
  -v $(pwd)/output:/app/output \
  ghcr.io/yourusername/comfyui-docker:latest-amd
```

#### For Mac ARM:
```bash
docker run -p 8188:8188 \
  -v $(pwd)/models:/app/models \
  -v $(pwd)/output:/app/output \
  ghcr.io/yourusername/comfyui-docker:latest-mac-arm
```

### Building Locally

1. Clone the Repository
```bash
git clone https://github.com/yourusername/ComfyUI-Docker.git
cd ComfyUI-Docker
```

2. Build the Image
```bash
# For NVIDIA
docker build -f Dockerfile.nvidia -t comfyui:nvidia .

# For AMD
docker build -f Dockerfile.amd -t comfyui:amd .

# For Mac ARM
docker build -f Dockerfile.mac-arm -t comfyui:mac-arm .
```

## Configuration

### Volume Mounts
- `/app/models`: For storing model files
- `/app/output`: For storing generated images and outputs

### Environment Variables
- `LISTEN_PORT`: The port ComfyUI listens on (default: 8188)
- `LISTEN_HOST`: The host ComfyUI binds to (default: 0.0.0.0)

## Releases

New releases are automatically created when commits are pushed to the main branch:
- Version numbers follow semantic versioning (vX.Y.Z)
- Patch version is automatically incremented
- Release notes include Docker image references and architecture support
- Images are tagged with both version numbers and 'latest'

## Troubleshooting

### Common Issues

1. NVIDIA Container Runtime not found:
```bash
# Install NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

2. ROCm Device not found:
```bash
# Ensure ROCm is properly installed and user has correct permissions
sudo usermod -a -G video $USER
sudo usermod -a -G render $USER
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the same terms as ComfyUI. See the LICENSE file for details.

## Acknowledgments

- [ComfyUI Project](https://github.com/comfyanonymous/ComfyUI)
- NVIDIA Container Toolkit team
- ROCm Development team
