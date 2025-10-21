# Điều khiển thiết bị
## 1. Giới thiệu chung
- controlDevicePower(): Điều khiển bật tắt
- controlGroupPower(): Điều khiển bật tắt theo nhóm
- controlMotorCurtain(): Điều khiển đóng mở
- controlLock(): Điều khiển khóa, mở khóa
- controlLightBrightness(): Điều khiển độ sáng đèn
- controlLightHsv(): Điều khiển màu HSV
- controlAC(): Điều khiển điều hòa
- locatePositionDevice(): Xác định vị trí của nhiều thiết bị
- requestSelfTestDevice(): Kiểm tra thiết bị có phản hồi báo hiệu hay không
- requestSelfTestDeviceNwk(): Kểm tra trên trên toàn bộ network, thiết bị nào đang online, thiết bị nào đang offline
- silenceAlarm(): tắt chuông báo

## 2. Sử dụng
**Cú pháp chung**
```kotlin
SmartSdk.controlHandler()
```

### 2.1. Điều khiển bật, tắt thiết bị
```kotlin
handler.controlDevicePower(devId, element, isOn)
```
**Đối số** :
- **devId**: ```String```  id của thiết bị
- **element**: ```Int``` id của element của thiết bị muốn điều khiển
- **isOn**: ```Boolean``` bật hoặc tắt

### 2.2. Điều khiển bật, tắt thiết bị theo nhóm
```kotlin
handler.controlGroupPower(groupId, isOn, deviceType)
```
**Đối số**:
- **groupId**: ```String``` id của group
- **isOn**: ```Boolean``` bật hoặc tắt
- **deviceType**: ```Int``` Loại thiết bị muốn điều khiển

### 2.3. Điều khiển bộ điều khiển động cơ
```kotlin
handler.controlMotorCurtain(id, isGroupTarget, command)
```
**Đối số**:
- **id**: ```String``` id của thiết bị hoặc id của nhóm
- **isGroupTarget**: ```Boolean``` có phải đang muốn điều khiển nhóm hay không
- **command**: ```Int``` đóng, dừng hoặc mở

### 2.4. Điều khiển khóa, mở khóa
```kotlin
handler.controlLock(devId, command, callback)
```
**Đối số** :
- **devId**: ```String``` id của thiết bị hoặc nhóm
- **command**: ```Int``` khóa hoặc mở khóa
- **callback**: ```AckStatusCallback```: Có thể truyền null

### 2.5. Điều khiển độ sáng và nhiệt dộ màu 
```kotlin
handler.controlLightBrightness(id, isGroupTarget, b, k)
```
**Đối số** :
- **id**: ```String``` id của thiết bị hoặc id của group
- **isGroupTarget**: ```Boolean``` có phải đang muốn điều khiển nhóm hay không
- **b**: ```Float hoặc Int``` Độ sáng. Nếu Float 0.1f -> 1.0f. Nếu Int  0 -> 1000
- **k**: ```Float hoặc Int``` Nhiệt độ màu. Nếu Float 0.1f -> 1.0f. Nếu Int  2200 -> 6500

### 2.6 Điều khiển màu HSV
```kotlin
handler.controlLightHsv(id, isGroupTarget, hsv, null)
```
**Đối số** :
- **id**: ```String``` id của thiết bị hoặc id của group
- **isGroupTarget**: ```Boolean``` có phải đang muốn điều khiển nhóm hay không
- **hsv**: ```FloatArray``` thông tin HSV - Tham khảo http://color.lukas-stratmann.com/color-systems/hsv.html

**Chú thích** :
- Khi điều khiển thiết bị theo location: truyền biến đầu vào "id" ở các hàm trên là null
- Các "command" ở trên đều được lấy từ class IoTCmdConst. Ví dụ: Đối với lệnh khóa: IoTCmdConst.Lock.

### 2.7. Điều khiển điều hòa
```kotlin
handler.controlAC(id, isOn, int mode, int temp, int fanMode, null)
```
**Đối số** :
- **id**: ```String``` id của thiết bị
- **isOn**: ```Boolean``` muốn điều khiển điều hòa bật hay tắt
- **temp**:```int`` Nhiệt độ muốn điều hòa sử dụng
- **mode**: ```int``` Chế độ muốn điều hòa sử dụng
- **fanMode**: ```int``` Chế độ quạt muốn điều hòa sử dụng

**Chú thích**:
- Các giá trị của temp, mode, fanMode được giới hạn như sau:
    + Đối với temp: Nhiệt độ được hỗ trợ trong khoảng 16 -> 30.
      + Đối với mode: Có 5 loại chế độ chính. Các giá trị này đã được define sẵn ở trong class IoTCmdConst
            IoTCmdConst.AC_MODE_AUTO = 0
            IoTCmdConst.AC_MODE_COOLING = 1
            IoTCmdConst.AC_MODE_DRY = 2
            IoTCmdConst.AC_MODE_HEATING = 3
            IoTCmdConst.AC_MODE_FAN = 4
  
      + Đối với fanMode: Có 5 loại chế độ quạt chính: Các giá tị này cũng đã được define sẵn ở class IoTCmdConst
           IoTCmdConst.FAN_SPEED_AUTO = 0
           IoTCmdConst.FAN_SPEED_LOW = 1
           IoTCmdConst.FAN_SPEED_NORMAL = 2
           IoTCmdConst.FAN_SPEED_HIGH = 3
           IoTCmdConst.FAN_SPEED_MAX = 4

### 2.8. Xác định vị trí của nhiều thiết bị
```kotlin
handler.locatePostitionDevice(devId, timeOutSeconds, callback)
```
**Đối số** :
- **devId**: ```String``` Id của thiết bị trung tâm
- **timeOutSeconds**: ```int``` khoảng thời gian để kiểm tra các thiết bị
- **callback**: ```SuccessStatus```

Khi có phát hiện khói, các cảm biến đồng thời sẽ báo liên tục. Khi sử dụng hàm này, chỉ những cảm biến đang phát hiện khói sẽ tiếp tục báo, các thiết bị còn lại sẽ dừng báo khói

### 2.9.  Xác định vị trí của một thiết bị
```kotlin
handler.locatePostitionDevice(devId, element, timeOutSeconds, callback)
```
**Đối số** :
- **devId**: ```String``` Id của thiết bị trung tâm
- **element**: ```int``` id của element muốn xác định vị trí
- **timeOutSeconds**: ```int``` khoảng thời gian để kiểm tra các thiết bị
- **callback**: ```SuccessStatus```

### 2.10. Kiểm tra thiết bị có phản hồi báo hiệu hay không
```kotlin
handler.requestSelfTestDevice(devId, int element, timeOutSeconds, callback)
```
**Đối số** :
- **devId**: ```String``` Id của thiết bị
- **element**: ```int``` element muốn kiểm tra
- **timeOutSeconds**: ```int``` khoảng thời gian để kiểm tra các thiết bị
- **callback**: ```RequestCallback<HashMap<String,  IoTSelfTestResult>>```

### 2.11.  Kiểm tra trên toàn bộ network, thiết bị nào đang online và thiết bị nào đang offline
```kotlin
handler.requestSelfTestDeviceNwk(devId, element, timeOutSeconds, callback)
```
**Đối số** :
- **devId**: ```String``` Id của thiết bị
- **element**: ```int``` element muốn kiểm tra
- **timeOutSeconds**: ```int``` khoảng thời gian để kiểm tra các thiết bị
- **callback**: ```RequestCallback<HashMap<String,  IoTSelfTestResult>>```

### 2.12. Silence Alarm
```kotlin
SmartSdk.controlHandler().silenceAlarm(deviceId, callback)
```
**Đối số** :
- **deviceId**: ```String``` UUID của thiết bị
- **callback**: ```SuccessStatus```

