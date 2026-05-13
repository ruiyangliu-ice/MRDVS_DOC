<p align="center">
<img width="1037" height="360" alt="banner" src="https://github.com/user-attachments/assets/8318add1-cb50-47f3-9bb0-cdbe53403457" /><p align="center">
</p>

<h1 align="center">MRDVS - Mobile Robot Vision Expert</h1>

<p align="center">
  <b>Empowering robots to comprehend the world</b>
</p>
<p align="center">
  <a href="https://www.mrdvs.com">Website</a> ·
  <a href="https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document">Docs</a> ·
  <a href="https://github.com/Lanxin-MRDVS/CameraSDK/releases">Releases</a>
</p>

---

## What's New

**MRDVS SDK v2.4.60 is now available.**

MRDVS SDK v2.4.60 is now available with breaking compatibility changes (installation path moved to `MRDVS`, `LX_E_NOT_SUPPORT` renamed to `LX_W_NOT_SUPPORT`, and `GetDeviceList` header fix), plus new fisheye intrinsics, full IMU and radar (XYZIRT) support, sparse RGB-D alignment, packet-loss frame filling, and split 3D/2D frame getters. This release also fixes memory leaks and point cloud exceptions, optimizes PTP sync to ±1ms, expands GPU acceleration, adds rolling logs, and ships updated firmware for S10, V2 Pro, and M-Series cameras. See the [release notes](https://github.com/Lanxin-MRDVS/CameraSDK/releases/tag/SDK-V2.4.60-SP) for details.

---

## About MRDVS
<p align="center">
  <img width="50%" height="50%" alt="MRDVS logo" src="https://github.com/user-attachments/assets/3a36a743-86e1-4bac-98d4-7f1c70a955d4" />
</p>

**MRDVS** focus on industry‑leading 3D vision mobile robot company in Chinese mainland, with large‑scale deployment in industrial applications. We are committed to empowering mobile robots through 3D vision perception technology and delivering value to end customers with industrial mobile robots as the deployment platform, thereby achieving an end-to-end closed loop from AI to the physical world.

---

## Why MRDVS

**We ship perception hardware that actually ships.**  
MRDVS is the vision division of Lanxin Robotics. Since 2016 we have deployed tens of thousands of 3D sensors in production environments—from high-bay warehouses to outdoor AGV yards.

| What you get | Why it matters |
|:---|:---|
| **All-in-One Perception SDK** | Depth streaming, obstacle avoidance, and pallet recognition. |
| **Edge-First Design** | Up to 6.0 TOPS onboard compute. Many workloads (navigation, detection, measurement) run on the camera itself—no external host PC required. |
| **Hardened for Industry** | IP54/IP67, -20 °C to 60 °C operating range. Built for warehouses with high-bay racking, steel structures, and dynamic floor layouts. |
| **Production-Ready Algorithms** | Pallet detection, volume measurement, bin occupancy, and people counting ship as built-in modules, not research demos. |
| **C++ / Python / ROS 2** | Full language bindings with consistent APIs. |

---

## Product Lineup

### S-Series -- dToF Visual Obstacle Detection
<p align="left">
  <img width="50%" height="50%" alt="S" src="https://github.com/user-attachments/assets/60f229c2-181e-496f-891f-b56ff06e97bd" />
</p>

Real-time 3D environmental perception with intelligent obstacle classification and dynamic collision avoidance.

| Specification | Value |
|:---|:---|
| Detection Range | Up to 18m |
| Customizable 3D avoidance zones | Yes |
| Semantic obstacle classification | Yes (person, forklift, pallet, etc.) |
| Communication Interfaces | I/O, Ethernet, USB 3.0 |
| Representative Models | S 10, S 10 Pro, S10 Ultra |

**Applications:** AMR/AGV obstacle avoidance, forklift collision prevention, human safety zones, volume measurement

---

### M-Series -- iToF Visual Docking & Measurement

<p align="left">
  <img width="50%" height="50%" alt="M-Series" src="https://github.com/user-attachments/assets/cf9dd7de-5f99-4013-9532-937a44f385e0" />
</p>

High-fidelity volumetric sensing for precise short-range object recognition and interaction.

| Specification | Value |
|:---|:---|
| Depth Range | 0.2m - 5m |
| Frame Rate | 15-25 fps |
| Measurement Accuracy | Millimeter-level |
| Built-in Algorithms | Pallet detection, volume measurement, bin occupancy check |

**Applications:** Pallet & cage docking, carton dimensioning, bin occupancy detection, rack safety monitoring

---

### V-Series -- Fusion-SLAM RTLS

<p align="left">
  <img width="50%" height="50%" alt="v" src="https://github.com/user-attachments/assets/cf8a6c07-10cc-49c9-888c-a24f1a990b81" />
</p>

Integrated Spatial Intelligence Module Fusing Perception & RTLS for AGVs and Forklifts.

| Specification | Value |
|:---|:---|
| Navigation Method | Ceiling feature recognition + IMU fusion |
| Working Range | 6-12m ceiling height |
| Onboard Compute | 6.0 TOPS (no external PC required) |
| Feature Detection Range | Up to 12m stable ceiling feature detection |

**Applications:** AMR/AGV SLAM navigation, large-scale fleet coordination, high-dynamic factory environments

---

## Quick Start

### Install the SDK

**Linux (Ubuntu)**
```bash
wget https://github.com/Lanxin-MRDVS/CameraSDK待补充
chmod +x 包名
sudo ./source install.sh
```

**Windows**
```powershell
# Download and run the installer from:
# https://github.com/Lanxin-MRDVS/CameraSDK待补充
```

### Run Your First Program

```bash
demo
```

### C++ Example

```cpp
#include <librealsense2/rs.hpp>
#include <iostream>

int main() {
    rs2::pipeline p;                 // Top-level API for streaming & processing frames
    p.start();                       // Configure and start the pipeline

    while (true) {
        rs2::frameset frames = p.wait_for_frames();        // Block until frames arrive
        rs2::depth_frame depth = frames.get_depth_frame(); // Get depth frame
        if (!depth) continue;

        int w = depth.get_width(), h = depth.get_height();
        float dist = depth.get_distance(w/2, h/2);         // Distance to center pixel
        std::cout << "The camera is facing an object " << dist << " meters away\r";
    }
}
```

### Python Example

```python
import pyrealsense2 as rs

pipeline = rs.pipeline() # Create a pipeline
pipeline.start() # Start streaming

try:
    while True:
        frames = pipeline.wait_for_frames()
        depth_frame = frames.get_depth_frame()
        if not depth_frame:
            continue

        width, height = depth_frame.get_width(), depth_frame.get_height()
        dist = depth_frame.get_distance(width // 2, height // 2)
        print(f"The camera is facing an object {dist:.3f} meters away", end="\r")

finally:
    pipeline.stop() # Stop streaming
```

---

## Platform Support

### Operating Systems

| Platform | Versions | Status |
|:---:|:---|:---:|
| <img src="https://img.shields.io/badge/Ubuntu-26.04%2F22.04-E95420?logo=ubuntu&logoColor=white" height="24"/> | 26.04 LTS / 24.04 LTS | Supported |
| <img src="https://img.shields.io/badge/Windows-10%2F11-0078D4?logo=windows&logoColor=white" height="24"/> | 10 / 11 | Supported |

### Language Bindings

| Language | Status | API Docs |
|:---:|:---:|:---:|
| <img src="https://img.shields.io/badge/C++-00599C?logo=c%2B%2B&logoColor=white" height="24"/> | Native | [C++ API](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| <img src="https://img.shields.io/badge/Python-3.8+-3776AB?logo=python&logoColor=white" height="24"/> | Full binding | [Python API](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| <img src="https://img.shields.io/badge/ROS_2-Humble%2FIron-22314E?logo=ros&logoColor=white" height="24"/> | Official driver | [ROS 2 Docs](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |

---

## Resources

### SDK & Tools

| Resource | Description |
|:---|:---|
| [MRDVS SDK](https://github.com/Lanxin-MRDVS/CameraSDK/releases) | Core SDK (C++ / Python) |
| [MRDVS ROS2](https://github.com/Lanxin-MRDVS/CameraSDK/releases) | Official ROS 2 driver |
| [MRDVS Tools](https://github.com/Lanxin-MRDVS/CameraSDK/releases) | Camera config & firmware update tools |

### Documentation

| Resource | Link |
|:---|:---|
| Online Documentation | [文档文件夹](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| C++ API Reference | [API文件夹](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| Python API Reference | [Python示例代码](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| Product Datasheets | [产品资料](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |

### Code Samples

| Sample | Languages | Description |
|:---|:---:|:---|
| [C++](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) | C++ / Python | 应用描述 |
| [C++](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) | C++ / Python | 应用描述 |
| [C++](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) | C++ / Python | 应用描述 |
| [C++](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) | C++ / Python | 应用描述 |
| [C++](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) | C++ / Python | 应用描述 |

---

## Use Cases

| Application | Description | Product |
|:---|:---|:---:|
| Obstacle Avoidance | Real-time 3D perception with intelligent obstacle classification | S-Series |
| SLAM Navigation | Ceiling-facing semantic SLAM for large-scale fleet navigation | V-Series |
| Automated Docking | Automatic pallet pose estimation for precision docking | M-Series |
| Dimensioning | Single-camera parcel volume measurement | M & S Series |
| Bin Occupancy Detection | 3D bin state detection, no training required | M-Series |
| Safety Monitoring | Human detection and dynamic obstacle protection | S-Series |
| Rack Safety | Pallet skew/tilt/deformation detection | M-Series |

---

## Specifications

| Specification | S-Series | M-Series | V-Series | H-Series |
|:---|:---|:---|:---|:---|
| **按需添加** | 按需添加 | 按需添加 | 按需添加 | 按需添加 |

See [product datasheets](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) for detailed specifications.

---

<p align="center">
  <b>Empowering robots to comprehend the world</b>
</p>

<p align="center">
  <a href="https://mrdvs.com" target="_blank">mrdvs.com</a> ·
  <a href="mailto:service@mrdvs.com" target="_blank">service@mrdvs.com</a>
</p>

<p align="center">
  <a href="https://github.com/mrdvs" target="_blank"><img src="https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.linkedin.com/company/mrdvs" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.youtube.com/@MRDVS-2024" target="_blank"><img src="https://img.shields.io/badge/YouTube-FF0000?logo=youtube&logoColor=white" height="28"/></a>
</p>

---

<p align="center">
  <sub>Copyright 2024-2025 Zhejiang Meirweishi Technology Co., Ltd. (MRDVS). All Rights Reserved.</sub>
</p>
