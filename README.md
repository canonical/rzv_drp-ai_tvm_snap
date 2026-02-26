# rzv_drp-ai_tvm_snap

Snap packaging for the example applications from the [Renesas DRP-AI TVM](https://github.com/renesas-rz/rzv_drp-ai_tvm) framework, targeting the **Renesas RZ/V2L** evaluation board (arm64, cross-compiled from amd64 or arm64).

## Overview

[DRP-AI TVM](https://www.renesas.com/application/key-technology/artificial-intelligence/ai-accelerator-drp-ai/ai-tool-drp-ai-tvm) is Renesas' AI framework for optimising and deploying AI models on RZ/V series hardware using the DRP-AI accelerator. This snap bundles the upstream example applications so they can be installed and managed via snapd on an Ubuntu-based RZ/V2L system.

## Packaged Applications

| Snap command | Binary | Upstream source |
|:---|:---|:---|
| `rzv-drp-ai-tvm-examples.tutorial-app` | `tutorial_app_v2ml` | [`apps/`](https://github.com/renesas-rz/rzv_drp-ai_tvm/tree/main/apps) — ResNet18/ResNet50 image-classification inference example |
| `rzv-drp-ai-tvm-examples.sample-app-drpai-tvm-usbcam-http` | `sample_app_drpai_tvm_usbcam_http` | [`how-to/sample_app/src/`](https://github.com/renesas-rz/rzv_drp-ai_tvm/tree/main/how-to/sample_app) — USB-camera inference with HTTP streaming output |

Both applications use the DRP-AI TVM runtime library (`libdrp_tvm_rt.so` and friends) which is also built from source as part of this snap.

## Prerequisites

- [Snapcraft](https://snapcraft.io/docs/installing-snapcraft) (LXD or Multipass backend recommended)
- Internet access to fetch upstream sources during the build

## Building

```sh
git clone https://github.com/canonical/rzv_drp-ai_tvm_snap.git
cd rzv_drp-ai_tvm_snap
snapcraft
```

The snap is built for `arm64` and can be cross-compiled on an `amd64` host. Snapcraft will automatically set up the required cross-compilation toolchain.

The resulting `rzv-drp-ai-tvm-examples_*.snap` file can be transferred to the target board for installation.

## Installing

Copy the snap file to the Renesas RZ/V2L board and install it in `devmode` (the snap currently uses `devmode` confinement):

```sh
sudo snap install --devmode rzv-drp-ai-tvm-examples_*.snap
```

## Running

### Tutorial app (ResNet inference)

The tutorial app requires the compiled model data and input files to be present on the board. Refer to the upstream [apps/README.md](https://github.com/renesas-rz/rzv_drp-ai_tvm/tree/main/apps) for details on preparing the model and input data.

```sh
rzv-drp-ai-tvm-examples.tutorial-app
```

### Sample app (USB camera + HTTP)

The USB-camera sample app requires a USB camera connected to the board. Refer to the upstream [how-to/sample_app](https://github.com/renesas-rz/rzv_drp-ai_tvm/tree/main/how-to/sample_app) for full usage instructions.

```sh
rzv-drp-ai-tvm-examples.sample-app-drpai-tvm-usbcam-http
```

## Snap Patches

Two patches are applied to the upstream sources during the build:

| Patch | Purpose |
|:---|:---|
| `cmake.patch` | Adds `install()` targets to the CMakeLists so the Snapcraft cmake plugin can install the built binaries |
| `ws.patch` | Adds missing `#include <cstdint>` to `system_analyzer.h` required for cross-compilation with newer toolchains |

## References

- [Renesas DRP-AI TVM repository](https://github.com/renesas-rz/rzv_drp-ai_tvm)
- [DRP-AI TVM application examples](https://github.com/renesas-rz/rzv_drp-ai_tvm/tree/main/apps)
- [RUHMI / DRP-AI TVM product page](https://www.renesas.com/en/software-tool/ruhmi-framework)
- [Snapcraft documentation](https://snapcraft.io/docs)
