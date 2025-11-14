# Quản lí địa điểm
## 1. Giới thiệu hàm
- all(): Lấy ra danh sách địa điểm mà người dùng đã tạo.
- setAppLocation(): Luôn phải có một địa điểm làm địa điểm mặc định. Nếu tài khoản chưa có địa điểm nào thì bắt buộc phải tạo địa điểm để đặt làm mặc định rồi mới có thể tiếp tục.
- createLocation(): Người dùng muốn tạo thêm một địa điểm mới.
- updateLocation(): Người dùng có thể cập nhật tên địa điểm hoặc kiểu địa điểm tùy theo mục đích sử dụng.
- delete(): Có thể xóa địa điểm khi không muốn sử dụng nữa. Khi đó phải xóa tất cả các thiết bị đã được kết nối thuộc địa điểm đó.

## 2. Sử dụng hàm
### 2.1. Tạo một địa điểm mới
```kotlin
SmartSdk.locationHandler().createLocation(label, desc, requestCallback)
```
**Đối số** :
- **label**: ```String```: Tên địa điểm
- **desc**: String - Mô tả kiểu địa điểm
- **requestCallback**:  callback trả về ```RequestCallback<IoTLocation>```

### 2.2. Lấy danh sách địa điểm
```kotlin
SmartSdk.locationHandler().all
```

### 2.3. Thiết lập địa điểm làm địa điểm mặc định
```kotlin
SmartSdk.setAppLocation(locationId)
```
**Đối số**:
- **locationId**: ```String``` id của địa điểm muốn sử dụng

### 2.4. Lấy uuid của địa điểm mặc định hiện tại
```kotlin
val currentLocation = SmartSdk.getAppLocation()
```
**Chú ý**:
- Nếu currentLocation trả về null nghĩa là chưa có địa điểm nào được thiết lập làm địa điểm mặc định

### 2.5. Cập nhật địa điểm
```kotlin
SmartSdk.locationHandler().updateLocation(id ,label, desc, requestCallback)
```
**Đối số** :
- **id**: ```String``` - Id của location cần update
- **label**: ```String``` - Tên mới của địa điểm
- **desc**: ```String``` - Mô tả kiểu địa điểm mới
- **requestCallback**: callback kiểu dữ liệu ```RequestCallback<IoTLocation>```

### 2.6. Xóa địa điểm
```kotlin
SmartSdk.locationHandler().delete(id, requestCallback)
```
**Đối số** :
- **id**: String - Id của location cần update
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<Boolean>```
