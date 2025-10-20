# Authentication

## 1. Giới thiệu hàm
- connectService(): Là hàm được gọi đến đầu tiên mỗi khi khởi tạo Application,  được sử dụng để kết nối đến service của Rogo, thiết lập kết nối MQTT. Ở hàm này, còn được dùng để thể kiểm tra hiện tại có người dùng nào đang đăng nhập vào ứng dụng hay không.
- closeConnectionService(): Khi kết nối tới service không thành công, thực hiện hủy kết nối và thử lại.
- signUp(): Tạo một tài khoản cho người dùng mới.
- signIn(): Người dùng đăng nhập để bắt đầu sử dụng ứng dụng.
- forgot(): Gọi tới khi người dùng muốn cập nhật mật khẩu.
- updatePasswordOrVerifyAccount(): Sau khi thực hiện “Đăng kí” hoặc “Quên mật khẩu”, người dùng sẽ nhận được một mã OTP qua Gmail. Khi đó, cần xác thực mã OTP để xác nhận hoàn tất đăng kí hay cập nhật mật khẩu mới cho người dùng.

## 2. Sử dụng
### 2.1. Kết nối service
```kotlin
val callback = object : SmartSdkConnectCallback {
            	override fun onConnected(isAuthenticated: Boolean) {
			// isAutheticated để chỉ người dùng đã đăng nhập hay chưa
		}

            	override fun onDisconnected() {}
        		}
```
```kotlin
SmartSdk.connectService(callback)
```
**Chú thích**:
- Mỗi khi gọi SmartSdk.connectService(smartSdkConnectCallback) thì callback trước đó sẽ bị xoá
    + **onConnected**: gọi khi đã kết nối service thành công
    + **onDisconnected**: Khi hàm này được gọi, hãy đăng xuất khỏi ứng dụng
- Nếu xác định rằng người dùng đã đăng nhập rồi, nên kiểm tra hiện tại đã có địa điểm nào được thiết lập làm địa điểm mặc định chưa từ hàm SmartSdk.getApplication(). Nếu đã thiết lập rồi thì có thể bắt đầu sử dụng các tính năng tiếp theo thay vì thiết lập địa điểm mặc định một lần nữa. Điều này có thể giúp người dùng tránh phải chọn lại địa điểm mỗi lần đăng nhập hay sử dụng ứng dụng. 
### 2.2. Hủy kết nối
```kotlin
SmartSdk.closeConnectionService()
```
### 2.3. Đăng ký
```kotlin
SmartSdk.signUp(username, email, phonenumber, password, authRequestCallback)
```
**Đối số**:
- **username**: ```String``` Username của người dùng
- **email**: ```String``` : Email của người dùng
- **phonenumber**: ```String``` : Số điện thoại của người dùng
- **password**: ```String``` : Mật khẩu của người dùng
- **authRequestCallback**: Đối tượng callback có kiểu dữ liệu ```AuthRequestCallback```

**Chú thích**:
- Khi tạo mới một tài khoản, có thể đăng kí với username, email, phonenumber, password hoặc chỉ cần email, password. Khi đó, các biến còn lại truyền null.
### 2.4. Đăng nhập
#### 2.4.1. Đăng nhập với username hoặc email
```kotlin
SmartSdk.signIn(username, email, phonenumber, password, authRequestCallback)
```
**Đối số** :
- **username**: ```String```
- **email**: ```String```
- **phonenumber**: ```String```
- **password**: ```String```
- **authRequestCallback**: Đối tượng callback có kiểu dữ liệu ```AuthRequestCallback```

**Chú thích**:
- Có thể đăng nhập với username, email, phonenumber, password hoặc chỉ cần email, password. Khi đó, các biến còn lại truyền null.

#### 2.4.2. Đăng nhập với token
```kotlin
SmartSdk.signIn(loginToken, authRequestCallback)
```
**Đối số** :
- **loginToken**: ```String```:
- **authRequestCallback**: Đối tượng callback có kiểu dữ liệu ```AuthRequestCallback```

**Chú thích**:
- Có thể đăng nhập với username, email, phonenumber, password hoặc chỉ cần email, password. Khi đó, các biến còn lại truyền null.

### 2.5. Quên mật khẩu
```kotlin
SmartSdk.forgot(email, authRequestCallback)
```
**Đối số** :
- **email**: ```String```  
- **authRequestCallback**: Đối tượng callback có kiểu dữ liệu ```AuthRequestCallback```

### 2.6. Cập nhật mật khẩu hoặc xác thực tài khoản khi đăng kí
```kotlin
SmartSdk.updatePasswordOrVerifyAccount(code, password, authRequestCallback)
```
**Đối số** :
- **code**: ```String```: Mã OTP được gửi về qua Gmail
- **password**: ```String``` Mật khẩu muốn cập nhật
- **authRequestCallback**: Đối tượng callback có kiểu dữ liệu ```AuthRequestCallback```

**Chú thích**:
- Biến "password" được sử dụng khi muốn cập nhật mật khẩu, trong trường hợp đăng kí thì truyền null.

