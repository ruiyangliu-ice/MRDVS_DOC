# V2PRO ROS1 Interface Documentation

V2PRO provides a ROS1 demo package `lx_camera_ros`. The following interface specifications and usage guidelines reference this demo program.

---

## Table of Contents

1. [Environment Setup and Compilation](#environment-setup-and-compilation)
   - [Prerequisites](#prerequisites)
     - [1. Install Qt5](#1-install-qt5)
     - [2. Install libzip](#2-install-libzip)
     - [3. Build the ROS Package](#3-build-the-ros-package)
2. [Execution](#execution)
3. [ROS Topic Interfaces](#ros-topic-interfaces)
   - [Mapping Phase](#mapping-phase)
     - [Mapping Enable Interface](#mapping-enable-interface)
     - [Mapping State Interface](#mapping-state-interface)
     - [Mapping Progress Interface](#mapping-progress-interface)
   - [Localization Phase](#localization-phase)
     - [Localization Pose Interface](#localization-pose-interface)
     - [Localization Status Interface](#localization-status-interface)
     - [Map Switching Interface](#map-switching-interface)
     - [Relocalization Interface](#relocalization-interface)
     - [Map Download Interface](#map-download-interface)
   - [Other Interfaces](#other-interfaces)
     - [Storage Status Interface](#storage-status-interface)
     - [Module Error Interface](#module-error-interface)
     - [Parameter Setting Interface](#parameter-setting-interface)
     - [Odometry Upload Interface](#odometry-upload-interface)
     - [Map Upload Interface (ZIP)](#map-upload-interface-zip)
4. [ROS Service Interfaces](#ros-service-interfaces)
   - [Service Categories](#service-categories)
   - [Mapping Services](#mapping-services)
     - [Start Mapping](#start-mapping)
     - [Stop Mapping](#stop-mapping)
     - [Incremental Mapping](#incremental-mapping)
   - [Localization Services](#localization-services)
     - [Relocalization](#relocalization-1)
     - [Switch Map](#switch-map)
   - [Map Management Services](#map-management-services)
     - [Get Current Map Name](#get-current-map-name)
     - [Get Map List](#get-map-list)
     - [Delete Map](#delete-map)
     - [Download Map](#download-map)
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

#### 3. Build the ROS Package

Navigate to the `/V2-ROS-SDK-service` directory (same level as `/src`) and run:

```bash
catkin_make
```

---

## Execution

> **Note:** Before running the ROS demo, disable any ROS master/slave configurations deployed on the camera to prevent communication conflicts. If the camera has a ROS environment, wait approximately 20 seconds after power-on before running the ROS demo program.

**Prerequisites for Testing:** Power on the camera (wait 30 seconds for the internal program to start), then launch the ROS SDK node.

Launch the system:

```bash
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

# Run the node
rosrun lx_camera_ros lx_camera_ros_node
```

---

## ROS Topic Interfaces

### Mapping Phase

| Interface            | Description                           |
| -------------------- | ------------------------------------- |
| **Mapping Enable**   | Start/stop mapping operations         |
| **Mapping State**    | Receive current mapping state         |
| **Mapping Progress** | Receive mapping completion percentage |

#### Mapping Enable Interface

> **Recommendation:** Service interface is strongly recommended for production use (see [ROS Service Interfaces](#ros-service-interfaces)).

**ROS Topic:** `/lx_localization_node/LxCamera_Mapping`

**Message Type:** `std_msgs/String`

**Format:** `<command>,<map_name>`

- `command`: `1` (start mapping) or `0` (stop mapping)
- `map_name`: Map identifier (max 30 characters, alphanumeric only, must not start with a number)
- **Separator:** English comma `,`

**Behavior:**

- After sending start command: Camera web interface displays "Mapping in Progress"
- After sending stop command: Web notification disappears and map appears in map library (requires sufficient vehicle movement)

**Example Commands:**

```bash
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

# Start mapping
rostopic pub -1 /lx_localization_node/LxCamera_Mapping std_msgs/String "1,fri"

# Stop mapping
rostopic pub -1 /lx_localization_node/LxCamera_Mapping std_msgs/String "0,fri"
```

---

#### Mapping State Interface

**Description:** Receives mapping state after mapping is initiated.

**ROS Topic:** `/lx_localization_node/LxCamera_MappingState`

**Message Type:** `std_msgs::Bool`

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_MappingState
```

---

#### Mapping Progress Interface

**Description:** Receives mapping completion percentage after mapping is initiated.

**ROS Topic:** `/lx_localization_node/LxCamera_Mapping_Progress` (Range: 0-100)

**Message Type:** `std_msgs::Int32`

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Mapping_Progress
```

---

### Localization Phase

| Interface               | Description                                       |
| ----------------------- | ------------------------------------------------- |
| **Localization Pose**   | Receive real-time localization data (x, y, theta) |
| **Localization Status** | Receive localization state (true/false)           |
| **Map Switching**       | Switch between stored maps                        |
| **Relocalization**      | Set pose for relocalization                       |
| **Map Download**        | Download map files from camera                    |

#### Localization Pose Interface

**Description:** Continuously receives localization data (x, y, theta) from the camera.

**ROS Topic:** `/lx_localization_node/LxCamera_LocationPoseResult`

**Data Format:**

- Position: x, y coordinates
- Orientation: Quaternion (converted from theta)
- TF Transform: Broadcasts pose via TF

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_LocationPoseResult
```

---

#### Localization Status Interface

**Description:** Continuously receives localization status at **1 Hz** indicating whether the system is successfully localized.

**ROS Topic:** `/lx_localization_node/LxCamera_LocationResult`

**Message Type:** `std_msgs::Bool`

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_LocationResult
```

---

#### Map Switching Interface

**ROS Topic:** `/lx_localization_node/LxCamera_SwitchMap`

**Message Type:** `std_msgs/String`

**Format:** Map name (must exist in camera map library)

**Verification:** Refresh camera web interface to confirm map change.

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

rostopic pub -1 /lx_localization_node/LxCamera_SwitchMap std_msgs/String "for"
```

---

#### Relocalization Interface

**ROS Topic:** `/lx_localization_node/LxCamera_UploadReloc`

**Message Type:** `geometry_msgs/PoseWithCovarianceStamped`

**Data:** Laser pose containing x, y, yaw

**Verification:** Relocalization successful when position updated in camera WebUI.

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

> **Note:** Quaternion values must satisfy normalization constraints. Use the [Euler-Quaternion Converter](https://www.andre-gaschler.com/rotationconverter/) to convert yaw angles to valid quaternions.

---

#### Map Download Interface

**ROS Topic:** `/lx_localization_node/LxCamera_DownloadMap`

**Message Type:** `std_msgs/String`

**Format:** `<save_path>,<map_name>`

- `save_path`: Absolute path on computer (no trailing `/`)
- `map_name`: Existing map name in camera (ZIP format)

**Output:** Creates a ZIP archive containing:

- `*.pgm` map file
- `*.yaml` configuration file

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

rostopic pub -1 /lx_localization_node/LxCamera_DownloadMap std_msgs/String "/home/zsure/LANXIN,TEST_999"
```

---

### Other Interfaces

| Interface             | Description                                     |
| --------------------- | ----------------------------------------------- |
| **Storage Status**    | Monitor camera storage usage                    |
| **Module Error**      | Receive fault module names                      |
| **Parameter Setting** | Configure camera and LiDAR extrinsic parameters |
| **Odometry Upload**   | Upload wheel odometry data                      |
| **Map Upload (ZIP)**  | Upload map files in ZIP format                  |

#### Storage Status Interface

**Description:** Continuously receives storage usage percentage at **2 Hz**.

**ROS Topic:** `/lx_localization_node/LxCamera_Capacity_Status`

**Message Type:** `std_msgs::Float32`

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Capacity_Status
```

---

#### Module Error Interface

**Description:** Continuously receives error module names from the camera.

**ROS Topic:** `/lx_localization_node/LxCamera_Module_Error`

**Message Type:** `std_msgs::String`

**Example Command:**

```bash
rostopic echo /lx_localization_node/LxCamera_Module_Error
```

---

#### Parameter Setting Interface

**ROS Topic:** `/lx_localization_node/LxCamera_SetParam`

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
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

rostopic pub -1 /lx_localization_node/LxCamera_SetParam std_msgs/String "11,11,10.2,11.2,12.2,13.2,2,2,2"
```

---

#### Odometry Upload Interface

**ROS Topic:** `/sim/odom` (configurable)

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

#### Map Upload Interface (ZIP)

**ROS Topic:** `/lx_localization_node/LxCamera_UploadMapZip`

**Message Type:** `std_msgs/String`

**Format:** Absolute path to ZIP file (e.g., `/home/fr1511b/map.zip`)

**Requirement:** Must use ZIP files downloaded via the download interface.

**Destination:** Camera extracts to `/home/fr1511b/hontai/map/`

**Example Command:**

```bash
# Navigate to workspace and source
cd ~/V2_ROS_SDK
source devel/setup.bash

rostopic pub -1 /lx_localization_node/LxCamera_UploadMapZip std_msgs/String "/home/fr1511b/map.zip"
```

---

## ROS Service Interfaces

> **Recommendation:** For mapping, map switching, map downloading, and relocalization operations, **service interfaces are strongly recommended** over topic interfaces.

**Usage:** Call via `rosservice` command. Returns `true` for success, `false` for failure.

### Service Categories

| Category           | Services                                                                       |
| ------------------ | ------------------------------------------------------------------------------ |
| **Mapping**        | Start Mapping, Stop Mapping, Incremental Mapping                               |
| **Localization**   | Relocalization, Switch Map                                                     |
| **Map Management** | Get Current Map Name, Get Map List, Delete Map, Download Map, Download Heatmap |

---

### Mapping Services

#### Start Mapping

**Service:** `/lx_localization_node/LxCamera_Mapping_srv`

**Service Definition:**

```yaml
bool status      # true: start mapping, false: stop mapping
string mapname   # Map name (max 30 chars, must not start with number)
---
bool result      # Operation result: true = success, false = failure
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_Mapping_srv "status: true
mapname: 'test_map'"
```

---

#### Stop Mapping

**Service Name:** `/lx_localization_node/LxCamera_ExitMapping_srv`

**Service Definition:**

```yaml
bool exit_mapping    # true to exit mapping mode
---
bool state          # Exit mapping state: true = success, false = failure
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_ExitMapping_srv "exit_mapping: true"
```

---

#### Incremental Mapping

**Service Name:** `/lx_localization_node/LxCamera_IncMapping_srv`

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
rosservice call /lx_localization_node/LxCamera_IncMapping_srv "status: true
mapname: 'test_map'"
```

---

### Localization Services

#### Relocalization

**Service Name:** `/lx_localization_node/LxCamera_UploadReloc_srv`

**Service Definition:**

```yaml
geometry_msgs/PoseWithCovarianceStamped pose
---
bool result        # Operation success status
```

**Example Command:**

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

#### Switch Map

**Service Name:** `/lx_localization_node/LxCamera_SwitchMap_srv`

**Service Definition:**

```yaml
string mapname    # Target map name (max 30 chars, must not start with number)
---
bool result       # Operation success status
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_SwitchMap_srv "mapname: 'zszs999'"
```

---

### Map Management Services

#### Get Current Map Name

**Service Name:** `/lx_localization_node/LxCamera_GetCurMapname_srv`

**Service Definition:**

```yaml
bool get_cur_mapname    # Trigger flag (set true)
---
string map_name        # Current active map name
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_GetCurMapname_srv "get_cur_mapname: true"
```

---

#### Get Map List

**Service Name:** `/lx_localization_node/LxCamera_GetMapList_srv`

**Service Definition:**

```yaml
bool get_map_list    # Trigger flag (set true)
---
string[] map_list   # Array of available map names
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_GetMapList_srv "get_map_list: true"
```

---

#### Delete Map

**Service Name:** `/lx_localization_node/LxCamera_EraseMap_srv`

**Service Definition:**

```yaml
string map_name    # Map name to delete (max 30 chars, must not start with number)
---
bool result       # Deletion success status
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_EraseMap_srv "map_name: 'TEST888'"
```

---

#### Download Map

**Service Name:** `/lx_localization_node/LxCamera_DownloadMap_srv`

**Service Definition:**

```yaml
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (e.g., "/home/lx/map", no trailing "/")
---
bool result        # Operation success status
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_DownloadMap_srv "mapname: 'TEST_999'
save_path: '/home/fr1511b/T/TEST'"
```

---

#### Download Heatmap

**Service Name:** `/lx_localization_node/LxCamera_DownloadHeatMap_srv`

**Service Definition:**

```yaml
string mapname     # Map name (max 30 chars, must not start with number)
string save_path   # Absolute save path (no trailing "/")
---
bool result        # Operation success status
```

**Example Command:**

```bash
rosservice call /lx_localization_node/LxCamera_DownloadHeatMap_srv "mapname: 'TEST_999'
save_path: '/home/fr1511b/T/TEST'"
```
