# Trạng thái của thiết bị
## 1. Giới thiệu hàm
- pingDeviceState(): yêu cầu thiết bị gửi về trạng thái mới nhất
- pingDeviceStateWithSucDevice(): yêu cầu một danh sách các thiết bị thuộc một thiết bị trung tâm gửi về trạng thái mới nhất
- getObjState(): lấy thông tin trạng thái của thiết bị

## 2. Sử dụng
### 2.1. Yêu cầu thiết bị trả về trạng thái mới nhất
#### 2.1.1. Không callback
```kotlin
handler.pingDeviceState(deviceId)
```
**Đối số** :
- **deviceId**: ```String```: UUID của thiết bị muốn lấy trạng thái

**Ghi chú** :
- Khi gọi tới hàm này, sẽ không xác định được đã lấy được trạng thái mới nhất hay chưa. Sau khi đã gọi tới hàm này thì nên cho một khoảng thời gian chờ rồi mới lấy trạng thái hiện tại ở bước tiếp theo. Có thể là 0,3s

#### 2.1.2. Có callback
```kotlin
handler.pingDeviceState(deviceId, callback)
```
**Đối số** :
- **deviceId**: ```String```: UUID của thiết bị muốn lấy trạng thái
- **callback**: ```SuccessRequestCallback``` được gọi tới khi trạng thái mới nhất của thiết bị đã được cập nhật.

**Ghi chú**:
- Khác với cách trên, không cần phải thiết lập thời gian chờ, khi callback được gọi đến thì có thể bắt đầu lấy trạng thái của thiết bị.

### 2.2. Yêu cầu một danh sách các thiết bị thuộc một thiết bị trung tâm gửi về trạng thái mới nhất
#### 2.2.1. Không callback
```kotlin
handler.pingDeviceStateWithSubDevice(deviceId)
```
**Đối số** :
- **deviceId**: ```String``` UUID của thiết bị trung tâm

**Ghi chú**:
- Khi gọi tới hàm này, sẽ không xác định được đã lấy được trạng thái mới nhất hay chưa. Sau khi đã gọi tới hàm này thì nên cho một khoảng thời gian chờ rồi mới lấy trạng thái hiện tại ở bước tiếp theo. Có thể là 0,3s

#### 2.2.2. Có callback
```kotlin
handler.pingDeviceStateWithSubDevice(deviceId, callback)
```
**Đối số** :
- **deviceId**: ```String``` UUID của thiết bị trung tâm
- **callback**: ```SuccessRequestCallback```, khi giá trị trả về onSuccess có thể bắt đầu lấy thông tin trạng thái mới nhất của thiết bị

**Ghi chú** :
- Khác với cách trên, không cần phải thiết lập thời gian chờ, khi callback được gọi đến thì có thể bắt đầu lấy trạng thái của các thiết bị.

### 2.3. Lấy trạng thái mới nhất của thiết bị
```kotlin
val objState:IoTObjState = handler.getObjState(devId)
```
**Đối số** :
- **devId**: ```String``` id của thiết bị

| STT | Trường | Kiểu | Giải thích |
|-----|---------|------|-------------|
| 1 | id | String | UUID của thiết bị đang lấy trạng thái |
| 2 | label | String | Nhãn của thiết bị đang lấy trạng thái |

#### 2.3.1. Thiết bị đang bật, tắt
```kotlin
val state: Boolean = objState.isOn()
```
Element của thiết bị đang bật - tắt
```kotlin
val state: Boolean = objState.isOn(elementId)
```
**Giải thích** :
- state == true => Thiết bị đang bật
- state == false => Thiết bị đang tắt

**Đối số** :
- **elementId**: ```Int``` id của element thuộc thiết bị mà ta muốn kiểm tra

#### 2.3.2.  Các trạng thái của đèn
```kotlin
// Độ sáng
val currentDim: Float = objState.getDimSlide
```
```kotlin
// Nhiệt độ màu
val currentKelvin: Float = objState.kelvinSlide
```
```kotlin
// Màu sắc hiện tại hsv
val currentHsv: FloatArray = objState.getHsv
```

#### 2.3.3. Nhiệt độ - độ ẩm
```kotlin
// Nhiệt độ
val currentTemp: Int = objState.temp
```
```kotlin
// Độ ẩm
val currentHumid: Int = objState.humid
```

#### 2.3.4. Đóng mở, gắn tường
```kotlin
// Đóng mở
val state: Boolean = objState.isDoorOpen(elementId)
```
**Giải thích** :
- state == true => Đang mở
- State == false => Đang đóng

```kotlin
// Gắn tường
val state: Boolean = objState.isWallMounted(elementId)
```
**Giải thích** :
- state == true => Đang gắn tường
- state == false => Đang không gắn tường

#### 2.3.5. Lux
```kotlin
val currentLux: Int = objState.lux
```
**Chú thích** :
- **currentLux**: Độ sáng Lux hiện tại

#### 2.3.6. Pin
```kotlin
val currentBattery: Int = objState.battery
```
**Chú thích** :
- **currentBattery**: Lượng pin hiện tại

#### 2.3.7. Khoá
```kotlin
val state: Boolean = objState.isLocked(elementId)
```
**Giải thích** :
- state == true => Đang khóa
- State == false => Đang không khóa

#### 2.3.8. Trạng thái điều hoà
```kotlin
// Chế độ
val currentMode: Int = objState.mode
```
```kotlin
val currentTemp: Int = objState.tempSet
```
```kotlin
//Tốc độ quạt
val currentFan: Int = objState.fanSpeed
```
**Chú thích** :
- **currentMode**: Chế độ hiện tại của quạt 

### 2.3.9. Lấy trạng thái online của thiết bị
```kotlin
val currentStatus: Int = objState.getOnlineStatus(elmId)
```
**Chú thích** :
- **currentStatus**: để chỉ thiết bị có đang online hay không.
    + currentStatus == 1: Đang online
    + currentStatus == 255: Đang offline

### 2.3.10.  Kiểm tra thiết bị có vấn đề hay không
```kotlin
val hasIssues: Boolean = objState.isHasIssues
```







