# Brainstorm: Website thiệp mời cưới trực tuyến

> Bản brainstorm tổng hợp ý tưởng và luồng sản phẩm đã trao đổi. Các phần kỹ thuật là đề xuất ban đầu, có thể điều chỉnh khi chốt MVP.

## 1. Ý tưởng sản phẩm

Xây dựng website cung cấp nhiều mẫu thiệp mời cưới trực tuyến. Mỗi mẫu là một trang thiệp có ảnh cô dâu chú rể, nội dung sự kiện, hiệu ứng chuyển động và nhạc nền. Người dùng chọn mẫu, điền thông tin, xem trước thay đổi ngay trên trang rồi gửi thiệp để nhận đường dẫn chia sẻ.

Thiệp sau khi xuất bản có một URL riêng để gửi cho khách mời. Trang thiệp công khai được dựng từ mẫu đã chọn và dữ liệu mà người dùng đã lưu.

## 2. Nguồn ảnh và nhạc

- **Ảnh mẫu để giới thiệu template:** dùng ảnh cặp đôi được tạo bằng AI hoặc ảnh có quyền sử dụng phù hợp. Thông tin tên và đám cưới trong bản mẫu là dữ liệu giả, cần ghi rõ đây là ảnh minh họa nếu có thể gây nhầm lẫn.
- **Ảnh của thiệp thật:** cho phép khách hàng tải ảnh của chính họ lên, sau đó chọn ảnh bìa và ảnh trong thư viện thiệp.
- Không lấy ảnh của các cặp đôi trên website khác nếu chưa được phép.
- Với nhạc nền, cung cấp các bản nhạc có quyền sử dụng phù hợp hoặc cho người dùng tải file của họ lên.
- Trang thiệp có nút bật/tắt nhạc. Trình duyệt thường chặn tự phát âm thanh trước khi khách mời tương tác, nên có thể bắt đầu nhạc sau khi khách mời nhấn nút **Mở thiệp**. Tham khảo: [MDN — Autoplay guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay).

## 3. Luồng chính của người dùng

1. Người dùng xem danh sách thiệp.
2. Người dùng chọn một mẫu thiệp.
3. Website mở trang tùy chỉnh của mẫu đó.
4. Người dùng điền nội dung, tải ảnh, chọn nhạc và chọn một số tùy chọn thiết kế.
5. Khung xem trước cập nhật ngay khi người dùng thay đổi dữ liệu.
6. Người dùng lưu/gửi thiệp.
7. Backend lưu dữ liệu và tài nguyên, sau đó trả về đường dẫn thiệp để chia sẻ.

## 4. Trang tùy chỉnh thiệp

Trang sử dụng toàn bộ chiều rộng màn hình, có thanh tiêu đề phía trên và hai cột bên dưới.

| Khu vực | Tỷ lệ khởi điểm | Chức năng |
| --- | ---: | --- |
| Xem trước | Khoảng 65% | Hiển thị thiệp đang được dựng từ template và dữ liệu hiện tại |
| Tùy chọn | Khoảng 35% | Biểu mẫu nhập nội dung và chọn ảnh, nhạc, giao diện, hiệu ứng |

Thanh tiêu đề có thể gồm nút quay lại danh sách mẫu, tên template, trạng thái lưu nháp và nút **Xuất bản**.

Khung xem trước nên hiển thị chính trang thiệp sẽ được khách mời mở. Có thể bổ sung các nút xem trên màn hình điện thoại/máy tính, chạy lại hiệu ứng mở thiệp và bật/tắt nhạc.

Trên điện thoại, hai cột chuyển thành một cột. Có thể cho người dùng chuyển qua lại giữa **Tùy chỉnh** và **Xem trước** để giữ giao diện dễ sử dụng.

## 5. Các nhóm trường trong biểu mẫu

### Thông tin cặp đôi
- Tên hai người
- Lời chào hoặc câu giới thiệu ngắn

### Thông tin sự kiện
- Ngày, giờ tổ chức
- Tên địa điểm, địa chỉ
- Liên kết bản đồ nếu có

### Ảnh
- Các loại ảnh và vị trí hiển thị phụ thuộc vào template được chọn, ví dụ ảnh bìa hoặc bộ sưu tập ảnh
- Thay ảnh hoặc xóa ảnh tại các vị trí được template hỗ trợ

### Nội dung thiệp
- Lời mời
- Lịch trình
- Câu chuyện của cặp đôi hoặc lời nhắn
- Thông tin RSVP và hạn xác nhận

### Giao diện và hiệu ứng
- Bảng màu, kiểu chữ, họa tiết
- Hiệu ứng mở thiệp, ví dụ mở phong bì hoặc lật thiệp
- Hiệu ứng xuất hiện nội dung, ví dụ fade hoặc slide
- Chọn nhạc và bật/tắt phát nhạc

MVP có thể bắt đầu với thông tin cặp đôi, sự kiện, ảnh bìa, nhạc, một vài lựa chọn giao diện và hiệu ứng. Các phần như câu chuyện cặp đôi, RSVP nâng cao và nhiều ảnh có thể bổ sung sau.

## 6. Cập nhật xem trước và lưu nháp

Dữ liệu biểu mẫu được giữ trong state của ứng dụng frontend. Mỗi lần người dùng thay đổi trường, state cập nhật và component xem trước render lại ngay. Việc thay đổi nội dung cơ bản không cần gọi AI hoặc chờ backend.

Dùng IndexedDB để lưu nháp cục bộ, chẳng hạn sau một khoảng debounce ngắn, nhằm khôi phục dữ liệu khi tải lại trang trong cùng trình duyệt. IndexedDB chỉ là bản lưu trên thiết bị/trình duyệt đó; dữ liệu chưa gửi lên backend không tự xuất hiện trên thiết bị khác.

Ảnh/nhạc vừa chọn có thể được xem trước cục bộ bằng object URL. Đây chỉ là địa chỉ tạm trong trình duyệt, không phải URL có thể lưu cho thiệp công khai.

## 7. Tách template và nội dung

Mỗi template giữ phần bố cục HTML/CSS/JavaScript, còn nội dung của từng thiệp được lưu riêng dưới dạng dữ liệu. Form cập nhật dữ liệu; template đọc dữ liệu và hiển thị thiệp. Như vậy người dùng có thể thay nội dung mà không cần sửa mã HTML.

Khi AI sinh template, hệ thống tạo kèm một bộ JSON mapping riêng cho template đó.

Dữ liệu thiệp có thể gồm:

- `templateId`: mã mẫu thiệp
- `couple`: tên hai người
- `event`: ngày giờ, địa điểm, địa chỉ, liên kết bản đồ
- `content`: lời mời và các nội dung hiển thị
- `assets`: URL ảnh và nhạc sau khi tải lên
- `appearance`: màu sắc, kiểu chữ, họa tiết
- `effects`: hiệu ứng mở thiệp và hiệu ứng chuyển động
- `rsvp`: cấu hình xác nhận tham dự

Ví dụ minh họa:

```json
{
  "templateId": "romantic-01",
  "couple": {
    "personOne": "Minh",
    "personTwo": "Lan"
  },
  "event": {
    "date": "2027-01-10",
    "time": "17:30",
    "venue": "Nhà hàng Hoa Sen",
    "address": "Hà Nội"
  },
  "assets": {
    "coverImageUrl": null,
    "musicUrl": null
  },
  "appearance": {
    "themeId": "warm",
    "fontId": "serif"
  },
  "effects": {
    "opening": "envelope",
    "content": "fade"
  }
}
```

## 8. Sử dụng AI và kiểm soát chi phí

Không cần gọi AI mỗi khi người dùng nhập một ký tự hoặc mở khung xem trước. Template dựng sẵn và dữ liệu có cấu trúc xử lý các thay đổi thông thường.

AI có thể là chức năng tùy chọn khi người dùng chủ động yêu cầu hỗ trợ sáng tạo, ví dụ viết lại lời mời theo giọng trang trọng hoặc lãng mạn hơn. Chỉ gọi AI khi người dùng nhấn nút tạo/sửa nội dung, sau đó lưu kết quả và dùng lại. Việc dựng trang thiệp công khai không cần gọi AI.

Ảnh AI có thể được tạo trước để làm ảnh minh họa cho thư viện mẫu. Ảnh của thiệp thật do người dùng tải lên, trừ khi sau này có thêm chức năng tạo ảnh riêng.

## 9. Gửi dữ liệu lên backend và xuất bản

Khi người dùng nhấn **Xuất bản**:

1. Kiểm tra các trường bắt buộc và định dạng dữ liệu.
2. Tải file ảnh/nhạc thật lên backend hoặc dịch vụ lưu trữ.
3. Gửi `templateId`, nội dung, cấu hình giao diện và URL tài nguyên lên API.
4. Backend lưu bản ghi thiệp và trả về mã thiệp/đường dẫn công khai.
5. Ứng dụng hiển thị đường dẫn để người dùng sao chép và gửi khách mời.

Không gửi object URL dạng `blob:...` làm địa chỉ ảnh/nhạc cho backend. Cần tải nội dung file lên, rồi lưu URL tài nguyên được hệ thống trả về.

## 10. Phạm vi MVP đề xuất

- Danh sách một số template có trang xem thử.
- Trang tùy chỉnh hai cột: xem trước và biểu mẫu.
- Cập nhật xem trước tức thì, không gọi AI cho thao tác thông thường.
- Tải ảnh cặp đôi và chọn nhạc.
- Một số lựa chọn màu sắc, hiệu ứng mở thiệp và hiệu ứng fade/slide.
- Lưu nháp cục bộ bằng IndexedDB.
- Gửi dữ liệu và tài nguyên lên backend để xuất bản URL chia sẻ.
- Chức năng AI viết lại lời mời có thể để sau hoặc để dưới dạng tùy chọn.

## 11. Những quyết định để sau

- Có cần tài khoản để lưu và quản lý nhiều thiệp hay không.
- RSVP sẽ dùng form nội bộ hay liên kết tới dịch vụ khác.
- Thiệp có thời hạn sử dụng hoặc giới hạn dung lượng ảnh/nhạc hay không.
- Có cung cấp tạo ảnh cặp đôi bằng AI cho khách hàng hay chỉ nhận ảnh tải lên.
- Chính sách xử lý, lưu trữ và xóa ảnh cá nhân của người dùng.
