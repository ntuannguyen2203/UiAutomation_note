```markdown
### **🔹 Những thông số cố định và không cố định sau mỗi lần mở ứng dụng**

Khi tự động hóa giao diện ứng dụng bằng **UIAutomation**, một số thuộc tính của **control (widget)** có thể thay đổi sau mỗi lần mở ứng dụng, trong khi một số khác luôn cố định.

---

## **✅ 1. Các thông số cố định (Stable Attributes)**
Những thông số này thường **không thay đổi** sau mỗi lần mở ứng dụng, giúp xác định control chính xác hơn.

### **1️⃣ AutomationId (Thường cố định)**
- Được đặt bởi lập trình viên khi phát triển ứng dụng.
- Thường không thay đổi giữa các phiên bản và lần chạy khác nhau.
- **Ưu tiên sử dụng nếu có**.

```python
button = app.ButtonControl(AutomationId="btnLogin")
```

---

### **2️⃣ ClassName (Cố định nếu không cập nhật UI)**
- Định danh loại control, ví dụ như `"Button"`, `"Edit"`, `"List"`, `"TreeItem"`, v.v.
- **Thường cố định**, nhưng có thể thay đổi nếu ứng dụng cập nhật UI hoặc dùng framework UI khác.

```python
input_box = app.EditControl(ClassName="TextBox")
```

---

### **3️⃣ Name (Có thể cố định, nhưng cũng có thể thay đổi)**
- **Tên hiển thị của control trên giao diện**.
- Nếu ứng dụng hỗ trợ nhiều ngôn ngữ, **tên có thể thay đổi** khi đổi ngôn ngữ.
- **Không nên dựa vào duy nhất nếu ứng dụng có thể thay đổi ngôn ngữ hoặc dữ liệu động**.

```python
submit_button = app.ButtonControl(Name="Submit")
```

⏩ **Trường hợp cố định**: `"OK"`, `"Submit"`, `"Cancel"`  
⏩ **Trường hợp thay đổi**: `"Hôm nay"`, `"Kết quả ngày 12/2/2024"`

---

### **4️⃣ ControlType (Thường cố định)**
- Kiểu control như **Button, Edit, CheckBox, ListItem, TreeItem...**  
- **Luôn ổn định**, trừ khi ứng dụng thay đổi framework UI.

```python
checkbox = app.CheckBoxControl(ControlType="CheckBox")
```

---

### **5️⃣ Window Name (Cố định nếu ứng dụng không đổi tiêu đề)**
- Tên cửa sổ ứng dụng.
- **Thường cố định**, nhưng có thể thay đổi theo nội dung bên trong.

```python
app = auto.WindowControl(Name="My Application v1.2.3")
```

⏩ Nếu phiên bản ứng dụng thay đổi (`v1.2.3` → `v1.2.4`), **tên cửa sổ có thể đổi**.

---

## **❌ 2. Các thông số có thể thay đổi (Unstable Attributes)**
Dưới đây là những thông số **không ổn định**, tránh dùng để tìm kiếm control.

### **1️⃣ NativeWindowHandle (HWND)**
- Mỗi lần mở ứng dụng, giá trị này thay đổi.
- Chỉ nên dùng **để thao tác với cửa sổ hiện tại**, không dùng để tìm control.

```python
app = auto.WindowControl(NativeWindowHandle=12345678)
```

---

### **2️⃣ ProcessId (PID)**
- **Thay đổi mỗi lần khởi chạy ứng dụng**.
- Có thể dùng để xác định ứng dụng đang chạy, nhưng **không ổn định để tìm control**.

```python
app = auto.WindowControl(ProcessId=5678)
```

---

### **3️⃣ BoundingRectangle (Tọa độ trên màn hình)**
- **Thay đổi khi cửa sổ di chuyển hoặc có độ phân giải khác**.
- Không nên dùng trừ khi cần click chính xác vị trí.

```python
control = auto.Control(BoundingRectangle=(100, 200, 300, 400))
```

---

## **🔹 3. Cách tìm control ổn định nhất**
Nếu **AutomationId** có sẵn, sử dụng nó trước tiên. Nếu không có, hãy kết hợp **ClassName, ControlType và Name** để tìm control chính xác.

```python
button = app.ButtonControl(AutomationId="btnSubmit") or \
         app.ButtonControl(Name="Submit", ClassName="Button")
```

---

## **🚀 Kết luận**
| **Thông số**               | **Cố định?** | **Có nên dùng?** | **Ghi chú** |
|---------------------------|-------------|------------------|-------------|
| **AutomationId**          | ✅ Cố định  | ✅ Rất nên dùng  | Ổn định nhất nếu có |
| **ClassName**             | ✅ Cố định  | ✅ Có thể dùng   | Ổn định, nhưng có thể thay đổi nếu UI cập nhật |
| **ControlType**           | ✅ Cố định  | ✅ Có thể dùng   | Ổn định, nhưng không đủ để tìm duy nhất |
| **Name**                  | ⚠ Có thể đổi | ✅ Cẩn thận khi dùng | Nếu ứng dụng đa ngôn ngữ, có thể thay đổi |
| **Window Name**           | ⚠ Có thể đổi | ✅ Cẩn thận khi dùng | Nếu có phiên bản trong tiêu đề, có thể thay đổi |
| **ProcessId (PID)**       | ❌ Thay đổi  | ❌ Không nên dùng | Mỗi lần mở ứng dụng có thể khác nhau |
| **NativeWindowHandle**    | ❌ Thay đổi  | ❌ Không nên dùng | Chỉ nên dùng để thao tác cửa sổ |
| **BoundingRectangle**     | ❌ Thay đổi  | ❌ Không nên dùng | Thay đổi khi cửa sổ di chuyển |

---

Nếu ứng dụng của bạn có **AutomationId**, hãy **luôn sử dụng nó** vì đây là cách ổn định nhất! 🚀
```
