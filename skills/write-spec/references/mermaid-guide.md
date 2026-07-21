# Mermaid Guide — Workflow Diagram Patterns

## Khi nào dùng loại diagram nào

| Loại | Dùng khi | Ví dụ |
|------|---------|-------|
| `flowchart TD` | Mô tả luồng hành động của 1 người dùng | Tạo mới chương trình |
| `sequenceDiagram` | Mô tả tương tác giữa nhiều thành phần (user, system, API) | Xác thực login, gọi API |
| `flowchart TD` + subgraph | Nhiều actor thực hiện song song | BA → Engineer → QA workflow |

---

## Flowchart — Pattern cơ bản

Dùng cho Basic Flow trong spec.

```mermaid
flowchart TD
    Start([Bắt đầu]) --> Step1[Bước 1 — User action]
    Step1 --> Step2[Bước 2 — System response]
    Step2 --> Valid{Điều kiện?}
    Valid -->|Có| Happy[Happy path action]
    Happy --> End([End])
    Valid -->|Không| Error[Error handling]
    Error --> Fix[User sửa lại]
    Fix --> Step2
```

## Flowchart — Có nhiều nhánh

```mermaid
flowchart TD
    Start([Truy cập màn hình Danh sách]) --> Add[Chọn Thêm mới]
    Add --> Type{Loại chương trình?}
    Type -->|On-invoice| OnInv[Hiển thị màn hình Tạo mới On-invoice]
    Type -->|Off-invoice| OffInv[Hiển thị màn hình Tạo mới Off-invoice]
    OnInv --> Form[Nhập thông tin tab Thông tin chung]
    OffInv --> Form
    Form --> Save[Nhấn Lưu]
    Save --> Valid{Tất cả trường bắt buộc đã điền?}
    Valid -->|Có| Create[Hệ thống tạo mới chương trình]
    Create --> Toast[Hiển thị thông báo Tạo mới thành công]
    Toast --> Edit[Chuyển sang màn hình Chỉnh sửa]
    Edit --> End([End])
    Valid -->|Không| Highlight[Highlight lỗi tại từng trường thiếu]
    Highlight --> Form
```

---

## Flowchart — Luồng login (ví dụ có nhiều failure path)

```mermaid
flowchart TD
    Start([Truy cập trang đăng nhập]) --> Show[Hệ thống hiển thị form login]
    Show --> Input[Người dùng nhập Email + Password]
    Input --> Submit[Nhấn nút Đăng nhập]
    Submit --> Auth{Xác thực hợp lệ?}
    Auth -->|Có| Token[Tạo session token]
    Token --> Redirect[Redirect đến dashboard]
    Redirect --> Toast[Hiển thị toast thành công]
    Toast --> End([End])
    Auth -->|Không| Lock{Sai quá 5 lần / 15 phút?}
    Lock -->|Có| Block[Khóa tài khoản tạm thời 15 phút]
    Block --> EndLock([End])
    Lock -->|Không| ErrMsg[Hiển thị lỗi Tên đăng nhập hoặc mật khẩu không chính xác]
    ErrMsg --> Input
```

---

## Sequence Diagram — User và System

Dùng khi cần mô tả tương tác giữa User, Frontend, Backend, Database.

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant BE as Backend API
    participant DB as Database

    User->>FE: Nhấn Lưu
    FE->>FE: Validate form phía client
    FE->>BE: POST /api/programs {payload}
    BE->>BE: Validate business rules
    BE->>DB: INSERT INTO programs
    DB-->>BE: success
    BE-->>FE: 201 Created {program_id}
    FE-->>User: Hiển thị "Tạo mới thành công"
    FE->>FE: Chuyển sang màn hình Chỉnh sửa
```

## Sequence Diagram — Có error case

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant BE as Backend API
    participant DB as Database

    User->>FE: Nhấn Lưu
    FE->>BE: POST /api/programs {payload}
    BE->>DB: CHECK duplicate program_code
    DB-->>BE: code đã tồn tại

    alt Mã chương trình trùng
        BE-->>FE: 422 Unprocessable {error: "Mã chương trình đã tồn tại"}
        FE-->>User: Hiển thị lỗi inline tại trường Mã chương trình
    else Mã chương trình hợp lệ
        BE->>DB: INSERT INTO programs
        DB-->>BE: success
        BE-->>FE: 201 Created
        FE-->>User: "Tạo mới thành công"
    end
```

---

## Flowchart với subgraph — Nhiều actor

Dùng khi workflow đi qua nhiều người (Trade Marketer → Manager → System).

```mermaid
flowchart TD
    subgraph TM [Trade Marketer]
        T1[Tạo mới chương trình Off-invoice]
        T1 --> T2[Điền thông tin và nhấn Lưu]
        T2 --> T3[Chương trình ở trạng thái Bản nháp]
    end

    subgraph MGR [Manager]
        M1[Nhận notification chương trình mới]
        M1 --> M2[Review thông tin chương trình]
        M2 --> Approve{Đồng ý duyệt?}
        Approve -->|Có| M3[Nhấn Duyệt]
        Approve -->|Không| M4[Nhấn Từ chối + nhập lý do]
    end

    subgraph SYS [System]
        S1[Cập nhật trạng thái = Đã duyệt\nGửi notification cho Trade Marketer]
        S2[Cập nhật trạng thái = Từ chối\nGửi notification cho Trade Marketer]
    end

    T3 --> M1
    M3 --> S1
    M4 --> S2
    S2 --> T4[Chỉnh sửa và submit lại]
    T4 --> M1
```

---

## Tips

- Mỗi node viết ngắn gọn — không quá 1 dòng
- Dùng tiếng Việt trong node labels để thống nhất với document
- Happy path luôn là nhánh chính, error là nhánh phụ
- Với workflow phức tạp (>10 bước): tách thành 2 diagram — overview và detail
- Dùng `([...])` cho Start/End nodes, `{...}` cho decision nodes, `[...]` cho action nodes
- Sau diagram, thêm mô tả text ngắn giải thích các nhánh quan trọng nếu cần