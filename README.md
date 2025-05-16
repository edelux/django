# django PoC Static API Simulator Bundle

A minimalist Docker image powered by NGINX and BusyBox, built from Debian to simulate API responses or serve static content in JSON format.

This project aims to provide a lightweight, multi-architecture container image ideal for demos, development environments, testing pipelines, and educational scenarios involving security, cost analysis, and service democratization.

---

## 🚀 Why "FROM scratch" Matters

Using `FROM scratch` means our final image includes **only the essential binaries and libraries**—nothing more. This offers multiple benefits:

- **🪶 Smaller Image Size**: Reduces bandwidth and storage usage.
- **🛡️ Improved Security**: Smaller attack surface by excluding unnecessary tools and packages.
- **🔍 Full Transparency**: By analyzing binaries with tools like `ldd`, we identify exactly which dynamic libraries are required.
- **📦 Clean Packaging**: We use `tar` to copy files from the build stage while preserving ownerships, modes, and symlinks.

For deeper understanding of how to construct minimal Linux systems manually, refer to the [Linux From Scratch](http://www.linuxfromscratch.org/) project. ;)

---

## 🧬 Multi-Architecture Support

This image supports a wide range of architectures:

```
amd64, arm64, ppc64le, s390x, mips64le, riscv64, arm32v6, arm32v7, arm64v8, arm32v5, i386, windows-amd64
```

This is possible thanks to Debian’s wide platform support and the broader Linux community. Support is also seamlessly enabled by Docker BuildKit and `buildx`.

### 🌍 Why Multi-Arch Matters

- **📱 Embedded Systems** (e.g., Raspberry Pi)
- **💻 Developer Workstations** (e.g., Apple M1/M2 ARM chips)
- **☁️ Cloud-Native Apps** (e.g., AWS Graviton, which offers lower cost compute for ARM)
- **🏢 Enterprise Platforms** (e.g., s390x in mainframes)

Adding multi-architecture support costs virtually **nothing** in terms of development time but greatly expands compatibility and portability. A win-win. 🎯

---

## 🏷️ Why Label Docker Images?

Labels add metadata to your images to:

- Document author, version, source, license, etc.
- Improve automation in CI/CD pipelines.
- Aid vulnerability scanners and catalog systems.
- Provide insights when running `docker inspect`.

This project follows the [OCI image label spec](https://github.com/opencontainers/image-spec/blob/main/annotations.md).

---

## 🔐 Why Alpine Linux?

- **📦 Tiny Size**: The base image is under 6MB.
- **⚡ Fast Boot Time**: Ideal for short-lived containers and serverless.
- **🔐 Simplicity Equals Security**: Fewer packages mean fewer vulnerabilities.
- **🔁 Common in Production**: Used by many official images (Node.js, Python, Go).
- **🧭 Great for Embedded**: Used in routers, gateways, and low-memory devices.

Alpine is the distribution of choice when you need a functional, minimal, POSIX-compliant system that excels in modern containerized workflows.

---

## 🛠 How to Build (with Docker CE + buildx)

Build multi-architecture images like this:

```sh
docker buildx create --use
docker buildx build \
  --platform linux/amd64,linux/arm64,linux/ppc64le \
  --build-arg APP=django \
  --build-arg VERSION=1.0 \
  --tag edelux/django:1.0 \
  --push .
```

### 🧩 Other build options include:

1. **BuildKit natively** via Dockerfile frontend (`# syntax=docker/dockerfile:1`)
2. **Kaniko**: Useful for secure builds inside Kubernetes
3. **Bazel + rules_docker**: Powerful and reproducible builds in monorepos

---

## ⚖️ License

This project is released under the **MIT License**, giving you full freedom to:

- Use it commercially 🧑‍💼
- Modify and relicense it 💡
- Restrict it internally 🏢
- Share it publicly 🌐

Use it as your base image, or evolve it into your own secured, branded gateway. 🛠️
