# V2PRO UDP Communication Protocol

V2PRO supports client communication via UDP. You can configure the target device's IP and port settings through the web interface:

- `portCode`: Target device listening port
- `portName`: Target device IP address

The V2PRO unit listens for incoming messages on port 8001.

## 1.1 Data Format Specifications

| Item                | Specification                                                                                        |
| ------------------- | ---------------------------------------------------------------------------------------------------- |
| **Endianness**      | Big-endian (MSB first, LSB last)                                                                     |
| **Frame Structure** | Header + Length + Command Byte + Data + Checksum                                                     |
| **Checksum**        | XOR of all bytes except header and checksum byte itself                                              |
| **Data Length**     | Excludes checksum byte and header. Note: The Length field is optional and not present in all frames. |
| **Map Name Format** | Restricted to alphanumeric characters and underscores (\_). The name must begin with a letter.       |

## 1.2 Inbound Data (V2 PRO Receives)

### a. Odometry Frames

V2 supports 6 odometer types (0-5), configurable via the WEB interface:

#### odomType 0: Velocity-based Odometry (VX, VY, W)

| Bytes        | Content  |
| ------------ | -------- |
| Byte 0       | `AC`     |
| Byte 1       | `ED`     |
| Byte 2       | `Length` |
| Byte 3 (CMD) | `0x0A`   |
| Byte 4       | `VX_3`   |
| Byte 5       | `VX_2`   |
| Byte 6       | `VX_1`   |
| Byte 7       | `VX_0`   |
| Byte 8       | `VY_3`   |
| Byte 9       | `VY_2`   |
| Byte 10      | `VY_1`   |
| Byte 11      | `VY_0`   |
| Byte 12      | `W_3`    |
| Byte 13      | `W_2`    |
| Byte 14      | `W_1`    |
| Byte 15      | `W_0`    |
| Byte 16      | `Check`  |

**Note:**`VX`,`VY`,`W`are transmitted as 4-byte (32-bit) integers.

- `VX`,`VY`: unit: mm/s
- `W`: unit: 0.001 rad/s

#### odomType 1: Position-based Odometry (X, Y, Theta)

| Bytes        | Content   |
| ------------ | --------- |
| Byte 0       | `AC`      |
| Byte 1       | `ED`      |
| Byte 2       | `Length`  |
| Byte 3 (CMD) | `0x0A`    |
| Byte 4       | `X_3`     |
| Byte 5       | `X_2`     |
| Byte 6       | `X_1`     |
| Byte 7       | `X_0`     |
| Byte 8       | `Y_3`     |
| Byte 9       | `Y_2`     |
| Byte 10      | `Y_1`     |
| Byte 11      | `Y_0`     |
| Byte 12      | `Theta_3` |
| Byte 13      | `Theta_2` |
| Byte 14      | `Theta_1` |
| Byte 15      | `Theta_0` |
| Byte 16      | `Check`   |

**Note:**`X`,`Y`,`Theta`are transmitted as 4-byte (32-bit) integers.

- `X`,`Y`: unit: mm
- `Theta`: unit: 0.001 rad

#### odomType 2: Differential Drive Velocity (V_Left, V_Right)

| Bytes        | Content  |
| ------------ | -------- |
| Byte 0       | `AC`     |
| Byte 1       | `ED`     |
| Byte 2       | `Length` |
| Byte 3 (CMD) | `0x0A`   |
| Byte 4       | `V_L_3`  |
| Byte 5       | `V_L_2`  |
| Byte 6       | `V_L_1`  |
| Byte 7       | `V_L_0`  |
| Byte 8       | `V_R_3`  |
| Byte 9       | `V_R_2`  |
| Byte 10      | `V_R_1`  |
| Byte 11      | `V_R_0`  |
| Byte 12      | `Check`  |

**Note:**`V_Left`,`V_Right`are transmitted as 4-byte (32-bit) integers.

- `V_Left`,`V_Right`: unit: mm/s

#### odomType 3: Differential Drive Distance (Mill_Left, Mill_Right)

| Bytes        | Content  |
| ------------ | -------- |
| Byte 0       | `AC`     |
| Byte 1       | `ED`     |
| Byte 2       | `Length` |
| Byte 3 (CMD) | `0x0A`   |
| Byte 4       | `M_L_3`  |
| Byte 5       | `M_L_2`  |
| Byte 6       | `M_L_1`  |
| Byte 7       | `M_L_0`  |
| Byte 8       | `M_R_3`  |
| Byte 9       | `M_R_2`  |
| Byte 10      | `M_R_1`  |
| Byte 11      | `M_R_0`  |
| Byte 12      | `Check`  |

**Note:**`Mill_Left`,`Mill_Right`are transmitted as 4-byte (32-bit) integers.

- `Mill_Left`,`Mill_Right`: unit: mm
- **Configuration Required:** Set wheelbase in `initParams` (unit: mm)

#### odomType 4: Single Steer Drive Velocity (Angle, V)

| Bytes        | Content   |
| ------------ | --------- |
| Byte 0       | `AC`      |
| Byte 1       | `ED`      |
| Byte 2       | `Length`  |
| Byte 3 (CMD) | `0x0A`    |
| Byte 4       | `Angle_3` |
| Byte 5       | `Angle_2` |
| Byte 6       | `Angle_1` |
| Byte 7       | `Angle_0` |
| Byte 8       | `V_3`     |
| Byte 9       | `V_2`     |
| Byte 10      | `V_1`     |
| Byte 11      | `V_0`     |
| Byte 12      | `Check`   |

**Note:**`Angle`,`V`are transmitted as 4-byte (32-bit) integers.

- `Angle`: unit: 0.01° (degrees, **not radians**)
- `V`: unit: mm/s

#### odomType 5: Single Steer Drive Distance (Angle + Mill)

| Bytes        | Content   |
| ------------ | --------- |
| Byte 0       | `AC`      |
| Byte 1       | `ED`      |
| Byte 2       | `Length`  |
| Byte 3 (CMD) | `0x0A`    |
| Byte 4       | `Angle_3` |
| Byte 5       | `Angle_2` |
| Byte 6       | `Angle_1` |
| Byte 7       | `Angle_0` |
| Byte 8       | `Mill_3`  |
| Byte 9       | `Mill_2`  |
| Byte 10      | `Mill_1`  |
| Byte 11      | `Mill_0`  |
| Byte 12      | `Check`   |

**Note:**`Angle`,`Mill`are transmitted as 4-byte (32-bit) integers.

- `Angle`: unit: 0.01° (degrees, **not radians**)
- `Mill`: unit: mm (is signed, positive/negative)
- **Configuration Required:** Set steer wheel offset from center of motion in `initParams` (unit: mm)

---

### b. Relocalization Frame (Set Pose X, Y, Theta)

| Bytes        | Content   |
| ------------ | --------- |
| Byte 0       | `AC`      |
| Byte 1       | `ED`      |
| Byte 2       | `0x0F`    |
| Byte 3 (CMD) | `0x02`    |
| Byte 4       | `X_3`     |
| Byte 5       | `X_2`     |
| Byte 6       | `X_1`     |
| Byte 7       | `X_0`     |
| Byte 8       | `Y_3`     |
| Byte 9       | `Y_2`     |
| Byte 10      | `Y_1`     |
| Byte 11      | `Y_0`     |
| Byte 12      | `Theta_3` |
| Byte 13      | `Theta_2` |
| Byte 14      | `Theta_1` |
| Byte 15      | `Theta_0` |
| Byte 16      | `Check`   |

**Note:** Both`x` `y`and`theta`are transmitted as 4-byte (32-bit) integers.

- `x` `y`: unit: mm
- `theta`: unit: 0.01° (e.g., 90° = 9000)

---

### c. Mapping Control (Start/Stop)

| Bytes           | Content                                 |
| --------------- | --------------------------------------- |
| Byte 0          | `AC`                                    |
| Byte 1          | `ED`                                    |
| Byte 2          | `0x0F`                                  |
| Byte 3 (CMD)    | `0x02`                                  |
| Byte 4 (Status) | `0x01`Start mapping, `0x00`Stop mapping |
| Byte 5          | `map_name_0`                            |
| ...             | ...                                     |
| Byte 34         | `map_name_29`                           |
| Byte 35         | `Check`                                 |

**Do not send consecutive mapping commands**

**Map Name Constraints:**

- 30 bytes fixed length, null-padded (`0x00`)
- ASCII encoding only
- Valid characters: `A-Z` `a-z` `0-9` `_`
- Must start with a letter

---

### d. Map Switching

| Bytes        | Content       |
| ------------ | ------------- |
| Byte 0       | `AC`          |
| Byte 1       | `ED`          |
| Byte 2       | `0x22`        |
| Byte 3 (CMD) | `0x0C`        |
| Byte 4       | `map_name_0`  |
| ...          | ...           |
| Byte 33      | `map_name_29` |
| Byte 34      | `Check`       |

**Note:** Starting at Byte 4 `map_name_0`, the next 30 bytes are reserved for the target map name. Please ensure the target map is finalized before switching.

## 1.3 Outbound Data (V2 PRO Sends)

### a. Localization Frame (25 bytes)

| Bytes        | Content       |
| ------------ | ------------- |
| Byte 0       | `AC`          |
| Byte 1       | `ED`          |
| Byte 2       | `Length`      |
| Byte 3 (CMD) | `0x01`        |
| Byte 4       | `X_3`         |
| Byte 5       | `X_2`         |
| Byte 6       | `X_1`         |
| Byte 7       | `X_0`         |
| Byte 8       | `Y_3`         |
| Byte 9       | `Y_2`         |
| Byte 10      | `Y_1`         |
| Byte 11      | `Y_0`         |
| Byte 12      | `Theta_3`     |
| Byte 13      | `Theta_2`     |
| Byte 14      | `Theta_1`     |
| Byte 15      | `Theta_0`     |
| Byte 16      | `TIMESTAMP_7` |
| Byte 17      | `TIMESTAMP_6` |
| Byte 18      | `TIMESTAMP_5` |
| Byte 19      | `TIMESTAMP_4` |
| Byte 20      | `TIMESTAMP_3` |
| Byte 21      | `TIMESTAMP_2` |
| Byte 22      | `TIMESTAMP_1` |
| Byte 23      | `TIMESTAMP_0` |
| Byte 24      | `Check`       |

**Once relocation is successful, V2PRO will continuously transmit the localization frame.**

**Note:** Both`X` `Y`and`Theta`are transmitted as 4-byte (32-bit) integers.

- `X` `Y`: unit: mm
- `Theta`: unit: 0.001 rad

---

### b. Task Result Frame (Mapping and switching status)

| Bytes           | Content                                                   |
| --------------- | --------------------------------------------------------- |
| Byte 0          | `AC`                                                      |
| Byte 1          | `ED`                                                      |
| Byte 2          | `0x22`                                                    |
| Byte 3 (CMD)    | `0xFF`                                                    |
| Byte 4 (ID)     | `0x01`Start mapping, `0x02`Stop mapping, `0x03`Switch map |
| Byte 5 (Status) | `0x01`Success, `0x00`Failed                               |
| Byte 6          | `Check`                                                   |

**Note:** Stop mapping operation takes approximately 1/3 of the actual mapping time. Status response will be sent upon completion.
