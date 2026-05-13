<!-- MRDVS GitHub Organization README Template -->
<!-- Image placeholders: Replace with actual company assets -->

<p align="center">
  <!-- Light mode banner -->
  <img src="https://s7.ezgif.com/tmp/ezgif-7e88b310008d069d.webp" alt="MRDVS - 3D Vision for Intelligent Robotics" width="80%"/>
  <!-- Dark mode banner -->
  <img src="https://s7.ezgif.com/tmp/ezgif-7e88b310008d069d.webp" alt="MRDVS - 3D Vision for Intelligent Robotics" width="80%"/>
  <br><br>
</p>

<h1 align="center">MRDVS SDK</h1>

<p align="center">
  <b>3D Vision SDK for Mobile Robots</b>
</p>

<p align="center">
  <a href="https://github.com/mrdvs/mrdvs-sdk/releases/latest">
    <img src="https://img.shields.io/github/v/release/mrdvs/mrdvs-sdk?sort=semver&style=flat-square&color=brightgreen" alt="Latest Release">
  </a>
  <a href="https://github.com/mrdvs/mrdvs-sdk/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/mrdvs/mrdvs-sdk?style=flat-square&color=green" alt="License">
  </a>
  <a href="https://github.com/mrdvs/mrdvs-sdk/issues">
    <img src="https://img.shields.io/github/issues/mrdvs/mrdvs-sdk?style=flat-square&color=orange" alt="Issues">
  </a>
  <a href="https://github.com/mrdvs/mrdvs-sdk/stargazers">
    <img src="https://img.shields.io/github/stars/mrdvs/mrdvs-sdk?style=flat-square&color=yellow" alt="Stars">
  </a>
  <br>
  <a href="https://www.mrdvs.com">
    <img src="https://img.shields.io/badge/Website-mrdvs.com-0066CC?style=flat-square&logo=google-chrome&logoColor=white" alt="Website">
  </a>
  <a href="https://docs.mrdvs.com">
    <img src="https://img.shields.io/badge/Docs-docs.mrdvs.com-2D8C3F?style=flat-square&logo=read-the-docs&logoColor=white" alt="Documentation">
  </a>
</p>

---

<p align="center">
  <a href="https://www.mrdvs.com">Website</a> ·
  <a href="https://docs.mrdvs.com">Docs</a> ·
  <a href="https://github.com/mrdvs/mrdvs-sdk/releases">Releases</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#community">Community</a>
</p>

---

## What's New

**MRDVS SDK v2.5 is now available.**

This release introduces Semantic SLAM 3.0 with 40% improved accuracy in low-texture environments, support for the S-Series Pro camera (up to 18m obstacle detection range), zero-copy intra-process communication for ROS 2, and multi-target pallet detection with mixed-scene adaptation for the H-Series. See the [release notes](https://github.com/mrdvs/mrdvs-sdk/releases) for details.

---

## About MRDVS

<p align="center">
  <img src="doc/img/logo.png" alt="MRDVS Logo" width="20%"/>
</p>

**MRDVS (Zhejiang Meirweishi Technology Co., Ltd.)** develops 3D vision sensors and AI-powered perception solutions for mobile robots. Since 2016, we have shipped tens of thousands of 3D vision sensors and established partnerships with leading mobile robotics companies worldwide.

Our mission is to enable robots to perceive, understand, and navigate the world with human-like 3D vision.

---

## Why MRDVS

| Feature | Capability |
|:---|:---|
| **Field of View** | Up to 270 x 70 (H x V) |
| **Depth Resolution** | Up to 1608 x 280 |
| **Frame Rate** | >15 fps depth + RGB |
| **Maximum Range** | 50m with +/-3cm accuracy |
| **Onboard AI** | Up to 6.0 TOPS, no external host required |
| **Multi-Modal Fusion** | Spatially aligned RGB + point cloud with semantic segmentation |
| **Ingress Protection** | IP65/IP67, -20 C to 60 C operating temperature |

---

## Product Lineup

### S-Series -- Visual Obstacle Detection

<p align="center">
  <img src="doc/img/product-s-series.png" alt="S-Series Camera" width="50%"/>
</p>

Real-time 3D environmental perception with intelligent obstacle classification and dynamic collision avoidance.

| Specification | Value |
|:---|:---|
| Detection Range | Up to 18m |
| Customizable 3D avoidance zones | Yes |
| Semantic obstacle classification | Yes (person, forklift, pallet, etc.) |
| Communication Interfaces | I/O, Ethernet, USB 3.0 |
| Representative Models | S-510, S-510 Pro |

**Applications:** AMR/AGV obstacle avoidance, forklift collision prevention, human safety zones, fork tip protection

---

### M-Series -- Visual Docking & Measurement

<p align="center">
  <img src="doc/img/product-m-series.png" alt="M-Series Camera" width="50%"/>
</p>

High-precision depth sensing for automated docking, dimensioning, and warehouse operations.

| Specification | Value |
|:---|:---|
| Depth Range | 0.2m - 5m |
| Frame Rate | 15-25 fps |
| Measurement Accuracy | Millimeter-level |
| Built-in Algorithms | Pallet detection, volume measurement, bin occupancy check |

**Applications:** Pallet & cage docking, carton dimensioning, bin occupancy detection, rack safety monitoring

---

### V-Series -- Visual Navigation

<p align="center">
  <img src="doc/img/product-v-series.png" alt="V-Series Camera" width="50%"/>
</p>

Ceiling-facing semantic SLAM for infrastructure-free robot navigation in large-scale deployments.

| Specification | Value |
|:---|:---|
| Navigation Method | Ceiling feature recognition + IMU fusion |
| Working Range | 6-12m ceiling height |
| Onboard Compute | 6.0 TOPS (no external PC required) |
| Feature Detection Range | Up to 12m stable ceiling feature detection |

**Applications:** AMR/AGV SLAM navigation, large-scale fleet coordination, high-dynamic factory environments

**Deployment Highlight:** A photovoltaic manufacturing facility operates 500+ AGVs across 80,000 m using the V-Series visual positioning module with zero downtime since deployment.

---

### H-Series -- Visual Picking & Recognition

<p align="center">
  <img src="doc/img/product-h-series.png" alt="H-Series Camera" width="50%"/>
</p>

High-precision recognition camera for robotic grasping and intelligent sorting.

| Specification | Value |
|:---|:---|
| Recognition Accuracy | +/-0.1mm |
| Onboard Compute | 6.0 TOPS |
| Working Distance | 300-600mm |
| Weight | 400g |
| Representative Model | H3310 |

**Applications:** Precision grasping positioning, depalletizing, bin picking, produce recognition, intelligent sorting

---

## Core Technologies

```
+------------------+ +------------------+ +------------------+ +------------------+
|  Depth Sensing   | |   AI Semantic    | | Visual Navigation | |  Vision-based    |
|                  | |     Engine       | |                  | |   Measurement    |
+------------------+ +------------------+ +------------------+ +------------------+
| - Stereo vision  | | - Semantic       | | - Semantic SLAM  | | - Pallet         |
| - iToF           | |   segmentation   | | - Ceiling        | |   detection      |
| - RGB-D fusion   | | - Semantic       | |   navigation     | | - Volume         |
| - Point cloud    | |   recognition    | | - IMU fusion     | |   measurement    |
|   generation     | | - Human/obstacle | | - Multi-robot    | | - Bin occupancy  |
|                  | |   detection      | |   coordination   | |   detection      |
+------------------+ +------------------+ +------------------+ +------------------+
```

---

## Quick Start

### Install the SDK

**Linux (Ubuntu)**
```bash
wget https://github.com/mrdvs/mrdvs-sdk/releases/download/v2.5.0/mrdvs-sdk-2.5.0-linux.run
chmod +x mrdvs-sdk-2.5.0-linux.run
sudo ./mrdvs-sdk-2.5.0-linux.run
```

**Windows**
```powershell
# Download and run the installer from:
# https://github.com/mrdvs/mrdvs-sdk/releases
```

**NVIDIA Jetson**
```bash
wget https://github.com/mrdvs/mrdvs-sdk/releases/download/v2.5.0/mrdvs-sdk-2.5.0-jetson.run
sudo ./mrdvs-sdk-2.5.0-jetson.run
```

### Run Your First Program

```bash
git clone https://github.com/mrdvs/mrdvs-samples.git
cd mrdvs-samples
mkdir build && cd build
cmake .. && make -j4
./samples/pointcloud_viewer
```

### C++ Example

```cpp
#include <mrdvs/mrdvs.hpp>
#include <iostream>

int main() {
    mrdvs::Device device;
    device.connect(mrdvs::DeviceType::S510);

    mrdvs::StreamConfig config;
    config.depth_resolution = {640, 480};
    config.fps = 15;
    device.configure(config);

    device.start();

    while (true) {
        auto frames = device.capture();
        auto depth = frames.get_depth_frame();

        float dist = depth.get_distance(depth.width() / 2, depth.height() / 2);
        std::cout << "Distance to center pixel: " << dist << " m\r" << std::flush;
    }

    device.stop();
    return 0;
}
```

### Python Example

```python
import mrdvs

device = mrdvs.Device()
device.connect(mrdvs.DeviceType.S510)

config = mrdvs.StreamConfig()
config.depth_resolution = (640, 480)
config.fps = 15
device.configure(config)

device.start()

try:
    while True:
        frames = device.capture()
        depth = frames.get_depth_frame()
        dist = depth.get_distance(depth.width // 2, depth.height // 2)
        print(f"Distance to center pixel: {dist:.3f} m", end="\r")
finally:
    device.stop()
```

---

## Platform Support

### Operating Systems

| Platform | Versions | Status |
|:---:|:---|:---:|
| <img src="https://img.shields.io/badge/Ubuntu-22.04%2F20.04-E95420?logo=ubuntu&logoColor=white" height="24"/> | 22.04 LTS / 20.04 LTS | Supported |
| <img src="https://img.shields.io/badge/Windows-10%2F11-0078D4?logo=windows&logoColor=white" height="24"/> | 10 / 11 | Supported |
| <img src="https://img.shields.io/badge/NVIDIA_Jetson-JetPack%205%2F6-76B900?logo=nvidia&logoColor=white" height="24"/> | Nano / TX2 / Xavier / Orin | Supported |

### Language Bindings

| Language | Status | API Docs |
|:---:|:---:|:---:|
| <img src="https://img.shields.io/badge/C++-00599C?logo=c%2B%2B&logoColor=white" height="24"/> | Native | [C++ API](https://docs.mrdvs.com/api/cpp) |
| <img src="https://img.shields.io/badge/Python-3.8+-3776AB?logo=python&logoColor=white" height="24"/> | Full binding | [Python API](https://docs.mrdvs.com/api/python) |
| <img src="https://img.shields.io/badge/ROS_2-Humble%2FIron-22314E?logo=ros&logoColor=white" height="24"/> | Official driver | [ROS 2 Docs](https://docs.mrdvs.com/ros2) |
| <img src="https://img.shields.io/badge/C%23-512BD4?logo=csharp&logoColor=white" height="24"/> | Planned | - |

### Third-Party Integrations

| Platform | Integration | Docs |
|:---|:---|:---|
| NVIDIA Isaac | Certified sensor | [Guide](doc/integration_isaac.md) |
| OpenCV | Built-in interface | [Guide](doc/integration_opencv.md) |
| PCL (Point Cloud Library) | Point cloud format compatible | [Samples](samples/pcl) |
| Unity | SDK plugin | [Guide](doc/integration_unity.md) |
| Halcon | GenTL interface | [Guide](doc/integration_halcon.md) |

---

## Resources

### SDK & Tools

| Resource | Description |
|:---|:---|
| [MRDVS SDK](https://github.com/mrdvs/mrdvs-sdk/releases) | Core SDK (C++ / Python) |
| [MRDVS ROS2](https://github.com/mrdvs/mrdvs-ros2) | Official ROS 2 driver |
| [MRDVS Tools](https://github.com/mrdvs/mrdvs-tools) | Camera config & firmware update tools |

### Documentation

| Resource | Link |
|:---|:---|
| Online Documentation | [docs.mrdvs.com](https://docs.mrdvs.com) |
| C++ API Reference | [docs.mrdvs.com/api/cpp](https://docs.mrdvs.com/api/cpp) |
| Python API Reference | [docs.mrdvs.com/api/python](https://docs.mrdvs.com/api/python) |
| Product Datasheets | [www.mrdvs.com/downloads](https://www.mrdvs.com/downloads) |

### Code Samples

| Sample | Languages | Description |
|:---|:---:|:---|
| [Basic Capture](samples/basic_capture) | C++ / Python | Connect camera and stream depth/RGB |
| [Point Cloud Viewer](samples/pointcloud_viewer) | C++ | Real-time 3D point cloud visualization |
| [Obstacle Avoidance](samples/obstacle_avoidance) | C++ / Python | Dynamic avoidance zone configuration |
| [Pallet Detection](samples/pallet_detection) | Python | M-Series pallet detection and pose estimation |
| [SLAM Navigation](samples/slam_navigation) | Python / ROS 2 | V-Series semantic SLAM mapping |
| [Multi-Camera Sync](samples/multi_camera) | C++ | Multi-camera synchronized capture |
| [Precision Grasping](samples/precise_grasping) | Python | H-Series pick-and-place positioning |
| [ROS 2 Navigation](samples/ros2_navigation) | ROS 2 | Complete ROS 2 navigation integration |

---

## Use Cases

| Application | Description | Product |
|:---|:---|:---:|
| Obstacle Avoidance | Real-time 3D perception with intelligent obstacle classification | S-Series |
| SLAM Navigation | Ceiling-facing semantic SLAM for large-scale fleet navigation | V-Series |
| Automated Docking | Automatic pallet pose estimation for precision docking | M-Series |
| Dimensioning | Single-camera parcel volume measurement | M-Series |
| Bin Occupancy Detection | 3D bin state detection, no training required | M-Series |
| Precision Picking | +/-0.1mm accuracy positioning for robotic grasping | H-Series |
| Safety Monitoring | Human detection and dynamic obstacle protection | S-Series |
| Rack Safety | Pallet skew/tilt/deformation detection | M-Series |

---

## Specifications

| Specification | S-Series | M-Series | V-Series | H-Series |
|:---|:---|:---|:---|:---|
| **Primary Function** | Obstacle Detection | Docking & Measurement | Navigation | Precision Picking |
| **Depth Technology** | Stereo / iToF | iToF / Stereo | Stereo + RGB-D | iToF |
| **Depth Resolution** | 640x480 - 1280x720 | 640x480 | 640x480 | 1280x1024 |
| **Depth Frame Rate** | 15-30 fps | 15-25 fps | 15 fps | 15 fps |
| **Field of View (H x V)** | 90x70 - 270x70 | 87x67 | 60x45 | 57x45 |
| **Operating Range** | 0.3m - 18m | 0.2m - 5m | 6m - 12m (ceiling) | 0.3m - 0.6m |
| **Depth Accuracy** | +/-1% @ 1m | +/-3mm @ 1m | +/-2cm @ 10m | +/-0.1mm |
| **Onboard Compute** | - | - | 6.0 TOPS | 6.0 TOPS |
| **Ingress Protection** | IP65 / IP67 | IP65 | IP54 | IP65 |
| **Operating Temperature** | -20 C to 60 C | -20 C to 60 C | -10 C to 50 C | 0 C to 50 C |
| **Interface** | Ethernet / USB 3.0 | Ethernet / USB 3.0 | Ethernet | Ethernet / USB 3.0 |

See [product datasheets](https://www.mrdvs.com/downloads) for detailed specifications.

---

## Community

### Support Channels

| Channel | Description | Link |
|:---|:---|:---|
| Documentation | Product and development docs | [docs.mrdvs.com](https://docs.mrdvs.com) |
| GitHub Issues | Bug reports and feature requests | [New Issue](https://github.com/mrdvs/mrdvs-sdk/issues/new/choose) |
| Discussion Forum | Community Q&A and knowledge sharing | [Discussions](https://github.com/orgs/mrdvs/discussions) |
| Email | Direct support contact | support@mrdvs.com |

### Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

---

<p align="center">
  <b>Enabling robots to see and understand the world.</b>
</p>

<p align="center">
  <a href="https://www.mrdvs.com" target="_blank">www.mrdvs.com</a> ·
  <a href="mailto:support@mrdvs.com" target="_blank">support@mrdvs.com</a>
</p>

<p align="center">
  <a href="https://github.com/mrdvs" target="_blank"><img src="https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.linkedin.com/company/mrdvs" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.youtube.com/@mrdvs" target="_blank"><img src="https://img.shields.io/badge/YouTube-FF0000?logo=youtube&logoColor=white" height="28"/></a>
</p>

---

<p align="center">
  <sub>Copyright 2024-2025 Zhejiang Meirweishi Technology Co., Ltd. (MRDVS). All Rights Reserved.</sub>
</p>
