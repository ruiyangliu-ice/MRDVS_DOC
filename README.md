# V2PRO UDP Communication Protocol

V2PRO supports client communication via UDP. You can configure the target device's IP and port settings through the web interface:

- `portCode`: Target device listening port
- `portName`: Target device IP address

The V2PRO unit listens for incoming messages on port 8001.

## 1.1 Data Format Specifications

| Item | Specification |
|-------------|-------------|
| **Endianness** | Big-endian (MSB first, LSB last) |
| **Frame Structure** | Header + Length + Command Byte + Data + Checksum |
| **Checksum** | XOR of all bytes except header and checksum byte itself |
| **Data Length** | Excludes checksum byte and header. Note: The Length field is optional and not present in all frames. |
| **Map Name Format** | Restricted to alphanumeric characters and underscores (_). The name must begin with a letter. |

## 1.2 Inbound Data (V2 PRO Receives)

### a. Odometry Frame (13 bytes)

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `Length` |
| Byte 3 (CMD) | `0x0a` |
| Byte 4 | `V_3` |
| Byte 5 | `V_2` |
| Byte 6 | `V_1` |
| Byte 7 | `V_0` |
| Byte 8 | `Theta_3` |
| Byte 9 | `Theta_2` |
| Byte 10 | `Theta_1` |
| Byte 11 | `Theta_0` |
| Byte 12 | `Check` |

**Note:** Both`v`and`theta`are transmitted as 4-byte (32-bit) integers.
- `v`: unit: mm/s
- `theta`: unit: 0.01° (e.g., 90° = 9000)

---

### b. Relocalization Frame (Set Pose X, Y, Theta)

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `0x0F` |
| Byte 3 (CMD) | `0x02` |
| Byte 4 | `X_3` |
| Byte 5 | `X_2` |
| Byte 6 | `X_1` |
| Byte 7 | `X_0` |
| Byte 8 | `Y_3` |
| Byte 9 | `Y_2` |
| Byte 10 | `Y_1` |
| Byte 11 | `Y_0` |
| Byte 12 | `Theta_3` |
| Byte 13 | `Theta_2` |
| Byte 14 | `Theta_1` |
| Byte 15 | `Theta_0` |
| Byte 16 | `Check` |

**Note:** Both`x` `y`and`theta`are transmitted as 4-byte (32-bit) integers.
- `x` `y`: unit: mm
- `theta`: unit: 0.01° (e.g., 90° = 9000)

---

### c. Mapping Control (Start/Stop)

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `0x0F` |
| Byte 3 (CMD) | `0x02` |
| Byte 4 (Status)| `0x01`Start mapping, `0x00`Stop mapping |
| Byte 5 | `map_name_0` |
| ... | ... |
| Byte 34 | `map_name_29` |
| Byte 35 | `Check` |
**Do not send consecutive mapping commands**

**Map Name Constraints:**
- 30 bytes fixed length, null-padded (`0x00`)
- ASCII encoding only
- Valid characters: `A-Z` `a-z` `0-9` `_`
- Must start with a letter

---

### d. Map Switching

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `0x22` |
| Byte 3 (CMD) | `0x0C` |
| Byte 4 | `map_name_0`|
| ... | ... |
| Byte 33 | `map_name_29` |
| Byte 34 | `Check` |

**Note:** Starting at Byte 4 `map_name_0`, the next 30 bytes are reserved for the target map name. Please ensure the target map is finalized before switching.

## 1.3 Outbound Data (V2 PRO Sends)

### a. Localization Frame (17 bytes)

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `Length` |
| Byte 3 (CMD) | `0x01` |
| Byte 4 | `X_3` |
| Byte 5 | `X_2` |
| Byte 6 | `X_1` |
| Byte 7 | `X_0` |
| Byte 8 | `Y_3` |
| Byte 9 | `Y_2` |
| Byte 10 | `Y_1` |
| Byte 11 | `Y_0` |
| Byte 12 | `Theta_3` |
| Byte 13 | `Theta_2` |
| Byte 14 | `Theta_1` |
| Byte 15 | `Theta_0` |
| Byte 16 | `Check` |
**Once relocation is successful, V2PRO will continuously transmit the localization frame.**

**Note:** Both`x` `y`and`theta`are transmitted as 4-byte (32-bit) integers.
- `x` `y`: unit: mm
- `theta`: unit: 0.01° (e.g., 90° = 9000)

---

### b. Task Result Frame (Mapping and switching status)

| Bytes | Content |
|------|---|
| Byte 0 | `AC` |
| Byte 1 | `ED` |
| Byte 2 | `0x22` |
| Byte 3 (CMD) | `0xff` |
| Byte 4 (ID) | `0x01`Start mapping, `0x02`Stop mapping, `0x03`Switch map |
| Byte 5 (Status) | `0x01`Success, `0x00`Failed |
| Byte 6 | `Check` |

**Note:** Stop mapping operation takes approximately 1/3 of the actual mapping time. Status response will be sent upon completion.
