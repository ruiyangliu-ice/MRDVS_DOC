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

## Resources

### SDK & Tools

| Resource | Description |
|:---|:---|
| [MRDVS LxCameraViewer](https://github.com/Lanxin-MRDVS/CameraSDK/releases) | Camera viewer software |

### Documentation

| Resource | Link |
|:---|:---|
| LxCameraSDK C/C++ Developer Guide | [LxCameraSDK C/C++ Developer Guide](https://github.com/Lanxin-MRDVS/CameraSDK/blob/master/Document_EN/MRDVS_LxCameraSDK_C-Cpp_DeveloperGuide_V1.0_20260604.pdf) |
| LxCameraSDK Python Developer Guide | [LxCameraSDK Python Developer Guide](https://github.com/Lanxin-MRDVS/CameraSDK/blob/master/Document_EN/LxCameraSDK-Python%20User%20Manual_EN.PDF) |
| LxCameraViewer User Manual | [LxCameraViewer User Manual](https://github.com/Lanxin-MRDVS/CameraSDK/blob/master/Document_EN/LxCameraViewer%20User%20Manual.pdf) |
| Product Datasheets | [Product Datasheets](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Datasheets) |

---

## Platform Support

### Operating Systems

| Platform | Versions | Status |
|:---|:---|:---:|
| <img src="https://img.shields.io/badge/Ubuntu-26.04%2F22.04-E95420?logo=ubuntu&logoColor=white" height="24"/> | 26.04 LTS / 24.04 LTS | Supported |
| <img src="https://img.shields.io/badge/Windows-10%2F11-0078D4?logo=windows&logoColor=white" height="24"/> | 10 / 11 | Supported |

### Language Bindings

| Language | Status | API Docs |
|:---|:---:|:---:|
| <img src="https://img.shields.io/badge/C++-00599C?logo=c%2B%2B&logoColor=white" height="24"/> | Native | [C++ API](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| <img src="https://img.shields.io/badge/Python-3.8+-3776AB?logo=python&logoColor=white" height="24"/> | Full binding | [Python API](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |
| <img src="https://img.shields.io/badge/ROS_2-22314E?logo=ros&logoColor=white" height="24"/> | Official driver | [ROS 2 Docs](https://github.com/Lanxin-MRDVS/CameraSDK/tree/master/Document) |

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

## Use Cases

| ![Mobile Robots](https://github.com/user-attachments/assets/62547909-2d18-4b5c-9a1a-25662141bd1c) | ![Logistics & Warehousing Automation](https://github.com/user-attachments/assets/0d36c964-382c-4949-a36c-4dc960aef995) | ![Lawnmowers](https://github.com/user-attachments/assets/027f1e02-bf6e-41d8-b69b-ae14caac57db) |
|:---:|:---:|:---:|
| Mobile Robots | Logistics & Warehousing Automation | Lawnmowers |
| ![Low-Speed Unmanned Vehicles](https://github.com/user-attachments/assets/2e102944-b175-44d0-93e7-aee0a0f87768) | ![Embodied AI](https://github.com/user-attachments/assets/60d97b2a-26a6-4b91-9622-8343a9b28b5c) | ![Drones](https://github.com/user-attachments/assets/ba9f2b9d-48b3-4198-a68a-cee14666e043) |
| Low-Speed Unmanned Vehicles | Embodied AI | Drones |

---

## About MRDVS
<p align="center">
  <img width="50%" height="50%" alt="MRDVS logo" src="https://github.com/user-attachments/assets/3a36a743-86e1-4bac-98d4-7f1c70a955d4" />
</p>

**MRDVS** focus on industry‑leading 3D vision mobile robot company in Chinese mainland, with large‑scale deployment in industrial applications. We are committed to empowering mobile robots through 3D vision perception technology and delivering value to end customers with industrial mobile robots as the deployment platform, thereby achieving an end-to-end closed loop from AI to the physical world.

<p align="center">
  <b>Empowering robots to comprehend the world</b>
</p>

<p align="center">
  <a href="https://mrdvs.com" target="_blank">mrdvs.com</a> ·
  <a href="mailto:service@mrdvs.com" target="_blank">service@mrdvs.com</a>
</p>

<p align="center">
  <a href="https://github.com/Lanxin-MRDVS/" target="_blank"><img src="https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.linkedin.com/company/mrdvs" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white" height="28"/></a>
  &nbsp;
  <a href="https://www.youtube.com/@MRDVS-2024" target="_blank"><img src="https://img.shields.io/badge/YouTube-FF0000?logo=youtube&logoColor=white" height="28"/></a>
</p>
