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
devId: ```String```  id của thiết bị
element: ```Int``` id của element của thiết bị muốn điều khiển
isOn: ```Boolean``` bật hoặc tắt
