
<div align="center">
  <img src="https://github.com/user-attachments/assets/77c4ec39-703f-4509-9fb1-f75c6dde53f8" alt="SDIS Logo" width="160" />

  # SDIS — Smart Driver Installation System
</div>

  ![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?logo=powershell) ![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11%2FServer-0078D6?logo=windows) ![License](https://img.shields.io/badge/License-Proprietary-red) ![Version](https://img.shields.io/badge/Version-v10.1%20Enterprise-brightgreen)

  > **Automated Hardware Detection & Silent Driver Deployment Engine for Enterprise Infrastructure**  
  > *Zero-Touch Automation | PowerShell / CLI Core* 
 
> **Lead Developer:** `Eng. Jameel Hawary`  
> **Legends Team:** `Shrouq Al-Hazmi` | `Raghad Al-Shanbari` | `Tala Al-Qurashi` | `Turki Al-Sulami`

---

> **Notice:** This repository contains conceptual architecture, documentation, and demonstration logic for SDIS. The full source code and core deployment engine are proprietary and protected under intellectual property registration.

---

## 📋 Table of Contents
- [Executive Overview](#executive-overview)
- [ Key Features](#-key-features)
- [ Production Validation & Enterprise Scale](#-production-validation--enterprise-scale)
- [JSON Configuration & Data Architecture](#️-json-configuration--data-architecture)
- [System Architecture & 5-Stage Pipeline](#system-architecture--5-stage-pipeline)
- [Driver Matching Hierarchy](#driver-matching-hierarchy-priority-tiers)
- [Prerequisites & Requirements](#prerequisites--requirements)
- [Quick Start / Demonstration](#-quick-start--demonstration)

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

## ✅ Production Validation & Enterprise Scale

SDIS was rigorously field-tested and validated across production networks, delivering seamless zero-touch driver deployments across **2,000+ total endpoints**.

┌───────────────────────────┬───────────────────────────┐ │ 🖨️ Verified Printers │ 🖥️ Workstations / PCs │ │ 495+ Units │ 1,499+ Endpoints │ └───────────────────────────┴───────────────────────────┘
### 🖨️ Supported & Verified Printers
> *Tested across high-volume office laser printers, MFPs, and critical healthcare/industrial barcode label printers.*

* **HP Enterprise & Office Series**
  * `HP LaserJet Pro M402-M403` — **117** units
  * `HP LaserJet P2055d / dn` — **135** units
  * `HP PageWide Managed MFP P57750dw` — **100** units
  * `Hewlett-Packard HP LaserJet M402dn` — **101** units

* **Zebra Industrial & Healthcare Thermal Printers**
  * `Zebra HC100` *(Patient Wristband Printer)* — **20** units
  * `Zebra Stripe S4M` — **13** units
  * `Zebra ZT200 / ZT221` — **7** units
  * `Zebra ZT411` — **2** units

---
### 📠 Document Scanners & Imaging Peripherals
> *Validated for automated driver staging, WIA imaging subsystem refresh, and automated active scan tests.*
* **HP Professional Document Scanners**
  * `HP ScanJet Pro N4000 snw1` *(Network & USB Sheet-fed Scanner)* — **50** units

### 🖥️ Tested Client Architecture
> *Successfully tested across enterprise desktop hardware, workstations, and mini PCs.*

| Vendor | Workstation / PC Model | Fleet Count | Status |
| :--- | :--- | :---: | :---: |
| **HP** | EliteDesk 800 G6 SFF | **802** | 🟢 Verified |
| **HP** | Z240 SFF Workstation | **455** | 🟢 Verified |
| **HP** | Z230 SFF Workstation | **188** | 🟢 Verified |
| **Dell** | OptiPlex 7010 | **54** | 🟢 Verified |


📑 [Back to Table of Contents](#-table-of-contents)

---

## 🗂️ JSON Configuration & Data Architecture
SDIS utilizes structured JSON files to drive core system memory, automation reporting, and shared enterprise intelligence:

### 1. `printer_index.json` — Script Memory & Self-Learning Engine
Acts as the cumulative knowledge base, mapping each printer model/device to its verified driver path to eliminate redundant file searches and reduce lookup times from seconds to sub-milliseconds.
* **Indexing (`Add-ToPrinterIndex`):** Automatically registers the Hardware ID alongside the successful driver directory path and deployment method after a successful installation.
* **Instant Lookup (`Find-InPrinterIndex`):** Queries incoming devices (e.g., `USB\VID_03F0&PID_5C17&MI_00`) directly against the index to bypass full directory scanning.
* **Network Synchronization:** Stored centrally on a network share (`\\YOUR_SERVER_IP\...\printer_index.json`), allowing every endpoint to share and benefit from newly acquired intelligence dynamically.
* **Bounded Memory & Lazy Writing:** Maintains a capped history (max 30 paths per printer) and implements a *Dirty flag* mechanism to write to disk only when state changes occur, minimizing unnecessary I/O overhead.

### 2. `result.json` — Execution Reporting & Automation Feed
Powers post-execution monitoring, auditing, and automated workflow triggers.
* **Summary Generation (`Write-ResultJson`):** Records global success/failure metrics, individual device statuses, matching plans, and applied deployment methods.
* **Smart Exit Codes (`Exit Code`):** `Get-ResultExitCode` translates session outcomes into standard exit codes (`0 = Full Success`, `1 = Partial Failure`, `2 = Critical Failure`).
* **Automation Integration:** Consumed natively by enterprise management systems (`SCCM / Intune / Scripts`) via `C:\DriverTemp\result.json` to track host computers, timestamps, and compliance states.

### 3. Shared Knowledge Repository (`knowledge / clixml`)
* Dedicated storage for tracking successful printer deployment strategies and verified `EXE / INF` installation patterns to prevent historical failures in future deployments.

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
- **Network Access:** Read access to the driver repository (local drive or network share e.g., `\DeployServer\DriverRepo`).
- **Permissions:** Local Administrator rights (required for INF staging, service control, and registry modifications).
- **Dependencies:** .NET Framework 4.8 (required for certain EXE unpackers and metadata parsing).

📑 [Back to Table of Contents](#-table-of-contents)

---

## 🚀 Quick Start / Demonstration

Click the links below to watch the full-resolution system demonstrations:

* 🎬 **[Watch Demo 1: Automated Printer Setup & Test Print](https://github.com/user-attachments/assets/08c10213-b089-44ee-b6f7-a2ceed95bb1b)**  
  *Automated dynamic USB port binding, silent INF driver installation, and functional test page execution.*

* 🎬 **[Watch Demo 2: Scanner Module Detection & WIA Scanning Test](https://github.com/user-attachments/assets/05c648e1-5f7f-43fa-9fe3-45aa3e35efcf)**  
  *Low-level scanner detection, driver staging, WIA cycle refresh, and active test scan execution.*

> 💡 **Key Automation Capabilities Featured:**
> - **Zero-Touch Execution:** Multi-device automatic hardware identification.
> - **Driver Staging:** Seamless silent INF package execution without user prompts.
> - **Operational Validation:** Automated post-installation testing for both print jobs and scanner imaging.

📑 [Back to Table of Contents](#-table-of-contents)
