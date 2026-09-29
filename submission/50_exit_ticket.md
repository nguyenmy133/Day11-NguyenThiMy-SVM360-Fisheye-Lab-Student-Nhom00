# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**
   - Đây **không phải lỗi `DUPLICATE`** mà là trường hợp hợp lệ bắt buộc cần có **quy tắc riêng (cross-camera policy)**.
   - **Vì sao:** Trong hệ thống Surround View Monitoring (SVM), mỗi camera mắt cá phụ trách một góc nhìn với hệ tọa độ ảnh và độ méo quang học riêng biệt. Một vật thể nằm tại vùng chồng lấn (seam) giữa hai camera (ví dụ: góc trước-trái giữa Front và Left camera) sẽ xuất hiện với hai hình chiếu phối cảnh hoàn toàn khác nhau (ở camera này nằm tại vùng `edge` bị kéo dẹt, ở camera kia nằm tại vùng `mid` rõ nét). Ở tầng gán nhãn 2D trên từng camera, việc vẽ cả hai box là hoàn toàn chuẩn xác để mỗi cảm biến ghi nhận trọn vẹn trường nhìn của nó. Nếu tự ý xóa một box và coi là DUPLICATE sẽ gây ra lỗi thiếu (False Negative) cho camera đó. Việc loại bỏ trùng lặp hoặc ghép nối thuộc trách nhiệm của thuật toán hợp nhất không gian (BEV Fusion) ở tầng sau.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**
   - **Khi nào giữ cùng track ID:** Khi vật thể liên tục xuất hiện trong trường nhìn và duy trì được tính liên tục danh tính (identity).
   - **Khi nào thêm keyframe:** Khi vật thể có sự thay đổi đột ngột về hình học (đổi hướng di chuyển, quay đầu, phóng to/thu nhỏ nhanh do thay đổi khoảng cách) để đảm bảo các frame nội suy ở giữa bám sát biên dạng thực tế.
   - **Khi nào thêm trạng thái Outside:** Khi vật thể di chuyển ra khỏi tầm nhìn của thấu kính hoặc bị che khuất hoàn toàn bởi vật thể khác, tránh việc bộ gán nhãn nội suy các box "ảo" tại vị trí vật thể không tồn tại.
   - **Bằng chứng cần thiết trước khi nối track qua hai camera:**
     1. Đồng bộ thời gian phần cứng (hardware timestamp synchronization) giữa các camera ở mức mili-giây.
     2. Bảng ma trận hiệu chuẩn chính xác gồm nội vi (intrinsics/fisheye distortion model) và ngoại vi (extrinsics liên kết giữa các camera về hệ tọa độ xe ego).
     3. Chính sách đầu ra (output policy) rõ ràng quy định việc gán track ID toàn cục (global track ID) được thực hiện trên không gian không gian 3D/BEV hay nối chuỗi 2D tracker.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**
   - **Trường hợp bất đồng:** Tại frame `adasind_102750.jpg`, vật thể L3 (người gán nhãn chọn `ThreeWheeler` nhưng reference R2 và model M2 chọn `Truck`). Nhóm quan sát thấy phần đầu buồng lái có kết cấu kính chắn gió dốc đứng và tay lái dạng xe gắn máy 3 bánh đặc thù chở hàng, nhưng do nằm sát mép trái (vùng rìa cong méo) nên thùng hàng sau bị nén dẹt trông tương tự xe tải nhỏ.
   - **Cách xử lý:** Nhóm không vội vàng sửa nhãn mù quáng chạy theo reference, mà lập **Escalation Ticket 1**, đề xuất **Rule Patch R04b** để làm rõ tiêu chí nhận diện phần buồng lái xe ba bánh, và ghi nhận quyết định vào `40_decision_log.csv` (mã D4).
   - **Bài học rút ra nếu làm lại:** Cần chủ động chụp lại chi tiết phóng to và đo đạc kỹ hơn chiều cao pixel $H$ cho các đối tượng xa/mờ ngay từ bước tự soát self-QC, ghi chú rõ các trường hợp biến dạng quang học rìa thấu kính vào nhật ký trước khi khóa bài để tiết kiệm thời gian phân xử ở các vòng sau.
