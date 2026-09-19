# Quy chuẩn Clean Skill

## A. Nguyên tắc chung
1. Giữ nguyên nội dung và ý nghĩa. Chỉ sửa định dạng, lỗi rõ ràng và mâu thuẫn nội bộ.
2. Sửa tối thiểu. Không viết lại câu chữ nếu không sai.
3. Mọi thay đổi ngoài định dạng thuần phải được liệt kê ngắn gọn cho người dùng.

## B. Loại bỏ ký tự escape thừa (do copy/export)
- `\`` -> `` ` ``, `\*\*` -> `**`, `\|` -> `|`, `\-` -> `-`, `\_` -> `_`, `\<` `\>` -> `<` `>`, `\[` `\(` -> `[` `(`
- `1\.` -> `1.`, `\---` -> `---`
- Bỏ bold bọc quanh cú pháp Markdown (vd `**#**`, `**---**`, `**##**`).

## C. Frontmatter
- `---` mở/đóng nằm riêng một dòng, không dòng trống bên trong.
- Chỉ giữ `name` và `description`.
- `name`: kebab-case, khớp tên thư mục skill.
- `description`: gồm **làm gì** + **khi nào dùng** (trigger), trên một dòng.

## D. Cấu trúc
- Heading dùng `#`/`##`/`###` thuần, không bold, không bỏ cấp, đánh số nhất quán.
- Separator `---` thuần, có dòng trống trước/sau.
- List lồng nhau: thụt đúng (3 space dưới list số, 2 space dưới `-`).
- Bold đúng dạng `**text**`; tên file/lệnh/biến đặt trong backtick.

## E. Bảng
- Có header + dòng căn cột (`| :--- | :---: |`).
- Mỗi hàng nằm trên một dòng, không dòng trống giữa các hàng.
- Số cột mọi hàng bằng nhau.

## F. Code block
- Luôn có language tag (`bash`, `text`, `markdown`, `mermaid`, ...).
- Block chứa block lồng nhau: dùng fence ngoài 4 backtick (````).
- Placeholder như `<track>`, `<feature-name>` phải nằm trong backtick hoặc code block để không bị parse thành HTML.

## G. Mermaid
- Nhãn node/edge chỉ dùng ký tự an toàn (bỏ ký tự Unicode lạ như `─`).
- Kiểm tra cú pháp: mũi tên, `|label|`, không trùng ID node.

## H. Toán/ký hiệu
- Bỏ LaTeX `$...$` khi chỉ để viết tắt thông thường (vd `$N+1$` -> `N+1`).

## I. Link và tham chiếu
- Link dạng `[text](path)`, không escape.
- Mọi file được tham chiếu (`references/...`) phải tồn tại đúng đường dẫn.

## J. Đối chiếu chéo skill <-> template
- Đường dẫn, tên cột, tên trạng thái, tên section phải khớp giữa SKILL.md và các file trong `references/`.
- Template phải là trạng thái ban đầu: hàng mẫu `PENDING`, không điền sẵn `DONE`/`APPROVED`.
- Không hardcode tên agent/người cụ thể, dùng placeholder.
- Bỏ dòng tự tham chiếu hoặc dư thừa.
- Legend trạng thái tách theo từng cột nếu các cột dùng bộ giá trị khác nhau.
- Danh sách giá trị (vd `[NEW/MODIFY/DELETE]`) phải đồng nhất giữa skill và template.

## K. Báo cáo kết quả
1. Một đoạn ngắn: đã sửa loại lỗi nào.
2. Bản đã clean trong một code block copy-ready.
3. Mục "Lưu ý": chỉ các điểm không tự sửa được hoặc cần người dùng quyết định.