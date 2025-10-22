#

### 2.1. 
```kotlin
SmartSdk.featureHandler().runFeatureMethod(
    getOrSet,
    devId,
    feature,
    functionLabel,
    featureValue,
    time,
    callback
)
```
**Đối số** :
- **getOrSet**: ```Boolean``` true: set, false: get
- **devId**: ```String``` uuid của thiết bị
- **feature**: ```int``` loại giao tiếp giữa ứng dụng và thiết bị
- **functionLabel**: ```String``` tên hàm
- **featureValue**: ```IoTFeature.FeatureValue``` nơi khai báo các biến gửi tới thiết bị
- **time**:```int``` thời gian timeout
- **callback**: ```FeatureRequestCallback``` 

#### 2.1.1. Cách khai báo biến trong FeatureValue
Dưới đây là cách khai báo một biến có kiểu String
```kotlin
new IoTFeature.FeatureValue() {
    @IoTInvokingProperty("flowBindingId")
    private String flowBindingId = bindingId;
}
```