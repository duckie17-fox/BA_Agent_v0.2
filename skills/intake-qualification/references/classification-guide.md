# Classification Guide

## New Feature (Mơ hồ)

Request is New Feature when it:
- Describes a **problem or pain point** without specifying the solution
- Uses language like: "khó khăn khi", "không thể", "cần cải thiện", "thiếu tính năng"
- Asks "how might we" or "is it possible to"
- Describes a user outcome, not a UI change

**Examples:**
- "CS team mất nhiều thời gian tra cứu trạng thái yêu cầu" → New Feature
- "Cần cách nào đó để khách hàng tự theo dõi đơn hàng" → New Feature

## Change Request (Áp đặt)

Request is Change Request when it:
- Specifies a **concrete change** to existing functionality
- Uses language like: "thêm button", "sửa field", "đổi logic", "tích hợp với"
- References a specific screen, component, or workflow that already exists
- Is framed as "build X" rather than "solve Y"

**Examples:**
- "Thêm button export Excel vào màn hình báo cáo" → Change Request
- "Đổi default value của dropdown Trạng thái từ Bản nháp sang Đang xử lý" → Change Request

## Ambiguous

Flag as ambiguous when the request mixes both — describes a problem but also
prescribes a specific solution. Note the ambiguity and ask BA to confirm
which lens to use: explore problem space (New Feature) or evaluate the
proposed solution (Change Request).
