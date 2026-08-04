# SDIS — Smart Driver Installation System

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?logo=powershell) ![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%2FServer-0078D6?logo=windows) ![License](https://img.shields.io/badge/License-Proprietary-red) ![Version](https://img.shields.io/badge/Version-v10.1%20Enterprise-brightgreen)

> **Automated Hardware Detection & Silent Driver Deployment Engine for Enterprise Infrastructure**  
> *Zero-Touch Automation | PowerShell / CLI Core*

---

> **Notice:** This repository contains conceptual architecture, documentation, and demonstration logic for SDIS. The full source code and core deployment engine are proprietary and protected under intellectual property registration.

---

## 📋 Table of Contents
- [Executive Overview](#executive-overview)
- [✨ Key Features](#-key-features)
- [System Architecture & 5-Stage Pipeline](#system-architecture--5-stage-pipeline)
- [Driver Matching Hierarchy](#driver-matching-hierarchy-priority-tiers)
- [Prerequisites & Requirements](#prerequisites--requirements)
- [🚀 Quick Start / Demonstration](#-quick-start--demonstration)
- [⚙️ Command-Line Parameters](#️-command-line-parameters)
- [🧩 Troubleshooting Common Issues](#-troubleshooting-common-issues)
- [Enterprise Business Impact](#enterprise-business-impact)
- [Contact & Licensing](#contact--licensing)

---

## Executive Overview
**SDIS** redefines enterprise driver management by transforming a notoriously manual, error-prone process into a fully automated, silent, and intelligent workflow. Designed for IT teams managing thousands of diverse endpoints, it slashes operational overhead, eliminates guesswork in driver selection, and accelerates hardware rollouts from days to mere minutes.

By automating the entire lifecycle—from low-level hardware detection and 5-tier intelligent matching to silent installation, port binding, and functional validation—SDIS guarantees that every printer, scanner, or specialized peripheral is deployed correctly on the first attempt, regardless of the user's technical expertise.

> 🚀 **Ready to see it in action?** Jump to the [Quick Start](#-quick-start--demonstration) guide.

📑 [Back to Table of Contents](#-table-of-contents)

---

## ✨ Key Features
- 🔍 **Zero-Configuration Detection** – Automatically scans and maps all connected peripherals via low-level port registry queries.
- ⚡ **Silent Multi-Format Installation** – Supports INF (direct), EXE (Inno/InstallShield with silent switches), and MSI packages without user prompts.
- 🧠 **Intelligent 5-Tier Matching Engine** – Employs a weighted scoring system (Exact → Token → Keyword → Hardware ID → Fallback) for pinpoint driver selection.
- 📊 **Comprehensive Reporting** – Generates detailed logs and summary reports (JSON/HTML) for audit and compliance.
- 🔄 **Port Binding & Validation** – Ensures drivers are correctly attached to virtual ports and performs functional readiness tests (e.g., test print jobs).
- 🌐 **Multi-Environment Ready** – Seamlessly works across Windows 10, Windows 11, and Windows Server 2019/2022.

📑 [Back to Table of Contents](#-table-of-contents)

---

## System Architecture & 5-Stage Pipeline
The core execution flow operates sequentially through five fully automated stages without requiring user intervention:

1️⃣ **Hardware Detection & Port Registry Mapping**  
Scans connected peripherals, filters out generic abstractions, extracts exact Hardware IDs, and programmatically maps USB devices directly to their OS virtual dynamic logical ports.

2️⃣ **5-Tier Model Identification & Weighted Driver Matching**  
Applies hierarchical priority matching (*Exact String → Token Substring → Keyword → Hardware ID → Class Fallback*) coupled with a multi-criteria scoring engine (*Architecture, Version metadata, OS compatibility*).

3️⃣ **Cascading Silent Installation (Hierarchy Mechanism)**  
Executes prioritized installation fallback sequence:  
**Priority 1** (*INF direct execution*) → **Priority 2** (*EXE unpack & silent switches*) → **Priority 3** (*MSI silent deployment*).

4️⃣ **Port Binding & Standardization**  
Verifies Plug-and-Play binding. Triggers OS-level dynamic disable/enable cycles if unattached, binds the driver to pre-extracted ports, and unifies display naming conventions.

5️⃣ **Functional Readiness Testing & Reporting**  
Dispatches active test payloads (e.g., test page print jobs) with initialization buffer timers, validating operational status and compiling an aggregated summary report.

📑 [Back to Table of Contents](#-table-of-contents)

---

## Driver Matching Hierarchy (Priority Tiers)

| Tier | Matching Level | Description | Target Matching Object |
| :--- | :--- | :--- | :--- |
| **Tier 1** | Exact String Matching | Direct comparison against vendor reference database models. | `Exact Model Name` |
| **Tier 2** | Tokenized Substring | Deconstructs device strings into structural tokens for partial matching. | `Model Tokens` |
| **Tier 3** | Keyword Analysis | Scans descriptive fields for verified hardware keywords. | `Verified Keywords` |
| **Tier 4** | Hardware ID Querying | Direct low-level match against Vendor (VID) & Product (PID) registry. | `VID / PID Registry` |
| **Tier 5** | Class Driver Fallback | Assigns verified generic/class drivers if specific binaries are absent. | `Generic OS Class` |

📑 [Back to Table of Contents](#-table-of-contents)

---

## Prerequisites & Requirements
- **Operating System:** Windows 10 (21H2+), Windows 11, or Windows Server 2019/2022.
- **PowerShell:** Version 5.1 or PowerShell 7.x (with `-ExecutionPolicy Bypass` allowed).
- **Network Access:** Read access to the driver repository (local drive or network share e.g., `\\DeployServer\DriverRepo`).
- **Permissions:** Local Administrator rights (required for INF staging, service control, and registry modifications).
- **Dependencies:** .NET Framework 4.8 (required for certain EXE unpackers and metadata parsing).

📑 [Back to Table of Contents](#-table-of-contents)

---

## 🚀 Quick Start / Demonstration

### Demo 1: Automated Printer Setup & Test Print
Here is a live demonstration showing **SDIS** automatically detecting connected printers, binding dynamic USB ports, executing silent driver installations, and running active test page jobs:

<video src="https://github.com/user-attachments/assets/a8db5803-7146-4a15-a88f-709379fd275f" controls width="100%"></video>

---

### Demo 2: Scanner Module Detection & WIA Scanning Test
Demonstrating low-level HP ScanJet scanner detection, INF driver staging, WIA refresh cycles, and executing an active test scan:

<video src="https://github.com/user-attachments/assets/8b925933-f18c-49e9-b91d-956faeadcc65" controls width="100%"></video>

> 💡 **Key Automation Capabilities:**
> - **Zero-Touch Execution:** Multi-device automatic hardware identification.
> - **Driver Staging:** Seamless silent INF package execution without user prompts.
> - **Operational Validation:** Automated post-installation testing for both print jobs and scanner imaging.
