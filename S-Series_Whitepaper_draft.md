# MRDVS S-Series dToF RGB-D Camera White Paper

---

## Table of Contents

- [Chapter 1: Overview](#chapter-1-overview)
  - [1.1 Company Introduction](#11-company-introduction)
  - [1.2 Document Purpose and Scope](#12-document-purpose-and-scope)
  - [1.3 Intended Audience](#13-intended-audience)
  - [1.4 Document Conventions](#14-document-conventions)
  - [1.5 Glossary](#15-glossary)
  - [1.6 Related Documents](#16-related-documents)
- [Chapter 2: Hardware System](#chapter-2-hardware-system)
  - [2.1 Product Family Overview](#21-product-family-overview)
  - [2.2 dToF Technology Fundamentals](#22-dtof-technology-fundamentals)
  - [2.3 Core Technical Advantages](#23-core-technical-advantages)
  - [2.4 S10 Series Technical Specifications](#24-s10-series-technical-specifications)
  - [2.5 S11 Series Technical Specifications](#25-s11-series-technical-specifications)
  - [2.6 Mechanical and Electrical Characteristics](#26-mechanical-and-electrical-characteristics)
  - [2.7 Product Comparison and Selection Guide](#27-product-comparison-and-selection-guide)
  - [2.8 Certifications and Compliance](#28-certifications-and-compliance)
- [Chapter 3: Software & Toolchain](#chapter-3-software--toolchain)
  - [3.1 Host Configuration Tool — LxCameraViewer](#31-host-configuration-tool--lxcameraviewer)
  - [3.2 Camera SDK](#32-camera-sdk)
  - [3.3 ROS & ROS2 Integration](#33-ros--ros2-integration)
  - [3.4 Software Installation and Updates](#34-software-installation-and-updates)
  - [3.5 Communication Protocols](#35-communication-protocols)
  - [3.6 Algorithm Deployment Toolchain](#36-algorithm-deployment-toolchain)
- [Chapter 4: Application Solutions](#chapter-4-application-solutions)
  - [4.1 Mobile Robot Obstacle Avoidance](#41-mobile-robot-obstacle-avoidance)
  - [4.2 Pallet Recognition](#42-pallet-recognition)
  - [4.3 Location Status Detection (Slot Detection)](#43-location-status-detection-slot-detection)
  - [4.4 Human Safety Detection](#44-human-safety-detection)
  - [4.5 Volume Measurement](#45-volume-measurement)
  - [4.6 Multi-Camera Volumetric Measurement](#46-multi-camera-volumetric-measurement)
- [Chapter 5: Integration & Development Guide](#chapter-5-integration--development-guide)
  - [5.1 SDK Quick Start](#51-sdk-quick-start)
  - [5.2 Communication Interfaces](#52-communication-interfaces)
  - [5.3 Algorithm Integration](#53-algorithm-integration)
  - [5.4 Multi-Camera Coordination](#54-multi-camera-coordination)
  - [5.5 System Integration Best Practices](#55-system-integration-best-practices)
- [Chapter 6: Deployment & Installation Guide](#chapter-6-deployment--installation-guide)
  - [6.1 Camera Mounting](#61-camera-mounting)
  - [6.2 Network Configuration](#62-network-configuration)
  - [6.3 Software Deployment](#63-software-deployment)
  - [6.4 Calibration Procedures](#64-calibration-procedures)
  - [6.5 Maintenance and Care](#65-maintenance-and-care)
- [Chapter 7: Frequently Asked Questions](#chapter-7-frequently-asked-questions)
  - [7.1 Hardware Issues](#71-hardware-issues)
  - [7.2 Software and Streaming Issues](#72-software-and-streaming-issues)
  - [7.3 Application Algorithm Issues](#73-application-algorithm-issues)
  - [7.4 Secondary Development Issues](#74-secondary-development-issues)
- [Chapter 8: Appendices](#chapter-8-appendices)
  - [Appendix A: Technical Specifications Summary](#appendix-a-technical-specifications-summary)
  - [Appendix B: Certifications and Compliance](#appendix-b-certifications-and-compliance)
  - [Appendix C: Ordering Information](#appendix-c-ordering-information)
  - [Appendix D: Contact and Support](#appendix-d-contact-and-support)
  - [Appendix E: Document Revision History](#appendix-e-document-revision-history)

---

# Chapter 1: Overview

## 1.1 Company Introduction

MRDVS is an industrial 3D vision technology company headquartered in Hangzhou, China. The company specializes in designing and manufacturing depth-sensing cameras and integrated vision solutions for autonomous mobile robots (AMRs), automated guided vehicles (AGVs), unmanned forklifts, humanoid robots, and industrial automation systems. Its technology enables robots to operate autonomously in GPS-denied indoor environments, performing real-time obstacle detection, environmental mapping, and spatial measurement across logistics, manufacturing, and service robotics sectors. MRDVS serves global enterprise customers and system integrators deploying robotic fleets in demanding 24/7 operational conditions.

The company's technical competencies span three integrated layers: (1) 3D camera hardware, incorporating dToF depth sensors with SPAD arrays, VCSEL emitters, RGB imaging modules, and ARM-based SoC computing; (2) embedded perception algorithms for obstacle avoidance, pallet recognition, location status detection, human safety monitoring, and volumetric measurement; and (3) software platform tools, including the LxCameraViewer host configuration utility and the Camera SDK supporting C/C++, Python, ROS, and ROS2 integration. Additional product information is available at https://mrdvs.com/.

[IMAGE: Company/product family photo placeholder]

## 1.2 Document Purpose and Scope

This white paper provides a comprehensive technical reference for evaluating, integrating, and deploying the MRDVS S-Series dToF RGB-D cameras in robotic and automation systems. It is the primary technical guide for engineering teams specifying 3D vision sensors for mobile robots and warehouse automation platforms.

**Products covered in this document:**

- **S10** (Ethernet; compact form factor; depth FoV 120° x 80°; range 0.3--8 m at 90% reflectivity)
- **S10 Pro** (Ethernet + MIPI + RGB; range up to 17 m; edge computing; IP67 enclosure)
- **S11** (Ethernet / USB / MIPI variants; depth FoV 140° x 56°; range 0.1--6 m outdoor, 10 m indoor)

**Document scope includes the following technical areas:**

- Hardware specifications: sensor architecture, interface definitions, mechanical dimensions, optical characteristics, and environmental ratings
- Software toolchain: Camera SDK, LxCameraViewer host configuration tool, ROS/ROS2 drivers, and firmware management
- Application solutions: Obstacle Avoidance, Pallet Recognition, Location Status Detection, Human Safety Detection, and Volume Measurement
- Integration procedures: device initialization, network configuration, calibration (intrinsic and extrinsic), and multi-camera deployment
- Deployment guidance: installation requirements, interference mitigation, accuracy compensation, and maintenance protocols

Target readers are overseas enterprise customers, system integrators, and robotics engineers responsible for perception system design and deployment.

## 1.3 Intended Audience

This document is written for technical professionals evaluating, integrating, and deploying 3D vision systems in robotic and industrial automation environments. The intended audience includes:

- **Robotics and automation engineers** responsible for sensor selection, perception pipeline design, and navigation system development
- **System integrators** designing mobile robot platforms, warehouse automation systems, and multi-vehicle fleets
- **Technical decision-makers** comparing 3D sensing technologies against operational requirements
- **Software developers** integrating camera SDKs into robot operating systems and custom applications
- **Commissioning engineers** planning installation, calibration, and maintenance of camera-based systems

## 1.4 Document Conventions

This document uses the following notation and formatting conventions:

- **Tables** present product specifications, parameter comparisons, and configuration options.
- **Bold text** highlights product names, key terms, and section headings.
- `Code-formatted text` indicates API names, protocol commands, file paths, data format identifiers, and network addresses.
- Metric units are used as the primary measurement system; imperial approximations are provided in parentheses where helpful.
- Temperature ranges use the Celsius scale.
- Model variant references use the format **S10 / S10 Pro / S11** unless a specific variant is indicated (e.g., S11 USB, S11 MIPI).
- The term "camera" or "device" refers to any S-Series model unless explicitly qualified by model name.
- Specification tables use "typical" to denote standard operating conditions and "max." to denote maximum rated capability.
- Reflectivity percentages (e.g., 90%, 10%, 5%) describe target surface reflectance at 940 nm wavelength.

## 1.5 Glossary

| Term | Definition |
|------|-----------|
| dToF | direct Time-of-Flight. Measures absolute photon travel time using SPAD sensors and TCSPC to compute depth. |
| iToF | indirect Time-of-Flight. Computes depth from the phase shift of modulated light. |
| RGB-D | Color plus Depth sensing; outputs registered color and depth data. |
| FOV | Field of View. Angular coverage of the camera lens, expressed as horizontal x vertical degrees. |
| SPAD | Single-Photon Avalanche Diode. High-sensitivity photon detector used in dToF sensors. |
| TCSPC | Time-Correlated Single-Photon Counting. Photon timing technique for dToF depth measurement. |
| TDC | Time-to-Digital Converter. On-chip circuit that converts photon arrival time to digital depth values. |
| VCSEL | Vertical-Cavity Surface-Emitting Laser. Emitter used in S-Series active illumination at 940 nm. |
| MPI | Multi-Path Interference. Signal overlap from reflective surfaces or adjacent sensors causing depth errors. |
| HDR | High Dynamic Range. Ability to measure accurately across extreme surface reflectivity variations. |
| ROI | Region of Interest. Configurable sub-area of the image frame for focused detection or processing. |
| SDK | Software Development Kit. MRDVS Camera SDK providing APIs for device control, data acquisition, and algorithm configuration. |
| SoC | System on Chip. Integrated ARM-based processor embedded in S-Series cameras for onboard depth calculation and algorithm execution. |
| AMR | Autonomous Mobile Robot. Self-navigating robotic platform using onboard sensors and computing for path planning. |
| AGV | Automated Guided Vehicle. Mobile robot that follows predefined paths, markers, or magnetic strips. |
| SLAM | Simultaneous Localization and Mapping. Technique for building environmental maps while tracking robot position in real time. |
| PTP | Precision Time Protocol (IEEE 1588). Network-based time synchronization for multi-device coordination. |
| IP54 | Ingress Protection rating: protected against dust ingress and water splashing from any direction. |
| IP67 | Ingress Protection rating: dust-tight and protected against temporary immersion in water. |
| DWS | Dimensioning, Weighing, and Scanning. Automated freight measurement system for logistics and warehousing. |
| FPS | Frames Per Second. Rate at which the camera captures and outputs image data. |
| Lux | Unit of illuminance; 1 lux = 1 lumen per square meter. Used to quantify ambient light tolerance. |
| Point Cloud | Set of 3D coordinates representing the spatial structure of a scene, computed from depth map data. |
| GMSL | Gigabit Multimedia Serial Link. High-speed serial interface for camera data transmission (S10 variant option). |
| I/O | Input/Output. Discrete digital signals for safety interlock and algorithm result output (available on select models). |

## 1.6 Related Documents

The following technical documents supplement this white paper and should be referenced during system design, integration, and deployment:

- **S10 and S11 Product Datasheets** — Hardware specifications, electrical parameters, and environmental ratings
- **S10/S11 Camera User Manuals** — Installation procedures, wiring diagrams, and quick-start guides
- **Camera SDK Development Guide** — API reference, code examples, and supported platform details
- **ROS/ROS2 Integration Guide** — Driver installation, topic mapping, and sample nodes
- **Application Solution Deployment Manuals** — Procedures for Obstacle Avoidance, Pallet Recognition, Location Status Detection, Human Safety Detection, and Volume Measurement
- **LxCameraViewer User Manual** — Host tool operation, parameter configuration, and firmware updates
- **CAD Drawings and 3D Models** — Mechanical drawings and enclosure models for integration design
- **Compliance Certification Documents** — CE, FCC, RoHS, and Class I laser safety certifications

# Chapter 2: Hardware System

## 2.1 Product Family Overview

The MRDVS S-Series is a family of industrial-grade RGB-D cameras built on direct Time-of-Flight (dToF) depth-sensing technology. Engineered for mobile robotics, automated logistics, and industrial automation, the S-Series delivers robust 3D perception in challenging environments ranging from dark warehouses to direct sunlight exposure. All models integrate an onboard ARM-based SoC that performs depth computation, RGB-D alignment, and algorithm execution at the edge, eliminating dependency on external processing hardware.

The product family consists of three models:

- **S10**: A cost-effective, compact camera designed for high-volume deployment. It provides dToF depth sensing with optional RGB via Gigabit Ethernet, plus discrete I/O for direct integration with vehicle safety controllers. Two field-of-view variants are available: 90° x 60° and 120° x 80°. A Lite version without RGB is offered for pure depth applications.

- **S10 Pro**: An enhanced long-range variant with a 61° x 90° FOV, extending detection to 17 m under high-reflectivity conditions. The S10 Pro delivers RGB imagery and depth data over Gigabit Ethernet, with discrete I/O for safety-critical obstacle avoidance signaling. Its larger form factor accommodates a higher-power VCSEL emitter and enhanced optics for extended-range operation.

- **S11**: An ultra-compact, all-solid-state RGB-D camera designed for size-constrained mobile robots. The S11 is available in three interface configurations: Gigabit Ethernet, USB3.0, and MIPI. With a 140° x 56° depth FOV and a weight of only 90-130 g depending on variant, the S11 targets cleaning robots, service robots, and compact AGVs where space and power budgets are severely limited. A MIPI module variant (33 mm x 18 mm x 13 mm, <10 g) is available for deeply embedded designs.

Target applications by model:

| Model | Primary Applications |
|-------|---------------------|
| S10 | Unmanned forklifts, AMRs, AGVs, high-speed conveyors, outdoor logistics |
| S10 Pro | Long-range outdoor navigation, large-format vehicles, yard trucks, high-bay warehouses |
| S11 (ETH/USB) | Cleaning robots, service robots, compact AMRs, indoor delivery bots |
| S11 (MIPI) | Embedded robot designs, consumer robotics, deeply integrated vision modules |

[IMAGE: Product family comparison photo showing S10, S10 Pro, and S11 side by side with scale reference]

## 2.2 dToF Technology Fundamentals

The S-Series employs direct Time-of-Flight (dToF) depth measurement, a fundamentally different approach from indirect ToF (iToF), structured light, or stereo vision.

### Operating Principle

The dToF sensor core consists of:

- **940 nm VCSEL emitter**: Generates nanosecond-scale laser pulses at eye-safe power levels
- **SPAD (Single-Photon Avalanche Diode) array**: Detects individual returning photons with extreme sensitivity
- **TDC (Time-to-Digital Converter)**: Measures photon arrival time on a per-pixel basis
- **TCSPC (Time-Correlated Single Photon Counting)**: Builds histograms of photon arrival times to determine the most probable distance

Distance is computed directly from the time interval between pulse emission and photon detection: `d = (c * t) / 2`, where `c` is the speed of light and `t` is the measured round-trip time. Because the system measures absolute time rather than phase difference, depth accuracy remains constant across the full operating range.

### Key Advantages Over iToF

| Characteristic | dToF (S-Series) | iToF |
|----------------|-----------------|------|
| Measurement basis | Direct photon arrival time | Phase shift of modulated light |
| Multi-path interference | Minimal -- first photon arrival is unambiguous | Significant -- phase aliasing from mixed paths |
| Motion blur | None -- single-pulse measurement | Present -- requires multi-phase capture |
| Sunlight immunity | Excellent -- time-gated SPAD rejects ambient photons | Moderate -- unmodulated light corrupts phase data |
| Accuracy vs. distance | Constant (<= 3 cm) | Degrades with distance |
| Maximum range | Up to 17 m (S10 Pro) | Typically limited to 5 m |
| Multi-device coexistence | Excellent -- pulse timing enables temporal isolation | Poor -- phase interference between units |

The S-Series uses a 940 nm VCSEL operating at Class I laser safety limits (IEC 60825-1), making it eye-safe under all normal operating conditions. The high fill-factor SPAD array ensures superior photon collection efficiency, enabling reliable detection of low-reflectivity targets (down to 5-10% reflectance) at maximum range.

[IMAGE: dToF vs iToF comparison diagram showing pulse-based vs phase-based measurement principles]

## 2.3 Core Technical Advantages

Based on the S10 and S11 platform architecture, the S-Series delivers the following technical advantages:

1. **Long-range precision**: The S10 achieves 0.3-8 m at 90% reflectivity with <= 3 cm accuracy; the S10 Pro extends this to 0.3-17 m. The S11 delivers 0.1-6 m outdoors (up to 10 m indoors) with 1 cm accuracy.

2. **Anti-motion blur**: Because dToF captures depth in a single laser pulse rather than through multi-phase integration, there is no motion blur from platform movement or dynamic scene changes. This is critical for AMRs operating at speed.

3. **Anti-multipath performance**: Direct time measurement resolves the first-returning photon, rejecting later-arriving multipath reflections. In narrow warehouse aisles with reflective floors and racking, this eliminates ghost objects and distance artifacts that plague phase-based systems.

4. **Sunlight immunity**: The combination of high-power pulsed VCSEL illumination and time-gated SPAD detection enables operation in up to 100 kLux ambient light -- equivalent to full direct sunlight. The temporal filter only accepts photons arriving within the expected return window, rejecting continuous background illumination.

5. **High frame rate**: The S10 achieves up to 20 fps maximum (15 fps typical); the S11 achieves up to 20 fps maximum (10 fps typical). The S10 Pro operates at up to 10 fps, trading frame rate for extended range.

6. **High fill-factor SPAD**: The SPAD pixel architecture provides superior photon sensitivity compared to conventional CMOS-based ToF sensors, enabling reliable detection of dark or oblique surfaces that would produce insufficient signal in other technologies.

7. **Built-in edge computing**: The integrated SoC performs all depth computation, RGB-D alignment, point cloud generation, and obstacle avoidance algorithm execution onboard. The S10 and S10 Pro support direct I/O output of safety signals (decelerate/stop) without requiring an external PC or controller.

## 2.4 S10 Series Technical Specifications

| Parameter | S10 | S10 Pro |
|-----------|-----|---------|
| **Depth Technology** | dToF (SPAD + TDC) | dToF (SPAD + TDC) |
| **Laser Wavelength** | 940 nm | 940 nm |
| **Laser Safety Class** | Class I (IEC 60825-1) | Class I (IEC 60825-1) |
| **Depth Resolution** | 240 x 160 | 240 x 160 |
| **Depth Frame Rate** | Max 20 fps (typical 15 fps) | Max 10 fps (typical 10 fps) |
| **Depth FOV (H x V)** | 90° x 60° ± 3° or 120° x 80° ± 3° | 61° x 90° ± 3° |
| **RGB Resolution** | 1920 x 1080 or 1632 x 1224 (Lite: no RGB) | 1920 x 1080 |
| **RGB Frame Rate** | Max 20 fps (typical 15 fps) | Max 10 fps (typical 10 fps) |
| **RGB FOV (H x V)** | 90° x 60° ± 3° or 120° x 80° ± 3° | 61° x 90° ± 3° |
| **Distance Range (90% reflectivity)** | 0.3-8 m | 0.3-17 m |
| **Distance Range (low reflectivity)** | 0.3-3 m (5%) | 0.3-13 m (10%) |
| **Precision** | <= 3 cm | <= 3 cm |
| **Dimensions (L x W x H)** | 80 x 37 x 25 mm (3.15 x 1.46 x 0.98 in) | 92 x 47 x 51 mm (3.62 x 1.85 x 2.01 in) |
| **Weight** | 190 g (0.42 lb) | 460 g (1.01 lb) |
| **Power Supply** | 12-28 V DC | 24 V DC |
| **Power Consumption** | <= 4 W average, 28 W peak | <= 7 W average |
| **Interface** | Gigabit Ethernet, GMSL (optional), I/O | Gigabit Ethernet, I/O |
| **I/O Channels** | 2x input, 2x output (Lite 120° version) | 2x input, 2x output |
| **Operating Temperature** | -20°C to +60°C | -20°C to +60°C |
| **Storage Temperature** | -40°C to +85°C | -40°C to +85°C |
| **Ingress Protection** | IP54 | IP67 |
| **Ambient Light Resistance** | Up to 100 kLux | Up to 100 kLux |
| **Time Synchronization** | PTP (IEEE 1588) | PTP (IEEE 1588) |
| **Cooling** | Passive (fanless) | Passive (fanless) |
| **SDK / OS Support** | C/C++, ROS / Windows 7+, Linux, Arm Linux/ROS | C/C++, ROS / Windows 7+, Linux, Arm Linux/ROS |

Notes:
- The S10 Lite version is offered without RGB for cost-sensitive pure-depth applications.
- The S10 120° FOV variant includes an I/O cable for direct safety signal integration.
- GMSL interface availability requires consultation with MRDVS sales.
- Product dimensions should be verified against the 3D mechanical model for integration design.

[IMAGE: S10 dimension drawing with mounting hole pattern and connector positions]

## 2.5 S11 Series Technical Specifications

| Parameter | S11 (Ethernet) | S11 (USB) | S11 (MIPI) |
|-----------|---------------|-----------|------------|
| **Depth Technology** | dToF (SPAD + TDC) | dToF (SPAD + TDC) | dToF (SPAD + TDC) |
| **Laser Wavelength** | 940 nm | 940 nm | 940 nm |
| **Laser Safety Class** | Class I (IEC 60825-1) | Class I (IEC 60825-1) | Class I (IEC 60825-1) |
| **Depth Resolution** | 240 x 96 | 240 x 96 | 240 x 96 |
| **Depth Frame Rate** | Max 20 fps (typical 10 fps) | Max 20 fps (typical 10 fps) | Max 20 fps (typical 10 fps) |
| **Depth FOV (H x V)** | 140° x 56° ± 3° | 140° x 56° ± 3° | 140° x 56° ± 3° |
| **RGB Resolution** | 1280 x 1080 | 1280 x 1080 | N/A |
| **RGB Frame Rate** | Max 20 fps (typical 10 fps) | Max 20 fps (typical 10 fps) | N/A |
| **RGB FOV (H x V)** | 120° x 110° ± 3° | 120° x 110° ± 3° | N/A |
| **RGBD Alignment** | Supported | Supported | N/A |
| **Distance Range (outdoor)** | 0.1-6 m (10%-90% reflectivity) | 0.1-6 m (10%-90% reflectivity) | 0.1-6 m (10%-90% reflectivity) |
| **Distance Range (indoor max)** | Up to 10 m | Up to 10 m | Up to 10 m |
| **Precision** | 1 cm | 1 cm | 1 cm |
| **Dimensions (L x W x H)** | 90 x 25 x 25 mm (3.54 x 0.98 x 0.98 in) | 90 x 25 x 25 mm (3.54 x 0.98 x 0.98 in) | 33 x 18 x 13 mm (1.30 x 0.71 x 0.51 in) |
| **Weight** | ~130 g (~0.29 lb) | ~90 g (~0.20 lb) | <10 g |
| **Power Supply** | 12-28 V DC | 5 V (USB3.0 bus power) | 3.3 V / 5 V DC |
| **Power Consumption** | <4 W average, 13 W peak | <4 W average, 30 W peak | <=2 W average, 8 W peak |
| **Interface** | Gigabit Ethernet | USB3.0 | MIPI |
| **Operating Temperature** | -20°C to +60°C | -20°C to +60°C | N/A (consult factory) |
| **Storage Temperature** | -40°C to +70°C | -40°C to +70°C | N/A (consult factory) |
| **Ingress Protection** | IP54 | N/A (consult factory) | N/A (consult factory) |
| **Ambient Light Resistance** | 100 kLux | 100 kLux | 100 kLux |
| **Time Synchronization** | PTP (IEEE 1588) | PTP (IEEE 1588) | N/A |
| **SDK / OS Support** | C/C++, ROS / Windows 7+, Linux, Arm Linux/ROS | C/C++, ROS / Windows 7+, Linux, Arm Linux/ROS | C/C++, ROS / Windows 7+, Linux, Arm Linux/ROS |

Notes:
- The S11 MIPI variant is a module-level product intended for deep integration into host systems. Mechanical, thermal, and electrical integration parameters should be confirmed with MRDVS engineering.
- The S11 USB variant peak power (30 W) occurs during brief VCSEL pulse bursts; average power remains below 4 W.
- Both depth and RGB FOV tolerances are ±3°.

[IMAGE: S11 dimension drawing showing all three variants (Ethernet, USB, MIPI module) with connector positions]

## 2.6 Mechanical and Electrical Characteristics

### Dimensions and Mounting

The S10 enclosure is a rectangular aluminum housing with integral mounting features. The S10 (80 x 37 x 25 mm) uses two M3 threaded mounting holes on the rear face, spaced 60 mm apart. The S10 Pro (92 x 47 x 51 mm) uses a four-point mounting pattern with M3 holes. Both models require a minimum clearance zone in front of the sensor window equal to the nominal FOV cone -- no structural elements may intrude into this volume, as near-field reflections will corrupt depth data.

The S11 (90 x 25 x 25 mm) employs a slim stick-form factor suitable for mounting along robot body edges or under protective shrouds. The MIPI module (33 x 18 x 13 mm) is designed for direct PCB mounting via connector retention.

### Connector Definitions

**S10 / S10 Pro:**

The camera end provides two or three interfaces depending on variant:

- **DC power interface**: 2-pin connector, accepts 12-28 V DC (S10) or 24 V DC (S10 Pro). A 24 V/2 A adapter is recommended.
- **Ethernet interface**: RJ45-compatible port, 100 Mbps/1 Gbps auto-negotiation. Category 6 cabling is recommended for stable data transmission.
- **I/O interface** (8-pin, S10 Lite 120° and S10 Pro): Provides discrete digital I/O for safety integration.

**S11:**

- **Ethernet variant**: DC power barrel connector (12-28 V) plus RJ45 Ethernet port
- **USB variant**: USB Type-C 3.0 connector (5 V bus power + data)
- **MIPI variant**: Board-to-board MIPI connector (3.3 V/5 V + data lanes)

### I/O Interface Specifications (S10 / S10 Pro)

| Pin | Signal | Type | Description |
|-----|--------|------|-------------|
| IN1 | Input | Discrete, high-active | 24 V compatible, open contact input |
| IN2 | Input | Discrete, high-active | 24 V compatible, open contact input |
| OUT1 | Output | Discrete, ground/open | 24 V pull-up compatible |
| OUT2 | Output | Discrete, ground/open | 24 V pull-up compatible |
| COM | GND | Common | Return for I/O signals |

The I/O lines support direct connection to vehicle safety controllers. The built-in obstacle avoidance algorithm can output decelerate/stop signals without requiring data transmission over Ethernet. Four groups of obstacle avoidance zones can be selected via the two input lines.

### Cable Requirements

- **Power**: 18 AWG minimum for S10/S10 Pro (peak current up to ~1.2 A at 24 V); standard DC barrel or terminal block connection
- **Ethernet**: Cat 6 or better, shielded recommended for industrial environments; maximum segment length per IEEE 802.3
- **USB (S11)**: USB 3.0 Type-C cable, rated for the target robot motion environment
- **I/O**: 8-conductor cable, 0.3 mm² minimum, with the following factory wire color coding:

| Wire Color | Function |
|------------|----------|
| Orange | IN1 |
| Yellow | IN_COM1 |
| Green | IN2 |
| Purple | OUT_1 |
| Grey | OUT_COM1 |
| Blue | IN_COM2 |
| Brown | OUT_2 |
| White | OUT_COM2 |

[IMAGE: Connector pinout diagram showing S10 rear panel with power, Ethernet, and I/O connectors labeled]

## 2.7 Product Comparison and Selection Guide

| Feature | S10 | S10 Pro | S11 (ETH) | S11 (USB) | S11 (MIPI) |
|---------|-----|---------|-----------|-----------|------------|
| **Distance Range (outdoor)** | 0.3-8 m | 0.3-17 m | 0.1-6 m | 0.1-6 m | 0.1-6 m |
| **Distance Range (indoor max)** | -- | -- | Up to 10 m | Up to 10 m | Up to 10 m |
| **Depth Resolution** | 240 x 160 | 240 x 160 | 240 x 96 | 240 x 96 | 240 x 96 |
| **Depth FOV (H x V)** | 90°x60° or 120°x80° | 61°x90° | 140°x56° | 140°x56° | 140°x56° |
| **RGB** | Optional (Lite w/o) | Yes | Yes | Yes | No |
| **RGB Resolution** | 1920x1080 / 1632x1224 | 1920x1080 | 1280x1080 | 1280x1080 | N/A |
| **Interface** | Ethernet / GMSL / I/O | Ethernet / I/O | Ethernet | USB3.0 | MIPI |
| **Built-in Algorithm** | Obstacle Avoidance with I/O | Obstacle Avoidance with I/O | -- | -- | -- |
| **Dimensions (mm)** | 80 x 37 x 25 | 92 x 47 x 51 | 90 x 25 x 25 | 90 x 25 x 25 | 33 x 18 x 13 |
| **Weight** | 190 g | 460 g | ~130 g | ~90 g | <10 g |
| **Power (avg / peak)** | <=4 W / 28 W | <=7 W / -- | <4 W / 13 W | <4 W / 30 W | <=2 W / 8 W |
| **Power Input** | 12-28 V DC | 24 V DC | 12-28 V DC | 5 V USB | 3.3/5 V DC |
| **IP Rating** | IP54 | IP67 | IP54 | N/A | N/A |
| **Best For** | General AMR/AGV, forklifts, indoor/outdoor mixed | Long-range outdoor, large vehicles, high-bay logistics | Compact AMR, indoor robots | Service robots, USB-native platforms | Embedded modules, PCB-level integration |

### Selection Guidance

**Choose S10 when:**
- You need a proven, cost-effective depth sensor for AGV/AMR obstacle avoidance
- Your application requires direct I/O safety signaling without a host PC
- Operating conditions include mixed indoor/outdoor environments up to 8 m range
- A wide FOV (120° x 80°) is needed for near-field coverage

**Choose S10 Pro when:**
- Maximum detection range is critical (17 m at 90% reflectivity)
- The platform operates primarily outdoors or in large open spaces
- IP67 protection is required for harsh or wash-down environments
- The wider physical envelope is acceptable for the application

**Choose S11 (Ethernet) when:**
- Size and weight are primary constraints (25 mm height profile)
- Standard Ethernet infrastructure is preferred for fleet deployment
- Indoor operation with occasional outdoor transition is expected
- RGB data is needed for semantic perception tasks

**Choose S11 (USB) when:**
- The host platform natively supports USB3.0 and bus-powered operation
- Minimum integration complexity is desired (single-cable power+data)
- The robot is primarily USB-centric (e.g., certain ROS-based development platforms)

**Choose S11 (MIPI) when:**
- The camera must be embedded directly on a host PCB
- The lowest possible weight and volume are required (<10 g, 33 x 18 x 13 mm)
- Custom housing and optics integration are acceptable
- RGB is not required for the target application

## 2.8 Certifications and Compliance

The S10 series has been tested and certified by accredited third-party laboratories. S11 series certifications are in progress and are expected to achieve equivalent compliance levels.

| Certification | Standard | Scope | Status |
|-------------|----------|-------|--------|
| **CE (EMC)** | Directive 2014/30/EU, EN IEC 61000-6-2:2019, EN IEC 61000-6-4:2019 | Electromagnetic compatibility (immunity & emissions) | S10: Certified (TUV Rheinland, 2025-06-30) |
| **RoHS 2.0** | Directive 2011/65/EU and amendment (EU) 2015/863 | Restriction of hazardous substances (Cd, Pb, Hg, CrVI, PBB, PBDE, phthalates) | S10: Certified (TUV Rheinland Shenzhen, 2025-09-19) |
| **REACH** | Regulation (EC) No 1907/2006 | SVHC screening -- all candidate list substances below 0.1% threshold | S10: Certified (TUV Rheinland Shenzhen, 2025-11-17) |
| **Laser Safety** | IEC 60825-1:2014 / FDA 21 CFR 1040.10 | Class I laser product -- eye-safe under all conditions | S10: Certified (Zhejiang Institute of Quality Sciences, 2025-10-31) |

### Certification Details

**CE EMC (S10)**
The S10 (model designations LXPS-SA730-79I-940B, LXPS-SA732-79I-940B, LXPS-SA730-7CI-940A, LXPS-SA732-7CI-940A) has been tested for electromagnetic compatibility per the EU EMC Directive 2014/30/EU. Testing covered immunity (EN IEC 61000-6-2) and emissions (EN IEC 61000-6-4) requirements for industrial environments. Certificate registration: AE 50684372 0001; Test report: CN25KQE6 001.

**RoHS 2.0 (S10)**
Test report 168563744a 001 (TUV Rheinland Shenzhen) confirms compliance with RoHS Directive 2011/65/EU Annex II and amendment Directive (EU) 2015/863 across 137 tested materials. All restricted substances (cadmium, lead, mercury, hexavalent chromium, PBB, PBDE, and four phthalates) are below maximum permissible limits. Applicable exemptions for high-melting-temperature solders and copper alloys are documented.

**REACH (S10)**
Test report 168576959a 001 (TUV Rheinland Shenzhen) screens for Substances of Very High Concern (SVHC) per Regulation (EC) No 1907/2006. All SVHC candidate list substances are below the 0.1% reporting threshold at the article level.

**Laser Class I (S10)**
Test report XZJHW-20251050010 (Zhejiang Institute of Quality Sciences) confirms Class I laser classification per IEC 60825-1:2014. Under Condition 3 measurement (7 mm aperture at 100 mm from laser lens), the Eagle-S10 camera emitted 1.528 mW radiant power against an AEL of 1.21 W, well within Class I limits. Wavelength: 937 nm; pulse width: 5.6 ns; repetition frequency: 7 MHz. The laser is invisible and eye-safe during all normal use conditions.

**S11 Certifications**
S11 series certification testing is planned to cover the same standards (CE EMC, RoHS 2.0, REACH, Laser Class I). Contact MRDVS for the current certification status of specific S11 variants.

# Chapter 3: Software & Toolchain

The S-Series dToF RGB-D camera ecosystem includes a complete software stack designed to minimize integration effort for system integrators and robotics engineers. This chapter covers the host configuration tool, the Camera SDK, ROS/ROS2 integration paths, installation procedures, communication protocols, and the algorithm deployment toolchain.

## 3.1 Host Configuration Tool -- LxCameraViewer

LxCameraViewer is the Windows-based host application for camera discovery, configuration, visualization, and diagnostics. It provides a graphical interface for all core camera operations and is the recommended starting point for any S-Series deployment.

**Purpose:** Camera configuration, real-time data visualization, parameter tuning, and firmware management.

**Main features:**

- **Camera discovery and connection:** Automatic network scanning lists cameras by ID and IP. Multiple devices can be opened simultaneously with a click-to-switch interface.
- **Real-time depth/RGB/point cloud preview:** Supports single through quadruple frame layouts. Frame rate indicators show green (>12 fps), orange (6-12 fps), or red (<6 fps).
- **3D parameter configuration:** Resolution, binning (1x1 / 2x2 / 4x4), exposure (manual/auto with bounds), HDR, multi-machine interference avoidance, RGB-D alignment.
- **2D parameter configuration:** Resolution, exposure, gain, auto-exposure, undistort.
- **Multi-machine interference avoidance:** Eliminates cross-camera interference. Adjustable after stopping the stream.
- **RGB-D alignment:** Aligns depth, amplitude, and point cloud to the RGB coordinate frame.
- **Firmware upgrade:** Upgrade camera and algorithm firmware via Basic Tools > Function Settings > Firmware Upgrade using `.bin` files.
- **Log export:** Configurable log levels and export paths.
- **Additional tools:** IP configuration, network card configuration, communication tool (TCP/UDP/Modbus/FTP/CAN), and packet capture.

**Supported platforms:** Windows 10 (64-bit), Windows 11 (64-bit)

**System requirements:**

| Component | Minimum Requirement |
|-----------|--------------------|
| Processor | Intel Core i5 7th generation or equivalent |
| Memory | 8 GB RAM |
| Network | Gigabit Ethernet adapter and cable |
| Graphics | Intel UHD Graphics or discrete GPU (required for point cloud display) |

Run as administrator on first launch. The firewall must be disabled for camera communication.

[IMAGE: LxCameraViewer main interface screenshot showing device list, image preview panel, and right-side configuration toolbar]

## 3.2 Camera SDK

The Camera SDK is a C/C++ library providing unified access control and parameter configuration across x86 Windows and x86/ARM Linux platforms. It abstracts device-specific differences, allowing the same application code to operate across the S-Series product line.

**Supported data streams:**

| Stream | Data Format | Description |
|--------|-------------|-------------|
| Depth map | Unsigned short / float | Per-pixel distance measurements |
| Amplitude/intensity map | Unsigned short | Signal strength for each depth pixel |
| Point cloud (XYZ) | Float array (x, y, z) | 3D coordinates computed from depth |
| RGB image | Unsigned char (1/3 channel) | 2D color image, when available |
| Algorithm results | Struct pointers | Obstacle, pallet, or location output |

**Two API call modes:**

- **Synchronous (blocking):** The application calls `DcGetPtrValue` or `LX_CMD_GET_NEW_FRAME` and waits until the next frame is available. Suitable for simple polling loops.
- **Asynchronous (callback-based):** The application registers a frame callback via `DcRegisterFrameCallback`. The SDK invokes the callback automatically when new data arrives.

**SDK directory structure:**

```
Lanxin-MRDVS/
├── Document/          # SDK documentation and API references
├── FirmWare/          # Camera firmware upgrade packages
├── Sample/            # Example code (C, C++, Java, ROS, ROS2)
│   ├── C/             # C/C++ sample programs
│   ├── java/          # HTTP server sample for indirect SDK access
│   ├── ros/           # ROS1 driver node
│   └── ros2/          # ROS2 driver node
└── SDK/               # Header files and library files
```

[IMAGE: SDK directory structure diagram showing Document, FirmWare, Sample, and SDK folders]

### 3.2.1 Supported Operating Systems

| OS | Architecture | Build Toolchain | ROS/ROS2 Support |
|----|-------------|-----------------|-----------------|
| Windows 7/10/11 | x86 | Visual Studio | Not applicable |
| Ubuntu 16.04 | x86 / ARM | CMake 3.5 / GCC 5 | ROS Kinetic; ROS2 Ardent, Bouncy |
| Ubuntu 18.04 | x86 / ARM | CMake 3.18 / GCC 7 | ROS Melodic; ROS2 Dashing, Eloquent, Crystal |
| Ubuntu 20.04 | x86 / ARM | CMake 3.22 / GCC 9.3 | ROS Noetic; ROS2 Foxy, Galactic |
| Ubuntu 22.04 | x86 / ARM | CMake 3.22 / GCC 11.3 | ROS Ninjemys; ROS2 Humble, Iron |

Before building, verify that OpenCV and PCL versions are unique in the system path. Duplicate library installations cause ROS node link errors.

### 3.2.2 Environment Setup

**Hardware:**

- Power the camera with 24V DC.
- Connect via Gigabit Ethernet. Cat 6 or Cat 7 cable is recommended.

**Network:**

- Default camera IP range: `192.168.100.*`
- Host IP: `192.168.100.x` (x = 3-254, no conflict with camera).
- Subnet mask: `255.255.255.0`

**Firewall:**

- Windows: Disable Windows Defender Firewall.
- Linux (Ubuntu):
  ```bash
  sudo systemctl stop ufw.service
  sudo systemctl disable ufw.service
  sudo ufw status
  ```

**Software installation:**

- Windows: Run `Lanxin-MRDVS-x.x.x.xxxx.exe`. Administrator privileges required for all-user installation.
- Linux: Extract and execute the install script as root. The SDK installs to `/opt/Lanxin-MRDVS` and updates `LD_LIBRARY_PATH`.

### 3.2.3 Development Workflow

The standard SDK integration sequence:

1. **Device discovery** -- `DcGetDeviceList` returns reachable cameras with ID, IP, MAC, serial number, firmware, and algorithm versions.
2. **Open device** -- `DcOpenDevice` accepts open-by-index, IP, SN, or ID modes. Returns a device handle.
3. **Configure parameters** -- Use `DcSetIntValue`, `DcSetFloatValue`, or `DcSetBoolValue` with `LX_CAMERA_FEATURE` enumerations.
4. **Start stream** -- `DcStartStream` initiates data transmission.
5. **Get frame data** -- Synchronous: poll `DcGetPtrValue` for depth, point cloud, RGB, or algorithm output. Asynchronous: the registered callback receives a `FrameInfo` structure.
6. **Stop stream** -- `DcStopStream` halts transmission.
7. **Close device** -- `DcCloseDevice` releases the handle.

**Key constraints:**

- ROI, binning, trigger mode, HDR, multi-machine mode, and RGB-D alignment cannot be modified while streaming or in WORK_FOREVER mode.
- Point cloud (`LX_PTR_XYZ_DATA`) requires depth streaming (`LX_BOOL_ENABLE_3D_DEPTH_STREAM`) to be enabled first.
- 2D undistort requires RGB-D alignment to be enabled first.
- Algorithm parameters (`LX_STRING_ALGORITHM_PARAMS`) can only be set after algorithm mode (`LX_INT_ALGORITHM_MODE`) is configured.

[IMAGE: SDK API call flow diagram showing the seven-step sequence from discovery to close]

### 3.2.4 Key API Categories

The SDK organizes parameters by type prefix:

| Prefix | API (Set) | API (Get) | Parameter Category |
|--------|-----------|-----------|-------------------|
| LX_INT_ | `DcSetIntValue` | `DcGetIntValue` | Integer parameters (exposure, resolution, binning, filters) |
| LX_FLOAT_ | `DcSetFloatValue` | `DcGetFloatValue` | Float parameters (FPS, temperature, filter level) |
| LX_BOOL_ | `DcSetBoolValue` | `DcGetBoolValue` | Boolean toggles (streams, auto-exposure, undistort, HDR) |
| LX_STRING_ | `DcSetStringValue` | `DcGetStringValue` | String parameters (algorithm params, firmware name) |
| LX_CMD_ | `DcSetCmd` | -- | Commands (get frame, restart, reset params, white balance) |
| LX_PTR_ | -- | `DcGetPtrValue` | Pointer access (depth, amplitude, RGB, point cloud, algorithm output, intrinsics/extrinsics) |

Additional APIs:

- **Frame callback:** `DcRegisterFrameCallback` / `DcUnregisterFrameCallback` for asynchronous acquisition.
- **Device status callback:** `DcRegisterCameraStatusCallback` / `DcUnregisterCameraStatusCallback`.
- **ROI:** `DcSetRoI` sets 2D/3D region-of-interest. Values round to the nearest multiple of 8.
- **Camera IP:** `DcSetCameraIp` modifies IP, subnet mask, and gateway. Call `DcGetDeviceList` again after changes.
- **Point cloud save:** `DcSaveXYZ` exports to .txt, .ply, or .pcd. Binary saves are ~206x faster than ASCII.
- **Logging:** `DcSetInfoOutput` configures log level, screen print, and file path.

## 3.3 ROS & ROS2 Integration

Official ROS and ROS2 driver nodes wrap the Camera SDK and publish standard ROS message types for rapid integration with existing robotic software stacks.

**Supported launch configurations:**

| Launch File | Purpose |
|-------------|---------|
| `lx_camera_ros.launch` | Standard streaming node (depth, amplitude, RGB, point cloud) |
| `obstacle.launch` | Obstacle avoidance algorithm node |
| `obstacleV2.launch` | Obstacle avoidance V2 algorithm node |
| `pallet.launch` | Pallet recognition algorithm node |

> Note: Localization, mapping, and sensor simulation launch files are also available for compatible camera models.

**Configurable parameters:**

- **Connection:** Camera IP, log path
- **Data streams:** `is_xyz`, `is_depth`, `is_amp`, `is_rgb`
- **Work mode:** `lx_work_mode` -- heartbeat or forever mode
- **Algorithm:** `lx_application` -- 0=off, 1=obstacle, 2=pallet, 3=location, 4=obstacle V2
- **Camera pose:** X/Y/Z translation (m) and Yaw/Roll/Pitch rotation (deg) for point cloud transform
- **2D/3D parameters:** Binning, undistort, auto-exposure, exposure, gain, HDR, multi-machine mode
- **Depth range:** `lx_min_depth`, `lx_max_depth`
- **Point cloud unit:** `lx_tof_unit` -- 0=mm, 1=m (set to 1 for RViz)

**Message types:** `FrameRate`, `Obstacle`, `Obstacle_box`, `Pallet`, `Result`

**Service types:** `LxBool`, `LxCmd`, `LxFloat`, `LxInt`, `LxString` -- used to read or write camera parameters of corresponding types.

To subscribe to algorithm output, use the provided shell scripts (`rate.sh`, `obstacle.sh`, `pallet.sh`) or standard ROS topic tools.

[IMAGE: ROS node architecture diagram showing the lx_camera_node publishing depth, RGB, point cloud, and algorithm topics, with service interfaces for parameter control]

### 3.3.1 ROS2 Compatibility

The ROS2 driver uses the `colcon` build system. Prerequisites:

```bash
sudo apt install python3-colcon-common-extensions
sudo apt-get install ros-<distro>-pcl*
```

Build and run:

```bash
cd lx_camera_node_ws
colcon build
source install/setup.bash
ros2 launch lx_camera_ros lx_camera_ros_launch.py
```

Launch files use `.launch.py` extension with the same parameter structure as ROS1. Service calls use YAML-format input instead of the compact ROS1 message format.

## 3.4 Software Installation and Updates

### 3.4.1 Windows Installation

1. Download the installer from the MRDVS knowledge base or GitHub repository.
2. Run `Lanxin-MRDVS-x.x.x.xxxx.exe` as administrator for all-user installation.
3. Select a non-C drive path. Check "Create desktop shortcut."
4. The installer configures environment variables automatically.
5. On first launch, run as administrator.

### 3.4.2 Linux Installation

```bash
tar xvf Lanxin-MRDVS-xxx.tar.gz
cd Lanxin-MRDVS
sudo su root
chmod +x install.sh
./install.sh
source ~/.bashrc  # Update LD_LIBRARY_PATH for current terminal session
```

The SDK installs to `/opt/Lanxin-MRDVS`. For non-root users, re-run `install.sh` or manually append `export LD_LIBRARY_PATH=/opt/Lanxin-MRDVS/lib/:$LD_LIBRARY_PATH` to `~/.bashrc`.

After installation, run `./set_socket_buffer_size.sh` to increase socket buffer sizes for high-bandwidth streams.

### 3.4.3 Firmware Upgrade

**Via LxCameraViewer:**

1. Open the camera and start the stream.
2. Navigate to Basic Tools > Function Settings > Firmware Upgrade.
3. Select the `.bin` firmware file provided by MRDVS technical support.
4. Click Execute. The camera reboots automatically.

**Via SDK:**

Use `DcSetCmd` with the firmware upgrade command, or set `LX_STRING_FIRMWARE_NAME` and trigger the upgrade programmatically.

> **Note:** Algorithm firmware upgrades require the base camera firmware to be at the MDS version level. Verify green status in the algorithm host tool after flashing.

## 3.5 Communication Protocols

S-Series cameras support multiple industrial communication interfaces for integration with PLCs, robot controllers, and WMS/ERP systems.

| Protocol | Port | Application | Direction |
|----------|------|-------------|-----------|
| TCP | 14951 | Algorithm results query | Device to Host |
| UDP | 14950 | Algorithm results broadcast | Device to Host |
| HTTP | 14952 | REST API for WMS integration | Bidirectional |
| Modbus-TCP | 16502 | Location status / slot detection | Device to PLC |
| RS232 | Configurable | Volume measurement trigger | Bidirectional |
| IO Output | Hardware pin | Obstacle status (0=OK, 1=Warn, 2=Stop) | Device to Robot |
| CANopen | Configurable | Obstacle status to robot controller | Device to Robot Controller |

**HTTP/WMS Integration:**

The HTTP interface accepts `RESULT_UPDATE` and returns JSON responses:

- **Human Detection:** `SG` -- 0=empty, 1=person detected (semantic+depth), 2=object detected (depth-only)
- **Location Status:** `SS` -- 0=empty, 1=occupied, 2=occupied with angle misalignment, 3=occupied with overhang
- **Volume Measurement:** `TM` array -- each entry contains center (cx, cy, cz), dimensions (length, width, height), and valid flag

**Modbus-TCP (Location Status):**

- Port: 16502; Register start: 0x100; Size: 16 registers; Byte order: Big-endian
- Each register: high byte = location status (0-2), low byte = marker status (reserved)

## 3.6 Algorithm Deployment Toolchain

### 3.6.1 LxApplicationViewer

LxApplicationViewer is the dedicated algorithm host tool for deploying detection algorithms that run on the camera's embedded processor. Install by copying the executable into `Lanxin-MRDVS\Tools`.

**Supported algorithms:**

| Algorithm | Model Support |
|-----------|--------------|
| Human Detection | S10 Pro |
| Location Status Detection | S10 Pro |
| Volume Measurement | S10, S10 Pro, S11 |

**Key features:**

- **Multi-camera management:** Simultaneous connection with per-camera panels. Green = normal; yellow = algorithm not running.
- **Extrinsic calibration wizard:** Automated vertical-installation or manual horizontal ground-plane alignment for camera height (Z), roll, pitch, and yaw.
- **Detection zone configuration:** 2D RGB zones (pixel coordinates) or 3D point cloud zones (metric coordinates). Mouse-drag adjustment with X/Y/Z bounds.
- **Real-time result display:** Live overlay on RGB and point cloud views.
- **Historical playback:** Saves a frame on every state transition; browse by timestamp.
- **Parameter save-and-push:** Commits parameters to camera non-volatile memory.
- **User privilege levels:** Standard users view only; administrator mode enables full configuration.

**Utilities:**

- **Slot copy:** Duplicate zones along X/Y/Z axis with spacing and quantity.
- **QR Code calibration:** Auto-generate location zones from a QR code board using known dimensions.

[IMAGE: LxApplicationViewer interface showing device list, point cloud view, and detection zone configuration panel]

### 3.6.2 Algorithm Firmware Flashing

1. Open the camera in LxCameraViewer and start the stream.
2. Navigate to Basic Tools > Function Settings > Firmware Upgrade.
3. Select the algorithm `.bin` file and click Execute. The camera reboots automatically.
4. Open LxApplicationViewer. Green camera status confirms successful activation.

Algorithm firmware is distributed by MRDVS technical support. Base firmware must be at MDS level before flashing.

### 3.6.3 Algorithm Selection Guide

| Algorithm | Supported Model | Installation | Key Features |
|-----------|----------------|--------------|--------------|
| Obstacle Avoidance | S10, S11 | Built-in / SDK | Multi-zone thresholds; IO/CAN/UDP output; WORK_FOREVER standalone mode |
| Pallet Recognition | S10, S11 | Built-in / SDK | Auto pallet leg detection; pose estimation (X, Y, yaw); configurable leg width |
| Location Status | S10 Pro | Algorithm flash | Occupancy, alignment, overhang detection; 2D RGB or 3D point cloud zones |
| Volume Measurement | S10, S10 Pro, S11 | Algorithm flash | Multi-object detection; trigger mode; configurable minimum dimensions |
| Human Detection | S10 Pro | Algorithm flash | Configurable ROI zones; semantic + depth fusion; temporal filtering |

**Built-in algorithms** (Obstacle Avoidance, Pallet Recognition) are activated via `LX_INT_ALGORITHM_MODE`. They operate in WORK_FOREVER mode without a host connection, streaming results over IO, CAN, or UDP.

**Flashed algorithms** (Location Status, Volume Measurement, Human Detection) require algorithm firmware on the camera. Configuration is performed through LxApplicationViewer. Results are accessible via TCP, UDP, HTTP, or Modbus-TCP.

The S10 Pro supports all detection modes including 2D RGB zone definition. The S10 and S11 rely on 3D point cloud zones for Location Status and Volume Measurement.

# Chapter 4: Application Solutions

The MRDVS S-Series dToF RGB-D cameras are designed as application-ready vision platforms. Each camera runs embedded detection algorithms on its internal SoC, eliminating the need for an external processing PC in standard deployments. This chapter describes the five core application solutions — Obstacle Avoidance, Pallet Recognition, Location Status Detection, Human Safety Detection, and Volume Measurement — as well as multi-camera volumetric measurement architecture for large-scale DWS (Dimensioning, Weighing, Scanning) stations.

## 4.1 Mobile Robot Obstacle Avoidance

### 4.1.1 Solution Overview

The S-Series Obstacle Avoidance solution integrates a built-in obstacle detection and classification algorithm that executes entirely on the camera SoC. The system captures real-time 3D depth maps and RGB texture, performs ground plane segmentation, clusters obstacles, and classifies them into configurable protection zones. No external PC is required for basic obstacle detection, making the solution suitable for AMR/AGV platforms with limited payload and power budgets.

Unlike 2D LiDAR, which scans in a single plane and misses low or suspended obstacles, the S-Series camera captures full 3D point cloud data. By combining depth information with RGB texture, the algorithm performs semantic recognition, distinguishing humans from inanimate objects and enabling context-aware navigation decisions.

The solution is deployed across a wide range of mobile robot types:

- Under-ride (latent) AGVs
- Forklift AMRs
- Stacker cranes
- Tote-carrying robots
- Industrial and medical cleaning robots

[IMAGE: S-Series camera installed on AMR front panel with obstacle zone overlay]

### 4.1.2 System Architecture

The obstacle avoidance pipeline operates as follows:

1. **Depth Map Capture** — The camera acquires a real-time depth frame at up to 15 fps.
2. **Ground Plane Segmentation** — The algorithm automatically detects and filters the ground plane, establishing a reference frame.
3. **Obstacle Clustering** — Remaining point cloud data is clustered into discrete objects.
4. **Zone Classification** — Each detected object is evaluated against configurable protection zones.
5. **Decision Output** — The system outputs a discrete state signal to the robot controller.

Two primary detection zones are defined for each configuration:

- **Warning zone (yellow)** — Obstacle detected within deceleration range. Robot reduces speed.
- **Stop zone (red)** — Obstacle detected within emergency stop range. Robot halts immediately.

The camera supports up to 20 configurable obstacle avoidance parameter groups. Groups can be switched dynamically via communication interfaces to adapt to different operational scenarios — for example, switching from a wide corridor configuration to a narrow aisle configuration.

[IMAGE: Obstacle avoidance zone configuration diagram showing warning and stop zones relative to vehicle center]

### 4.1.3 Key Features

- **Multi-zone configuration** — Up to 20 parameter groups, switchable via API, UDP, TCP, CAN, or IO.
- **Ground plane auto-detection** — Automatic ground reference extraction; manual extrinsic calibration available for fine-tuning.
- **Obstacle clustering with bounding box output** — Each detected object is enclosed in a 3D bounding box with position and size.
- **Low and suspended obstacle detection** — Detects objects that 2D LiDAR cannot perceive.
- **Configurable height and width thresholds** — Filter out irrelevant ground features or noise.
- **Real-time streaming** — Result output synchronized with camera frame rate (up to 15 fps).
- **Model compatibility** — S10, S10 Pro, and S11 variants supported.

### 4.1.4 Deployment Considerations

**Camera Selection and Mounting**

| Robot Type | Recommended Model | Mounting Orientation | Typical Height |
|------------|-----------------|---------------------|--------------|
| Under-ride (latent) AGV | S10 | Front surface, forward-facing | 300-500 mm |
| Forklift AMR | S10 / S10 Pro | Fork frame, downward 30-55° | 500-1500 mm |
| Stacker crane | S10 Pro | Forward or downward | 800-1500 mm |

For forklift installations, the camera should be tilted downward 30 to 55 degrees so that the field-of-view edge aligns with the front surface of the vehicle. Installation height should generally not exceed 2,000 mm to maintain detection accuracy for ground-level obstacles.

**Parameter Configuration**

Key obstacle avoidance parameters configured via LxCameraViewer include:

| Parameter | Typical Value | Description |
|-----------|---------------|-------------|
| Warning distance (X-far) | 1,500-2,300 mm | Deceleration trigger distance from vehicle center |
| Alarm distance (X-mid) | 1,000-1,500 mm | Emergency stop trigger distance from vehicle center |
| Shielding distance (X-near) | 400-600 mm | Blind zone below which obstacles are ignored |
| Range left (Y-left) | -500 to -810 mm | Left boundary from vehicle center |
| Range right (Y-right) | 500-1,270 mm | Right boundary from vehicle center |
| Height lower (Z-lower) | 0-10 mm | Obstacles below this ground-relative height are ignored |
| Height upper (Z-upper) | 500-1,500 mm | Obstacles above this height are ignored |

**Multi-Camera Interference**

When multiple S-Series cameras operate in close proximity, enable **Multi-Machine Mode** in the 3D Settings panel of LxCameraViewer. This mode prevents VCSEL cross-interference between cameras by coordinating exposure timing.

**Communication Interfaces**

The S10 supports obstacle avoidance result output via the following interfaces:

- **IO** — Two output lines: 0=safe, 1=warning, 2=stop. Push-pull or open-drain configurable.
- **UDP / TCP** — Protocol on port 6688; hexadecimal packet format with result code.
- **CANopen** — Available on S10 Pro and S11 variants; 250K baud default.
- **API (SDK)** — C/C++, C#, Java, ROS1, ROS2 supported.

[IMAGE: AMR installation example photo showing camera mounted on forklift frame]

---

## 4.2 Pallet Recognition

### 4.2.1 Solution Overview

The S-Series cameras include a built-in pallet detection and pose estimation algorithm accessible via LxCameraViewer. The algorithm identifies pallet legs in the point cloud, estimates pallet position and orientation, and outputs the pallet center coordinates (X, Y) and yaw angle relative to the camera frame. This enables autonomous pallet picking operations by forklift AMRs without requiring an external vision PC.

[IMAGE: Pallet recognition visualization showing detected pallet legs and bounding overlay]

### 4.2.2 Key Capabilities

- **Automatic pallet leg detection** — Locates standard pallet leg structures in the depth map.
- **Pose estimation** — Computes pallet center position (X, Y) and yaw angle for fork alignment.
- **Multi-type support** — Compatible with wooden, plastic, and metal pallet constructions (within configured leg width limits).
- **Configurable detection range** — Min and max Z (depth), min and max X (horizontal span), and pallet leg width range are all configurable.

### 4.2.3 Deployment Considerations

- **Camera mounting** — Typically forward-facing at fork height, with the optical axis approximately level with the intended fork insertion plane.
- **Extrinsic calibration** — Required to align the camera coordinate system with the vehicle coordinate frame. Parameters include camera position (X, Y, Z) and orientation (roll, pitch, yaw) relative to the vehicle rotation center.
- **Ground height** — Must be configured to establish the vertical reference for pallet detection.
- **Pallet distance** — The expected distance from camera to pallet after fork insertion completion; used as a reference for pose estimation.
- **Automatic calibration** — Available when the ground is flat, no obstructions exist in front of the pallet, and the pallet is of standard size.

**Note**: Pallet recognition details are primarily embedded in the camera firmware and configured via LxCameraViewer. For detailed deployment procedures, refer to the Camera User Manual and LxCameraViewer documentation.

---

## 4.3 Location Status Detection (Slot Detection)

### 4.3.1 Solution Overview

The Location Status Detection solution automates the monitoring of storage slots in warehouse and distribution environments. The system determines whether each slot is empty, occupied, or has an alignment anomaly, providing real-time inventory status to WMS (Warehouse Management Systems) and AGV dispatch systems.

The **S10 Pro is recommended** for this application because its integrated RGB stream enables AI-based semantic detection in addition to 3D point cloud analysis.

Two detection modes are available:

- **3D point cloud mode** — Direct volumetric detection based on point cloud occupancy within defined slot boundaries. No RGB stream required.
- **2D RGB mode** — AI-based semantic detection using the RGB image stream. Requires an S10 Pro or equivalent RGB-capable model and a trained semantic model.

### 4.3.2 Detection Capabilities

The algorithm classifies each slot into one of four states:

| Status | Output Code | Meaning |
|--------|-------------|---------|
| Empty | 0 | No object detected in the slot |
| Occupied | 1 | Object detected, properly aligned within slot boundaries |
| Misaligned | 2 | Object detected but rotation angle exceeds configured threshold |
| Overhang | 3 | Object extends beyond defined slot boundary |

**Alignment verification** — When enabled, the algorithm computes the object's orientation relative to the slot frame. If the deviation exceeds the configured alignment angle threshold, the slot is flagged as misaligned (code 2).

**Overhang detection** — When alignment verification is active, the algorithm checks whether any portion of the detected object exceeds the defined slot boundaries in X or Y. If so, the slot is flagged as overhang (code 3).

**QR code calibration** — For rapid deployment, a QR code calibration board (recommended 1 m x 1 m or larger) can be placed in the field of view. After entering slot dimensions (width, length, min depth, max depth), the system automatically generates the slot detection zone based on the QR code reference position, eliminating manual zone drawing.

**Batch slot copy** — Once a single slot is defined, the configuration can be duplicated along X, Y, or Z directions with configurable spacing and quantity. This is essential for rack arrays where tens or hundreds of identical slots must be configured.

### 4.3.3 Deployment Considerations

- **Installation** — Mount the camera overhead (top-down view) or at a slight angle over the storage rack. Fixed-mount installation on a column or ceiling is recommended.
- **Extrinsic calibration** — Align the camera coordinate system with the rack/ground reference. After calibration, the X (red) and Y (green) axes must be parallel to the ground, and the Z (blue) axis must point vertically upward.
- **Slot definition** — Use LxApplicationViewer to draw detection zones on either the 3D point cloud or 2D RGB view. For 3D mode, press the X key to begin/end drawing a rectangular prism around each slot.
- **Multiple slots** — Use the copy-paste function to replicate slot configurations for arrays.

[IMAGE: Slot detection installation diagram showing camera mounted above rack with detection zones]

### 4.3.4 WMS Integration

The camera communicates slot status to upstream systems via the following protocols:

| Protocol | Port | Purpose |
|----------|------|---------|
| TCP | 14951 | JSON result query and response |
| UDP | 14950 | Broadcast JSON results |
| HTTP | 14952 | REST-style request/response |
| Modbus-TCP | 16502 | Register-mapped slot status for PLC integration |

**JSON Output Format**

The camera returns a JSON object containing the slot status array. Example:

```json
{
  "IP": "192.168.100.83",
  "SS": {
    "1": 1,
    "2": 1,
    "3": 3,
    "1__detail__info": [-1, -1, -1, -1, -1, -1, -1],
    "2__detail__info": [-7, 258, 261, 545, 403, 204, 89],
    "3__detail__info": [114, 73, 37, 92, 42, 75, -88]
  },
  "SS_DATA_TIME": "2023_03_02_22_13_01.286",
  "SS_RES_TIME": "2023_03_02_22_13_01.572",
  "nick_name": "Rack_A_Camera_1",
  "ret_code": 0
}
```

Where `detail_info` fields contain [center_x, center_y, center_z, length, width, height, angle_offset] for each occupied slot.

**Modbus-TCP Register Mapping**

Results are automatically sorted by slot ID. Up to 16 slots per device are mapped to registers starting at address 0x100:

| Register Address | Content | Format |
|-----------------|---------|--------|
| 0x100 + ID | Slot status code (high byte) + marker status (low byte) | uint16_t, Big-Endian |

Status codes: 0 = unoccupied, 1 = normal occupied, 2 = abnormal occupied (misalignment or overhang).

---

## 4.4 Human Safety Detection

### 4.4.1 Solution Overview

The Human Safety Detection solution provides 3D spatial monitoring for industrial safety zones. The system detects when a person enters a restricted or dangerous area and triggers alerts to safety PLCs or relay systems. The solution is built on the **S10 Pro**, which provides the RGB-D data stream required for semantic human recognition.

The algorithm fuses depth-based point cloud clustering with semantic recognition to achieve high detection accuracy while suppressing false positives from inanimate objects. Two operating modes are available:

- **Depth-only mode** — Detects all objects; suitable for general intrusion detection.
- **Semantic + depth mode** — Filters clusters by semantic class (human); objects not classified as human are ignored.

### 4.4.2 Key Features

- **3D space monitoring** — Detection zones are defined as rectangular prisms in world coordinates (min/max X, Y, Z), enabling precise volumetric region definition rather than simple 2D polygons.
- **Human-specific recognition** — Semantic filtering reduces false positives from equipment, moving carts, or other non-human objects.
- **Real-time alert triggering** — Detection events are immediately transmitted to safety PLCs or relays via IO, TCP, UDP, or HTTP.
- **Multiple detection zones** — Each zone has an independent ID and sensitivity configuration.
- **Low false detection rate** — Under laboratory conditions (unobstructed view, good lighting, target within 5 m, speed below 3 m/s), the single-frame false detection rate is as low as 1 in 3,000,000. With temporal filtering enabled (requiring N consecutive consistent frames), the rate improves to 1 in 25,000,000.

**Temporal Filtering**

The algorithm supports configurable temporal filtering. When set to a window size of N, the system requires N consecutive frames with identical detection results before confirming a state change. This eliminates sporadic noise-induced triggers.

### 4.4.3 Deployment Considerations

- **Camera installation** — Horizontal or tilted (less than 30°) for maximum area coverage. The camera should be mounted so that the detection zone is fully within the depth operating range.
- **Detection distance** — Reliable human detection is achieved within the camera's operating range; for the S10 Pro, detection up to 5 m is validated under standard conditions.
- **Zone configuration** — Define detection zones using min/max X, Y, Z coordinates in LxApplicationViewer. Zones can be dragged and resized interactively in the point cloud view.
- **Lighting** — The system operates across a range of industrial lighting conditions. Avoid direct sunlight on the lens or extreme low-light environments where RGB semantic detection may degrade.
- **Target speed** — For optimal detection, target movement should be below 3 m/s. Higher speeds may introduce motion blur in RGB frames, affecting semantic classification.

[IMAGE: Human safety detection zone setup showing 3D rectangular prism detection region in a factory aisle]

### 4.4.4 Output and Integration

**Detection Result Codes**

| Mode | No Person | Person Detected | Object Detected |
|------|-----------|-----------------|-----------------|
| Depth-only | 0 | 2 | 2 |
| Semantic + Depth | 0 | 1 | 0 |

In semantic + depth mode, only clusters classified as human trigger a positive detection (code 1). All other objects return 0, dramatically reducing nuisance alarms.

**Communication**

- **TCP / UDP / HTTP** — JSON-formatted result packets with timestamp, camera ID, and detection state.
- **Direct IO output** — Safety relay integration via configurable IO lines for hardwired emergency stop circuits.
- **WMS/PLC integration** — RESULT_UPDATE command triggers immediate result transmission; continuous streaming also available.

---

## 4.5 Volume Measurement

### 4.5.1 Solution Overview

The single-camera Volume Measurement solution captures object dimensions — length, width, and height — and computes volume for logistics, warehousing, and quality control applications. The algorithm executes entirely on the camera SoC, requiring no external PC for measurement computation. It supports both single-object and multi-object detection within the configured measurement zone.

Typical applications include parcel dimensioning at inbound/outbound stations, cargo verification for truck loading optimization, and dimensional quality control in manufacturing.

[IMAGE: Volume measurement installation diagram showing downward-facing camera over measurement surface]

### 4.5.2 Key Features

- **Downward-facing top-down measurement** — Camera installed vertically above the measurement surface for optimal geometric accuracy.
- **Automatic ground plane detection** — The algorithm detects the reference surface (ground or pallet) automatically after extrinsic calibration.
- **Configurable detection plane distance** — Set the plane distance to measure from the ground, or adjust to the pallet top surface to measure only the cargo height above the pallet.
- **Minimum object size filtering** — Configurable minimum X, Y, and Z thresholds filter out noise or objects too small for the application.
- **Normal vector filtering** — Adjustable threshold for filtering rounded edges and tilted surfaces. Recommended default: 60 for general objects; increase if measured volumes are smaller than actual.
- **Trigger modes** — Continuous measurement or single-trigger mode. In single-trigger mode, send the `TM_TRIGGER` command via TCP or UDP to capture one measurement cycle.
- **Multi-object detection** — Outputs individual bounding boxes and dimensions for each detected object within the measurement zone.

### 4.5.3 Measurement Output

For each detected object, the camera returns the following parameters:

| Parameter | Unit | Description |
|-----------|------|-------------|
| length | mm | Object length along the longest horizontal axis |
| width | mm | Object width along the shorter horizontal axis |
| height | mm | Object height from the detection plane to the top surface |
| angle | degrees | Object rotation relative to the camera coordinate frame |
| cx, cy, cz | mm | Object center coordinates in camera/world frame |
| valid | bool | Measurement confidence flag (false if confidence is low) |

The JSON output includes an object array under the `TM` field:

```json
{
  "IP": "192.168.100.82",
  "TM": [{
    "angle": -6.82,
    "cx": 39.98,
    "cy": 257.01,
    "cz": 2400.64,
    "height": 616.72,
    "length": 525.51,
    "width": 419.76,
    "valid": true
  }],
  "TM_DATA_TIME": "2024_11_13_16_37_47.422",
  "TM_RES_TIME": "2024_11_13_16_37_48.209",
  "nick_name": "DWS_Station_1",
  "ret_code": 0
}
```

### 4.5.4 Deployment Considerations

- **Camera mounting** — Install vertically downward with the camera lens parallel to the measurement surface. Any tilt introduces cosine error in height measurements.
- **Distance to surface** — Mount within the camera's depth operating range. For typical warehouse applications, 1,500-3,000 mm mounting height provides a balance between coverage area and measurement accuracy.
- **Measurement area** — Configure detection zone boundaries (min/max X, Y, Z) to exclude peripheral structures, conveyors, or walls.
- **Surface condition** — A flat, non-reflective measurement surface is recommended. Highly reflective or black surfaces (below 5% reflectivity) may require exposure parameter adjustment.
- **Object placement** — Single isolated objects yield the highest accuracy. Multi-object measurement is supported but may have reduced accuracy when objects touch or overlap in the depth map.

### 4.5.5 Communication Interface

| Protocol | Port | Command / Function |
|----------|------|-------------------|
| TCP | 14951 | RESULT_UPDATE query; TM_TRIGGER single-shot |
| UDP | 14950 | RESULT_UPDATE query; TM_TRIGGER single-shot |
| HTTP | 14952 | JSON POST request with RESULT_UPDATE body |

The trigger command `TM_TRIGGER` initiates a single measurement capture and returns the result once. In continuous mode, results are streamed at the configured detection frequency.

---

## 4.6 Multi-Camera Volumetric Measurement

### 4.6.1 Application Scenario

Single-camera volume measurement is effective for objects that fit within one camera's field of view. However, certain applications require multi-camera architectures:

- **Large pallet or parcel measurement** exceeding a single camera's FOV (e.g., 2,400 x 2,400 x 2,400 mm freight)
- **High-accuracy requirements** demanding multi-view fusion to reduce occlusion and perspective error
- **Industrial DWS stations** where full dimensioning, weighing, and scanning integration is required

### 4.6.2 Technical Approach

Drawing on industry-standard multi-camera dimensioning systems, a multi-camera volumetric measurement architecture with S-Series cameras involves the following workflow:

1. **Multi-Camera Array** — Multiple S-Series cameras installed at fixed positions around the measurement volume. Common configurations include overhead corner mounts for top-down coverage or side-mounted views for height verification.
2. **Unified Coordinate System** — All cameras calibrated to a common world coordinate system using a calibration board or known-dimension reference object.
3. **Point Cloud Fusion** — Individual camera point clouds are transformed using extrinsic calibration matrices and merged into a unified scene representation.
4. **Object Segmentation and Measurement** — The fused point cloud is segmented to detect object boundaries, from which length, width, height, and volume are computed.

### 4.6.3 Key Technical Considerations

- **Camera synchronization** — Simultaneous capture across all cameras is essential to prevent motion artifacts when objects move through the station. Options include hardware trigger input or software synchronization.
- **Calibration accuracy** — Precise extrinsic calibration between cameras using a known-size calibration object (e.g., calibration cube or checkerboard) directly impacts final measurement accuracy.
- **Overlap optimization** — Adjacent cameras must have sufficient FOV overlap to enable seamless point cloud stitching and eliminate gaps in coverage.
- **Interference management** — When multiple dToF cameras operate in proximity, enable multi-machine mode to prevent VCSEL cross-interference.
- **Processing architecture** — The system can use edge processing per camera (measurement primitives computed on each SoC) followed by host-level fusion, or stream raw point clouds to a central PC for centralized processing.

### 4.6.4 Reference Implementation

Commercial multi-camera dimensioning systems provide useful benchmarks for the S-Series multi-camera approach:

| Feature | BeeVision 272 | vMeasure.ai | MRDVS S-Series (Potential) |
|---------|--------------|----------|---------------------------|
| Camera Count | 2 IP units | Multiple OEM | 2-4 S10 / S11 |
| Accuracy | +/- 0.5 cm | +/- 0.2 inch (~0.5 cm) | Depends on calibration |
| Max Object | 2,400 x 2,400 x 2,400 mm | Various | Camera FOV and mounting dependent |
| Calibration | 300 mm cube reference | 5-7 second auto-calibration | Calibration board / reference object |
| Output Interfaces | Web API, TCP, RS232 | API, WMS | TCP / UDP / HTTP / Modbus |
| Built-in Processor | Central processor unit | Central processor | Edge (per camera) + optional host fusion |

### 4.6.5 MRDVS S-Series Multi-Camera Deployment

The S-Series hardware and software foundation supports multi-camera deployments:

- **Synchronization** — External trigger input for frame-synchronized capture; PTP (Precision Time Protocol) support for timestamp alignment across cameras.
- **Networking** — Multiple cameras connect through a Gigabit Ethernet switch to a host PC. The Linux SDK includes a `multi_cameras` sample demonstrating multi-camera stream acquisition.
- **Interference avoidance** — Multi-machine mode in firmware coordinates VCSEL emission timing across cameras.
- **Typical configuration** — 2-4 S10 or S11 cameras mounted overhead, angled toward the measurement area to maximize overlap and coverage.
- **Host software** — A central PC receives multiple streams and runs fusion and measurement algorithms. The MRDVS Camera SDK provides the data acquisition layer; application-level fusion and measurement logic is implemented on the host.

> **Note**: MRDVS provides the multi-camera hardware platform (S-Series cameras with trigger sync, multi-machine mode, and SDK multi-camera support) and the single-camera measurement algorithm foundation. For complete turnkey multi-camera volumetric measurement systems — including multi-view calibration tooling, point cloud fusion, and unified measurement algorithms — contact MRDVS technical support for customized development.

### 4.6.6 Comparison with Competitor Systems

| Feature | BeeVision 272 | vMeasure | MRDVS S-Series (Potential) |
|---------|--------------|----------|---------------------------|
| Camera Count | 2 units | Multiple OEM cameras | 2-4 S10 / S11 |
| Accuracy | +/- 0.5 cm | +/- 0.2 inch (~0.5 cm) | Calibration-dependent; mm-level per camera |
| Max Object | 2,400 x 2,400 x 2,400 mm | Various | Determined by camera count and mounting geometry |
| Calibration | 300 mm cube | 5-7 sec auto-calibration | Calibration board / known reference object |
| Interfaces | Web Service API, TCP/IP, FTP/SFTP/Samba, RS232 | API, WMS integration | TCP, UDP, HTTP, Modbus-TCP |
| Built-in PC | Yes (central processor) | Yes (central processor) | Edge per camera + external host for fusion |
| RGB-D Fusion | No (IP cameras only) | No (CV-based only) | Yes (dToF + RGB aligned per camera) |

The S-Series multi-camera approach offers a unique advantage: each camera provides natively aligned dToF depth and RGB data, enabling both geometric and semantic analysis at each viewpoint. This RGB-D fusion capability at the edge is not available in traditional IP-camera-based dimensioners.

# Chapter 5: Integration & Development Guide

This chapter provides practical guidance for integrating the MRDVS S-Series dToF RGB-D cameras into industrial automation systems. It covers SDK workflows, communication protocols, algorithm integration patterns, multi-camera coordination, and system-level best practices.

## 5.1 SDK Quick Start

The MRDVS Camera SDK provides a unified C/C++ interface for device discovery, stream control, parameter configuration, and data acquisition. It supports Windows 7/10/11 and Ubuntu 16.04 or later on both x86 and ARM architectures. The SDK is distributed with CMake build scripts, sample applications, and language bindings.

### 5.1.1 Synchronous API Workflow

The synchronous pattern is the simplest way to acquire frames. The application calls `DcGetPtrValue` to block until a new frame arrives, processes the data, and repeats. This model fits sequential applications such as offline inspection or triggered measurement cycles.

```cpp
// Pseudocode illustration
DcGetDeviceList(&deviceList);      // Discover cameras
DcOpenDevice(&deviceInfo, &handle); // Open connection
DcSetBoolValue(handle, LX_BOOL_ENABLE_3D_DEPTH_STREAM, true);
DcStartStream(handle);              // Begin streaming
DcGetPtrValue(handle, LX_PTR_DEPTH_DATA, &depthData); // Get frame
DcStopStream(handle);
DcCloseDevice(handle);
```

Key characteristics:
- Blocking frame acquisition pattern
- Simple error handling: each call returns an `LX_STATE` code
- Suitable for simple sequential applications
- Must not be used in GUI main threads because `DcGetPtrValue` blocks until the next frame or timeout

### 5.1.2 Asynchronous API Workflow

For real-time robotics applications, register a callback with `DcRegisterFrameCallback`. The SDK delivers frames on an internal thread, leaving the main thread free for control logic.

```cpp
DcRegisterFrameCallback(handle, OnFrameReceived, userData);
DcStartStream(handle);

// Callback executes on SDK thread
void OnFrameReceived(DcHandle handle, FrameInfo* frame, void* userData) {
    // Thread-safe: copy data and post to application queue
}
```

Key characteristics:
- Callback registration: `DcRegisterFrameCallback`
- Non-blocking operation; the application thread never waits for sensor data
- Preferred for real-time applications requiring continuous processing
- Thread-safe frame delivery; copy data out of the callback if processing is lengthy
- Also available: `DcRegisterCameraStatusCallback` for connection-state events

### 5.1.3 Java HTTP Wrapper

For non-C++ environments (Java, Python, C#), the SDK distribution includes a Java-based HTTP server that wraps core SDK functions behind a RESTful interface.

- HTTP server middleware for non-C++ environments
- RESTful API wrapping SDK functions (`DcOpenDevice`, `DcStartStream`, parameter get/set)
- Postman collection (`LxCamera.postman_collection.json`) provided for testing
- Suitable for web-based management systems and WMS integration layers

All requests and responses use JSON payloads. A typical exchange opens a camera by IP, starts the stream, and polls pointer data via HTTP POST.

[IMAGE: SDK architecture diagram showing C++ native layer, Java HTTP middleware, and client applications]

## 5.2 Communication Interfaces

S-Series cameras expose multiple industrial interfaces so that a single device can serve both real-time control loops and higher-level management systems. The table below summarizes the primary options.

| Interface | Port / Medium | Direction | Best For |
|-----------|---------------|-------------|----------|
| IO Output | Hardware pin | Camera to PLC | Hard real-time safety stop |
| CANopen | CAN bus (configurable ID) | Bidirectional | AGV/AMR motion controllers |
| UDP | 14950 (broadcast) | Camera to host | Low-latency fleet status |
| TCP | 14951 | Camera to host | Reliable algorithm results |
| HTTP REST | 14952 | Request/response | WMS/ERP integration |
| Modbus-TCP | 16502 | Request/response | PLC and SCADA systems |

### 5.2.1 IO Output (Obstacle Avoidance)

The camera provides hardware IO pins that output obstacle status directly to a robot safety PLC or relay.

- Hardware IO pin output with configurable push-pull or open-drain modes
- Three states: 0 = Normal, 1 = Warning, 2 = Stop
- Electrical characteristics: refer to the hardware manual for voltage and current ratings
- Typical wiring: connect Output 1 and Output 2 to a robot safety PLC or relay
- IO has the highest priority among all communication methods; when IO is active, other channels cannot override the current parameter set

For multi-mode AGVs, two input pins can select among four pre-configured obstacle-avoidance parameter sets (0 to 3) without software intervention.

### 5.2.2 CANopen Interface

For AGV fleets already running CANopen, the camera can appear as a standard node on the bus.

- CANopen protocol for industrial robot controllers
- Configurable CAN ID and baud rate (default 250 kbps, node ID 0x212)
- Output format: obstacle status codes via SDO protocol
- Standard CAN frame (11-bit COB-ID split into 4-bit function code + 7-bit node ID)
- Integration with AGV/AMR controllers supporting CANopen

A typical SDO read from the host returns the current obstacle parameter index and result code in the 8-byte CAN payload.

### 5.2.3 UDP/TCP Protocol

Algorithm results are available over both UDP and TCP on separate ports.

- UDP Port 14950: broadcast-style, stateless, lowest latency
- TCP Port 14951: connection-oriented, reliable delivery
- Command: send `RESULT_UPDATE` to query the latest detection result
- JSON response with algorithm-specific data fields
- Suitable for LAN-based robot fleet management

The camera responds with a JSON envelope containing `IP`, `ret_code`, `nick_name`, timestamps, and the algorithm payload (e.g., pallet pose or slot occupancy array).

### 5.2.4 HTTP REST API

WMS and ERP systems that prefer standard HTTP can use the REST endpoint.

- Port 14952
- POST requests with `RESULT_UPDATE` body
- JSON response format identical to the TCP/UDP payload
- Headers: `Content-Type: application/json`
- WMS/ERP integration friendly

Example request:
```
POST http://192.168.100.83:14952
Content-Type: application/json

{ "cmd": "RESULT_UPDATE" }
```

### 5.2.5 Modbus-TCP

For direct PLC integration, the camera implements a Modbus-TCP slave on port 16502.

- Port 16502
- Read holding registers (function code 03)
- Register base address: 0x100
- One register per slot (for location status detection)
- Max 16 slots per device
- Big-endian byte order

Register layout:

| Register Address | Content | Format |
|------------------|---------|--------|
| 0x100 + ID | Slot status + marker status | uint16_t |
| High byte | Slot status code | 0 = empty, 1 = occupied, 2 = abnormal |
| Low byte | Marker status code | 0 = no marker, 1 = detected, 2 = abnormal |

## 5.3 Algorithm Integration

All built-in algorithms run on the camera's embedded processor, producing results that the host can consume via SDK, ROS, or any of the network protocols described above. The general integration pattern is:

1. Select the algorithm mode (obstacle, pallet, volume, or location).
2. Configure algorithm parameters via `LX_STRING_ALGORITHM_PARAMS` or the LxCameraViewer tool.
3. Start the stream and open the desired output channel.
4. Consume results in the host application.

### 5.3.1 Obstacle Avoidance Integration

The obstacle avoidance algorithm detects objects within configurable 3D zones and reports status in real time.

- Enable built-in algorithm: set work mode to `WORK_FOREVER` and algorithm mode to `MODE_AVOID_OBSTACLE` or `MODE_AVOID_OBSTACLE2`
- Configure detection zones via LxCameraViewer: warning distance, alarm distance, blind zone, lateral limits, and height range
- Select output interface (IO / CAN / UDP / TCP)
- Test and validate with LxCameraViewer visualization
- Deploy to robot control system

Up to 20 parameter sets can be stored in the camera and switched at runtime via IO, UDP, TCP, or API. S-Series cameras support UDP and IO for parameter switching.

### 5.3.2 Pallet Recognition Integration

The pallet detection algorithm identifies standard pallet structures and returns their pose relative to the camera.

- Enable pallet detection algorithm (`MODE_PALLET_LOCATE`)
- Configure detection parameters: height range, pallet dimensions
- Receive pose data: `X_offset`, `Y_offset`, `yaw_angle`
- Feed to robot path planning system
- Coordinate frame: camera optical center as origin

In ROS, the `LxCamera_Pallet` topic publishes `status`, `x`, `y`, and `yaw` after the pallet node is launched.

### 5.3.3 Volume Measurement Integration

Volume measurement computes bounding-box dimensions for parcels and freight.

- Enable volume detection algorithm
- Configure detection plane distance (the Z plane on which objects rest) and minimum object sizes (min X, min Y, min Z) to filter noise
- Set trigger mode: continuous streaming or command-triggered (`TM_TRIGGER` via TCP/UDP)
- Parse JSON output for `length`, `width`, `height`, and center coordinates (`cx`, `cy`, `cz`)
- Integrate with WMS for automatic freight dimension recording

The JSON array key `TM` contains one entry per detected object. Each entry includes `valid`, `length`, `width`, `height`, `angle`, and center coordinates.

### 5.3.4 Location Status Integration

Location status (slot/库位 detection) monitors inventory presence, alignment, and overhang.

- Enable slot detection algorithm
- Define slot regions: 2D mode (RGB-based rectangles) or 3D mode (point-cloud boxes)
- Configure alignment angle threshold and overhang check
- Read slot status array via TCP/UDP/HTTP/Modbus
- Map to WMS slot inventory database

Result codes:
| Code | Meaning |
|------|---------|
| 0 | Empty |
| 1 | Occupied (normal) |
| 2 | Occupied with alignment error |
| 3 | Occupied and overhanging the slot boundary |

[IMAGE: Algorithm deployment flowchart showing configuration via LxCameraViewer, algorithm activation, and multi-protocol output]

## 5.4 Multi-Camera Coordination

Warehouse and fleet deployments often require multiple cameras. The S-Series provides mechanisms to prevent interference and synchronize capture.

### 5.4.1 Multi-Machine Interference Avoidance

When two or more cameras are mounted close together, their VCSEL emitters can create cross-interference in the time-of-flight measurement.

- Enable `LX_BOOL_ENABLE_MULTI_MACHINE` in multi-camera deployments
- Prevents VCSEL cross-interference between adjacent cameras
- Required when camera FOVs overlap or cameras face each other
- May slightly reduce effective frame rate because the camera time-multiplexes its emission window

### 5.4.2 Network Architecture for Multi-Camera

```
[Host PC] <--- Gigabit Switch <--- [Camera 1] [Camera 2] [Camera 3] ...
```

- All cameras on the same subnet (192.168.100.x/24)
- Switch capacity: Gigabit or higher; each camera can generate sustained depth + RGB traffic
- Host PC: multi-core CPU, 8 GB+ RAM for multi-stream handling
- Firewall on the host PC should be disabled or configured to allow camera traffic

### 5.4.3 Synchronization Strategies

Three strategies are available depending on the application's timing requirements:

| Strategy | Mechanism | Jitter | Use Case |
|----------|-----------|--------|----------|
| Software trigger | SDK sends `LX_CMD_GET_NEW_FRAME` sequentially | Tens of milliseconds | Non-critical fused views |
| External hardware trigger | Synchronized capture via trigger input pin | Sub-millisecond | Stereo or 360-degree rigs |
| PTP time sync | Precision Time Protocol for timestamp alignment | Microsecond | Precise event correlation |

When multiple streams must be fused, match frames by `sensor_timestamp` or `recv_timestamp` fields in the `FrameInfo` structure.

## 5.5 System Integration Best Practices

### 5.5.1 WMS Integration Architecture

A typical warehouse integration stacks the camera as an edge sensor feeding into higher-level systems.

```
[Camera] --> [Gateway / PLC] --> [WMS / ERP]
     |                |
     +--> [Robot controller] (real-time loop)
```

- Camera as edge sensor delivers results to Gateway/PLC or directly to WMS/ERP
- Recommended for WMS: HTTP REST API or Modbus-TCP
- Real-time robot control: UDP for low-latency status updates
- Reliable data logging: TCP for audit trails and traceability

### 5.5.2 JSON Output Parsing

All algorithm results use a consistent JSON envelope regardless of transport protocol:

| Field | Meaning |
|-------|---------|
| `IP` | Source camera address |
| `ret_code` | 0 = success |
| `nick_name` | Configured camera name |
| `DATA_TIME` / `RES_TIME` | Data extraction and result output timestamps |
| Algorithm key | `SG` (human safety), `SS` (slot status), `TM` (volume) |

Host applications should validate `ret_code == 0` before parsing the algorithm-specific payload.

### 5.5.3 Error Handling

Production systems must tolerate transient faults without stopping the robot or conveyor.

- SDK returns `LX_STATE` error codes; always check the return value of `DcOpenDevice`, `DcStartStream`, and parameter setters
- Network timeout handling: `LX_E_TIME_OUT` indicates a missed frame; retry or log
- Automatic reconnection logic: when `LX_E_DEVICE_NOT_CONNECTED` occurs, call `DcGetDeviceList` and reopen
- Graceful degradation: if the camera disconnects, the robot should stop or switch to a backup sensor. The IO pin state `0V / 0V` signals "not initialized or abnormal" and can be wired into the safety chain

### 5.5.4 Performance Optimization

Bandwidth and CPU load can be tuned to match application requirements.

- Use binning (`2x2` or `4x4`) to reduce bandwidth and processing load. `LX_INT_3D_BINNING_MODE` and `LX_INT_2D_BINNING_MODE` control this independently
- Adjust frame rate to application requirements; algorithms running at 10 fps often suffice for AMR obstacle avoidance
- Enable ROI (`DcSetRoI`) if only a partial FOV is needed; the sensor transmits fewer pixels
- Use UDP for high-frequency status updates, TCP for configuration and infrequent queries
- For multi-camera cells, stagger software triggers or enable `LX_BOOL_ENABLE_MULTI_MACHINE` rather than consuming full bandwidth from every camera simultaneously

[IMAGE: WMS integration architecture diagram showing camera, PLC gateway, robot controller, and ERP/WMS layers]

---

*For detailed API signatures, data-type definitions, and platform-specific build instructions, refer to the Camera SDK Development Guide and the Linux Sample Program Guide included in the SDK distribution.*

# Chapter 6: Deployment & Installation Guide

## 6.1 Camera Mounting

### 6.1.1 S10 Series Mounting

Proper mechanical installation is the foundation of reliable depth perception. The S10 series requires a keep-clear zone in front of the lens assembly that must remain free of any mechanical structure, cabling, or reflective surfaces.

**Keep-Clear Zone Requirements**

The keep-clear zone (shown as the pink shaded area in the FOV diagrams) extends forward from the lens cover and matches the camera's full angular field of view. Any intrusion into this zone produces multi-path interference, near-field reflection artifacts, or IR crosstalk, resulting in depth holes, noise, and false obstacle detections.

- S10 (90°×60° FOV): Maintain a cone-shaped keep-clear zone of at least 10 cm (4 in) radius immediately in front of the lens; extend outward following the full 90° horizontal by 60° vertical angle.
- S10 Lite (120°×80° FOV): Wider keep-clear zone required; ensure mounting structure does not clip the periphery.
- S10 Pro (61°×90° FOV): Vertical FOV is wider; verify top and bottom clearance when angled downward.

Common keep-clear violations to avoid: mounting bezels intruding into the FOV cone, protective cover glass placed closer than 10 cm to the lens, cables routed across the front aperture, and small mounting apertures that crop the FOV periphery.

[IMAGE: S10 mounting dimensions and keep-clear diagram showing FOV cones and minimum clearance zones for 90°×60° and 120°×80° variants]

**Mounting Hole Specifications**

The S10 housing includes integrated mounting features. Use machine screws appropriate for the housing threads. Recommended maximum torque is 0.5 Nm to avoid inducing mechanical stress on the lens barrel, which can shift the optical axis and cause systematic range bias.

**Typical Installation Orientations**

| Orientation | Application | Key Considerations |
|-------------|-------------|-------------------|
| Forward-facing | Obstacle avoidance on AMRs/AGVs | Mount on front face of vehicle; ensure ground is visible at lower FOV edge |
| Downward-facing | Volume measurement, slot/pallet detection | Optical axis perpendicular to ground; rigid bracket to prevent vibration-induced tilt |
| Angled / horizontal | Human safety detection | Tilt angle typically under 30° from horizontal; for aisle or doorway scanning |

For forklift installations, mount the S10 on the mast with a downward tilt of 30°–55° so the lower FOV edge grazes the front surface of the vehicle. Mounting height should not exceed 2 m to maintain ground visibility and angular resolution.

For under-ride AMR installations, mount low on the front face with the camera positioned close to the direction of travel. Verify in the point cloud view that the vehicle's own structure does not appear in the detection zone.

**Vibration Damping Recommendations**

Mobile robots subject the camera to continuous vibration. To maintain calibration stability:
- Mount on a rigid bracket rather than sheet-metal flexures
- Use nylon washers or rubber grommets at mounting points to isolate high-frequency vibration
- Avoid cantilevered mounting arms that amplify chassis vibration
- Re-verify extrinsic calibration after the first 100 hours of operation, as bolt settling can shift the camera pose

### 6.1.2 S11 Series Mounting

The S11 series features a compact form factor (90 × 25 × 25 mm for Ethernet/USB variants; 33 × 18 × 13 mm for the MIPI module) enabling flexible mounting in space-constrained robots.

- **M3 mounting holes** on the housing allow direct attachment to brackets or custom frames
- **Lightweight**: approximately 130 g (Ethernet) or 90 g (USB)
- **IP54-rated housing** (Ethernet variant) resists dust and light water spray

**Typical Mounting Scenarios for 3.5-Inch-Class Mobile Robots**

- Front bumper integration: can be recessed behind a thin IR-transparent window rated for 940 nm transmission
- Under-chassis mounting: downward-facing for cliff/edge detection; maintain keep-clear zone below the deck
- Pan-tilt head mounting: for service robots requiring active gaze control; ensure cable strain relief follows the motion axis

**Cable Routing Considerations**

The S11 Ethernet variant uses a single combined connector for power and data. Route cables so they do not pull on the connector during robot motion. Provide a service loop near the camera body and secure the cable to the robot frame within 50 mm of the connector.

[IMAGE: S11 mounting dimensions and bracket integration schematic]

### 6.1.3 Lens Protection

- A **protective cover plate** is available as an accessory for the S10 series. The cover must be manufactured from 940 nm high-transmission optical glass; ordinary acrylic or window glass will attenuate the dToF signal and cause data loss.
- **Never allow direct physical contact** with the front lens surface. The anti-reflection coating is vulnerable to scratching.
- **Cleaning**: Power off the camera. Blow away loose dust with a clean air blower. For smudges, use a lint-free lens tissue moistened with isopropyl alcohol (99%). Wipe in a circular motion from the center outward. Do not use abrasive cloths, paper towels, or ammonia-based cleaners.
- In environments with heavy airborne particulate (e.g., coal yards, sawmills), clean the lens weekly; otherwise monthly inspection is sufficient.

## 6.2 Network Configuration

### 6.2.1 Initial Setup

All S-Series cameras ship with a default static IP address. The host PC must be configured to the same subnet before first connection.

1. Connect the camera to the host PC using a Cat 6 or higher Gigabit Ethernet cable.
2. Set the host PC Ethernet adapter to a static IP in the `192.168.100.x` range (e.g., `192.168.100.88`).
3. Set the subnet mask to `255.255.255.0`.
4. Disable the Windows Defender Firewall, or create an inbound rule that allows the camera application.
5. Launch LxCameraViewer. The camera should appear in the device list with its factory IP (`192.168.100.82` by default). Click **Open** to verify streaming.

If the camera does not appear, confirm the blue power LED on the camera front is slowly flashing, verify the Ethernet link LED on the host network adapter is lit, and check that no other device is using the same IP address.

### 6.2.2 Linux Network Setup

```bash
# Disable firewall (Ubuntu)
sudo systemctl stop ufw.service
sudo systemctl disable ufw.service
sudo ufw status

# Configure static IP
sudo ip addr add 192.168.100.88/24 dev eth0
```

For persistent configuration, add the static IP to `/etc/netplan/01-netcfg.yaml` (Ubuntu 18.04+) or the appropriate NetworkManager connection profile.

### 6.2.3 Multi-Camera Network

Systems requiring multiple cameras should be deployed on a shared Gigabit Ethernet infrastructure.

- Use a **Gigabit Ethernet switch** (unmanaged is acceptable; managed switches may require disabling IGMP snooping if multicast discovery is blocked).
- Assign a **unique IP address** to each camera via LxCameraViewer (Others → IP Configuration Tool) or the SDK API.
- Ensure **all devices share the same subnet mask** (`255.255.255.0`).
- **Bandwidth planning**: Account for approximately 100–300 Mbps per camera depending on enabled streams. A four-camera system streaming depth + RGB + point cloud simultaneously requires a full Gigabit backbone with headroom.
- For multi-camera interference avoidance, enable **Multi-Machine Mode** in the 3D Settings panel after stopping the stream.

## 6.3 Software Deployment

### 6.3.1 Host PC Software Installation

The MRDVS host software suite provides camera configuration, algorithm deployment, and debugging tools.

1. Download the **Lanxin-MRDVS** installer from the MRDVS support portal.
2. Run the installer with administrator privileges.
3. Select a non-system drive for installation (e.g., `D:\Program Files\Lanxin-MRDVS`) to avoid Windows permission issues.
4. After installation, right-click the desktop shortcut and select **Run as administrator** for the first launch.
5. Launch LxCameraViewer and verify the camera appears in the device list.

**Minimum host PC requirements**

| Component | Requirement |
|-----------|-------------|
| OS | Windows 10/11 (64-bit) or Ubuntu (64-bit) |
| Memory | 4 GB or more |
| Network | Gigabit Ethernet adapter and cable |
| Display | Intel UHD Graphics or discrete GPU with OpenGL support |

### 6.3.2 Algorithm Deployment Workflow

MRDVS application algorithms run on the camera's embedded SoC. Deployment is performed via the host PC tools.

1. Open the camera in LxCameraViewer and start the stream.
2. Check firmware version under Device Information. If an upgrade is needed, navigate to **Basic Tools → Function Settings → Firmware Upgrade**, select the algorithm firmware binary, and click **Execute**. The camera reboots automatically.
3. Install the algorithm host tool (**LxApplicationViewer.exe**) by copying it into the `Lanxin-MRDVS/Tools` folder.
4. Launch LxApplicationViewer as administrator. The camera should display a **green status**, indicating the algorithm is active.
5. Navigate to **Camera Configuration → Extrinsic Calibration** and perform calibration for the installed orientation (see Section 6.4).
6. Switch to the target algorithm tab and configure detection zones and parameters.
7. Click **Save and Push** to write parameters to the camera non-volatile memory.
8. Verify detection results in real time within the tool's visualization pane.

Algorithm host tools can be upgraded by simply replacing the `.exe` file; no re-installation of the base package is required.

### 6.3.3 Linux SDK Deployment

For systems integrating via the C/C++ or ROS SDK on Linux:

1. Extract the SDK archive: `tar -xzf Lanxin-MRDVS-xxx.tar.gz`
2. Run the installation script as root: `sudo bash install.sh`
3. Verify installation under `/opt/Lanxin-MRDVS`
4. Execute the network optimization script: `sudo bash set_socket_buffer_size.sh`
5. Build the sample applications:
   ```bash
   mkdir build && cd build
   cmake ..
   make
   ```
6. Run the appropriate sample for your application.

After installation, ensure the `LD_LIBRARY_PATH` includes `/opt/Lanxin-MRDVS/lib` or run `source install.sh` to configure environment variables. The SDK supports C, C++, C#, Java, ROS1, and ROS2 interfaces.

## 6.4 Calibration Procedures

### 6.4.1 Extrinsic Calibration Overview

Extrinsic calibration establishes the camera's pose relative to a world or ground coordinate system. This step is mandatory for all application algorithms because detection zones and measurement results are computed in world coordinates.

Calibration computes:
- Camera height above ground (Z translation)
- Tilt angles (roll, pitch, yaw) relative to the ground plane
- X/Y offset from the vehicle or cell origin

Algorithms requiring extrinsic calibration include Obstacle Avoidance, Volume Measurement, Pallet / Slot Detection, Human Safety Detection, and Location Status Detection.

[IMAGE: Coordinate axis convention diagram showing X (red), Y (green), Z (blue) relative to ground plane and vehicle body]

### 6.4.2 Vertical Installation Calibration (Volume / Slot Detection)

For downward-facing installations where the camera looks vertically at the ground or a conveyor:

1. In LxApplicationViewer, open **Camera Configuration**.
2. Reset all extrinsic parameters to 0: set x, y, z, roll, pitch, yaw to `0`.
3. Click **Save and Push** to apply the reset.
4. Click the **Extrinsic Calibration** button.
5. The software automatically detects the ground plane from the point cloud and computes the calibration.
6. Upon success, verify in the point cloud view that the **Z-axis (blue arrow) points downward** perpendicular to the ground, and the **X-Y plane is parallel to the ground**.
7. Record the calibrated Z value. This represents the camera height above the detection plane and is required for setting the "detection plane distance" in volume measurement configuration.

**Important**: For volume measurement, after recording the calibrated Z value, set the camera Z parameter back to `0` and click **Save and Push** again. Then enter the recorded Z value into the volume detection **Detection Plane Distance** field. This prevents double-counting the height offset in the measurement algorithm.

### 6.4.3 Horizontal / Angled Installation Calibration (Obstacle / Human)

For forward-facing or tilted installations on mobile robots:

1. Reset all extrinsic parameters to 0 and click **Save and Push**.
2. In the point cloud view, manually align the ground plane to the coordinate axes:
   - The **X-axis (red)** and **Y-axis (green)** should lie flat on the ground plane.
   - The **Z-axis (blue)** should point **upward**, perpendicular to the ground.
3. Adjust **roll** and **Z** primarily to achieve alignment. Keep pitch and yaw at `0` initially unless the mounting orientation specifically requires correction.
4. Click **Save and Push**.
5. Verify alignment by rotating to a side view and confirming the ground plane appears as a flat horizontal surface at Z ≈ 0.

For obstacle avoidance on forklifts, a typical starting configuration is roll adjusted until the horizon is level, pitch near 0 (if tilt is purely mechanical), and Z equal to the camera mounting height in millimeters. Fine-tune iteratively until vehicle-mounted structures disappear below the ground plane and real obstacles appear above it.

### 6.4.4 Calibration Verification

After calibration, perform these checks before production deployment:

- **Coordinate axes**: Confirm in the point cloud view that axis colors match expected ground alignment.
- **Known-position test**: Place a box of known dimensions at a measured location within the detection zone. Verify reported coordinates match physical placement within the camera's accuracy specification (≤ 3 cm for S10, ≤ 1 cm for S11).
- **Volume tolerance check**: Place a calibration cube of known size under the camera. Verify reported dimensions are within ±5 mm of ground truth.
- **Zone triggering check**: Walk through configured early-warning and alarm zones. Confirm correct zone color transitions and IO output toggling.
- **Ground-plane stability**: With no objects present, confirm the point cloud does not show phantom obstacles above the floor.

## 6.5 Maintenance and Care

### 6.5.1 Regular Inspection

Establish a preventive inspection schedule based on environment severity:

- **Lens cleanliness**: Check monthly in clean indoor environments; weekly in dusty, oily, or outdoor environments.
- **Cable integrity**: Verify Ethernet and power connectors are fully seated. Inspect cables for abrasion at chassis entry points.
- **Firmware version**: Check periodically via LxCameraViewer Device Information panel. Update when release notes indicate relevant bug fixes or features.
- **Physical condition**: Inspect housing for cracks, connector damage, or corrosion. Confirm mounting screws remain tight.

### 6.5.2 Cleaning Procedure

1. **Power off** the camera and allow it to cool.
2. Use a clean, dry air blower to remove loose dust.
3. For fingerprints or oil, moisten a lint-free lens tissue with isopropyl alcohol (99%).
4. Wipe gently in a **circular motion from the center outward**.
5. Allow the lens surface to dry completely before powering on.

Do not use paper towels, abrasive cloths, or ammonia-based cleaners.

### 6.5.3 Firmware Updates

- Check the current firmware version in LxCameraViewer (Device Information panel).
- Download the appropriate firmware `.bin` file for your camera model from the MRDVS support portal.
- Backup the current configuration using **Function Settings → Export to File** before major updates.
- Apply the update via **Basic Tools → Function Settings → Firmware Upgrade → Update Version**. Select the `.bin` file and confirm. The camera reboots automatically.
- After reboot, verify the new version in the Device Information panel. Re-import saved configuration if settings were reset.

### 6.5.4 Service Life and Environment

| Parameter | Specification |
|-----------|--------------|
| Operating temperature | -20°C to +60°C (-4°F to +140°F) |
| Storage temperature | -40°C to +85°C (-40°F to +185°F) for S10; -40°C to +70°C for S11 |
| Humidity | Max 90% RH (non-condensing) |
| Illumination | 0 kLux to 100 kLux |
| Housing temperature rise | < 25°C above ambient under normal operation |
| Expected service life | 5+ years under normal industrial conditions |
| ESD | Contact ±4 kV, Air ±8 kV |

Operational guidelines:
- Avoid prolonged direct sunlight exposure on the lens surface. While the sensor operates at 100 kLux ambient, continuous solar heating of the front window can cause thermal drift.
- Avoid high-humidity condensation environments. If condensation occurs, power off and allow the camera to acclimate before restarting.
- Ensure adequate ventilation around the housing. Enclosed mounting cavities should include thermal relief paths.

### 6.5.5 Troubleshooting Quick Reference

| Symptom | Likely Cause | Quick Fix |
|---------|-------------|-----------|
| No stream data / camera not found | Firewall blocking; IP mismatch | Disable firewall; verify host IP is in `192.168.100.x` subnet; ensure unique IP |
| Depth holes / black regions | Multi-camera interference | Stop stream; in 3D Settings enable **Multi-Machine Mode**; restart stream |
| Frequent reboots or unstable stream | Insufficient power supply | Verify 24 V DC, 2 A minimum supply; check cable gauge and length |
| Poor accuracy at edges; bright artifacts | Glare from reflective surfaces | Enable glare suppression; adjust integration time; set low-signal threshold to 10–20 |
| Point cloud layering | Hardware calibration issue | Contact MRDVS technical support |
| Near-field white object edge poor (S-Series) | dToF glare effect | Enable glare optimization in firmware |
| Reflective column overexposure (S-Series) | Specular reflection | Enable glare suppression in filtering settings |
| Black object imaging incomplete (S-Series) | Low reflectivity | Increase high integration time to 1400; set low-signal threshold to 20 |
| Extrinsic calibration fails, "ground not detected" | Ground not visible; exposure too low | Increase high integration time to 1400; set low-signal threshold to 10; retry |
| Algorithm host tool shows yellow status | Algorithm not loaded | Confirm algorithm firmware flashed; click **Auto Refresh** or restart camera |
| Exclusive control permission failure | Multiple clients connecting | Ensure only one instance controls the camera; wait 3–5 s after disconnect |

# Chapter 7: Frequently Asked Questions

This chapter addresses common issues encountered during deployment, configuration, and secondary development of S-Series cameras. Questions are organized by category and reflect the most frequent support cases from system integrators and robotics engineers. Each entry provides a concise problem description, root cause, and actionable resolution.

## 7.1 Hardware Issues

| # | Question | Cause | Solution |
|---|----------|-------|----------|
| 1 | Power LED off or dim | Unstable or insufficient power supply | Ensure 24 VDC, 2 A power input. Verify cable integrity and connector seating. |
| 2 | Camera reboots repeatedly | Bandwidth saturation or power instability | (1) Check network bandwidth utilization; (2) Verify power supply stability under peak load. |
| 3 | Obstacle avoidance output normal but IO result abnormal | Incorrect wiring, damaged cable, or outdated firmware | (1) Verify IO wiring against the pinout; (2) Check signal cable continuity; (3) Ensure firmware is dated May 2025 or later. |
| 4 | Network interface shows disconnected | Physical connection failure | Inspect Ethernet cable, switch port, and host NIC. Replace cable if damaged. |
| 5 | CANopen communication: no data or mismatched frames | Baud rate / CAN ID mismatch or incorrect wiring | (1) Confirm matching baud rate and CAN ID; (2) Verify CAN_H/CAN_L wiring and termination resistor. |

## 7.2 Software and Streaming Issues

| # | Question | Cause | Solution |
|---|----------|-------|----------|
| 1 | LxCameraViewer shows no data stream after camera open | Firewall blocking; IP subnet mismatch; switch filtering; host PC anomaly | (1) Disable firewall or whitelist camera IP; (2) Set host NIC to the camera subnet; (3) Inspect switch for IGMP snooping or port isolation; (4) Test with alternate PC; (5) Configure multi-subnet routing if required. |
| 2 | Depth map shows black rectangular holes on a known flat target | Multi-device interference | Stop streaming. In **3D Settings**, disable multi-device mode. (If an embedded algorithm is active, change algorithm mode to "disconnect" first.) |
| 3 | S10 / S10 Pro fails to stream or depth image appears compressed | LxCameraViewer version incompatibility | Update LxCameraViewer to version 1.3.66.0606 or later. |
| 4 | Camera open fails with network communication error | Host and camera on different IP subnets | Reconfigure host NIC or camera IP to share the same subnet. |
| 5 | Exclusive application permission failed | Multiple clients attempting simultaneous connection | Ensure only one client connects at a time. Versions 2.4.50.0720+ support multi-user permissions with restricted access. |
| 6 | Camera opens in restricted mode but no data appears | Multiple processes holding device handles | The first process holds primary control. Restricted clients can view the stream only after the primary client initiates streaming. |
| 7 | Target appears sparse or partially missing in depth image | Insufficient exposure or aggressive filtering | (1) Check high-exposure parameter (recommended: 600–1200); (2) Check low-signal threshold (recommended: 10–30). |
| 8 | Stream active, frame rate normal, but no image data; laser emitter dim or off | Laser emitter degradation | Use a smartphone camera to inspect the front VCSEL array. If emitters are dim or off, replace the hardware. |
| 9 | Side edges of target generate noise when viewed head-on | Glare from high-reflectivity surfaces | (1) Enable glare suppression in **3D Settings**; (2) Increase low-signal threshold. |
| 10 | Point cloud shows visible layering (S-Series) | Depth quantization or calibration artifact | Contact MRDVS support for hardware evaluation. |
| 11 | White object edges exhibit reduced accuracy at close range (S-Series) | Glare causing blooming at high-reflectivity boundaries | Enable glare optimization in **3D Settings**. Note: may reduce completeness on dark surfaces. |
| 12 | Reflective pillars exhibit large-area overexposure (S-Series) | Specular reflection overwhelming SPAD pixels | Enable glare suppression in **3D Settings**. |
| 13 | Black objects appear incomplete or fragmented (S-Series) | Insufficient returned signal from low-reflectivity surfaces | Enable high-exposure mode. Set ToF register `100130B0` to `240`. Increase low-signal threshold if needed. |
| 14 | No RGB data after enabling RGB-D alignment | Software version lacking RGB-D sync support | Update LxCameraViewer and Camera SDK to the latest release.

## 7.3 Application Algorithm Issues

| # | Question | Cause | Solution |
|---|----------|-------|----------|
| 1 | Black pallet recognition fails | Black pallet returns insufficient signal | (1) In **3D Settings**, set high exposure to 1400; (2) In **Filter Settings**, set low-signal threshold to 20 and small-signal detection to 1. |
| 2 | PalletPro detection normal but vehicle receives abnormal results | Embedded algorithm firmware version mismatch | Update embedded algorithm firmware to match LxCameraViewer version. |
| 3 | Extrinsic calibration reports "no ground detected" | Ground point cloud not visible due to low exposure | Temporarily set high exposure to 1400 and low-signal threshold to 10. Perform extrinsic calibration, then restore original parameters. |
| 4 | Near-end extrinsic calibration fails | Incomplete pallet imaging or incorrect distance | (1) Verify pallet appears complete in point cloud; (2) Ensure camera-to-pallet distance is approximately 1300 mm. |
| 5 | Pallet recognition returns failed-1 / -11 | No data or insufficient data | Confirm camera streams normally via LxCameraViewer. Check if aggressive filtering has removed too many points. |
| 6 | Pallet recognition returns failed-2 / -12 / -16 | Insufficient point cloud; pallet legs not detected | Possible causes: (1) camera or fork height changed; (2) legs occluded; (3) excessive camera pitch; (4) weak leg returns; (5) reflective marker too close to leg. Use PalletPro to diagnose, then adjust configuration or correct forklift pose. |
| 7 | Pallet recognition returns failed-3 / -13 | Leg dimensions out of supported range | Adjust leg-width or pallet-width parameters to match actual geometry. |
| 8 | Pallet recognition returns failed-4 / -14 | Camera installation height not configured | Set installation height or complete extrinsic calibration. |
| 9 | Pallet recognition returns failed-5 / -15 | Pallet validation failed; cross-beam anomaly or excessive tilt | Disable flying-pixel removal in LxCameraViewer, reduce standoff distance, or adjust cross-beam detection ratio. |
| 10 | Pallet recognition returns failed-7 | Excessive width difference between detected legs | Check for reflective markers or nearby walls interfering with outer-leg edge imaging. |
| 11 | Pallet recognition returns failed-8 | Pallet yaw angle exceeds 30 degrees | Align vehicle or pallet to within 30 degrees head-on before detection. |
| 12 | Obstacle avoidance configuration switch via IO/communication does not reflect in software | Target configuration not pre-saved; stream not restarted | (1) Confirm target configuration was saved before switching; (2) Stop and restart streaming, then verify. |
| 13 | Obstacle output shows anomalous values (e.g., 1000) | Algorithm firmware version mismatch | Upgrade embedded algorithm firmware to the latest compatible release. |
| 14 | IO produces no output | Incorrect wiring topology or outdated firmware | For S10 / S10 Pro, upgrade algorithm firmware to V1.1.050E06_250527 or later. Verify IO wiring against the hardware manual. |
| 15 | Obstacle alarm triggers when open space contains no obstacles | Configuration or firmware desync | (1) Verify LxCameraViewer version; (2) Verify camera firmware version; (3) Confirm obstacle avoidance parameters are correctly downloaded. |
| 16 | Location / volume / human detection application cannot open camera after firmware update | Device not fully initialized after flash | Restart the camera. In the application software, enable **Real-Time Refresh** in global settings. |

## 7.4 Secondary Development Issues

### SDK Error Codes

The following table lists common error codes returned by the Camera SDK and their resolutions.

| Error Code | Meaning | Solution |
|------------|---------|----------|
| LX_ERROR = -1 | Unknown error | Pull device logs and contact MRDVS support. |
| LX_E_NOT_SUPPORT = -2 | Function not supported | Verify device model and firmware version. |
| LX_E_NETWORK_ERROR = -3 | Network communication error | Check IP configuration, cable, and switch integrity. |
| LX_E_INPUT_ILLEGAL = -4 | Illegal input parameter | Validate all parameters against API documentation. |
| LX_E_RECONNECTING = -5 | Device reconnecting | Wait briefly and retry; verify camera power and network stability. |
| LX_E_DEVICE_ERROR = -6 | Device fault or no response | Restart the application or power-cycle the camera. |
| LX_E_DEVICE_NEED_UPDATE = -7 | Device firmware too low | Upgrade camera firmware. |
| LX_E_API_NEED_UPDATE = -8 | SDK API too low | Update Camera SDK to the latest release. |
| LX_E_CTRL_PERMISS_ERROR = -9 | Exclusive control permission failed | Camera supports one primary client. If not closed cleanly, wait 3–5 s for session timeout. |
| LX_E_IMAGE_SIZE_ERROR = -11 | Image size mismatch | Close and re-open the device. |
| LX_E_IMAGE_PARTITION_ERROR = -12 | Image parse failure | Close and re-open the device. |
| LX_E_DEVICE_NOT_CONNECTED = -13 | Camera not connected | Ensure `OpenDevice` succeeds before streaming. |
| LX_E_DEVICE_INIT_FAILED = -14 | Camera initialization failed | Re-open the camera; check for resource contention. |
| LX_E_FILE_INVALID = -16 | File error | Verify file path, extension, and access permissions. |
| LX_E_CRC_CHECK_FAILED = -17 | File CRC/MD5 failure | Retry transfer; check network stability. |
| LX_E_TIME_OUT = -18 | Operation timeout | Retry; inspect network latency and camera frame rate. |
| LX_E_FRAME_LOSS = -19 | Frame loss | Occasional loss in callback mode is acceptable. For persistent loss, check bandwidth and buffer sizes. |
| LX_E_NOT_RECEIVE_STREAM = -21 | No stream data received | Follow Section 7.2 diagnostic steps for no-data issues. |
| LX_E_PARSE_STREAM_FAILED = -22 | Stream enabled but parse failure | Verify firmware/SDK compatibility or contact MRDVS support. |
| LX_E_PROCESS_IMAGE_FAILED = -23 | Image processing failure | Verify `Dataprocess` algorithm library version matches the SDK. |
| LX_E_SETTING_NOT_ALLOWED = -24 | Setting not allowed in always-on mode | In always-on mode, ROI and alignment settings are prohibited. Set working mode first, then configure. |
| LX_E_LOAD_DATAPROCESSLIB_ERROR = -25 | Failed to load image processing library | Ensure `Dataprocess` shared library is present and in the library path. Run `source install.sh`. |
| LX_E_FUNCTION_CALL_LOGIC_ERROR = -26 | Function call logic error | Review call sequence. Common mistakes: changing size parameters without stopping stream; enabling HDR while auto-exposure is active; algorithm upload while embedded algorithm is running. |
| LX_E_IPAPPDR_UNREACHABLE_ERROR = -27 | IP unreachable | Ensure SDK host can ping the camera IP. |
| LX_E_FRAME_ID_NOT_MATCH = -28 | RGB-D sync mismatch | When RGB-D sync is enabled, frame IDs may diverge. Drop the frame or ignore based on priority. |
| LX_E_FRAME_MULTI_MACHINE = -29 | Multi-device interference detected | Discard the frame or ignore based on application tolerance. |

### Common Development Issues

**Linux Environment Setup**

If the SDK reports error -25 or fails to load `LxDataProcess` on Linux, run `source install.sh` from the SDK root to configure `LD_LIBRARY_PATH`. Ensure the `Dataprocess` shared object exists in the correct architecture subdirectory (`x86_64` or `aarch64`).

**Image Size Mismatch**

If acquired image dimensions do not match expected resolution, close the device with `CloseDevice`, then re-open with `OpenDevice` to renegotiate frame format.

**Frame Loss in Callback Mode**

Under heavy network load, the callback thread may drop frames. Occasional single-frame loss is acceptable. For persistent high-volume loss: (1) increase receive buffer size; (2) verify host NIC supports jumbo frames; (3) reduce concurrent streams; (4) use a dedicated Gigabit Ethernet segment.

**Multi-User Connection Conflicts**

The S-Series supports one primary-control client and multiple restricted-view clients. A second primary session returns `LX_E_CTRL_PERMISS_ERROR` (-9). If an application crashes without closing the device, the camera holds the session for 3–5 s. Wait for timeout or power-cycle the camera.

**ROS / ROS2 Compatibility**

If linking the SDK within a ROS2 node causes symbol conflicts, use the wrapper library in the SDK `util` folder, which links the camera SDK indirectly. Include this wrapper in `CMakeLists.txt` instead of directly linking the core SDK library.

**Detecting Lite vs. Standard RGB Variant in Software**

To determine at runtime whether the connected device is a Lite variant (no RGB stream), call `DcGetBoolValue(handle, LX_BOOL_ENABLE_2D_STREAM, &test_rgb)`. A Lite variant returns `LX_E_NOT_SUPPORT` (-2).

**Time Synchronization**

The S-Series SDK automatically synchronizes timestamps with the camera host via PTP on every API call. No manual sync is required.

# Chapter 8: Appendices

## Appendix A: Technical Specifications Summary

### A.1 S10 Series Specifications

The following table summarizes the key technical parameters for the S10 standard and S10 Pro models. The S10 is available in two depth FOV variants: 90 degrees x 60 degrees (standard) and 120 degrees x 80 degrees (Lite). The Lite variant omits the RGB module and includes an IO cable.

| Parameter | S10 | S10 Pro |
|-----------|-----|---------|
| Depth Resolution | 240 x 160 | 240 x 160 |
| RGB Resolution | 1920 x 1080 @ max. 20 fps (typical 15 fps) | 1920 x 1080 @ max. 10 fps (typical 10 fps) |
| Frame Rate (Depth) | Max. 20 fps, typical 15 fps | Max. 10 fps, typical 10 fps |
| Frame Rate (RGB) | Max. 20 fps, typical 15 fps | Max. 10 fps, typical 10 fps |
| Distance Range | 0.3 - 8 m (90% reflectivity); 0.3 - 3 m (5% reflectivity) | 0.3 - 17 m (90% reflectivity); 0.3 - 13 m (10% reflectivity) |
| Precision | less than or equal to 3 cm | less than or equal to 3 cm |
| H-FOV / V-FOV (Depth) | 90 degrees x 60 degrees plus or minus 3 degrees (standard); 120 degrees x 80 degrees plus or minus 3 degrees (Lite) | 61 degrees x 90 degrees plus or minus 3 degrees |
| H-FOV / V-FOV (RGB) | 90 degrees x 60 degrees plus or minus 3 degrees (standard); 120 degrees x 80 degrees plus or minus 3 degrees (Lite) | 61 degrees x 90 degrees plus or minus 3 degrees |
| Dimensions (L x W x H) | 80 x 37 x 25 mm (3.15 x 1.46 x 0.98 in) | 92 x 47 x 51 mm (3.62 x 1.85 x 2.01 in) |
| Weight | 190 g (0.42 lb) | 460 g (1.01 lb) |
| Power Supply | 12 - 28 V DC | 24 V DC |
| Power Consumption | less than or equal to 4 W (average) | less than or equal to 7 W (average) |
| Interface | Ethernet, GMSL (optional), IO | Ethernet, IO |
| IP Rating | IP54 | IP67 |
| Operating Temperature | -20 degrees C to 60 degrees C (-4 degrees F to 140 degrees F) | -20 degrees C to 60 degrees C (-4 degrees F to 140 degrees F) |
| Laser Class | Class 1 | Class 1 |

### A.2 S11 Series Specifications

The S11 is a compact, fully solid-state dToF RGB-D camera offered in three interface variants. Core depth performance is identical across all variants; differences are in interface type, dimensions, weight, and power configuration.

| Parameter | S11-ETH | S11-USB | S11-MIPI |
|-----------|---------|---------|----------|
| Depth Resolution | 240 x 96 @ max. 20 fps (typical 10 fps) | 240 x 96 @ max. 20 fps (typical 10 fps) | 240 x 96 @ max. 20 fps (typical 10 fps) |
| RGB Resolution | 1280 x 1080 @ max. 20 fps (typical 10 fps) | 1280 x 1080 @ max. 20 fps (typical 10 fps) | N/A |
| Frame Rate (Depth) | Max. 20 fps, typical 10 fps | Max. 20 fps, typical 10 fps | Max. 20 fps, typical 10 fps |
| Frame Rate (RGB) | Max. 20 fps, typical 10 fps | Max. 20 fps, typical 10 fps | N/A |
| Distance Range | 0.1 - 6 m (outdoor, 10% - 90% reflectivity); up to 10 m indoor | 0.1 - 6 m (outdoor, 10% - 90% reflectivity); up to 10 m indoor | 0.1 - 6 m (outdoor, 10% - 90% reflectivity); up to 10 m indoor |
| Precision | less than or equal to 1 cm | less than or equal to 1 cm | less than or equal to 1 cm |
| H-FOV / V-FOV (Depth) | 140 degrees x 56 degrees | 140 degrees x 56 degrees | 140 degrees x 56 degrees |
| H-FOV / V-FOV (RGB) | 120 degrees x 110 degrees plus or minus 3 degrees | 120 degrees x 110 degrees plus or minus 3 degrees | N/A |
| Dimensions (L x W x H) | 90 x 25 x 25 mm (3.54 x 0.98 x 0.98 in) | 90 x 25 x 25 mm (3.54 x 0.98 x 0.98 in) | 33 x 18 x 13 mm (1.30 x 0.71 x 0.51 in) |
| Weight | approx. 130 g | approx. 90 g | less than 10 g |
| Power Supply | 12 - 28 V DC | 5 V (USB 3.0) | 3.3 V / 5 V DC |
| Power Consumption | less than 4 W (average), 13 W (peak) | less than 4 W (average), 30 W (peak) | less than or equal to 2 W (average), 8 W (peak) |
| Interface | Gigabit Ethernet | USB 3.0 | MIPI |
| IP Rating | IP54 | N/A | N/A |
| Operating Temperature | -20 degrees C to 60 degrees C (-4 degrees F to 140 degrees F) | -20 degrees C to 60 degrees C (-4 degrees F to 140 degrees F) | N/A |
| Laser Class | Class 1 | Class 1 | Class 1 |

> **Note:** S11 specifications marked N/A indicate parameters not defined in the source documentation for that interface variant. MIPI variant parameters such as operating temperature and IP rating should be confirmed with MRDVS technical support for specific integration requirements.

---

## Appendix B: Certifications and Compliance

待附上证书链接附件，Google drive

### B.1 CE Certification

- **Certificate Number:** AE 50684372 0001
- **Report Number:** CN25KQE6 001
- **Issuing Body:** TÜV Rheinland LGA Products GmbH
- **Directive:** 2014/30/EU Electromagnetic Compatibility
- **Standards:** EN IEC 61000-6-2:2019 (EMC Immunity), EN IEC 61000-6-4:2019 (EMC Emission)
- **Applicable Models:** LXPS-SA730-79I-940B, LXPS-SA732-79I-940B, LXPS-SA730-7CI-940A, LXPS-SA732-7CI-940A (S10 Series)
- **Date of Issue:** 2025-06-30
- **Product Description:** Video Camera (Eagle-S10 camera)

> This certificate of conformity confirms the tested sample conforms with all provisions of Annex I of Council Directive 2014/30/EU. The certificate does not imply assessment of production and does not permit the use of a TÜV Rheinland mark of conformity.

### B.2 RoHS 2.0 Compliance

- **Report Number:** 168563744a 001
- **Issuing Body:** TÜV Rheinland (Shenzhen) Co., Ltd.
- **Testing Standard:** IEC 62321 series (IEC 62321-3-1, IEC 62321-4, IEC 62321-5, IEC 62321-6, IEC 62321-7-1, IEC 62321-7-2, IEC 62321-8)
- **Regulatory Basis:** RoHS Directive 2011/65/EU Annex II and amendment Directive (EU) 2015/863
- **Tested Substances:** Cadmium (Cd), Lead (Pb), Mercury (Hg), Hexavalent Chromium (Cr VI), Polybrominated Biphenyls (PBB), Polybrominated Diphenyl Ethers (PBDE), Bis(2-ethylhexyl) phthalate (DEHP), Benzyl butyl phthalate (BBP), Dibutyl phthalate (DBP), Diisobutyl phthalate (DIBP)
- **Result:** PASS - All tested substances below permissible limits
- **Applicable Models:** LXPS-SA730-79I-940B, LXPS-SA730-7CI-940A, LXPS-SA732-79I-940B, LXPS-SA732-7CI-940A
- **Testing Period:** 2025-07-11 to 2025-08-19
- **Report Date:** 2025-09-19

### B.3 REACH Compliance

- **Report Number:** 168576959a 001
- **Issuing Body:** TÜV Rheinland (Shenzhen) Co., Ltd.
- **Regulation:** (EC) No 1907/2006 (REACH)
- **Testing Period:** 2025-09-28 to 2025-11-11
- **Report Date:** 2025-11-17
- **Tested Substances:** 240+ Substances of Very High Concern (SVHCs) subject to the ECHA candidate list
- **Result:** Compliant - All SVHC concentrations below the 0.1% reporting limit (< RL)
- **Applicable Models:** LXPS-SA730-79I-940B, LXPS-SA730-7CI-940A, LXPS-SA732-79I-940B, LXPS-SA732-7CI-940A, LXPS-SA732-79I-940A, LXPS-SA730-79I-940A

### B.4 Laser Safety Class I

- **Report Number:** XZJHW-20251050010
- **Issuing Body:** Zhejiang Institute of Quality Sciences
- **Standard:** IEC 60825-1:2014 / EN 60825-1:2014
- **Classification:** Class 1 Laser Product
- **Tested Model:** LXPS-SA732-7CI-940A
- **Measured Wavelength:** 937 nm
- **Pulse Width:** 5.6 ns
- **Repetitive Frequency:** 7 MHz
- **Maximum Output (Radiant Power):** 1.528 mW (measured under Condition 3 with 7 mm aperture at 100 mm)
- **Accessible Emission Limit (AEL):** 1.21 W
- **Test Date:** 2025-10-21
- **Report Date:** 2025-10-31

> **Note:** Class 1 classification means the laser is safe under all conditions of normal use. The measured radiant power of 1.528 mW is well below the AEL of 1.21 W, confirming compliance with Class 1 requirements per IEC 60825-1:2014. If the protective housing is removed, the laser radiation classification may be higher than the measured result.

---

## Appendix C: Ordering Information

### C.1 Standard Accessories

| Accessory | Part Number | Description |
|-----------|-------------|-------------|
| Power Cable | [to be confirmed] | DC power cable with aviation connector (for S10 / S11-ETH) |
| Ethernet Cable | [to be confirmed] | Shielded Cat 6/7 cable, Gigabit Ethernet rated |
| Mounting Bracket | [to be confirmed] | Standard L-bracket for S10 series |
| USB Cable | [to be confirmed] | USB 3.0 Type-C cable (for S11-USB) |

> **Note:** For exact part numbers, availability, and pricing, contact MRDVS sales or your authorized distributor.

---

## Appendix D: Contact and Support

### D.1 Global Headquarters

- **Company:** MRDVS Technology Co., Ltd.
- **Website:** https://www.mrdvs.com/
- **S-Series Product Page:** https://mrdvs.com/s-series-obstacle-avoidance-robot-cameras/
- **Knowledge Base:** https://hub.mrdvs.cn/
- **Address:** No.902, Building No.7, Wenyi West Road No.1818-2, Yuhang District, Hangzhou City, Zhejiang Province, China

### D.2 Technical Support Resources

- **Online Knowledge Base (Chinese):** https://hub.mrdvs.cn/ - Product documentation, FAQs, deployment guides, and application notes
- **GitHub SDK Repository:** https://github.com/Lanxin-MRDVS/CameraSDK/ - Camera SDK releases, sample code, and API documentation
- **Software Downloads:** https://github.com/Lanxin-MRDVS/CameraSDK/releases LxCameraViewer host configuration tool and firmware updates available via the MRDVS knowledge base portal
- **Documentation:** 待完善 Product manuals, integration guides, SDK API references, and algorithm configuration guides

### D.3 Community and Resources

- **ROS / ROS2 Support:** Open-source driver nodes available for integration with ROS and ROS2 robotics frameworks
- **Sample Code:** 待完善 C/C++, Python, and HTTP wrapper examples provided with the SDK

---

## Appendix E: Document Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | March 2026 | MRDVS Marketing Team | Draft release |
