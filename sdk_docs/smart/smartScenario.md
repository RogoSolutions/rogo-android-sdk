# Smart Scenario
## 1. Giới thiệu chung
- createSmartScene(): Tạo một kịch bản mới
- bindDeviceSmartCmd(): Thiết lập lệnh điều khiển cho thiết bị
- activeSmart(): Kích hoạt kịch bản

## 2. Sử dung
### 2.1. Tạo một kịch bản mới
```kotlin
handler.createSmartScene(label, sceneType, ownerId, requestCallback)
```
**Đối số** :
- **label**: ```String``` Tên kịch bản
- **sceneType**: ```Integer``` Loại kịch bản(Hiện tại truyền 0)
- **ownerId**: ```String```  (Hiện tại truyền null)
- **requestCallback**: ```RequestCallback<IoTSmart>```

### 2.2. Thiết lập lệnh điều khiển cho thiết bị
```kotlin
handler.bindDeviceSmartCmd(smartId, devId, cmds, requestCallback)
```
**Đối số** :
- **smartId**: ```String``` ID của smart muốn thiết lập lệnh điều khiển
- **devId**: ```String``` ID của thiết bị muốn điều khiển
- **cmds**: ```HashMap<Integer, IoTTargetCmd>``` lệnh điều khiển thiết bị. Ở đây, giá trị Integer thể hiện cho ID của element của thiết bị muốn điều khiển, và giá trị IoTTargetCmd để thể hiện lệnh điều khiển nào được thiết lập cho element đó. Phần này có thể truyền nhiều element khác nhau. Nhưng các element phải thuộc cùng duy nhất 1 thiết bị.

**Giải thích class IoTTargetCmd**

| STT | Thuộc tính | Kiểu giá trị | Mô tả | Ghi chú |
|-----|-------------|---------------|--------|---------|
| 1 | `delay` | Integer | Thời gian đợi để thực thi lệnh điều khiển tiếp theo | |
| 2 | `reversing` | Integer | - Thời gian đảo ngược lệnh (tính bằng giây (s))<br>- Hiện tại chỉ thực hiện đối với lệnh bật-tắt (on-off) | Ví dụ: Khi thực hiện lệnh điều khiển là lệnh bật ổ cắm và chúng ta thiết lập giá trị `reversing` là **5s**. Thì sau 5s khi ổ cắm được bật thì sẽ tự động chuyển về trạng thái tắt. |
| 3 | `cmd` | IntArray | Lệnh điều khiển | |

**Các lệnh điều khiển đang hỗ trợ**
- Điều khiển bật, tắt
```kotlin
cmd = intArrayOf(
    IoTAttribute.ONOFF,
    if (isOn) IoTCmdConst.POWER_ON else IoTCmdConst.POWER_OFF
)
```

- Điều khiển đóng, mở
```kotlin
val state = 
IoTCmdConst.OPENCLOSE_MODE_CLOSE||   IoTCmdConst.OPENCLOSE_MODE_OPEN|| IoTCmdConst.OPENCLOSE_MODE_STOP|| IoTCmdConst.OPENCLOSE_MODE_MOVING
cmd = intArrayOf(IoTAttribute.OPEN_CLOSE_CTL, state)
```

- Điều chỉnh nhiệt độ màu đèn
```kotlin
cmd = intArrayOf(
IoTAttribute.BRIGHTNESS_KELVIN, 
brightness, 
kelvin
)
```

- Đổi màu đèn
```kotlin
val hsv = [h, s, v]     // màu HSV
cmd = intArrayOf(
IoTAttribute.COLOR_HSV,
hsv[0], 
hsv[1], 
hsv[2]
)
```

- Điều khiển điều hòa
```kotlin
cmd = intArrayOf(
IoTAttribute.AC, 
if (isOn) IoTCmdConst.POWER_ON else IoTCmdConst.POWER_OFF,
 mode, 
temp, 
fan, 
swing, 
extra
)
```

Giải thích class IoTSmartCmd
### IoTSmartCmd

| STT | Thuộc tính | Kiểu giá trị | Mô tả | Ghi chú |
|-----|-------------|---------------|--------|---------|
| 1 | `uuid` | String | ID của lệnh điều khiển | |
| 2 | `smartId` | String | ID của smart chứa lệnh điều khiển này | |
| 3 | `targetId` | String | | |
| 4 | `target` | Integer | | |
| 5 | `filter` | Integer | | |
| 6 | `cmds` | `HashMap<Integer, IoTTargetCmd>` | Các lệnh điều khiển | - Giá trị Integer để chỉ giá trị element được thiết lập lệnh điều khiển.<br>- Giá trị `IoTTargetCmd` để chỉ lệnh điều khiển đối với element này.<br>- Có thể bao gồm nhiều lệnh điều khiển trong một `IoTSmartCmd`, nhưng các lệnh này là các lệnh điều khiển các element của cùng một thiết bị. |
| 7 | `cflm` | Integer | | |
| 8 | `linkId` | String | | |
| 9 | `linkType` | Integer | | |
| 10 | `ownerId` | String | | |
| 11 | `userId` | String | | |


### 2.3. Kích hoạt kịch bản
```kotlin
handler.activeSmart(smartId)
```
**Đối số** :
- **smartId**: ```String``` Id của kịch bản mà người dùng muốn sử dụng



