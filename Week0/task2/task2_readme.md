# 🔧 EDA Tools Installation Guide for Ubuntu

> A comprehensive guide for setting up essential open-source Electronic Design Automation (EDA) tools on Ubuntu systems.

[![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04%2B-orange?logo=ubuntu)](https://ubuntu.com/)
[![VirtualBox](https://img.shields.io/badge/VirtualBox-Compatible-blue?logo=virtualbox)](https://www.virtualbox.org/)
[![License](https://img.shields.io/badge/License-Open%20Source-green)]()

## 📋 Table of Contents

- [System Prerequisites](#-system-prerequisites)
- [Tool Installation](#-tool-installation)
  - [Yosys](#1-yosys)
  - [Icarus Verilog](#2-icarus-verilog-iverilog)
  - [GTKWave](#3-gtkwave)
  - [OpenSTA](#4-OpenSTA)
  - [ngspice](#5-ngspice)
  - [Magic](#6-magic)
  - [OpenLane](#7-openlane)
- [Verification](#-verification)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)

---

## 🖥️ System Prerequisites

Before proceeding, ensure your system meets these minimum requirements:

| Component | Requirement |
|-----------|-------------|
| **Virtual Machine** | Oracle VirtualBox ([Download](https://www.virtualbox.org/wiki/Downloads)) |
| **Operating System** | Ubuntu 20.04 LTS or newer |
| **CPU** | 4 vCPUs minimum |
| **Memory** | 6 GB RAM |
| **Storage** | 50 GB free disk space |

> **⚠️ Note:** These tools can also be installed directly on native Ubuntu installations.

---

## 🛠️ Tool Installation

### 1. **Yosys**
*Framework for Verilog RTL synthesis*

Yosys is a powerful synthesis tool that converts Verilog RTL descriptions into gate-level netlists.

```bash
# Update package manager
sudo apt-get update

# Clone Yosys repository
git clone https://github.com/YosysHQ/yosys.git
cd yosys

# Install build dependencies
sudo apt install make
sudo apt-get build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev

# Configure and build
make config-gcc
make -j$(nproc)
sudo make install

# Verify installation
yosys
```

---

### 2. **Icarus Verilog (iverilog)**
*Verilog simulation and synthesis compiler*

A robust Verilog simulator that compiles Verilog source code into executable simulation format.

```bash
# Install via package manager
sudo apt-get update
sudo apt-get install iverilog

# Verify installation
iverilog -v
```

---

### 3. **GTKWave**
*Advanced waveform viewer*

GTKWave provides comprehensive waveform analysis capabilities for various file formats including VCD, LXT, and FST.

```bash
# Install GTKWave
sudo apt-get update
sudo apt install gtkwave

# Verify installation
gtkwave 
```
---

### 4. **OpenSTA**
*Static Timing Analysis Tool*

OpenSTA is an open-source static timing analysis (STA) tool used to verify the timing of digital circuits. It reads synthesized netlists and Liberty timing libraries to compute path delays, setup/hold violations, and slack, helping ensure that designs meet timing constraints before fabrication.

```bash
# Install dependencies
sudo apt-get update && sudo apt install git build-essential cmake tcl-dev tk-dev swig bison flex libeigen3-dev

# Clone OpenSTA repository
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA

# Create build directory
mkdir build
cd build

# Build OpenSTA
cmake ..
make -j$(nproc)

# Optionally, install system-wide
sudo make install

# Verify installation
sta --version
```
---
### 5. **ngspice**
*Mixed-signal circuit simulator*

ngspice is a powerful SPICE-compatible circuit simulator for analog and mixed-signal designs.

```bash
# Download from official source
# Visit: https://sourceforge.net/projects/ngspice/files/
# For version 45.2 (latest as of 2025):
wget https://sourceforge.net/projects/ngspice/files/ng-spice-rework/45.2/ngspice-45.2.tar.gz
tar -zxvf ngspice-45.2.tar.gz
cd ngspice-45.2

# Create build directory
mkdir release && cd release

# Configure build with GUI support
../configure --with-x --with-readline=yes --disable-debug

# Compile and install
make -j$(nproc)
sudo make install

# Verify installation
ngspice
```

---

### 6. **Magic**
*VLSI layout design tool*

Magic is a venerable and powerful tool for creating and editing integrated circuit layouts.

```bash
# Install dependencies
sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev \
    libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev

# Clone Magic repository
git clone https://github.com/RTimothyEdwards/magic.git
cd magic

# Configure and build
./configure
make -j$(nproc)
sudo make install

# Verify installation
magic
```

---

### 7. **OpenLane**
*Complete RTL-to-GDSII flow automation*

OpenLane provides an automated digital ASIC implementation flow using multiple open-source tools.

#### Step 1: Install Docker and Prerequisites

```bash
# Update system
sudo apt-get update && sudo apt-get upgrade 

# Install basic dependencies
sudo apt install -y build-essential python3 python3-venv python3-pip make git

# Install Docker prerequisites
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker GPG key and repository
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

# Configure Docker for non-root usage
sudo groupadd docker
sudo usermod -aG docker $USER

```

#### Step 2: Post-Reboot Setup

```bash
# Verify Docker installation
docker run hello-world

# Check all dependencies
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
```

#### Step 3: Install OpenLane

```bash
# Clone OpenLane repository
cd $HOME
git clone --depth 1 https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane

# Build OpenLane environment (this will take significant time)
make

# Run test to verify installation
make test
```

---

## 📊 Installed Tools Summary

| Tool | Purpose | Key Features |
|------|---------|--------------|
| **Yosys** | RTL Synthesis | Verilog → Gate-level conversion |
| **Icarus Verilog** | Simulation | Fast Verilog compilation & simulation |
| **GTKWave** | Waveform Analysis | Multi-format waveform viewer |
| **ngspice** | Circuit Simulation | SPICE-compatible analog/mixed-signal |
| **Magic** | Layout Design | VLSI physical design & DRC |
| **OpenLane** | Complete Flow | Automated RTL → GDSII |

---

## 🐛 Troubleshooting

### Common Issues

<details>
<summary><strong>Docker permission denied</strong></summary>

```bash
# Ensure user is in docker group
sudo usermod -aG docker $USER
newgrp docker

# Or restart your session
```
</details>

<details>
<summary><strong>Missing dependencies during compilation</strong></summary>

```bash
# Install additional development packages
sudo apt-get install -y build-essential cmake pkg-config
sudo apt-get install -y libx11-dev libxaw7-dev libxmu-dev
```
</details>

<details>
<summary><strong>Missing pdk</strong></summary>
I had to make changes in the make file. Below are the images and commands.
<br>
<img width="646" height="55" alt="image" src="https://github.com/user-attachments/assets/bcf95615-ac5d-49c7-98c5-68815dd384fe" />
<img width="614" height="127" alt="image" src="https://github.com/user-attachments/assets/a1c82d9b-556e-430d-9c3a-3bd5933b0c16" />


```bash
#Changes made in the commands
sudo chown -R $USER:$USER /home/revant-annam/.ciel
make pdk
make test PDK=sky130A
```
</details>

## 📚 Additional Resources

- 📖 [Icarus Verilog Manual](https://iverilog.fandom.com/)
- 📖 [Magic Tutorial](http://opencircuitdesign.com/magic/)
- 📖 [OpenLane Documentation](https://openlane.readthedocs.io/)



