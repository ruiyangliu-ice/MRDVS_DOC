# V2PRO ROS1 Interface Documentation

V2PRO provides a ROS1 demo package `lx_camera_ros`. The following interface specifications and usage guidelines reference this demo program.

---

## Table of Contents

1. [Compilation](#compilation)
   - [Prerequisites](#prerequisites)
     - [Install Qt5](#1-install-qt5)
     - [Install libzip](#2-install-libzip)
     - [Build the ROS Package](#3-build-the-ros-package)
2. [Execution](#execution)
   - [ROS Node Setup](#ros-node-setup)
3. [ROS Topic Interfaces](#ros-topic-interfaces)
   - [Overview](#mapping-phase)
   - [Published Topics](#localization-pose-interface)
     - [Localization Pose Interface](#localization-pose-interface)
     - [Localization Status Interface](#localization-status-interface)
     - [Storage Status Interface](#storage-status-interface)
     - [Module Error Interface](#module-error-interface)
     - [Mapping Status Interface](#mapping-status-interface)
     - [Mapping Progress Interface](#mapping-progress-interface)
   - [Subscribed Topics](#mapping-enable-interface-topic-based)
     - [Mapping Enable Interface (Topic-based)](#mapping-enable-interface-topic-based)
     - [Parameter Setting Interface](#parameter-setting-interface)
     - [Map Switching Interface](#map-switching-interface)
     - [Contour Map Download Interface](#contour-map-download-interface)
     - [Map Upload Interface (ZIP)](#map-upload-interface-zip)
     - [Relocalization Interface](#relocalization-interface)
     - [Odometry Upload Interface](#odometry-upload-interface)
4. [ROS Service Interfaces](#ros-service-interfaces)
   - [Start Mapping](#start-mapping)
   - [Stop Mapping](#stop-mapping)
   - [Incremental Mapping](#incremental-mapping)
   - [Switch Map](#switch-map)
   - [Download Map](#download-map)
   - [Relocalization](#relocalization-1)
   - [Get Current Map Name](#get-current-map-name)
   - [Get Map List](#get-map-list)
   - [Delete Map](#delete-map)
   - [Download Heatmap](#download-heatmap)
5. [Document Information](#document-information)

---

## Compilation

### Prerequisites

#### 1. Install Qt5

Ensure Qt5 is installed on your host machine. The compilation requires Qt5 Core and Network modules.

```bash
# Installation example (may vary by Linux distribution):
sudo apt install qtbase5-dev qtbase5-dev-tools libqt5network5
```

#### 2. Install libzip

Download the source code from the [official libzip website](https://libzip.org/download/), then compile according to the official tutorial.

#### 3. Build the ROS Package

Navigate to the `/V2-ROS-SDK-service` directory (same level as `/src`) and run:

```bash
catkin_make
```

---

## Execution

> **Important:** Before running the ROS demo, disable any ROS master/slave configurations deployed on the camera to prevent communication conflicts. If the camera has a ROS environment, wait approximately 20 seconds after power-on before running the ROS demo program.

### ROS Node Setup

1. Navigate to the demo program directory: `~/V2_ROS_SDK` (same level as the `src` folder)
2. Build the package:
   ```bash
   catkin_make
   ```
3. Source the setup file:
   ```bash
   source devel/setup.bash
   ```

**Prerequisites for Testing:** Power on the camera (wait 30 seconds for the internal program to start), then launch the ROS SDK node (see above).

---

## ROS Topic Interfaces

### Mapping Phase

| Interface            | Description                           |
| -------------------- | ------------------------------------- |
| **Mapping Enable**   | Start/stop mapping operations         |
| **Mapping Status**   | Receive current mapping state         |
| **Mapping Progress** | Receive mapping completion percentage |

### Localization Phase

| Interface               | Description                             |
| ----------------------- | --------------------------------------- |
| **Relocalization**      | Upload initial pose for relocalization  |
| **Localization Status** | Receive localization state (true/false) |
| **Map Switching**       | Switch between stored maps              |

### Configuration, Exceptions, and Other Interfaces

| Interface                | Description                                     |
| ------------------------ | ----------------------------------------------- |
| **Odometry Upload**      | Upload wheel odometry data                      |
| **Parameter Setting**    | Configure camera and LiDAR extrinsic parameters |
| **Storage Status**       | Monitor camera storage usage                    |
| **Module Error**         | Receive fault module names                      |
| **Contour Map Download** | Download laser contour maps (laser only)        |
| **Map Upload (ZIP)**     | Upload map files in ZIP format                  |

---

### Localization Pose Interface

**Description:** Continuously receives localization data (x, y, theta) from the camera.

**Published Topic:** `/lx_localization_node/LxCamera_LocationPoseResult`

**Data Format:**

- Position: x, y coordinates
- Orientation: Quaternion (converted from theta)
- TF Transform: Broadcasts pose via TF

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_LocationPoseResult
```

---

### Localization Status Interface

**Description:** Continuously receives localization status at **1 Hz** indicating whether the system is successfully localized.

**Published Topic:** `/lx_localization_node/LxCamera_LocationResult`

**Message Type:** `std_msgs::Bool`

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_LocationResult
```

---

### Storage Status Interface

**Description:** Continuously receives storage usage percentage at **2 Hz**.

**Published Topic:** `/lx_localization_node/LxCamera_Capacity_Status`

**Message Type:** `std_msgs::Float32`

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Capacity_Status
```

---

### Module Error Interface

**Description:** Continuously receives error module names from the camera.

**Published Topic:** `/lx_localization_node/LxCamera_Module_Error`

**Message Type:** `std_msgs::String`

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Module_Error
```

---

### Mapping Status Interface

**Description:** Publishes mapping state after mapping is initiated.

**Published Topic:** `/lx_localization_node/LxCamera_MappingState`

**Message Type:** `std_msgs::Bool`

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_MappingState
```

---

### Mapping Progress Interface

**Description:** Publishes mapping completion percentage after mapping is initiated.

**Published Topic:** `/lx_localization_node/LxCamera_Mapping_Progress`

**Message Type:** `std_msgs::Int32` (Range: 0-100)

**Verification Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Mapping_Progress
```

---

### Mapping Enable Interface (Topic-based)

> **Note:** Service interface is recommended for production use (see [ROS Service Interfaces](#ros-service-interfaces)).

**Subscribed Topic:** `/lx_localization_node/LxCamera_Mapping`

**Message Type:** `std_msgs/String`

**Format:** `<command>,<map_name>`

- `command`: `1` (start mapping) or `0` (stop mapping)
- `map_name`: Map identifier (max 30 characters, alphanumeric only, must not start with a number)
- **Separator:** English comma `,`

**Behavior:**

- After sending start command: Camera web interface displays "Mapping in Progress"
- After sending stop command: Web notification disappears and map appears in map library (requires sufficient robot movement)

**Example Commands:**

```bash
# Start mapping
rostopic pub -1 /lx_localization_node/LxCamera_Mapping std_msgs/String "1,fri"

# Stop mapping
rostopic pub -1 /lx_localization_node/LxCamera_Mapping std_msgs/String "0,fri"
```

---

### Parameter Setting Interface

**Subscribed Topic:** `/lx_localization_node/LxCamera_SetParam`

**Message Type:** `std_msgs/String`

**Format:** Nine comma-separated floating-point values: `x,y,z,yaw,pitch,roll,x,y,yaw`

| Parameter Index        | Description        |
| ---------------------- | ------------------ |
| 1-3 (x, y, z)          | Camera position    |
| 4-6 (yaw, pitch, roll) | Camera orientation |
| 7-8 (x, y)             | LiDAR position     |
| 9 (yaw)                | LiDAR orientation  |

**Configuration File:** Upon successful modification, updates `/home/fr1511b/hontai/params/config_install_FL21500HAF.ini` sections `[Laser1]` and `[Camera1]`.

**Example Command:**

```bash
rostopic pub -1 /lx_localization_node/LxCamera_SetParam std_msgs/String "11,11,10.2,11.2,12.2,13.2,2,2,2"
```

---

### Map Switching Interface

**Subscribed Topic:** `/lx_localization_node/LxCamera_SwitchMap`

**Message Type:** `std_msgs/String`

**Format:** Map name (must exist in camera map library)

**Verification:** Refresh camera web interface to confirm map change.

**Example Command:**

```bash
rostopic pub -1 /lx_localization_node/LxCamera_SwitchMap std_msgs/String "for"
```

---

### Contour Map Download Interface

**Subscribed Topic:** `/lx_localization_node/LxCamera_DownloadMap`

**Message Type:** `std_msgs/String`

**Format:** `<save_path>,<map_name>`

- `save_path`: Absolute path on host machine (no trailing `/`)
- `map_name`: Existing map name in camera (ZIP format)

**Output:** Creates a ZIP archive containing:

- `*.pgm` map file
- `*.yaml` configuration file

**Example Command:**

```bash
rostopic pub -1 /lx_localization_node/LxCamera_DownloadMap std_msgs/String "/home/zsure/LANXIN,TEST_999"
```

---

### Map Upload Interface (ZIP)

**Subscribed Topic:** `/lx_localization_node/LxCamera_UploadMapZip`

**Message Type:** `std_msgs/String`

**Format:** Absolute path to ZIP file (e.g., `/home/fr1511b/map.zip`)

**Requirement:** Must use ZIP files downloaded via the download interface.

**Destination:** Camera extracts to `/home/fr1511b/hontai/map/`

**Example Command:**

```bash
rostopic pub -1 /lx_localization_node/LxCamera_UploadMapZip std_msgs/String "/home/fr1511b/map.zip"
```

---

### Relocalization Interface

**Subscribed Topic:** `/lx_localization_node/LxCamera_UploadReloc`

**Message Type:** `geometry_msgs/PoseWithCovarianceStamped`

**Data:** Laser pose containing x, y, yaw

**Verification:** Camera web interface updates to reflect the sent position.

**Example Command:**

```bash
rostopic pub -1 /lx_localization_node/LxCamera_UploadReloc geometry_msgs/PoseWithCovarianceStamped "
header:
  frame_id: ''
pose:
  pose:
    position:
      x: 0.0
      y: 0.0
      z: 0.0
    orientation:
      x: 0.0
      y: 0.0
      z: -0.694658
      w: 0.71934
  covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]"
```

> **Note:** Quaternion values must satisfy mathematical constraints. Use the [Euler-Quaternion Converter](https://www.itutool.com/tools/euler-quaternion-converter) to convert yaw angles to quaternions.

---

### Odometry Upload Interface

**Subscribed Topic:** `/sim/odom` (configurable)

**Message Type:** `nav_msgs/Odometry`

**Example Command:**

```bash
rostopic pub /odom nav_msgs/Odometry "header:
  seq: 0
  stamp:
    secs: 0
    nsecs: 0
  frame_id: 'odom'
  child_frame_id: 'base_link'
pose:
  pose:
    position:
      x: 1.0
      y: 2.0
      z: 0.0
    orientation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
  covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
twist:
  twist:
    linear:
      x: 0.5
      y: 0.0
      z: 0.0
    angular:
      x: 0.0
      y: 0.0
      z: 0.1
  covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]"
```

---

## ROS Service Interfaces

> **Recommendation:** For mapping, map switching, map downloading, and relocalization operations, **service interfaces are preferred** over topic interfaces as they provide operation success/failure feedback.

**Usage:** Call via `rosservice` command. Returns `true` for success, `false` for failure.

### Service Categories

| Category         | Services                                                                       |
| ---------------- | ------------------------------------------------------------------------------ |
| **Mapping**      | Start Mapping, Stop Mapping, Incremental Mapping                               |
| **Localization** | Relocalization, Switch Map                                                     |
| **Utilities**    | Get Current Map Name, Download Map, Get Map List, Delete Map, Download Heatmap |

---

### Start Mapping

**Service Name:** `/lx_localization_node/LxCamera_Mapping_srv`

**Service Definition:**

```c++
bool status      # true: start mapping, false: stop mapping
string mapname   # Map name (max 30 chars, must not start with number)
---
bool result      # Operation success status
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_Mapping_srv "status: true
mapname: 'test_map'"
```

---

### Stop Mapping

**Service Name:** `/lx_localization_node/LxCamera_ExitMapping_srv`

**Service Definition:**

```c++
bool exit_mapping    # true to exit mapping mode
---
bool state          # Success status
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_ExitMapping_srv "exit_mapping: true"
```

---

### Incremental Mapping

**Service Name:** `/lx_localization_node/LxCamera_IncMapping_srv`

**Description:** Performs incremental mapping on existing maps with automatic merging.

**Service Definition:**

```c++
bool status      # true: start, false: stop
string mapname   # Map name (max 30 chars, must not start with number)
---
bool result
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_IncMapping_srv "status: true
mapname: 'test_map'"
```

---

### Switch Map

**Service Name:** `/lx_localization_node/LxCamera_SwitchMap_srv`

**Service Definition:**

```c++
string map_name    # Target map name (max 30 chars, must not start with number)
---
bool result
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_SwitchMap_srv "map_name: 'zszs999'"
```

---

### Download Map

**Service Name:** `/lx_localization_node/LxCamera_DownloadMap_srv`

**Service Definition:**

```c++
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (e.g., "/home/lx/map", no trailing "/")
---
bool result
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_DownloadMap_srv "mapname: 'TEST_999'
save_path: '/home/fr1511b/T/TEST'"
```

---

### Relocalization

**Service Name:** `/lx_localization_node/LxCamera_UploadReloc_srv`

**Service Definition:**

```c++
geometry_msgs/PoseWithCovarianceStamped pose
---
bool result
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_UploadReloc_srv "pose:
  header:
    seq: 0
    stamp:
      secs: 0
      nsecs: 0
    frame_id: 'map'
  pose:
    pose:
      position:
        x: 4.5
        y: -15.5
        z: 0.0
      orientation:
        x: 0.0
        y: 0.0
        z: 0.694658
        w: 0.71934
    covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]"
```

---

### Get Current Map Name

**Service Name:** `/lx_localization_node/LxCamera_GetCurMapname_srv`

**Service Definition:**

```c++
bool get_cur_mapname    # Trigger flag
---
string map_name        # Current active map name
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_GetCurMapname_srv "get_cur_mapname: true"
```

---

### Get Map List

**Service Name:** `/lx_localization_node/LxCamera_GetMapList_srv`

**Service Definition:**

```c++
bool get_map_list    # Trigger flag
---
string[] map_list   # Array of available map names
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_GetMapList_srv "get_map_list: true"
```

---

### Delete Map

**Service Name:** `/lx_localization_node/LxCamera_EraseMap_srv`

**Service Definition:**

```c++
string map_name    # Map name to delete (max 30 chars, must not start with number)
---
bool result       # Deletion success status
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_EraseMap_srv "map_name: 'TEST888'"
```

---

### Download Heatmap

**Service Name:** `/lx_localization_node/LxCamera_DownloadHeatMap_srv`

**Service Definition:**

```c++
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (no trailing "/")
---
bool result
```

**Example Call:**

```bash
rosservice call /lx_localization_node/LxCamera_DownloadHeatMap_srv "mapname: 'TEST_999'
save_path: '/home/fr1511b/T/TEST'"
```

---

## Document Information

**Version:** V2PRO ROS1
**Package:** `lx_camera_ros`
**Last Updated:** 2026-02-28
