# Smart Schedule
## 1. Giới thiệu chung
- createSmartSchedule(): Tạo một lập lịch mới
- bindSmartSchedule(): Thiết lập hẹn giờ cho lập lịch
- reboundSmartSchedule(): Cập nhật lại hẹn giờ của lập lịch
- bindSmartDeviceCmd(): Thiết lập đối tượng điều khiển cho lập lịch. Phần này hãy sang xem ở Smart Scenario
- unboundSmartSchedule(): Xóa hẹn giờ khỏi lập lịch
- getSmartScheduleItem(): Lấy thông tin của một hẹn giờ trong lập lịch
- getSmartSchedule(): Lấy danh sách các hẹn giờ trong lập lịch

## 2. Sử dung
### 2.1. Tạo một lập lịch mới
```kotlin
handler.createSmartSchedule(label, ownerId, requestCallback)
```
**Đối số** :
- **label**: ```String``` Tên lập lịch
- **ownerId**: ```String```  (Hiện tại truyền null)
- **requestCallback**: ```RequestCallback<IoTSmart>```

### 2.2. Thiết lập hẹn giờ cho lập lịch
```kotlin
handler.bindSmartSchedule(label, timeSchedule, daySchedules, requestCallback)
```
**Đối số** :
- **smartId**: ```String``` ID của smart
- **timeSchedule**: ```Integer``` Thời điểm kích hoạt trong ngày (tính bằng phút)
    + Ví dụ : Lập lịch vào 18h30p = 1110p
- **daySchedules**: ```IntArray``` Ngày trong tuần
    + Từ Chủ nhật đến Thứ 7 tương ứng từ 0 đến 6
    + Ví dụ: Thiết lập lập lịch vào thứ 2, 4, Chủ nhật sẽ là: [1, 3, 0]
- **requestCallback**: ```RequestCallback<IoTSmart>```

### 2.3. Cập nhật lại hẹn giờ của lập lịch
```kotlin
handler.reboundSmartSchedule(smartId, smartScheduleId, timeSchedule, daySchedules,  requestCallback)
```
**Đối số** :
- **smartId**: ```String``` ID của smart
- **smartScheduleId**: ```String``` ID  của IoTSmartSchedule
- **timeSchedule**: ```Integer``` Thời điểm kích hoạt trong ngày (tính bằng phút)
    + Ví dụ : Lập lịch vào 18h30p = 1110p
- **daySchedules**: ```IntArray``` Ngày trong tuần
    + Từ Chủ nhật đến Thứ 7 tương ứng từ 0 đến 6
    + Ví dụ: Thiết lập lập lịch vào thứ 2, 4, Chủ nhật sẽ là: [1, 3, 0]
- **requestCallback**: ```RequestCallback<IoTSmart>```

### 2.4. Xóa hẹn giờ khỏi lập lịch
```kotlin
handler.unboundSmartSchedule(smartScheduleId, requestCallback)
```
**Đối số** :
- **smartScheduleId**: ```String``` ID  của IoTSmartSchedule
- **requestCallback**: ```RequestCallback<IoTSmartSchedule>```

### 2.5. Lấy thông tin của một hẹn giờ trong lập lịch
```kotlin
val ioTSmartSchedule: IoTSmartSchedule = handler.getSmartScheduleItem(smartScheduleId)
```
**Đối số** :
- **smartScheduleId**: ```String``` ID  của IoTSmartSchedule

### 2.6. Lấy danh sách các hẹn giờ trong lập lịch
```kotlin
handler.getSmartSchedule(smartId)
```
**Đối số** :
- **smartId**: ```String``` ID  của IoTSmart








