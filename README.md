<div align="center">

<img src="snap/gui/rembg.png" alt="rembg logo" width="128" />

# rembg

**Remove image backgrounds**

[![rembg](https://snapcraft.io/rembg/badge.svg)](https://snapcraft.io/rembg)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [rembg](https://github.com/danielgatis/rembg), a tool that removes image backgrounds locally using AI models.

</div>

---

## Getting Started

<a href="https://snapcraft.io/rembg"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install rembg
rembg i ~/Pictures/photo.jpg ~/Pictures/photo-cut.png
```

The first run downloads the default model, `bria-rmbg`, which is about 1 GB. For a quicker start, pick a smaller one such as `-m u2net` (176 MB) or `-m u2netp` (5 MB). Process a whole folder, or pipe an image through:

```bash
rembg p ~/Pictures/input ~/Pictures/output
cat photo.jpg | rembg i > photo-cut.png
```

Start the HTTP API and browser interface at `http://127.0.0.1:7000`. On a desktop, it opens your default browser:

```bash
rembg s
```

To allow access from other devices on your network:

```bash
rembg s --host 0.0.0.0
```

Open `http://<your-computer-ip>:7000` on the other device and allow port 7000 through your firewall if needed.

## Configure

This snap has no `snap set` options. Choose a model with `-m` and see `rembg i --help` for the other options.

```bash
sudo snap connect rembg:removable-media   # optional: /media, /mnt
```

Models are stored in `~/snap/rembg/common/models` and are kept across snap refreshes. Download one ahead of time with `rembg d <model>`. Custom ONNX models passed with `model_path` must be placed under `~/snap/rembg/common`.

CPU support is included. For NVIDIA acceleration, install the optional CUDA component:

```bash
sudo snap install rembg+cuda
```

This requires an NVIDIA Turing or newer GPU and a [compatible NVIDIA driver](https://docs.nvidia.com/deeplearning/cudnn/backend/latest/reference/support-matrix.html) for the bundled CUDA 13 and cuDNN 9 libraries. Restart rembg after installing or removing the component. To return to CPU-only operation, run `sudo snap remove rembg+cuda`.

## Limitations

- AMD ROCm acceleration is not included.
- Hidden files directly under your home directory are inaccessible, including models in an existing `~/.u2net` folder. Copy them into `~/snap/rembg/common/models/<model>/` to avoid downloading them again.
- During CPU-only operation, ONNX Runtime warnings about GPU detection can be ignored.

## Build

```bash
git clone https://github.com/sidalit/rembg-snap.git
cd rembg-snap
sudo snap install snapcraft --classic
snapcraft pack
```

The build also produces the optional CUDA `.comp` package.

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/rembg-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/danielgatis/rembg/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/rembg-snap/blob/main/LICENSE).
