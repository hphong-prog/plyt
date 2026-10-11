# Plyt – cải thiện luồng Join Shared Plan

Files đã sửa: `index.html` (desktop) và `m.html` (mobile) – cùng một bản vá, script giống hệt nhau.
Không đổi: Firestore schema, đường dẫn tài liệu, security rules, cơ chế merge/sync, `firestore.rules`, `README.md`.

## Nguyên nhân gốc
Ô tên trong modal Join được điền sẵn `value="${esc(ME)}"` (ME mặc định là Pablo, đọc từ localStorage), và `finishJoin`
ghi đè `ME` của cả thiết bị. Vì vậy người mới có thể bấm Join và vô tình mang tên Pablo.

## Modal Join mới
- Tiêu đề `Join “[Tên plan]”`; mô tả “Choose your name to join this trip. Select an existing travel-mate or enter a new name.”
- Pills: owner (có nhãn OWNER) + các travel-mates. **Không chọn sẵn tên nào**, ô nhập trống, placeholder `Enter your name`,
  không đọc `ME`/localStorage.
- Bấm pill → điền tên + tô pill (kiểu `.mc.on` hiện có). Gõ tay → bỏ chọn pill cũ; nếu tên gõ trùng một thành viên
  (không phân biệt hoa/thường) thì pill đó được tô và dòng gợi ý ghi “You will join as …”.
- Join chỉ chạy khi có tên hợp lệ. Nút mờ (`aria-disabled`) nhưng vẫn bấm được để hiện lý do, không chặn câm.
- Kiểm tra: cắt khoảng trắng đầu/cuối; không rỗng/chỉ khoảng trắng; tối đa 40 ký tự; so trùng không phân biệt hoa/thường
  (Minh = minh = MINH → dùng thành viên có sẵn, giữ nguyên cách viết gốc); không tự tạo “Minh 2”.
- Footer của modal Join dính đáy (sticky) nên nút Join luôn thấy trên màn hình nhỏ; Enter trong ô tên = Join.

## Logic danh tính
- Thành viên có sẵn → không thêm vào `mates`, không tạo bản ghi trùng.
- Tên mới → thêm vào `mates` của plan; thay đổi này đi qua đúng cơ chế đồng bộ/merge hiện có lên Firestore.
- Owner và tên các thành viên khác không bị đổi.
- Phương án kiến trúc (ít ảnh hưởng nhất): giữ `ME` toàn thiết bị cho lời chào/avatar/owner plan mới; thêm bảng
  **danh tính theo plan** lưu riêng ở localStorage `plyt-me-plans` (`{planId: tên}`) – KHÔNG nằm trong dữ liệu plan, nên không
  đổi cấu trúc dữ liệu, không được đồng bộ, không ảnh hưởng merge. `meOf(plan)` = danh tính trong plan, mặc định về `ME`
  (tương thích các plan đã join trước đây).
- Khi join: lưu danh tính cho plan đó; chỉ khi thiết bị chưa từng xác nhận tên (chưa có `plyt-me`) mới đặt luôn `ME`
  (để lời chào không còn là “Pablo” mặc định). Thiết bị đã lưu Pablo vẫn giữ Pablo cho các plan của mình.
- Budget dùng `meOf(plan)`: người trả mặc định, nhãn “(you)”, mặc định của Record payment. Đổi tên ở “Your name” cũng cập nhật bảng này.
- Snapshot link (bản sao một lần): không hỏi tên, không lưu danh tính, hoạt động như trước.

## Follow-up: Share Plan modal spacing and repeat-join identity
- Tightened vertical spacing between Share Plan sections, labels, descriptions, action buttons and invite rows.
- Reduced unused padding at the bottom and removed the extra divider before the modal footer.
- Scoped the styling to the Share Plan modal so other dialogs are not affected.
- Kept the no-default-name Join flow, name validation, case-insensitive duplicate handling and per-plan identity storage.
- Fixed the edge case where an invite is opened for a shared plan already present on the same device: the confirmed identity is now saved to that plan; if it is a new travel-mate, the name is added and normal sync is triggered.
- Static JavaScript syntax checks passed for desktop and mobile files. Live Firebase behavior still needs confirmation against the user's Firebase project.
