# Harbor v2.14.2 for ARM64

[![Harbor Version](https://img.shields.io/badge/Harbor-v2.14.2-blue)](https://github.com/goharbor/harbor/releases/tag/v2.14.2)
[![Architecture](https://img.shields.io/badge/Architecture-ARM64-green)](https://github.com/retif/harbor-arm)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**Harbor** is an open source registry that secures artifacts with policies and role-based access control, ensures images are scanned and free from vulnerabilities, and signs images as trusted.

This repository provides **ARM64/aarch64** builds of Harbor v2.14.2, enabling deployment on ARM-based infrastructure like Oracle Cloud ARM instances, AWS Graviton, Raspberry Pi clusters, and Apple Silicon.

## 📦 Pre-built Images

All 10 Harbor components are available as pre-built ARM64 images on GitHub Container Registry:

```bash
ghcr.io/retif/harbor/harbor-core-arm64:v2.14.2
ghcr.io/retif/harbor/harbor-jobservice-arm64:v2.14.2
ghcr.io/retif/harbor/harbor-portal-arm64:v2.14.2
ghcr.io/retif/harbor/harbor-db-arm64:v2.14.2
ghcr.io/retif/harbor/registry-photon-arm64:v2.14.2
ghcr.io/retif/harbor/harbor-registryctl-arm64:v2.14.2
ghcr.io/retif/harbor/harbor-trivy-adapter-arm64:v2.14.2
# See complete list in releases
```

See [Releases](https://github.com/retif/harbor-arm/releases/tag/v2.14.2) for complete image list.

## 🚀 Quick Start

### Pull and Run

```bash
# Pull images from GitHub Container Registry
docker pull ghcr.io/retif/harbor/harbor-core-arm64:v2.14.2
docker pull ghcr.io/retif/harbor/harbor-portal-arm64:v2.14.2
# ... (or use docker-compose/kubernetes to pull automatically)
```

### Using Kubernetes

See [examples/kubernetes/](examples/kubernetes/) for Helm values and manifests.

## 🔨 Building from Source

### Prerequisites
- ARM64 host or VM
- nerdctl v2.2.1+ with containerd  
- BuildKit v0.27.1+
- Go 1.24.11+, Node.js 16.18.0+

### Build

```bash
git clone https://github.com/retif/harbor-arm.git
cd harbor-arm

# Download Harbor source
make download

# Apply ARM64 patches  
make pre_update

# Build all images (uses all CPU cores)
make -j4 build \
  GOBUILDTAGS="include_oss include_gcs" \
  BUILDBIN=true \
  BUILDREG=true \
  BUILDTRIVYADP=true \
  TRIVYFLAG=true
```

**Build flags:**
- `BUILDREG=true` - Build registry binary from source (17.9 MB ARM64)
- `BUILDTRIVYADP=true` - Build Trivy adapter from source (15 MB ARM64)
- `TRIVYFLAG=true` - Include vulnerability scanning

## 📋 Components

**Core Services (5):**
- `harbor-core` (192 MB) - Main API and authentication
- `harbor-jobservice` (168 MB) - Async task processor  
- `harbor-portal` (52 MB) - Angular web UI
- `prepare` (282 MB) - Configuration generator
- `nginx-photon` (43 MB) - Reverse proxy

**Registry (3):**
- `registry-photon` (77 MB) - OCI image storage ⭐
- `harbor-registryctl` (150 MB) - Registry controller ⭐


**Security (2):**
- `harbor-trivy-adapter` (371 MB) - Vulnerability scanner ⭐


**Data (4):**
- `harbor-db` (196 MB) - PostgreSQL 15
- `harbor-log` (162 MB) - rsyslog


**Infrastructure**

⭐ = Compiled from source for ARM64

## 🎯 System Requirements

**Minimum:** 2 CPU, 4 GB RAM, 40 GB storage  
**Recommended:** 4+ CPU (ARM64), 8+ GB RAM, 160+ GB storage

## 🔐 Features

- ✅ OCI-compliant container registry
- ✅ Vulnerability scanning (Trivy v0.68.2)
- ✅ RBAC and user management
- ✅ Multi-registry replication  
- ✅ Webhook notifications
- ✅ Helm chart repository
- ✅ RESTful API

## 📝 Changes from v2.3.0

- ✅ Updated to Harbor v2.14.2
- ✅ Removed chartserver (deprecated)
- ✅ Removed notary (removed from Harbor)
- ✅ PostgreSQL 14 → 15
- ✅ Trivy v0.68.2
- ✅ Go 1.24.11, Node.js 16.18.0
- ✅ All binaries built from source for ARM64

## 🐛 Troubleshooting

**Build fails with "docker not found":**  
This build uses `nerdctl` instead of Docker.

**Registry binary not found:**  
Set `BUILDREG=true` to build from source (pre-built binaries are x86_64 only).

**Out of memory:**  
Reduce parallel jobs: `make -j2 build` instead of `-j4`.

## 📄 License

Apache 2.0 - Same as upstream Harbor

## 🙏 Credits

- Harbor Team - https://goharbor.io
- goharbor/harbor-arm - Original ARM patches (v2.3.0)
- Aqua Security Trivy - https://trivy.dev

---

Built for the ARM64 community 🚀
