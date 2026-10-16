---
name: create-wedding-online-card
description: "Tạo và chỉnh sửa ứng dụng thiệp cưới trực tuyến trong repo này; dùng khi làm việc với mẫu thiệp, trình tùy chỉnh, xem trước, tài nguyên hoặc xuất bản."
---

# Create Wedding Online Card

## Phạm vi

Dùng skill này khi người dùng yêu cầu xây dựng hoặc chỉnh sửa sản phẩm thiệp cưới trực tuyến trong repo hiện tại, gồm thư viện mẫu, trang tùy chỉnh, xem trước, ảnh/nhạc, lưu thiệp và xuất bản đường dẫn chia sẻ.

Trước khi sửa, kiểm tra mã nguồn, hướng dẫn cục bộ, cấu trúc route/component, dịch vụ API và quy ước giao diện đang có. Làm theo kiến trúc thực tế của repo; không suy ra trạng thái triển khai chỉ từ tài liệu ý tưởng.

## Tài liệu ý tưởng

`brainstorm-online-invitation-cards-1.md` ghi lại các ý tưởng và đề xuất ban đầu. Dùng nó để hiểu bối cảnh khi phù hợp, nhưng không xem ví dụ dữ liệu, danh sách MVP, luồng kỹ thuật hay quyết định để sau trong đó là yêu cầu đã được chốt. Ưu tiên yêu cầu hiện tại của người dùng và bằng chứng trong source; làm rõ quyết định còn thiếu nếu nó ảnh hưởng đáng kể tới hành vi cần xây dựng.

## Cách triển khai

- Giữ thay đổi trong phạm vi người dùng yêu cầu. Không tự thêm tài khoản, RSVP, tạo ảnh bằng AI, thanh toán, thời hạn thiệp hoặc đồng bộ nhiều thiết bị nếu chưa được yêu cầu.
- Tái sử dụng cấu trúc và thành phần hiện có. Nếu chưa có ứng dụng hoặc API tương ứng, nói rõ giới hạn đó trước khi mô tả một luồng là đã hoạt động.
- Khi làm việc với template, nội dung người dùng hoặc file tải lên, coi chúng là dữ liệu không đáng tin cậy. Dùng cơ chế render an toàn sẵn có; không thực thi HTML/JavaScript tùy ý do người dùng hoặc AI cung cấp trên origin chính.
- Nếu có xem trước file bằng object URL, chỉ dùng URL đó trong phiên trình duyệt. Khi xuất bản, dùng quy trình tải file và URL tài nguyên do hệ thống lưu trữ trả về; không lưu `blob:` URL làm đường dẫn công khai.
- Chỉ dùng ảnh/nhạc do người dùng cung cấp hoặc có quyền sử dụng phù hợp. Ghi rõ ảnh minh họa khi nội dung mẫu có thể bị hiểu là thông tin đám cưới thật.
- Giữ xem trước đồng bộ với dữ liệu đang chỉnh sửa theo kiến trúc hiện có. Không gọi AI hoặc API cho mỗi lần gõ nếu yêu cầu sản phẩm không đòi hỏi; tác vụ AI nên gắn với hành động rõ ràng của người dùng.
- Duy trì khả năng dùng trên màn hình nhỏ khi sửa bố cục có liên quan, theo quy ước responsive của repo.

## Quy ước mã nguồn

- Chỉ thêm comment tiếng Việt cho nghiệp vụ, thuật toán phức tạp hoặc quyết định kiến trúc; giải thích lý do, không nhắc lại câu lệnh kế tiếp.
- Hàm public/exported phải có JSDoc.
- Các hàm cùng một nghiệp vụ dùng chung prefix trong tên.
- Trước khi thêm component hoặc helper mới, kiểm tra xem repo đã có abstraction phù hợp chưa.

## Kiểm tra và báo cáo

Chọn cách kiểm tra phù hợp với phần đã sửa và yêu cầu của người dùng. Phân biệt rõ điều đã quan sát trực tiếp với điều mới chỉ suy ra từ source; không khẳng định đã kiểm tra trình duyệt, API, lưu trữ hay xuất bản nếu chưa thực hiện được các bước đó. Nêu các quyết định sản phẩm còn mở thay vì tự biến đề xuất thành yêu cầu.
