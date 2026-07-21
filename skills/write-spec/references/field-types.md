# Field Types — Standard patterns per field type

All constraint descriptions must be written in **Vietnamese** in the output.

---

## Textbox (Text input)

**"Loại thông tin" column value:** `Textbox`

**Constraint pattern:**
```
• **Trường bắt buộc nhập**
  • Trường hợp không nhập mà nhấn lưu, hệ thống thông báo lỗi "[Message]" tại trường.
• Chỉ được nhập tối đa [N] ký tự
  • Khi nhập đủ ký tự thứ [N], hệ thống không cho nhập tiếp
• [Disallowed characters if any]
• [Unique constraint if any]: "[Error message when duplicate]"
• **Không được** chỉnh sửa ở màn hình [screen name] (if readonly in certain states)
```

**Example — Program code field:**
```
• **Trường bắt buộc nhập**
  • Trường hợp không nhập mà nhấn lưu, hệ thống thông báo lỗi "Vui lòng nhập Mã chương trình" tại trường.
• Không được phép nhập khoảng trắng, tiếng việt có dấu, các ký tự đặc biệt trừ: - _ /\
• Chỉ được nhập tối đa 50 ký tự
• Mã chương trình bắt buộc phải unique trên toàn hệ thống
  • Trường hợp trùng mã, hệ thống hiển thị thông báo lỗi "Mã chương trình đã tồn tại"
• **Không được** chỉnh sửa ở màn hình chi tiết
```

---

## Dropdown list

**"Loại thông tin" column value:** `Dropdown list`

**Constraint pattern:**
```
• Có [N] giá trị:
  • [Value 1]
  • [Value 2]
  • [Value N]
• Khi truy cập màn hình [Screen name], trường thông tin [Field name] mặc định chọn giá trị [Default value]
• [Required if applicable]
  • Trường hợp không chọn mà nhấn lưu, hệ thống thông báo lỗi "[Message]" tại trường.
• [Enable/disable conditions if any]
• Khi chọn vào, khung dropdown list được viền đậm
```

**Example — Program status:**
```
• Có 4 trạng thái chương trình:
  • Bản nháp (mặc định)
  • Đã duyệt
  • Từ chối
  • Kết thúc
• Trường disable ở màn hình Tạo mới và Chỉnh sửa
• Khi truy cập màn hình Tạo mới, trường mặc định hiển thị giá trị Bản nháp
```

---

## Switch (Toggle)

**"Loại thông tin" column value:** `Switch`

**Constraint pattern:**
```
• Hệ thống mặc định chọn [Active/Un-active]
• [Enable condition]: Hệ thống hiển thị trạng thái [Active] khi:
  1. [Condition 1]
  2. [Condition 2]
• [Disable condition if any]
```

**Example — Activity status:**
```
• Hệ thống mặc định chọn Un-active và disable
• Hệ thống hiển thị trạng thái Active khi:
  1. Trạng thái chương trình = Đã duyệt
  2. Ngày bắt đầu =< Ngày hiện tại =< Ngày kết thúc
```

---

## Date picker

**"Loại thông tin" column value:** `Date picker`

**Constraint pattern:**
```
• **Trường bắt buộc chọn**
  • Trường hợp không chọn mà nhấn lưu, hệ thống thông báo lỗi "[Message]" tại trường.
• Ngày bắt đầu phải lớn hơn hoặc bằng ngày hiện tại
  • Trường hợp ngày bé hơn ngày hiện tại thì disable không chọn được
• Phải chọn đầy đủ ngày bắt đầu và ngày kết thúc
• Ngày kết thúc phải lớn hơn ngày bắt đầu
• Date picker focus vào ngày, tháng, năm hiện tại
```

---

## Button (Action)

**"Loại thông tin" column value:** `Button`

**Constraint pattern:**
```
• Khi [action], hệ thống [behavior]
  • [Sub-behavior 1]
  • [Sub-behavior 2]
• Button sẽ **hiển thị và cho chọn** khi [condition]
• Button sẽ **ẩn khỏi màn hình** khi [condition]
• Button sẽ **disable** khi [condition if applicable]
```

**Example — Save button:**
```
• Tại màn hình Tạo mới khi bấm chọn, hệ thống kiểm tra các điều kiện ràng buộc tại từng trường và tạo mới khi thỏa toàn bộ điều kiện.
• Khi lưu thành công hệ thống hiển thị thông báo "Tạo mới thành công"
```

**Example — Button with show/hide conditions:**
```
• Button sẽ **hiển thị và cho chọn** khi:
  • Trạng thái kế hoạch là Mới
  • Tab Thông tin chung đang hiển thị màn hình Chi tiết
• Button sẽ **ẩn khỏi màn hình** khi:
  • Trạng thái kế hoạch là Thực thi, Dừng đánh giá
  • Tab Thông tin chung đang hiển thị màn hình Tạo mới, Chỉnh sửa
```

---

## Text (Display only)

**"Loại thông tin" column value:** `Text`

**Constraint pattern:**
```
• Hệ thống tự động hiển thị theo [condition/logic]
• Không cho chỉnh sửa
• Hiển thị theo định dạng: [format if applicable]
```

**Example — Plan status:**
```
• Hệ thống sẽ tự động hiển thị theo các điều kiện:
  • Kế hoạch đang được tạo mới: Mới
  • Kế hoạch đã được phê duyệt: Thực thi
  • Kế hoạch được dừng: Dừng đánh giá
• Không cho chỉnh sửa
```

---

## Chart (Line)

**"Loại" column value:** `Chart (Line)`

Dùng cho biểu đồ đường thể hiện trend theo thời gian.

**Constraint pattern:**
```
• Trục X: [đơn vị thời gian — ngày/tuần/tháng]
• Trục Y: [metric — Cases hoặc VND]
• Toggle [Cases / VND]: chuyển đổi metric hiển thị
• Hover vào điểm dữ liệu: hiển thị tooltip chi tiết ([date], [value])
• Click vào điểm: [hành động — drill-down / không có]
• Khi không có data: hiển thị trạng thái empty ("Không có dữ liệu")
```

**Example:**
```
• Trục X: ngày trong kỳ báo cáo
• Trục Y: Số cases hoặc VND tùy toggle
• Toggle Cases / VND ở góc trên phải: chuyển đổi metric
• Hover: tooltip hiển thị ngày và giá trị
• Khi không có data: hiển thị "Không có dữ liệu cho kỳ đã chọn"
```

---

## Chart (Bar) / Chart (Bar - horizontal)

**"Loại" column value:** `Chart (Bar)` hoặc `Chart (Bar - horizontal)`

Dùng cho biểu đồ cột so sánh các nhóm (region, SKU, brand...).

**Constraint pattern:**
```
• Mỗi cột đại diện cho [đơn vị — Region / SKU / Brand...]
• Giá trị: [metric]
• Sắp xếp: [giảm dần / theo alphabet / theo hierarchy]
• Hover: tooltip hiển thị [tên] và [giá trị]
• Click vào cột: drill-down xuống [level tiếp theo]
• Giới hạn hiển thị: tối đa [N] cột (VD: Top 10)
• Khi không có data: hiển thị trạng thái empty
```

**Example — Top 10 SKU (horizontal):**
```
• Hiển thị top 10 SKU theo sell-out cases, sắp xếp giảm dần
• Label: "[Brand] - [Flavor] - [Pack]" (VD: Mirinda - Green Cream - PET 390)
• Hover: tooltip hiển thị tên SKU đầy đủ và giá trị
• Click vào bar: drill-down xem chi tiết SKU đó
```

---

## Table (Data table)

**"Loại" column value:** `Table`

Dùng cho bảng dữ liệu có sort, pagination, drill-down.

**Constraint pattern:**
```
• Cột hiển thị: [danh sách cột]
• Sort: click vào header cột để sort tăng/giảm dần. Mặc định sort theo [cột]
• Phân trang: [N] rows/trang. Footer hiển thị "Hiển thị [from]-[to] của [total]"
• Hover row: highlight row
• Click row: [hành động — drill-down / xem chi tiết / không có]
• Số âm/giảm: hiển thị màu đỏ (#EB5757). Số dương/tăng: hiển thị màu xanh (#27AE60)
• Cột số: căn phải
• Khi không có data: hiển thị "Không có dữ liệu"
```

**Example:**
```
• Cột: Tên | Sell-out (Cases) | Sell-out (VND) | % vs kỳ trước | % Đóng góp
• Sort: click header, mặc định sort theo Sell-out (Cases) giảm dần
• Phân trang: 50 rows/trang. Footer: "Hiển thị 1-50 của 128 < 1 2 3 >"
• Click row: drill-down vào level tiếp theo trong hierarchy
• % vs kỳ trước: dương → xanh, âm → đỏ
```

---

## General rules

### Validation error messages
Standard format: `"Vui lòng nhập [Field name]"` or `"Vui lòng chọn [Field name]"`

### Required fields
Always bold: `**Trường bắt buộc nhập**` or `**Bắt buộc chọn**`

### Conditional logic — AND conditions
Use numbered list:
```
Hệ thống [action] khi:
  1. [Condition 1]
  2. [Condition 2]
```

### Conditional logic — OR conditions
Use bullets:
```
Hệ thống [action] khi thỏa 1 trong các điều kiện:
  • [Condition 1]
  • [Condition 2]
```

### Popups
Popup gets its own separate field description table, placed after the main screen table:
`Bảng mô tả trường thông tin **Popup [Popup name]**`
