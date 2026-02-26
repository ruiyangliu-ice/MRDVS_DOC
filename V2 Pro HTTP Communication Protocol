# MRDVS V2 Pro HTTP Communication Protocol

The V2PRO supports HTTP (POST, GET) protocols for mapping, map switching, relocalization, real-time location tracking, map upload/download, and other functions. The supported interfaces are listed below:

### Map Operation Interfaces

1.  Start Mapping
2.  Apply Map (Switch Map)
3.  Stop Mapping
4.  Start Incremental Mapping
5.  Stop Incremental Mapping
6.  Get Local Map List
7.  Download Map (including visual map)
8.  Upload Map (To Camera)
9.  Get Laser Contour Map (Get Map Package From the Host)

### Runtime Operation Interfaces

1.  Set Camera Height
2.  Relocalization
3.  Get Real-time Pose

### Other Configuration and Management Interfaces

1.  Restart Camera Program
2.  Reboot Device
3.  Program Update
4.  Get Program Version
5.  Get Basic Device Information

---

## 1. System Utilities

### 1.1 Update Program

**Request URL**
`http://{ip}:9998/tool/rob/quiet_update`
**Request Example**

```json
{
  "url": "http://192.168.2.179:8080/hontai_2.1.20211114.zip"
}
```

### 1.2 Set Camera Height

**Request URL**
`http://{ip}:9998/tool/rob/setHeight`
**Request Example**

```json
{
  "height": 1.3
}
```

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

---

## 2. Retrieve Global Information

### 2.1 Get Program Version

**Request URL**
`http://{ip}:9998/tool/rob/getRobotVersion`
**Response Example**

```json
{
  "code": 200,
  "data": "version12313",
  "message": "success"
}
```

### 2.2 Get Basic Device Information

**Request URL**
`http://{ip}:9998/tool/rob/getVehicle`
**Response Example**

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "vehicleId": "Vehicle uuid",
    "mapId": "Current Map",
    "vehicleType": "Vehicle Type Forklift"
  }
}
```

### 2.3 Get Map Package From the Host

**Request URL**
`http://{ip}:9998/tool/rob/getRunMapZip`
**Request Parameters**
Content-Type: application/json
| Required | Parameters | Description |
| --- | --- | --- |
| YES | NAME | MAP_NAME |

**Response Example**
Returns a file with content-type: application/zip

### 2.4 GetPose

**Request URL**
`http://{ip}:9998/tool/rob/getPose`
**Request Parameters**
`None`
**Response Example**

```json
{
  "code": 200,
  "pose": {
    "x": -1.4972912073135376,
    "y": -0.5784364342689514,
    "yaw": 1.6737515926361084
  }
}
```

### 2.5 Relocate (Go Online)

**Request URL**
`http://{ip}:9998/tool/rob/relocate`
**Request Parameters**
Content-Type: application/json
| Required | Parameters | Description |
|----------|------------|----------------------|
| Yes | x | X Position (meters) |
| Yes | y | Y Position (meters) |
| Yes | angle | -- |
| Yes | noisyX | Noisy (meters) |
| Yes | noisyY | Noisy (meters) |
| Yes | noisyAngle | Noisy (meters) |

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

---

## 3. Map Operations

### 3.1 Apply Map (Switch Map)

**Request URL**
`http://{ip}:9998/tool/rob/changeCurrentRunMap`
**Request Parameters**
Content-Type: application/json
| Required | Parameters | Description |
| --- | --- | --- |
| YES | NAME | MAP_NAME |

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.2 Start Mapping

**Request URL**
`http://{ip}:9998/tool/rob/startMapping`
**Request Parameters**
| Required | Parameters | Description |
| --- | --- | --- |
| YES | NAME | MAP_NAME |

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.3 Stop Mapping

**Request URL**
`http://{ip}:9998/tool/rob/stopMapping`
**Request Parameters**
`None`

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.4 Start Incremental Mapping

**Request URL**
`http://{ip}:9998/tool/rob/sendCMD`
**Request Parameters**

```json
{
  "cmd": 904,
  "attach": {
    "sub_command": 0,
    "param": "mapname"
  }
}
```

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.5 Stop Incremental Mapping

**Request URL**
`http://{ip}:9998/tool/rob/sendCMD`
**Request Example**

```json
{
  "cmd": 905,
  "attach": {
    "sub_command": 0,
    "param": "mapname"
  }
}
```

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.6 Get Local Map List (Recommend)

**Request URL**
`http://{ip}:9998/tool/rob/getRunMapLists`
**Request Parameters**
`None`

**Response Example**

```json
{
  "code": 200,
  "message": "success成功",
  "run_maps": [
    "chejianceshi",
    "pf",
    "xz9_test9",
    "comm"
  ]
}
```

### 3.7 Upload Map Package to Camera

**Request URL**
`http://{ip}:9998/tool/rob/uploadMapZip2RunMaps`
**Request Parameters**
`None`

**Request Example**

```json
{
  "url": "http://192.168.2.179:8080/map.zip"
}
```

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 3.8 Download Map

**Request URL**
`http://{ip}:9998/tool/rob/downloadVisiomWithLaserMap`
**Request Parameters**
`None`

**Request Example**

```json
{
  "url": "http://192.168.2.179:8080/map.zip"
}
```

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

---

## 4. System Administration

### 4.1 Restart Camera Program

**Request URL**
`http://{ip}:9998/tool/rob/restart`
**Request Parameters**
`None`

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```

### 4.2 Reboot Camera

**Request URL**
`http://{ip}:9998/tool/rob/Reboot`
**Request Parameters**
`None`

**Response Example**

```json
{
  "code": 200,
  "message": "success"
}
```
