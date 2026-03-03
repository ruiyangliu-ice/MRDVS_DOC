# V2PRO ROS2 Interface Documentation

V2PRO provides a ROS2 demo package `lx_camera_ros`. The following interface specifications and usage guidelines reference this demo program. Note that most of ROS2 interfaces correspond one-to-one with ROS1 interfaces.

---

## Table of Contents

1. [Environment Setup and Compilation](#environment-setup-and-compilation)
   - [Prerequisites](#prerequisites)
     - [1. Install Qt5](#1-install-qt5)
     - [2. Install libzip](#2-install-libzip)
     - [3. Build the ROS2 Package](#3-build-the-ros2-package)
2. [Execution](#execution)
3. [ROS2 Topic Interfaces](#ros2-topic-interfaces)
   - [Mapping Phase](#mapping-phase)
   - [Localization Phase](#localization-phase)
   - [Other Interfaces](#other-interfaces)
   - [Localization Pose Interface](#localization-pose-interface)
   - [Localization Status Interface](#localization-status-interface)
   - [Storage Status Interface](#storage-status-interface)
   - [Module Error Interface](#module-error-interface)
   - [Mapping Progress Interface](#mapping-progress-interface)
   - [Mapping State Interface](#mapping-state-interface)
   - [Mapping Enable Interface](#mapping-enable-interface)
   - [Parameter Setting Interface](#parameter-setting-interface)
   - [Map Switching Interface](#map-switching-interface)
   - [Map Download Interface](#map-download-interface)
   - [Odometry Upload Interface](#odometry-upload-interface)
   - [Relocalization Interface](#relocalization-interface)
4. [ROS2 Service Interfaces](#ros2-service-interfaces)
   - [Service Categories](#service-categories)
   - [Start Mapping](#start-mapping)
   - [Stop Mapping](#stop-mapping)
   - [Incremental Mapping](#incremental-mapping)
   - [Switch Map](#switch-map)
   - [Download Map](#download-map)
   - [Relocalization](#relocalization)
   - [Get Current Map Name](#get-current-map-name)
   - [Get Map List](#get-map-list)
   - [Delete Map](#delete-map)
   - [Download Heatmap](#download-heatmap)

---

## Environment Setup and Compilation

### Prerequisites

#### 1. Install Qt5

Ensure Qt5 is installed on your computer. The compilation requires Qt5 Core and Network modules.

```bash
# For Debian / Ubuntu / Linux Mint (APT)
sudo apt install qtbase5-dev qtbase5-dev-tools libqt5network5
```

#### 2. Install libzip

Download the source code from the [official libzip website](https://libzip.org/download/), then compile according to the official tutorial.

#### 3. Build the ROS2 Package

Navigate to the `/lx_camera_ros` directory and run:

```bash
./build.sh  # For ROS2 versions
```

---

## Execution

> **Note:** If the YAML configuration file is modified, recompilation is required for changes to take effect.

Launch the system:

```bash
./start.sh
```

---

## ROS2 Topic Interfaces

> **Prerequisites for Testing:** Power on the camera (wait 30 seconds for the internal program to start), then launch the ROS2 SDK node.

### Mapping Phase

| Interface            | Description                           |
| -------------------- | ------------------------------------- |
| **Mapping Enable**   | Start/stop mapping operations         |
| **Mapping Progress** | Receive mapping completion percentage |
| **Mapping State**    | Receive current mapping state         |

### Localization Phase

| Interface               | Description                                       |
| ----------------------- | ------------------------------------------------- |
| **Localization Pose**   | Receive real-time localization data (x, y, theta) |
| **Localization Status** | Receive localization state (true/false)           |
| **Map Switching**       | Switch between stored maps                        |
| **Relocalization**      | Set pose for relocalization                       |
| **Map Download**        | Download map files from camera                    |

### Other Interfaces

| Interface             | Description                                     |
| --------------------- | ----------------------------------------------- |
| **Storage Status**    | Monitor camera storage usage                    |
| **Module Error**      | Receive fault module names                      |
| **Parameter Setting** | Configure camera and LiDAR extrinsic parameters |
| **Odometry Upload**   | Upload wheel odometry data                      |

---

### Localization Pose Interface

**Description:** Continuously receives localization data (x, y, theta) from the camera.

**ROS Topic:** `LxCamera_LocationPoseResult`

**Data Format:**

- Position: x, y coordinates
- Orientation: Quaternion (converted from theta)
- TF Transform: TF transform via x, y coordinates and quaternion

**Example Command:**

```bash
ros2 topic echo /LxCamera_LocationPoseResult
```

---

### Localization Status Interface

**Description:** Continuously receives localization status at **1 Hz** indicating whether the system is successfully localized.

**ROS Topic:** `LxCamera_LocationResult`

**Message Type:** `std_msgs::msg::Bool`

**Example Command:**

```bash
ros2 topic echo /LxCamera_LocationResult
```

---

### Storage Status Interface

**Description:** Continuously receives storage usage percentage at **2 Hz**.

**ROS Topic:** `LxCamera_Capacity_Status`

**Message Type:** `std_msgs::msg::Float32`

**Example Command:**

```bash
ros2 topic echo /LxCamera_Capacity_Status
```

---

### Module Error Interface

**Description:** Continuously receives error module names from the camera.

**ROS Topic:** `LxCamera_Module_Error`

**Message Type:** `std_msgs::msg::String`

**Example Command:**

```bash
ros2 topic echo /LxCamera_Module_Error
```

---

### Mapping Progress Interface

**Description:** Receives mapping completion percentage after mapping is initiated.

**ROS Topic:** `LxCamera_Mapping_Progress` (Range: 0-100)

**Message Type:** `std_msgs::msg::Int32`

**Example Command:**

```bash
ros2 topic echo /LxCamera_Mapping_Progress
```

---

### Mapping State Interface

**Description:** Receives mapping state after mapping is initiated.

**ROS Topic:** `LxCamera_MappingState`

**Message Type:** `std_msgs::msg::Bool`

**Example Command:**

```bash
ros2 topic echo /LxCamera_MappingState
```

---

### Mapping Enable Interface

**ROS Topic:** `LxCamera_Mapping`

**Message Type:** `std_msgs::msg::String`

**Format:** JSON:

- `status` (bool): `true` to start mapping, `false` to stop
- `map_name` (string): Map identifier

**Behavior:**

- After sending start command: Camera web interface displays "Mapping in Progress"
- After sending stop command: Web notification disappears and map appears in map library (requires sufficient vehicle movement)

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/lx_camera_ros2_ws
source install/setup.bash

# Start mapping
ros2 topic pub -1 /LxCamera_Mapping std_msgs/msg/String "{data: '{\"status\": true, \"map_name\": \"fri\"}'}"

# Stop mapping
ros2 topic pub -1 /LxCamera_Mapping std_msgs/msg/String "{data: '{\"status\": false, \"map_name\": \"fri\"}'}"
```

---

### Parameter Setting Interface

**ROS Topic:** `LxCamera_SetParam`

**Message Type:** `std_msgs::msg::String`

**Format:** JSON with two arrays:

- `camera_pose`: [x, y, z, yaw, pitch, roll]
- `lidar_pose`: [x, y, z, yaw, pitch, roll]

**Configuration File:** Upon successful modification, updates `/home/fr1511b/hontai/params/config_install_FL21500HAF.ini` sections `[Laser1]` and `[Camera1]`.

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/lx_camera_ros2_ws
source install/setup.bash

ros2 topic pub -1 /LxCamera_SetParam std_msgs/msg/String "{data: '{\"camera_pose\": [11, 11, 10.2, 11.2, 12.2, 13.2], \"lidar_pose\": [2, 2, 2, 2, 2, 2]}'}"
```

---

### Map Switching Interface

**ROS Topic:** `LxCamera_SwitchMap`

**Message Type:** `std_msgs::msg::String`

**Format:** JSON, map name (must exist in camera map library)

**Verification:** Refresh camera web interface to confirm map change.

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/lx_camera_ros2_ws
source install/setup.bash

ros2 topic pub -1 /LxCamera_SwitchMap std_msgs/msg/String "{data: 'map_name'}"
```

---

### Map Download Interface

**ROS Topic:** `LxCamera_DownloadMap`

**Message Type:** `std_msgs::msg::String`

**Format:** JSON with two fields:

- `save_path`: Absolute path on computer
- `map_name`: Existing map name in camera (ZIP format)

**Output:** Creates a ZIP archive containing:

- `*.pgm` map file
- `*.yaml` configuration file

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/lx_camera_ros2_ws
source install/setup.bash

ros2 topic pub -1 /LxCamera_DownloadMap std_msgs/msg/String "{data: '/home/user/LANXIN\,map_name'}"
```

---

### Odometry Upload Interface

**ROS Topic:** `odom`

**Message Type:** `nav_msgs::msg::Odometry`

**Description:** Upload wheel odometry data to the camera.

---

### Relocalization Interface

**ROS Topic:** `LxCamera_UploadReloc`

**Message Type:** `geometry_msgs::msg::PoseWithCovarianceStamped`

**Data:** Laser pose containing x, y, yaw

**Verification:** relocalization successful when position updated in camera WebUI.

**Example Command:**

```bash
ros2 topic pub -1 /LxCamera_UploadReloc geometry_msgs/msg/PoseWithCovarianceStamped '{
  "header": {
    "frame_id": ""
  },
  "pose": {
    "pose": {
      "position": {
        "x": 0.0,
        "y": 0.0,
        "z": 0.0
      },
      "orientation": {
        "x": 0.0,
        "y": 0.0,
        "z": -0.694658,
        "w": 0.71934
      }
    },
    "covariance": [
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
      0.0, 0.0, 0.0, 0.0, 0.0, 0.0
    ]
  }
}'
```

> **Note:** Quaternion values must satisfy normalization constraints. Use the [Euler-Quaternion Converter](https://www.andre-gaschler.com/rotationconverter/) to convert yaw angles to valid quaternions.

---

## ROS2 Service Interfaces

> **Recommendation:** For mapping, map switching, map downloading, and relocalization operations, **service interfaces are strongly recommended** over topic interfaces.

**Usage:** Call via `ros2 service` command. Returns `true` for success, `false` for failure.

### Service Categories

| Category         | Services                                                                                            |
| ---------------- | --------------------------------------------------------------------------------------------------- |
| **Mapping**      | Start Mapping, Stop Mapping                                                                         |
| **Localization** | Relocalization, Switch Map                                                                          |
| **Utilities**    | Get Current Map Name, Download Map, Get Map List, Delete Map, Download Heatmap, Incremental Mapping |

---

### Start Mapping

**Service:** `/LxCamera_Mapping_srv`

**Service Definition:**

```yaml
bool status      # true: start mapping, false: stop mapping
string mapname   # Map name (max 30 chars, must not start with number)
---
bool result      # Operation result: true = success, false = failure
```

**Example Command:**

```bash
ros2 service call /LxCamera_Mapping_srv lx_camera_ros/srv/LxMapping "{
  status: true,
  mapname: 'test_map'
}"
```

---

### Stop Mapping

**Service Name:** `/LxCamera_ExitMapping_srv`

**Service Definition:**

```yaml
bool exit_mapping    # true to exit mapping mode
---
bool state          # Exit mapping state: true = success, false = failure
```

**Example Command:**

```bash
ros2 service call /LxCamera_ExitMapping_srv lx_camera_ros/srv/LxCameraExitMappingSrv "{
  exit_mapping: true
}"
```

---

### Incremental Mapping

**Service Name:** `/LxCamera_IncMapping_srv`

**Description:** Performs incremental mapping on existing maps with automatic merging.

**Service Definition:**

```yaml
bool status       # true: start mapping, false: stop mapping
string mapname    # Map name (max 30 chars, must not start with number)
---
bool result       # Operation success status
```

**Example Command:**

```bash
ros2 service call /LxCamera_IncMapping_srv lx_camera_ros/srv/LxCameraMappingSrv "{
  status: true,
  mapname: 'test_map'
}"
```

---

### Switch Map

**Service Name:** `/LxCamera_SwitchMap_srv`

**Service Definition:**

```yaml
string mapname    # Target map name (max 30 chars, must not start with number)
---
bool result       # Operation success status
```

**Example Command:**

```bash
ros2 service call /LxCamera_SwitchMap_srv lx_camera_ros/srv/LxSwitchMap "{
  mapname: 'target_map_name'
}"
```

---

### Download Map

**Service Name:** `/LxCamera_DownloadMap_srv`

**Service Definition:**

```yaml
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (e.g., "/home/lx/map", no trailing "/")
---
bool result        # Operation success status
```

**Example Command:**

```bash
ros2 service call /LxCamera_DownloadMap_srv lx_camera_ros/srv/LxCameraDownloadMapSrv "{
  mapname: 'target_map_name',
  save_path: '/home/fr1511b/TEST_Path'
}"
```

---

### Relocalization

**Service Name:** `/LxCamera_UploadReloc_srv`

**Service Definition:**

```yaml
geometry_msgs/PoseWithCovarianceStamped pose
---
bool result        # Operation success status
```

**Example Command:**

```bash
ros2 service call /LxCamera_UploadReloc_srv lx_camera_ros/srv/LxUploadReloc "{
  pose: {
    header: {
      stamp: {
        sec: 0,
        nanosec: 0
      },
      frame_id: 'map'
    },
    pose: {
      pose: {
        position: {
          x: 4.5,
          y: -15.5,
          z: 0.0
        },
        orientation: {
          x: 0.0,
          y: 0.0,
          z: 0.694658,
          w: 0.71934
        }
      },
      covariance: [
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0
      ]
    }
  }
}"
```

---

### Get Current Map Name

**Service Name:** `/LxCamera_GetCurMapname_srv`

**Service Definition:**

```yaml
bool get_cur_mapname    # Trigger flag (set true)
---
string map_name        # Current active map name
```

**Example Command:**

```bash
ros2 service call /LxCamera_GetCurMapname_srv lx_camera_ros/srv/LxGetmapname "{
  get_cur_mapname: true
}"
```

---

### Get Map List

**Service Name:** `/LxCamera_GetMapList_srv`

**Service Definition:**

```yaml
bool get_map_list    # Trigger flag (set true)
---
string[] map_list   # Array of available map names
```

**Example Command:**

```bash
ros2 service call /LxCamera_GetMapList_srv lx_camera_ros/srv/LxGetmaplist "{
  get_map_list: true
}"
```

---

### Delete Map

**Service Name:** `/LxCamera_EraseMap_srv`

**Service Definition:**

```yaml
string map_name    # Map name to delete (max 30 chars, must not start with number)
---
bool result       # Deletion success status
```

**Example Command:**

```bash
ros2 service call /LxCamera_EraseMap_srv lx_camera_ros/srv/LxCameraEraseMapSrv "{
  map_name: 'test_map_name'
}"
```

---

### Download Heatmap

**Service Name:** `/LxCamera_DownloadHeatMap_srv`

**Service Definition:**

```yaml
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (no trailing "/")
---
bool result        # Operation success status
```

**Example Call:**

```bash
ros2 service call /LxCamera_DownloadHeatMap_srv lx_camera_ros/srv/LxCameraDownloadMapSrv "{
  map_name: 'test_map_name',
  save_path: '/home/fr1511b/TEST_Path'
}"
```
