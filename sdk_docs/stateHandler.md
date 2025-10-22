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

#### 2.3.9. Lấy trạng thái online của thiết bị
```kotlin
val currentStatus: Int = objState.getOnlineStatus(elmId)
```
**Chú thích** :
- **currentStatus**: để chỉ thiết bị có đang online hay không.
    + currentStatus == 1: Đang online
    + currentStatus == 255: Đang offline

#### 2.3.10.  Kiểm tra thiết bị có vấn đề hay không
```kotlin
val hasIssues: Boolean = objState.isHasIssues
```

### 2.4. Đồng bộ trạng thái thời gian thực
Khi trạng thái của thiết bị thay đổi do người dùng điều khiển thiết bị, hoặc do người dùng sử dụng ứng dụng trên một điện thoại khác để điều khiển thiết bị, hay, hay mỗi lần số lượng pin của thiết bị thay đổi, đều có thể nhận biết khi ta đồng bộ trạng thái thời gian thực.
#### 2.4.1. Khởi tạo callback
Đầu tiên, cần khởi tạo một callback để có thể lắng nghe những trạng thái thay đổi mỗi khi thiết bị trả về.
```kotlin
val callback = object : SmartSdkDeviceStateCallback(){
	override fun onDeviceStateUpdated(uuid: String?) {
// Những giá trị được trả về tại hàm này có nghĩa rằng thiết bị đang sử dụng các bản tin cũ. Cần yêu cầu người dùng cập nhật thiết bị lên version firmware mới nhất
}
	override fun onEventAttrStateChange(devId: String?, element: Int, values: IntArray)
	override fun onEventAttrStateControl(devId: String?, element: Int, values: IntArray)
	override fun onDeviceLogSensorUpdated(devId: String?, elm: Int, attr: Int)
}
```
**Đối số** :
- **devId**: ```String``` uuid thiết bị
- **element**: ```Int``` element của thiết bị
- **attr**: ```Int``` thuộc tính cảm biến
- **values**: ```IntArray``` dữ liệu cảm biến

#### 2.4.2. Lắng nghe sự kiện 
```kotlin
SmartSdk.registerDeviceStateCallback(callback)
```

#### 2.4.3. Hủy lắng nghe sự kiện
```kotlin
SmartSdk.unregisterDeviceStateCallback(callback)
```
**Chú thích** :
- Phương thức này trả về trạng thái hiện tại của thiết bị
- Giá trị "values" thường được trả về theo cấu trúc ở bảng sau:

# Bảng thuộc tính IoT

| STT | Loại thuộc tính | Giá trị | Cấu trúc bản tin (IntArray) | Giải thích |
|-----|------------------|----------|-----------------------------|-------------|
| 1   | ACT_ONOFF | 1 | [IoTAttribute.ACT_ONOFF, Int] | Khi giá trị Int được trả về là:<br>• IoTCmdConst.POWER_ON: đang bật<br>• IoTCmdConst.POWER_OFF: đang tắt |
| 2   | ACT_OPEN_CLOSE | 2 | [IoTAttribute.ACT_OPEN_CLOSE, Int] | Khi giá trị Int được trả về là:<br>• IoTCmdConst.OPEN_CLOSE_MODE_OPEN: đang mở<br>• IoTCmdConst.OPEN_CLOSE_MODE_CLOSE: đang đóng |
| 3   | ACT_LOCK_UNLOCK | 3 | [IoTAttribute.ACT_LOCK_UNLOCK, Int] | Khi giá trị Int được trả về là:<br>• IoTCmdConst.DOOR_LOCKED: đang khóa<br>• IoTCmdConst.DOOR_UNLOCKED: đang mở |
| 4   | EVT_BATTERY (Pin/Battery) | 9 | [IoTAttribute.EVT_BATTERY, Int] | Giá trị Int là Pin hiện tại của thiết bị |
| 5   | ACT_BRIGHTNESS | 28 | [IoTAttribute.ACT_BRIGHTNESS, Int1, Int2] | Giá trị Int1 thể hiện cho Brightness (Lưu ý: Phải lấy giá trị Int1/1000) |
| 6   | ACT_BRIGHTNESS_KELVIN | 29 | [IoTAttribute.ACT_BRIGHTNESS_KELVIN, Int1, Int2] | Giá trị Int2 thể hiện cho Kelvin |
| 7   | ACT_KELVIN | 30 | [IoTAttribute.ACT_KELVIN, Int] | Giá trị Int thể hiện cho Kelvin |
| 8   | EVT_LUX | 54 | [IoTAttribute.EVT_LUX, Int] | Giá trị Int thể hiện cho độ sáng Lux |
| 9   | EVT_SMOKE (Khói/Smoke) | 55 | [IoTAttribute.EVT_SMOKE, Int] | Giá trị Int thể hiện có khói hay không:<br>• 0: Không có khói<br>• 1: Có khói |
| 10  | EVT_WALL_MOUNTED | 56 | [IoTAttribute.EVT_WALL_MOUNTED, Int] | Khi giá trị Int được trả về là:<br>• IoTCmdConst.WALL_MOUNTED: gắn tường<br>• IoTCmdConst.WALL_NOT_MOUNTED: không gắn tường |
| 11  | EVT_TEST_ALARM | 63 | [EVT_TEST_ALARM, Int1, Int2] | Giá trị Int 1 để chỉ loại thiết bị nào(Giá trị có thể kiểm tra trong class IoTDeviceType).Giá trị Int2 để chỉ thiết bị có đang được kiếm thử hay không: 1: Có 0: Không |
| 12  | EVT_PRESENCE | 70 | [IoTAttribute.EVT_PRESENCE, Int] | Khi giá trị Int được trả về là: IoTCmdConst.PRESENCE_STATUS_NONE: không phát hiện hiện diện, IoTCmdConst.PRESENCE_STATUS_DETECTED: phát hiện hiện diện |
| 13  | EVT_ONLINE_STATUS | 128 | [IoTAttribute.EVT_ONLINE_STATUS, Int] | Khi giá trị Int được trả về là: 1: đang trực tuyến 0: đang không trực tuyến |
| 14  | INFO_NETWORK_RF_SIGNAL_STRENGTH | 45530 | [IoTAttribute.INFO_NETWORK_RF_SIGNAL_STRENGTH, Int] | Giá trị Int biểu thị cho cường độ tín hiệu của thiết bị |

### 2.5. Lịch sử trạng thái
Có một số loại thiết bị sẽ lưu lại trạng thái mỗi khi có sự thay đổi. Mỗi lần lưu lại trạng thái sẽ được tính là 1 Log. Ví dụ: Với công tắc cửa cổng, ta có thể biết được cửa được đóng hay mở vào giờ nào thuộc ngày nào. Hay với cảm biến hiện diện, ta có thể biết vào giờ nào hoặc ngày nào phát hiện có người.
**Chú thích** :
Những loại thiết bị sở hữu các thuộc tính đi kèm sau có hỗ trợ lưu lịch sử trạng thái:
#### 2.5.1. Gửi lệnh để lấy dữ liệu từ thiết bị
Gửi yêu cầu đến thiết bị để lấy được các Log.
##### 2.5.1.1. Lấy theo ngày hiện tại
```kotlin
handler.pingLogSensorDevice(devId, elm, attr)
```

##### 2.5.1.2. Lấy theo ngày tùy chọn
```kotlin
handler.pingLogSensorDevice(devId, elm, attr, year, day)
```
**Đối số** :
- **devId**: ```String``` uuid của thiết bị
- **elm**: ```Int``` element của thiết bị
- **attr**: ```Int``` attribute: loại thuộc tính của cảm biến(hãy xem bảng 4.1)
- **year**, **day**: ```Int```

**Chú thích** :
- Ví dụ: Hôm nay là ngày 10/09/2024 -> day = 264 

#### 2.5.2. Lấy log thiết bị
Sau khi gửi bản tin đến thiết bị để yêu cầu trả về bản tin, hãy để một khoảng thời gian chờ khoảng 0.3s. Sau đó hãy bắt đầu gọi đến hàm tiếp theo dưới đây.

##### 2.5.2.1. Lấy dữ liệu log mới nhất được cập nhật
```kotlin
val ioTSensorLog: IoTSensorLog = handler.getSensorLogNewest(devId, elm, attr)
```

##### 2.5.2.2. Lấy theo part
- Ví dụ: Một thiết bị đã lưu được 1000 Log. Các Log này được lưu lần lượt từ part 0 cho đến part 50. Khi ta đọc giá trị Log trả về từ thiết bị, sẽ đọc các log từ part mới nhất là 50 rồi mới đến 49.
- Chế độ đọc theo part đang hỗ trợ đối với hầu hết các thiết bị ngoại trừ Cảm biến hiện diện.
```kotlin
val ioTSensorLog = handler.getSensorLog(devId, elm, attr,  cp)
```
**Đối số** :
- **devId**: ```String``` uuid của thiết bị
- **elm**: ```Int``` element của thiết bị
- **attr**: ```Int``` attribute: loại thuộc tính của thiết bị(xem bảng 4.1)
- **cp**: ```Int``` current part: vị trí đoạn log hiện tại đang lấy

**Giá trị trả về**
IoTLogSensor

| STT | Trường | Kiểu | Giải thích |
|-----|---------|------|-------------|
| 1 | `cp` | Int | Vị trí data log đang đọc |
| 2 | `pp` | Int | Vị trí data log trước |
| 3 | `day` | Int | Ngày mà log được lưu (ngày cũng được lưu theo cách đổi đã nêu trên) |
| 4 | `year` | Int | Năm mà log được lưu |
| 5 | `data` | IntArray | **Dữ liệu log:**<br>+ Cấu trúc của `IoTSensorLog.data` có thể lấy một ví dụ là `[17, 17 số 0 liên tiếp, bắt đầu giá trị của log]`.<br>+ Giá trị đầu tiên sẽ là vị trí bắt đầu đọc log trong dãy của `IoTSensorLog.data`. |

##### 2.5.2.3. Lấy toàn bộ log theo ngày
```kotlin
val ioTSensorLog: Collection<IoTSensorLog> =  handler.getSensorLogs(devId, elm, attr, year, day)
```
**Đối số** :
- **devId**: ```String``` UUID của thiết bị
- **elm**: ```Int``` element của thiết bị mà cần lấy được lịch sử  Log
- **attr**: ```Int``` Cần lấy log theo thuộc tính nào. (bảng 4.1)
- **year**: ```Int``` Năm
- **day**: ```Int``` Ngày bao nhiêu của năm.
**Ví dụ** :
Hôm nay ngày 17/2/2025 tương đương sẽ là ngày thứ 48 của năm 2025. Biến year sẽ là 2025, day là 48 

### 2.6. Đọc data
a. Nhiệt độ - độ ẩm
```kotlin
IoTAttribute.TEMP_HUMID_EVT
val step = 3 //chia mảng data thành các đoạn 3 giá trị tương ứng:
data[pos + 0] = minute Of Day
data[pos + 1] = nhiệt độ
data[pos + 2] = độ ẩm
```
```kotlin
IoTAttribute.TEMP_HUMID_EVT
val step = 3 //chia mảng data thành các đoạn 3 giá trị tương ứng:
data[pos + 0] = minute Of Day
data[pos + 1] = nhiệt độ
data[pos + 2] = độ ẩm
```
b. Cảm biến cửa  
```kotlin
IoTAttribute.OPEN_CLOSE_EVT
step = 2
data[pos + 0] = minute Of Day
data[pos + 1] = giá trị đóng-mở (mở = 1)
```
c. Cảm biến chuyển động
```kotlin
IoTAttribute.MOTION_EVT
step = 2
data[pos + 0] = minute Of Day
data[pos + 1] = giá trị (phát hiện chuyển động = 1)
```
d. Khóa
```kotlin
IoTAttribute.LOCK_UNLOCK
step = 2
data[pos + 0] = minute Of Day
data[pos + 1] = giá trị (locked = 1)
```
e. Cảm biến hiện diện
```kotlin
IoTAttribute.EVT_PRESENCE_SINGLE_ZONE
step = 2
data[pos + 0] = minute Of Day
data[pos + 1] = giá trị (phát hiện = 1)
```
f. Công tắc cửa cổng  
```kotlin
IoTAttribute.OPEN_CLOSE_CTL
step = 2
data[pos + 0] = minute Of Day
data[pos + 1] = giá trị đóng-mở (mở = 1)
```







