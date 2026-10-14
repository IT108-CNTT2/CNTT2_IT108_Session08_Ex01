# Báo cáo xử lý sự cố tệp nhật ký (Log) lớn

## Phần 1: Phân tích

### 1. Nguyên nhân Notepad bị "Not Responding"
- **Tràn bộ nhớ RAM:** Notepad là ứng dụng giao diện đồ họa (GUI) có cơ chế đọc toàn bộ nội dung tệp từ ổ cứng và **tải thẳng vào RAM** dưới dạng cấu trúc văn bản trước khi hiển thị lên màn hình.
- Với tệp log nặng tới **800MB**, lượng dữ liệu này vượt quá khả năng xử lý tạm thời của bộ nhớ hoặc tiến trình ứng dụng, khiến CPU và RAM bị nghẽn cổ chai, dẫn đến việc ứng dụng bị treo cứng ("Not Responding").

### 2. Nguyên nhân lệnh `cat` lỗi trên CMD và hoạt động trong PowerShell
- **Trên Windows CMD (Command Prompt truyền thống):** Lệnh `cat` hoàn toàn không tồn tại (CMD dùng lệnh `type`). Khi gõ `cat`, hệ thống không tìm thấy chương trình tương ứng nên báo lỗi không nhận diện lệnh.
- **Trên PowerShell:** Lệnh `cat` hoạt động ngon lành vì đây thực chất là một **Alias (bí danh)** mặc định của hệ thống trỏ đến lệnh chính chủ **`Get-Content`** (tương tự các lệnh `ls`, `cp`, `mv` từ hệ sinh thái Linux được PowerShell hỗ trợ sẵn).

---

## Phần 2: Hoàn thiện (Bộ lệnh chuẩn trên PowerShell)

Để xử lý tệp log lớn an toàn và chuyên nghiệp trong ca trực đêm, ta sử dụng các câu lệnh PowerShell sau:

### 1. Xem toàn bộ nội dung tệp (có phân trang để tránh trôi màn hình)
```powershell
Get-Content server_log_800MB.log | Out-Host -Paging
