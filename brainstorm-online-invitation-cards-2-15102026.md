# Brainstorm: Thiết kế template thiệp cưới bằng AI và component React

**Ngày trao đổi:** 15/10/2026

> Tài liệu ghi lại hướng dự kiến của người dùng và các đề xuất trong buổi trao đổi. Ví dụ component, cấu hình và cách kiểm tra bên dưới chưa phải yêu cầu đã chốt hoặc chức năng đã triển khai.

## 1. Hướng dự kiến của người dùng

- Dùng React cho việc tạo template thông qua skill.
- Xây dựng trước thư viện component và chỉ cho AI sử dụng những component đó để tạo template.
- Mỗi template do AI sinh có một bộ JSON mapping riêng.
- Mối băn khoăn chính là tính sáng tạo của AI khi bị giới hạn trong thư viện component có sẵn.

## 2. Không gian sáng tạo của AI

[Suy luận] Nếu component có bố cục cố định và ít tùy chọn, các template được tạo ra có thể chỉ khác màu sắc hoặc kiểu chữ. Mức độ đa dạng còn phụ thuộc vào khả năng kết hợp component, các props được hỗ trợ và hướng dẫn trong skill; hiện chưa có kết quả thử nghiệm để xác nhận mức độ sáng tạo.

[Đề xuất] Thiết kế thư viện để AI có thể tạo nhiều cấu trúc trang từ cùng các component: thay đổi thứ tự, cách lồng, bố cục, tỷ lệ ảnh, phân cấp chữ, khoảng trống và cách trang trí.

React hỗ trợ kết hợp, sắp xếp và lồng component để xây dựng trang; props và `children` cung cấp các cách tùy biến component. Đây là khả năng kỹ thuật của React, chưa phải bằng chứng về chất lượng thiết kế do AI tạo. Tham khảo: [Your First Component](https://react.dev/learn/your-first-component), [Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component).

## 3. Cấu trúc thư viện component đề xuất

[Đề xuất] Thư viện có hai tầng để hỗ trợ cả việc sắp xếp bố cục và thể hiện nội dung thiệp.

| Tầng | Ví dụ component | Không gian kết hợp |
| --- | --- | --- |
| Khối bố cục | `Section`, `Grid`, `Split`, `Stack`, `Overlay`, `Spacer` | Sắp xếp, chia cột, lồng khối, tạo lớp và điều chỉnh khoảng trống |
| Khối nội dung thiệp | `Hero`, `CoupleNames`, `Quote`, `EventTimeline`, `Gallery`, `Map` | Chọn biến thể trình bày, liên kết dữ liệu và ghép thành từng phần của thiệp |

Các tên trên là ví dụ đề xuất, không xác nhận component đã tồn tại hoặc tất cả chức năng tương ứng đều thuộc phạm vi cần xây dựng.

## 4. Phạm vi AI được tùy biến

[Đề xuất] Cho AI lựa chọn trong khả năng mà component đã công bố.

| Yếu tố | Khả năng tùy biến đề xuất |
| --- | --- |
| Cấu trúc trang | Thứ tự các phần, cách nhóm và lồng các khối theo quy tắc hỗ trợ |
| Bố cục | Xếp dọc, chia đôi, lưới hoặc chồng lớp |
| Biến thể component | Chọn các `variant` đã triển khai |
| Ảnh | Chọn slot, tỷ lệ, cách crop, vị trí và lớp phủ theo props được hỗ trợ |
| Chữ | Kiểu chữ, phân cấp, kích thước và căn chỉnh trong phạm vi cho phép |
| Phong cách | Màu sắc, khoảng cách, họa tiết và các giá trị theme hợp lệ |
| Chuyển động | Chọn hiệu ứng và tham số từ tập hiệu ứng được hỗ trợ |

Ví dụ, `Hero` có thể hỗ trợ `variant`, `align`, `imageRatio` và `overlay`; `Gallery` có thể có biến thể collage, filmstrip hoặc một ảnh lớn. Đây là các khả năng đề xuất để cân nhắc khi thiết kế API component.

## 5. Template và JSON mapping

[Đề xuất] Với luồng dùng skill để sinh mã trước khi phát hành, mỗi template gồm phần ghép các component React có sẵn và JSON mapping tương ứng. Mapping mô tả các trường được chỉnh sửa, đường dẫn dữ liệu và các vị trí ảnh mà template hỗ trợ.

Dữ liệu của từng thiệp được giữ riêng với cấu trúc template. Khi người dùng chỉnh nội dung, form và phần xem trước sử dụng cùng dữ liệu thiệp; ảnh và các tùy chọn chỉ hiển thị theo khả năng mà template công bố.

[Đề xuất] Nếu sau này AI sinh template trực tiếp trong ứng dụng lúc chạy, có thể để AI trả về JSON mô tả cây component, sau đó kiểm tra cấu trúc và render qua registry component được cho phép. Đây là phương án cần quyết định riêng, chưa xác nhận là luồng triển khai hiện tại.

## 6. Kiểm soát việc sử dụng component

[Đề xuất] Skill cung cấp tài liệu API component, props, quy tắc kết hợp, JSON mapping và nhiều ví dụ bố cục khác nhau. Bổ sung bước kiểm tra kỹ thuật để phát hiện đầu ra dùng component hoặc props ngoài phạm vi cho phép.

- Với mã React được sinh trước khi phát hành: kiểm tra danh sách import và component được dùng trong bước kiểm tra mã/build.
- Với JSON được sinh lúc chạy: kiểm tra schema, tên component, props, đường dẫn dữ liệu và URL trước khi render qua registry.
- Khi cần một kiểu trình bày chưa được hỗ trợ: ghi nhận đề xuất mở rộng component hoặc variant để được xây dựng và xem xét trước khi đưa vào thư viện.

## 7. Đánh giá tính đa dạng của thiết kế

[Đề xuất] Tạo thử các template theo những hướng thị giác khác nhau, chẳng hạn botanical, editorial, art deco và tối giản, rồi so sánh ảnh xem trước.

Các điểm cần xem xét gồm cấu trúc trang, phân cấp chữ, cách dùng ảnh, khoảng trống và họa tiết. Nếu các mẫu chủ yếu khác màu và font, xem lại khả năng bố trí, slot và variant của thư viện.

Đây là cách đánh giá được đề xuất; tài liệu chưa ghi nhận việc tạo mẫu thử hoặc kiểm tra giao diện thực tế.

## 8. Những điểm cần chốt tiếp

- Layout của ảnh.
- Hiệu ứng của các component đơn lẻ.
- Tạo bộ ảnh cặp đôi từ khuôn mặt có sẵn, theo phong cách ảnh cưới.
- Danh sách component, props và các quy tắc lồng được hỗ trợ.
- Mức độ tự do của theme: chọn giá trị có sẵn hay cho AI đề xuất giá trị mới trong giới hạn kiểm tra.
- Luồng tạo template: skill sinh mã React trước khi phát hành hay AI sinh cấu hình trực tiếp lúc chạy.
- Cấu trúc và phiên bản của JSON mapping.
- Cách xem xét, bổ sung component hoặc variant mới khi thư viện chưa đáp ứng một hướng thiết kế.
