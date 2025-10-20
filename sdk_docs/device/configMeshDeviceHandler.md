# Kết nối thiết bị BLE MESH
## 1. Giới thiệu hàm
- checkMeshGatewayAvailable(): Như đã đề cập, những thiết bị BLE Mesh khi kết nối sẽ thuộc quản lí của một thiết bị trung tâm. Do đó, mỗi khi muốn kết nối một thiết bị BLE Mesh mới, ta cần kiểm tra thiết bị trung tâm nào đang khả dụng(đang trực tuyến).
- cancelCheckMeshGateway(): Được gọi tới khi muốn dừng quét các thiết bị trung tâm đang khả dụng
- discoveryMeshDevice(): Thực hiện để tìm kiếm các thiết bị BLE đang khả dụng xung quanh.
- stopDiscovery(): Khi không muốn quét thiết bị mới nữa có thể gọi tới để dừng lại quy trình quét.
  Thời gian quét thiết bị tùy thuộc vào mục đích sử dụng của người phát triển, có thể 5s, 10s, 30s… Nhưng khi đã quét xong, cần dừng quét thiết bị.
- connectAndIdentifyDevice(): Thiết bị trung tâm sẽ kiểm tra mạng mesh xem có thể thêm thiết bị hay không. Nếu có thể, thiết bị trung tâm sẽ tiếp tục nhận dạng thiết bị muốn kết nối và phản hồi xem đã sẵn sàng để thêm một thiết bị mới hay chưa.
- setupDeviceIntoMeshNetwork(): Sau khi thiết bị trung tâm cho phép kết nối, hàm này được gọi để thiết lập kết nối. Trong hàm này ta sẽ có thể lắng nghe được tiến độ kết nối của thiết bị cũng như khi nào quá trình thiết lập hoàn tất.
- setDeviceInfoToCloud(): Khi đã thiết lập kết nối xong, cấu hình các thông tin cơ bản của thiết bị: Nhãn thiết bị, nhóm phòng muốn thêm thiết bị vào,... lên hệ thống
- cancelSetupDevice(): Gọi tới khi muốn hủy tiến trình khi đang kết nối tới một thiết bị

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.configMeshDeviceHandler()
```
### 2.1. Kiếm tra các thiết bị trung tâm đang khả dụng
```kotlin
handler.checkMeshGatewayAvailable(seconds, callback)
```
**Đối số**: 
- **callback**: ```CheckListDeviceAvailableCallback```: Trả về id của thiết bị trung tâm đang khả dung.
- **seconds**: ```int``` : Khoảng thời gian để lấy về thông tin của các thiết bị trung tâm đang khả dụng

### 2.2. Dừng kiểm tra
```kotlin
handler.cancelCheckMeshGatewayAvailable()
```

### 2.3. Quét thiết bị
```kotlin
handler.discoveryMeshDevice(callback)
```
**Đối số** :
- **callback**: ```DiscoveryIoTBleCallback```: Trả về giá trị mỗi khi quét được thiết bị khả dụng.

### 2.4. Dừng quét thiết bị
```kotlin
handler.stopDiscovery()
```

### 2.5. Thiết lập thông tin của thiết bị tới thiết bị trung tâm
```kotlin
val callback = object: ConnectMeshDeviceCallback{
	override fun onConnectionFailure(errorCode,  msg) {}
	override fun onConnecting() {}
	override fun onConnected() {}
	override fun onDeviceIndentifiedAndReadyForSetup() {
	//Khi callback trả về tại hàm này mới thực hiện thiết lập kết nối mesh
}
}
```
```kotlin
handler.connectAndIdentifyDevice(gatewayId, ioTBleScanned, callback)
```
**Đối số** :
- **gatewayId**: ```String```: ID của thiết bị trung tâm mà thiết bị cần kết nối tới.
- **ioTBleScanned**: ```IoTBleScanned```: thiết bị vừa được quét mà người dùng muốn kết nối
- **callback**: ```ConnectMeshDeviceCallback```

### 2.6. Thiết lập kết nối
```kotlin
val callback = object: SetupDeviceMeshCallback {
	override fun onSetupMeshProgressStatus(id, progress, msg) {}
	override fun onSetupMeshConfigurationCompleted{
	//Khi callback trả về tại hàm này tiếp tục thực hiện bước tiếp theo
}
	override fun onSetupFailure(errorCode, msgFailure) {}
}
```
```kotlin
handler.setupDeviceIntoMeshNetwork(callback)
```
**Đối số** :
- **callback**: ```SetupDeviceMeshCallback```
- **progress**: ```Int``` Tiến độ kết nối thiết bị (giá trị trả về từ 10 đến 90)
- **msg**: ```String``` Tiến độ này đang thực hiện công việc gì
- **errorCode**: ```Int``` Mã lỗi
- **msgFailure**: ```String``` Thông tin lỗi
### 2.7. Hoàn tất thiết lập cấu hình
```kotlin
handler.syncDeviceInfoToCloud(label, groupId, devSubType, callback)
```
**Đối số** :
- **label**: ```String``` Tên thiết bị
- **groupId**: ```String``` id của nhóm phòng muốn thêm thiết bị
- **devSubType**: ```Int```
- **callback**: ```RequestCallback<IoTDevice>```

**Ghi chú** :
- Đối với đa số các sản phẩm, khi đã tìm thấy được thiết bị muốn quét (ioTBleScanned), biến devSubType có thể được truyền vào dựa trên cách định nghĩa sẵn: ioTBleScanned.ioTProductModel.devSubType.
- Đối với bộ điều khiển động cơ (**iotBleScanned.ioTProductModel.devType == IoTDeviceType.MOTOR_CONTROLLER**), có thể thiết lập thiết bị là bộ điều khiển động cơ rèm hay bộ điều khiển động cơ cổng. Khi đó, có thể truyền vào biến devSubType = IoTDeviceSubType.DC_CTL_CURTAIN_TYPE hoặc devSubType = IoTDeviceSubType.DC_CTL_GATE_TYPE

### 2.8. Thiết lập kết nối nhanh
Sau khi đã xác định được thiết bị trung tâm muốn kết nối và quét được thiết bị muốn kết nối, có thể thực hiện hàm sau để cấu hình cho thiết bị thay vì thực hiện từ bước 2.2.5 đến 2.2.8
```kotlin
handler.quickSetupAndSyncDeviceToCloud(gatewayId, ioTBleScanned, label, groupId, devSubType, callback)
```
**Đối số** :
- **gatewayId**:```String``` id của thiết bị trung tâm
- **ioTBleScanned**:```IoTBleScanned``` Thiết bị muốn kết nối được lấy ra sau bước quét thiết bị
- **label**: ```String``` Tên thiết bị
- **groupId**: ```String``` id của nhóm phòng muốn thêm thiết bị
- **devSubType**: ```Int```
- **callback**: ```RequestCallback<IoTDevice>```
### 2.9. Hủy thiết lập
```kotlin
handler.cancelSetupDevice()
```

