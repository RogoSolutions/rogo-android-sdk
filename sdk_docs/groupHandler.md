# A.Quản lí nhóm phòng
## 1. Giới thiệu hàm
- all(): Lấy ra danh sách địa điểm mà người dùng đã tạo thuộc địa điểm đã được thiết lập làm địa điểm mặc định.
- createGroup(): Người dùng muốn tạo thêm một nhóm phòng mới.
- updateGroup(): Người dùng có thể cập nhật tên nhóm phòng hoặc kiểu nhóm phòng tùy theo mục đích sử dụng.
- delete(): Có thể xóa nhóm phòng khi không muốn sử dụng nữa. Khi đó phải xóa tất cả các thiết bị đã được kết nối thuộc nhóm phòng đó đó.

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.groupHandler()
```
### 2.1. Tạo một nhóm phòng mới
```kotlin
handler.createGroup(label, desc, requestCallback)
```
**Đối số** :
- **label**: ```String``` Nhãn của nhóm
- **desc**: ```String``` - Mô tả
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<IoTGroup>```

### 2.2. Lấy danh sách nhóm phòng
```kotlin
handler.all
```

### 2.3. Cập nhật nhóm phòng
```kotlin
handler.updateGroup(id ,label, desc, requestCallback)
```
**Đối số** :
- **id**: ```String``` - Id của location cần update
- **label**: ```String```:  Tên của nhóm phòng
- **desc**: ```String``` - Mô tả kiểu nhóm phòng
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<IoTGroup>```

### 2.4. Xóa nhóm phòng
```kotlin
handler.delete(id, requestCallback)
```
**Đối số** :
- **id**: ```String``` - Id của location cần xóa
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<Boolean>```

# B. Quản lí nhóm chức năng
## 1. Giới thiệu hàm
- groupCtls(): Lấy danh sách các nhóm chức năng đã được tạo.
- createGroupCtl(): Người dùng muốn tạo thêm một nhóm chức năng mới.
- bindGroupCtlMember(): Thêm thiết bị vào nhóm chức năng
- unbindGroupCtlMember(): Xóa thiết bị khỏi nhóm chức năng

## 2. Sử dụng
**Cú pháp chung**
```kotlin
val handler = SmartSdk.groupHandler()
```
### 2.1. Tạo một phòng mới
```kotlin
handler.createGroupCtl(label, requestCallback)
```
**Đối số** :
- **label**: ```String``` Tên của nhóm chức năng
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<IoTGroup>```

### 2.2. Lấy danh sách tất cả nhóm phòng
```kotlin
handler.groupCtls
```

### 2.3. Thêm thiết bị vào nhóm ảo
```kotlin
handler.bindGroupCtlMember(groupId, deviceId, elements, callback)
```
**Đối số** :
- **groupId**: ```String``` id của nhóm ảo
- **deviceId**: ```String``` id của thiết bị muốn thêm vào nhóm
- **elements**: ```IntArray``` giá trị của element muốn sử dụng cho nhóm ảo
- **callback**: ```RequestCallback<IoTGroupMember>```

### 2.4. Xóa thiết bị khỏi nhóm ảo
```kotlin
handler.unbindGroupCtlMember(groupId, deviceId, callback)
```
**Đối số** :
- **groupId**: ```String``` id của nhóm ảo
- **deviceId**: ```String``` id của thiết bị muốn thêm vào nhóm
- **callback**: ```SuccessStatus```

### 2.5. Xóa nhóm phòng
```kotlin
handler.delete(id, requestCallback)
```
**Đối số** :
- **id**: ```String``` - Id của location cần xóa
- **requestCallback**: Đối tượng callback có kiểu dữ liệu ```RequestCallback<Boolean>```
