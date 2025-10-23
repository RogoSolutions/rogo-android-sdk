# Smart
## 1. Giới thiệu chung
- all(): Lấy toàn bộ danh sách các smart
- getSmartByType(): Lấy danh sách smart dựa theo loại
- delete(): xóa một smart
- setSmartLabel(): thiết lập nhãn cho smart

## 2. Sử dung
### 2.1. Lấy danh sách các smart
```kotlin
handler.all
```

### 2.2. Lấy danh sách smar dựa theo loại
```kotlin
handler.getSmartByType(type)
```
**Đối số** :
- **type**: ```Integer``` loại Smart
    + Có thể lấy ở class IoTSmartType.
  
      Bao gồm:

      IoTSmartType.TYPE_SCENE,
  
      IoTSmartType.TYPE_SCHEDULE,
  
      IoTSmartType.TYPE_AUTOMATION

### 2.3. Xóa một smart
```kotlin
handler.delete(smartId, requestCallback)
```
**Đối số** :
- **smartId**: ```String``` ID của smart muốn xóa
- **requestCallback**: ```RequestCallback<Boolean>```

### 2.4. Thiết lập nhãn cho smart
```kotlin
handler.setSmartLabel(smartId, label, requestCallback)
```
**Đối số** :
- **smartId**: ```String``` ID của Smart
- **label**: ```String``` Nhãn mới muốn cập nhật cho smart
- **requestCallback**: ```RequestCallback<IoTSmart>```



