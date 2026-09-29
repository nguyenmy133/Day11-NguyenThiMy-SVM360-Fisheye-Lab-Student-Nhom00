# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngược sáng cực mạnh khi mặt trời lặn (glare); vệt nước mưa đọng trên vòm kính; người đi bộ băng cắt sát đầu xe | Lóa sáng làm mất biên viền vật thể; hạt nước tán xạ ánh sáng gây false positive; góc nhìn xiên làm méo hình người đi bộ | Raw fisheye space với intrinsic K/D cố định; giữ nguyên tỷ lệ khung hình gốc để chuẩn hóa bán kính méo tâm-rìa | 2 chuyên gia độc lập (dual-blind review), phóng to vùng lóa sáng, kiểm tra độ ôm sát viền (tightness) và đối chiếu video frame lân cận |
| rear | Đèn pha xe sau rọi trực diện ban đêm; bùn đất/bụi bẩn bám bề mặt vòm kính sau; vật thể thấp sát cản sau | Quầng sáng đèn pha lấn át viền thân xe (blooming); mảng bẩn tạo giả thể; chướng ngại vật thấp rơi vào ranh giới ego-body | Raw fisheye space có polygon mask chuẩn cho ego-body/cản sau; extrinsic chuẩn hóa theo mặt phẳng đường | Đối chiếu chuỗi frame liên tiếp trước/sau khi xe lùi; thống nhất phân biệt giữa che khuất thực tế vs che mờ do bẩn kính |
| left | Xe máy/người đi bộ đi sát sườn xe lọt vào mép kính (zone edge); vật nằm đúng góc seam tiếp giáp giữa cam front và left | Méo radial cực đại kéo giãn biên dạng vật thể thành hình vòng cung; vật bị chia cắt giữa 2 camera làm box bị cụt (truncated) | Raw fisheye space kèm lens circle chuẩn; đồng bộ timestamp (< 5ms) với camera front và rear | Xem song song 2 ảnh cùng timestamp của cam left và front; đảm bảo box ôm sát phần thấy trên cam left, không tự phóng đại phần bị cắt |
| right | Xe hai bánh vượt trong điểm mù bên phụ; bóng đổ của gương chiếu hậu và thân xe ego; phương tiện di chuyển với vận tốc góc lớn | Vệt mờ chuyển động (motion blur); bóng râm tối che khuất bánh xe; méo rìa làm box phình to không khớp thể tích thực | Raw fisheye space có ignore_region cho gương/thân xe; intrinsic/extrinsic đã khóa và thẩm định calibration | Hội đồng 3 chuyên gia thẩm định ca motion blur và mép seam; kiểm tra tính nhất quán gán nhãn Bike vs Pedestrian + Bike |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):
  1. Khi thay đổi phần cứng cảm biến hoặc quang học: thay đổi ống kính fisheye, góc mở FOV, độ phân giải hoặc vị trí gắn cam trên thân xe.
  2. Khi hiệu chuẩn lại (re-calibration): cập nhật ma trận thông số nội suy (intrinsic K, distortion D) hoặc thông số ngoại suy (extrinsic translation/rotation).
  3. Khi cập nhật tài liệu quy chuẩn (guideline/taxonomy patch): thay đổi định nghĩa nhãn (ví dụ tách rider thành xe + người, đổi quy tắc vùng ignore_region, đổi ngưỡng kích thước tối thiểu).
  4. Khi mở rộng phạm vi vận hành thiết kế (ODD): bổ sung các điều kiện môi trường mới chưa từng có trong tập gold cũ (tuyết rơi dày, mưa giông đêm, hầm mỏ thiếu sáng).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:
  Một người đi bộ đứng ở góc trước-trái của xe, xuất hiện đồng thời trên cả camera front (ở vùng rìa mép phải) và camera left (ở vùng rìa mép trái).
  - Bằng chứng bắt buộc trước khi ghép: Timestamp đồng bộ mức phần cứng giữa 2 camera (< 5ms), thông số ngoại suy (extrinsic matrix) chính xác giữa 2 camera, và hình chiếu 3D/mặt phẳng đường để chứng minh hai hình chiếu 2D xuất phát từ cùng một thực thể vật lý trong không gian thực.
  - Policy xử lý: Ở tầng gán nhãn 2D sensor level, annotator TUYỆT ĐỐI KHÔNG tự ý ghép track hoặc xóa một trong hai box; mỗi camera phải giữ nguyên một box độc lập ôm sát phần quang học nhìn thấy được trên camera đó (gán nhãn truncated = true do mép FOV). Việc liên kết identity (Tracking ID) hoặc gộp thành một 3D bounding box duy nhất thuộc trách nhiệm của tầng fusion/BEV tracking hạ tầng phía sau theo quy tắc chiếu hình học.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  1. Thiếu tính đại diện về hình học và quang học: Mỗi camera có vị trí lắp đặt, góc chúc, độ cao và điều kiện phơi sáng hoàn toàn khác nhau (cam trước chịu gió bụi và nắng đối diện, cam sau chịu bùn đất và đèn xe sau, cam sườn chịu méo rìa cực lớn khi vật ở cự ly gần). Chất lượng trên camera trước không phản ánh độ chính xác trên 3 camera còn lại.
  2. Không bao quát bài toán seam và multi-view: Đánh giá đơn camera không thể phát hiện các lỗi bất đồng bộ thời gian (time sync drift), lỗi lệch hiệu chuẩn hình học giữa các camera, hoặc hiện tượng cùng một vật nhưng bị gán nhãn kích thước/vị trí mâu thuẫn giữa 2 camera tiếp giáp.
  3. Xu hướng đồng thuận sai (shared bias): Hai người soát (peer) có thể cùng bỏ sót một lỗi viền méo fisheye hoặc cùng hiểu sai một quy tắc phân loại nếu guideline đơn camera chưa chặt chẽ. Local quality chỉ là độ đo tương đối so với một tập teaching reference nhất định trên một góc nhìn hạn chế, không đủ cơ sở xác nhận tính chuẩn mực (gold standard) cho toàn bộ hệ thống SVM 360°.

