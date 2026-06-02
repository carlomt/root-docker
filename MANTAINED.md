# Maintained images in this fork

This repository is a fork of [`root-project/root-docker`](https://github.com/root-project/root-docker).

The purpose of this fork is to provide multi-architecture ROOT Docker images built from source for:

- `linux/amd64`
- `linux/arm64`

The upstream repository mainly provides x86_64 images. This fork adds and maintains selected multi-architecture images.

## Actively maintained recipes

The actively maintained build recipes in this fork are:

| Directory | Base image | Architectures | Notes |
|---|---|---|---|
| `ubuntu24_from_source` | Ubuntu 24.04 | `linux/amd64`, `linux/arm64` | Default image |
| `alma10_from_source` | AlmaLinux 10 | `linux/amd64`, `linux/arm64` | RHEL-like image with PyROOT and XRootD |

Other directories are inherited from the upstream repository and are kept mainly to simplify comparison and future synchronization with upstream. They may not be tested by this fork.

## Published images

Images are published on [`Docker Hub`](https://hub.docker.com/r/carlomt/root) and GitHub Container Registry.

### Docker Hub

```bash
docker pull carlomt/root:6.40.00
docker pull carlomt/root:6.40.00-ubuntu24
docker pull carlomt/root:6.40.00-alma10
```