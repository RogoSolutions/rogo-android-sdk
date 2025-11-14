# Kết nối thiết bị RF
## 1. Giới thiệu hàm
- checkRfGatewayAvailable(): Những thiết bị RF cũng được quản lí thông qua một thiết bị trung tâm. Do đó, khi kết nối, cần kiểm tra thiết bị trung tâm nào đang khả dụng để kết nối thiết bị.
- cancelCheckRfGatewayAvailable(): Được gọi tới khi đã phát hiện được thiết bị trung tâm khả dụng muốn sử dụng
- startPairingRf(): Được gọi tới sau khi đã chọn được thiết bị trung tâm khả dụng và bắt đầu quét những thiết bị xung quanh
- stopPairingDevice(): Được gọi tới sau khi đã quét thiết bị được một khoảng thời gian nhất định.
  Thời gian quét thiết bị tùy thuộc vào mục đích sử dụng của người phát triển, có thể 5s, 10s, 30s… Nhưng khi đã quét xong, cần dừng quét thiết bị.
- syncDeviceToCloud(): Thiết lập cấu hình cơ bản như tên thiết bị, nhóm phòng muốn thêm thiết bị vào,... và hoàn thành kết nối.
- cancelSetupDevice(): Hủy tiến trình thiết lập với thiết bị đang kết nối

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.configRfDeviceHandler()
```
### 2.1. Kiểm tra thiết bị trung tâm khả dụng
#### 2.1.1. Trả về id của từng thiết bị
```kotlin
handler.checkRfGatewayAvailable(callback)
```
**Đối số** :
- **callback**: Là một ```CheckDeviceAvailableCallback``` dùng để lắng nghe id thiết bị mỗi khi tìm thấy một thiết bị trung tâm khả dụng

**Ghi chú**
- Khi gọi tới hàm này, chỉ cần đợi 8s. Khi phát hiện có trung tâm khả dụng sẽ tự động trả về giá trị ID của thiết bị của callback.

#### 2.1.2. Trả về một danh sách các thiết bị khả dụng
```kotlin
handler.checkGatewayAvailable(callback)
```
**Đối số** :
- **callback**: Là một ```CheckListDeviceAvailableCallback``` dùng để lắng nghe id thiết bị mỗi khi tìm thấy một thiết bị trung tâm khả dụng

### 2.2. Dừng kiểm tra
```kotlin
handler.cancelCheckRfGatewayAvailable()
```

### 2.3. Quét thiết bị
```kotlin
handler.startPairingRf(gatewayId, second, deviceType, isMultiPairing, callback)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung tâm đang khả dụng
- **second**: ```int``` khoảng thời gian thiết bị trung gian quét thiết bị (Đơn vị: s)
- **deviceType**: ```int``` Loại thiết bị(Lấy thông tin từ class IoTDeviceType)
- **isMultiPairing**: ```Boolean``` Có tìm kiếm để kết nối tới nhiều cảm biến trong một lần hay không
- **callback**: ```PairRfDeviceCallback``` gọi khi tìm thấy thiết bị đang sẵn sàng kết nối

### 2.4. Dừng tìm kiếm thiết bị
```kotlin
handler.stopPairingRf(gatewayId)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung tâm

### 2.5. Thiết lập thông tin cơ bản của thiết bị lên hệ thống
```kotlin
handler.syncDeviceToCloud(gatewayId, label, groupId, ioTPairedRfDevice, callback)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung gian khả dụng
- **groupId**: ```String``` id của nhóm phòng muốn thêm thiết bị
- **label**: ```String``` Nhãn thiết bị mà người dùng muốn sử dụng
- **ioTPairedRfDevice**: ```IoTPairedRfDevice``` Thông tin của thiết bị Rf muốn kết nối vừa quét được
- **callback**: ```RequestCallback<IoTDevice>```

### 2.6. Hủy thiết lập
```kotlin
handler.cancelSetupDevice()
```

### 2.7. Thiết lập kết nối nhanh
Thay vì thực hiện từ bước 2.1  đến 2.6, ta có thể sử dụng hàm này để có thể thực hiện thiết lập cho thiết bị gần nhất có thể quét được
```kotlin
handler.quickPairRfDevice(gatewayId, second, deviceType, callback)
```
**Đối số** :
- **gatewayId**: ```String``` id của thiết bị trung gian khả dụng
- **second**: ```int``` khoảng thời gian để quét thiết bị
- **callback**: ```RequestCallback<IoTElementInfo>``` thông tin của thiết bị mới được thêm vào thiết bị trung tâm




