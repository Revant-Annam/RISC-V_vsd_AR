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
  - [ngspice](#4-ngspice)
  - [Magic](#5-magic)
  - [OpenLane](#6-openlane)
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
sudo apt install make build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev

# Configure and build
make config-gcc
make -j$(nproc)
sudo make install

# Verify installation
yosys -V
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
gtkwave --version
```

---

### 4. **ngspice**
*Mixed-signal circuit simulator*

ngspice is a powerful SPICE-compatible circuit simulator for analog and mixed-signal designs.

```bash
# Download from official source
# Visit: https://sourceforge.net/projects/ngspice/files/
# For version 42 (latest as of 2024):
wget https://sourceforge.net/projects/ngspice/files/ng-spice-rework/42/ngspice-42.tar.gz
tar -zxvf ngspice-42.tar.gz
cd ngspice-42

# Create build directory
mkdir release && cd release

# Configure build with GUI support
../configure --with-x --with-readline=yes --disable-debug

# Compile and install
make -j$(nproc)
sudo make install

# Verify installation
ngspice --version
```

---

### 5. **Magic**
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
magic -noconsole -T min2 <<< "quit -noprompt"
```

---

### 6. **OpenLane**
*Complete RTL-to-GDSII flow automation*

OpenLane provides an automated digital ASIC implementation flow using multiple open-source tools.

#### Step 1: Install Docker and Prerequisites

```bash
# Update system
sudo apt-get update && sudo apt-get upgrade -y

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

# Reboot required for group changes
echo "⚠️  REBOOT REQUIRED! Please restart your system and continue with Step 2."
```

#### Step 2: Post-Reboot Setup

```bash
# Verify Docker installation
docker run hello-world

# Check all dependencies
echo "Checking installed versions:"
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

## ✅ Verification

After installation, verify all tools are working correctly:

```bash
# Create a simple verification script
cat << 'EOF' > verify_tools.sh
#!/bin/bash
echo "🔍 Verifying EDA Tools Installation..."
echo "======================================"

tools=("yosys" "iverilog" "gtkwave" "ngspice" "magic")

for tool in "${tools[@]}"; do
    if command -v $tool &> /dev/null; then
        echo "✅ $tool: INSTALLED"
    else
        echo "❌ $tool: NOT FOUND"
    fi
done

# Check OpenLane
if [ -d "$HOME/OpenLane" ]; then
    echo "✅ OpenLane: INSTALLED"
else
    echo "❌ OpenLane: NOT FOUND"
fi

echo "======================================"
echo "✨ Verification Complete!"
EOF

chmod +x verify_tools.sh
./verify_tools.sh
```

---

## 📊 Tool Summary

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
<summary><strong>OpenLane build failures</strong></summary>

```bash
# Ensure adequate disk space (>50GB)
df -h

# Clean and retry
make clean
make
```
</details>

---

## 📚 Additional Resources

- 📖 [Yosys Documentation](https://yosyshq.readthedocs.io/)
- 📖 [Icarus Verilog Manual](https://iverilog.fandom.com/)
- 📖 [Magic Tutorial](http://opencircuitdesign.com/magic/)
- 📖 [OpenLane Documentation](https://openlane.readthedocs.io/)
- 🎥 [Video Tutorials Playlist](https://youtube.com/playlist)

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add some improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ⭐ Support

If you find this helpful, please consider giving it a star! ⭐

For questions or support, please [open an issue](https://github.com/yourusername/eda-tools-setup/issues).

---

<div align="center">
  <strong>Happy Designing! 🚀</strong>
  <br>
  <em>Built with ❤️ for the open-source EDA community</em>
</div>
