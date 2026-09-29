# Guideline patch

- **Rule mới đề xuất:** R04b — Phân định hình thái xe ba bánh chở hàng (ThreeWheeler cargo) và xe tải nhỏ (Mini-truck) tại vùng rìa thấu kính mắt cá (edge zone r/R ≥ 0.6). Nếu phương tiện có buồng lái gắn liền với cụm tay lái dạng xe gắn máy/kính chắn gió dốc đứng và thùng chở hàng lộ thiên phía sau thì ưu tiên phân loại `ThreeWheeler` ngay cả khi chỉ quan sát thấy 1 bên bánh xe do góc khuất và méo rìa.
- **Áp dụng cho:** Class `ThreeWheeler` và `Truck`, thuộc tính `edge_zone`, `truncated`.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại R04 chỉ định nghĩa chung "xe ba bánh chở người/hàng → ThreeWheeler; xe tải nhỏ → Truck", chưa có quy định định danh chi tiết khi hình ảnh bị biến dạng quang học fisheye nghiêm trọng ở rìa khung hình, dẫn tới bất đồng nhãn giữa annotator (chọn ThreeWheeler) và reference (chọn Truck) tại frame `adasind_102750.jpg`.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Vòng rework P5 và áp dụng cho toàn bộ các batch dữ liệu fisheye tiếp theo.
