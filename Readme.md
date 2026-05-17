# Minimal Zephyr Workspace for Espressif + Nordic

Small and opinionated Zephyr workspace setup focused on:

* Espressif targets
* Nordic targets
* smaller workspace
* faster `west update`
* minimal dependency checkout

The goal is to avoid rebuilding the same Zephyr workspace structure over and over again for small projects, prototypes, and experiments. Maybe it is useful for somebody else as well. It might be updated to different Zephyr versions in future.

---

## Table of Contents

* [Overview](#overview)
* [Workspace Layout](#workspace-layout)
* [Requirements](#requirements)
* [Initial Setup](#initial-setup)
* [Building Applications](#building-applications)
* [Example Boards](#example-boards)
* [Flashing](#flashing)
* [Further Information](#further-information)

---

## Overview

This repository uses a T2 Star Topology workspace layout.

For more information:

* [Zephyr Workspace Topologies](https://docs.zephyrproject.org/latest/develop/west/workspaces.html#t2-star-topology-application-is-the-manifest-repository)

Based on:

* Zephyr v4.3.0

Linux paths/examples are used throughout this repository.

---

## Workspace Layout

Some overlays and board configuration files are already included to avoid recreating them for every project. Have a look in the [boards folder](/applications/hello_world/boards/).

---

## Requirements

The Zephyr SDK must already be installed.

See:

* [Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)

The `.gitignore` file is committed for convenience.

If desired, it can later be ignored locally via:

```bash
git update-index --skip-worktree .gitignore
```

> [!IMPORTANT]
> This workspace intentionally uses a reduced module set.
> Additional Zephyr features may require adding missing modules manually.

---

## Initial Setup

1. Create workspace and clone repository

```bash
mkdir <your-workspace>
cd <your-workspace>
git clone git@github.com:jstr00/zephyr-reduced-workspace.git .
```

2. Create Python virtual environment

```bash
python -m venv venv
source venv/bin/activate
```

3. Install west

```bash
pip install west
```

4. Fetch repositories and Python dependencies

```bash
west update -n
west zephyr-export
west packages pip --install
```
> [!TIP]
> `-n` enables narrow cloning and reduces checkout size and update time.

5. Fetch Espressif binary blobs (required for WiFi/Bluetooth/OpenThread support)

```bash
west blobs fetch hal_espressif
```

---

## Building Applications

```bash
cd applications/hello_world
```

List available boards:

```bash
west boards
```

---

## Example Boards

### Nordic (seeed)

```bash
west build --board=xiao_nrf54l15/nrf54l15/cpuapp .
```

### Espressif ESP32-C6

```bash
west build --board=esp32c6_devkitc/esp32c6/hpcore .
```

<p float="left">
  <img src="/ressources/esp32_c6.jpg" width="280">
</p>

### Espressif ESP32-H2

```bash
west build --board=esp32h2_devkitm .
```

<p float="left">
  <img src="/ressources/esp32_h2.jpg" width="280">
</p>

---

## Flashing

```bash
west flash
```

---

## Further Information

* [Official Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)
* [West Documentation](https://docs.zephyrproject.org/latest/develop/west/index.html)
* [Zephyr Board Documentation](https://docs.zephyrproject.org/latest/boards/index.html)
* My tip for the most applications is the esp32-c6 because it supports WiFi and 802.15.4 and B(LE), [see](https://www.espressif.com/en/products/socs/esp32-c6)


> [!NOTE]
> This repository is intentionally minimalistic.
> If something is missing, it was probably removed on purpose.