# Kết nối thiết bị Zigbee
## 1. Giới thiệu hàm
- checkZigbeeGatewayAvailable(): Những thiết bị Zigbee cũng được quản lí thông qua một thiết bị trung tâm. Do đó, khi kết nối, cần kiểm tra thiết bị trung tâm nào đang khả dụng để kết nối thiết bị.
- cancelCheckZigbeeGatewayAvailable(): Được gọi tới khi đã phát hiện được thiết bị trung tâm khả dụng muốn sử dụng
- startPairZigbee(): Được gọi tới sau khi đã chọn được thiết bị trung tâm khả dụng và bắt đầu quét những thiết bị xung quanh
- stopPairZigbeeDevice(): Được gọi tới sau khi đã quét thiết bị được một khoảng thời gian nhất định.
  Thời gian quét thiết bị tùy thuộc vào mục đích sử dụng của người phát triển, có thể 5s, 10s, 30s… Nhưng khi đã quét xong, cần dừng quét thiết bị.
- syncDeviceToCloud(): Thiết lập cấu hình cơ bản như tên thiết bị, nhóm phòng muốn thêm thiết bị vào,... và hoàn thành kết nối.
- cancelSetupDevice(): Hủy tiến trình thiết lập với thiết bị đang kết nối

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.configZigbeeDeviceHandler()
```
### 2.1. Kiểm tra thiết bị trung tâm khả dụng
#### 2.1.1. Trả về id của một thiết bị trung tâm đang khả dụng
```kotlin
handler.checkZigbeeGatewayAvailable(callback)
```
**Đối số** :
- **callback**: Là một ```CheckDeviceAvailableCallback``` dùng để lắng nghe id thiết bị mỗi khi tìm thấy một thiết bị trung tâm khả dụng

**Ghi chú**: 
- Khi gọi tới hàm này, chỉ cần đợi 8s. Khi phát hiện có trung tâm khả dụng sẽ tự động trả về giá trị ID của thiết bị của callback.
- Trong trường hợp có nhiều hơn một thiết bị đang khả dụng, callback sẽ được gọi tới 4 lần. Mỗi lần sẽ trả về id của thiết bị đó.

#### 2.1.2. Trả về một danh sách các thiết bị khả dụng
```kotlin
handler.checkGatewayAvailable(seconds, callback)
```
**Đối số** :
- **seconds**: ```int``` là khoảng thời gian để lấy về danh sách các thiết bị trung tâm đang khả dụng
- **callback**: Là một ```CheckListDeviceAvailableCallback``` dùng để lắng nghe id thiết bị mỗi khi tìm thấy một thiết bị trung tâm khả dụng

### 2.2. Dừng kiểm tra
```kotlin
handler.cancelCheckZigbeeGatewayAvailable()
```

### 2.3. Quét thiết bị
```kotlin
handler.startPairZigbee(gatewayId, second, deviceType, callback)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung tâm đang khả dụng
- **second**: ```int``` khoảng thời gian thiết bị trung gian quét thiết bị (s)
- **deviceType**: ```int```.  Đối với những USB Zigbee không cần có kiểu thiết bị truyền 0
- **onDeviceFound**: ```PairZigbeeDeviceCallback``` gọi khi tìm thấy thiết bị đang sẵn sàng kết nối

### 2.4. Dừng tìm kiếm thiết bị
```kotlin
handler.stopPairZigbeeDevice(gatewayId)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung tâm

### 2.5. Thiết lập thông tin cơ bản của thiết bị lên hệ thống
```kotlin
handler.syncDeviceToCloud(gatewayId, groupId, deviceSubType, callback)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung gian khả dụng
- **groupId**: ```String``` id của nhóm phòng muốn thêm thiết bị
- **deviceSubType**: ```Int```
- **callback**: ```RequestCallback<IoTDevice>```

### 2.6. Hủy thiết lập thiết bị
```kotlin
handler.cancelSetupDevice()
```

