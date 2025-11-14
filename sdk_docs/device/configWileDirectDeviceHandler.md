# Kết nối thiết bị Wile
## 1. Giới thiệu hàm
- discovery(): Được gọi tới để tìm kiếm các thiết bị BLE xung quanh
- stopDiscovery(): Được gọi tới sau khi đã quét thiết bị được một khoảng thời gian nhất định.
  Thời gian quét thiết bị tùy thuộc vào mục đích sử dụng của người phát triển, có thể 5s, 10s, 30s… Nhưng khi đã quét xong, cần dừng quét thiết bị.
- connectAndIdentifyDevice(): Bắt đầu quá trình thiết lập cho thiết bị. Khi gọi tới hàm này, ta còn có thể lắng nghe tiến độ thiết lập thiết bị hiện tại.
- scanWifi(): Như đã đề cập, những thiết bị WILE sẽ được thiết lập thêm mạng wifi. Do đó, cần quét các Wifi đang khả dụng, người dùng sẽ chọn một Wifi để thiết lập thiết bị.
- requestConnectWifiNetwork(): Thiết lập để thiết bị có thể kết nối tới mạng Wifi đang khả dụng.
- setupAndSycDeviceToCloud(): Khi đã thiết lập kết nối xong, cấu hình các thông tin cơ bản của thiết bị: Nhãn thiết bị, nhóm phòng muốn thêm thiết bị vào,...
- cancel(): Gọi tới khi muốn hủy tiến trình khi đang thiết lập thiết bị.

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.configWileDirectDeviceHandler()
```
### 2.1. Quét thiết bị
```Kotlin
SmartSdk.discoverySmartDeviceHandler().discovery(callback)
```
**Đối số** :
- **callback**: ```DiscoverySmartDeviceCallback``` trả về thông tin của thiết bị vừa quét được

**Giải thích class TypeConnect** :

| STT | Loại                    | Cách sử dụng                                 | Ghi chú |
|-----|--------------------------|----------------------------------------------|----------|
| 1   | `MESH`                  | Dễ chỉ các thiết bị kết nối qua **Ble Mesh** |          |
| 2   | `WILEDIRECTBLE_ACTIVE`  | Dễ chỉ các thiết bị kết nối qua **WILE**     | Thiết bị HUB của SafeFire hiện đang sử dụng loại này |
| 3   | `WILEDIRECTBLE_PASSIVE` |                                              |          |
| 4   | `WILEDIRECTLAN`         |                                              |          |

### 2.2. Hủy tìm kiếm 
```kotlin
SmartSdk.discoverySmartDeviceHandler().stopDiscovery()
```

### 2.3. Thiết lập thiết bị
```kotlin
val callback = object: SetupWileDirectCallback{
	override fun onDeviceIdentifiedAndReadySetup(
                mac, 
                firmwareVersion, 
                networkConnectivities) {

}
	override fun onProgress(progress, prgMsg) {
}
	override fun onSetupFailure(errorCode, msg) {
}
}
```
**Tiến trình khi thiết lập được trả về tại hàm onProgress**:
Tiến độ thiết lập thiết bị

| STT | Tiến độ | Giải thích |
|-----|----------|------------|
| 1 | 0 → 10 | Kết nối tới thiết bị thông qua **BLE** |
| 2 | 20 | Cung cấp **WiFi** cho thiết bị vào mạng |
| 3 | 30 | Kết quả khi thiết lập thông tin WiFi thành công hay thất bại |
| 4 | 40 | Thiết lập **URL** tới cloud |
| 5 | 50 | Thiết lập chứng chỉ mã hóa cloud cho thiết bị |
| 6 | 60 | Thiết lập **URL** tới MQTT |
| 7 | 70 | Thiết lập chứng chỉ mã hóa **MQTT** cho thiết bị |
| 8 | 80 | Thiết lập khóa **Mesh** cho thiết bị |
| 9 | 90 | Các bước đã hoàn tất, gửi cấu hình cơ bản như nhãn, nhóm phòng của thiết bị lên cloud |

**Đối số** :
- **progress**: ```Int```: Tiến độ thiết lập hiện tại
- **msgPrg**: ```String```: Tiến độ đang thực hiện làm gì
- **firmwareVersion**: ```String``` Phiên bản của firmware hiện tại của thiết bị
- **networkConnectivities**: ```Collection<IoTNetworkConnectivity>``` Các loại kết nối mà thiết bị hỗ trợ

```kotlin
handler.connectAndIdentifyDevice(ioTDirectDeviceInfo, setupWileDirectDeviceCallback)
```
**Đối số** :
- **ioTWileScanned**: ```IoTBleScanned``` thiết bị vừa được quét
- **setupDeviceWileCallback**: ```SetupDeviceWileCallback```

### 2.4. Tìm kiếm mạng WiFi khả dụng
```kotlin
handler.scanWifi(seconds, callback)
```
Hàm scanWifi cần được gọi sau callback của hàm connectAndIdentifyDevice() gọi tới onDeviceIdentifiedAndReadySetup. Khi này đã kết nối tới thiết bị và bắt đầu thực hiện quá trình thiết lập. Khi gọi đến hàm này sẽ gửi bản tin yêu cầu thiết bị muốn thiết lập quét các mạng Wifi cho phép kết nối ở xung quanh. Khi thiết bị phát hiện một mạng wifi thì sẽ trả về giá trị tại chính callback của hàm setUpWileDevice ở hàm on WifiScanned

**Đối số**:
- **seconds**: ```Int``` thời gian để thiết bị quét các WiFi đang khả dụng xung quanh.
- **callback**: ```RequestCallback<Collection<IoTWifiInfo>>

**Giải thích class `IoTWifiInfo` **

| STT | Trường     | Kiểu   | Chú thích             |
|-----|-------------|--------|------------------------|
| 1   | `ssid`      | String | Tên của WiFi          |
| 2   | `remember`  | int    |                       |
| 3   | `authType`  | int    | Kiểu bảo mật của WiFi |
| 4   | `rssi`      | int    | Rssi của WiFi         |
| 5   | `freq`      | int    |                       |

**Giải thích về `authType`**

Các giá trị của `authType` được giải thích ở class `IoTWifiAuthType`.  
Dưới đây là các kiểu bảo mật của WiFi:

| STT | Loại bảo mật       | Giá trị tương ứng | Chú thích |
|-----|---------------------|------------------:|------------|
| 1   | OPEN                | 0                |            |
| 2   | WEP                 | 1                |            |
| 3   | WPA_PSK             | 2                |            |
| 4   | WPA2_PSK            | 3                |            |
| 5   | WPA_WPA2_PSK        | 4                |            |
| 6   | WPA2_ENTERPRISE     | 5                |            |
| 7   | WPA3_PSK            | 6                |            |
| 8   | WPA2_WPA3_PSK       | 7                |            |
| 9   | WAPI_PSK            | 8                |            |
| 10  | MAX                 | 9                |            |



### 2.5. Thiết lập WiFi cho thiết bị
```kotlin
handler.requestConnectWifiNetwork(infNo, ssid, pwd, isConfirm, callback)
```
Sử dụng hàm này sau khi đã quét được WiFi đang khả dụng muốn kết nối. Ngoài ra, nếu muốn nhập thủ công thông tin WiFi thì có thể bỏ qua bước quét WiFi và thực hiện bước này luôn. Nhưng khi đó cần lưu ý là tên WiFi cần phải chính xác

**Đối số** :
- **infNo**: ```Int``` Mặc định truyền 0
- **ssid**: ```String``` SSID của Wifi
- **pwd**: ```String``` password  của Wifi
- **isConfirm**: ```Boolean``` : lựa chọn có xác thực thông tin wifi hay không
- **callback**: ```SuccessRequestCallback```



### 2.6. Hoàn tất thiết lập
```kotlin
handler.setupAndSyncDeviceToCloud(label, groupId, devSubType, callback)
```
Hàm này cần được gọi tới sau khi đã thiết lập thông tin WiFi cho thiết bị thành công. Nghĩa là callback tại requestConnectWifiNetwork() gọi tới onSuccess(). Tiến độ của quá trình thiết lập thiết bị sẽ được trả về tại hàm onProgress() của hàm connectAndIdentifyDevice()

**Đối số**
- **label**: ```String``` Tên thiết bị
- **groupId**: ```String``` id của nhóm phòng muốn thêm thiết bị
- **callback**: ```RequestCallback<IoTDevice>```
- **devSubType**: ```Int``` Giá trị này có thể truyền mặc định như sau: 
```kotlin
SmartSdk.getProductModel(ioTDirectDeviceInfo.productId).devSubType
```
