# Quản lí thiết bị
## 1. Giới thiệu hàm
- all(): Lấy danh sách các thiết bị đã kết nối
- getSubDevices(): Lấy danh sách các thiết bị thuộc một thiết bị trung tâm
- updateDeviceLabel(): Cập nhật nhãn cho thiết bị
- setDeviceGroup(): Cập nhật nhóm của thiết bị
- delete(): Xóa thiết bị
- getProductModel(): Lấy thông tin về mẫu sản phẩm

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.deviceHandler()
```
### 2.1. Lấy danh sách thiết bị
#### 2.1.1. Lấy toàn bộ thiết bị
```kotlin
val deviceList: Collection<IoTDevice> = handler.all
```
#### 2.1.2. Lấy danh sách các thiết bị thuộc một thiết bị trung tâm
```kotlin
val deviceList: Collection<IoTDevice> = handler.getSubDevices(devId)
```
**Đối số**:
- **devId**: ```String``` UUID của thiết bị trung tâm cần lần danh sách các thiết bị trực thuộc

### 2.2. Cập nhật nhãn thiết bị
```Kotlin
handler.updateDeviceLabel(devId, label, elmMaps, requestCallback)
```
**Đối số** :
- **devId**: ```String``` deviceUUID
- **label**: ```String``` nhãn mới của thiết bị
- **elemMaps**: ```Map<int, String>```: key là element muốn cập nhật label, value là label muốn cập nhật cho element đó
- **requestCallback**: ```RequestCallback<IoTDevice>```

### 2.3. Cập nhật nhóm thiết bị
```kotlin
handler.setDeviceGroup(devId, groupId, requestCallback)
```
**Đối số** : 
- **devId**: ```String``` deviceUUID
- **groupId**: ```String``` UUID của group muốn thiết lập cho thiết bị
- **requestCallback**: ```RequestCallback<IoTDevice>```

### 2.4. Xóa thiết bị
```kotlin
handler.delete(devId, requestCallback)
```
**Đối số** :
- **devId**: ```String``` deviceUUID
- **requestCallback**: ```RequestCallback<IoTDevice>```
### 2.5. Xóa element
Hiện tại tính năng này đang chỉ hỗ trợ đối với thiết bị RF.
```kotlin
handler.deleteElementDevice(devId, element, requestCallback)
```
**Đối số**
- **devId**: ```String``` deviceUUID
- **element**: ```int``` element muốn xóa
- **requestCallback**: ```RequestCallback<IoTDevice>```

### 2.6. Lấy thông tin của một mẫu sản phẩm
```kotlin
val productModel: IoTProductModel = SmartSdk.getProductModel(productId)
```
**Đối số** :
- **productId**: ```String``` Mã sản phẩm của dòng thiết bị





