# TinyUSB ESP-IDF Component

[![Component Registry](https://components.espressif.com/components/roma-jam/tinyusb/badge.svg)](https://components.espressif.com/components/roma-jam/tinyusb)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## Overview

This repository is non-offical (not mainained by Espressif) fork of [TinyUSB](https://github.com/hathach/tinyusb) for integration with the ESP-IDF build system.

For the official component, please refer to the [Espressif TinyUSB component](https://github.com/espressif/tinyusb).

### Versioning and branching

Main goal of the component is to provide the ESP-IDF compatible version.

Every upstream version (e.g., `0.20.0`) has a separate branch: `idf_component/v0.20.0` from which the releases are being made.

#### Release plan

Release version is usually available after the official release from the upstream branch.

In addition to that, 4 (or more, depending on the state of the beta) pre-release versions are available during the year to allow users to use the updated version and detect issues before the official release.

Component release version follows an original component versioning suffix scheme, that might be found in [Espressif component versioning](https://docs.espressif.com/projects/idf-component-manager/en/latest/reference/versioning.html)

To use the pre-release version, add the `pre-release: true` to the component manifest file:

```yaml
## IDF Component Manager Manifest File
  roma-jam/tinyusb:
    version: '>=0.20.0'
    pre_release: true
    public: true
```

### Examples

N/A

## How to use

This component is a part of the ESP-IDF compatible usb wrapper, that allows user to use different backend without changing the USB public API.

This component can be used directly and via the wrapper according to the goals.

### 1. Using via wrapper

N/A

### 2. Using directly

N/A
