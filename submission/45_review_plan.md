# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_102750.jpg` (Vùng edge & mid) | 4 ca: 1 WRONG_CLASS (L3/R2), 1 MISSING (L5/R), 1 SPURIOUS (L2), 1 ATTRIBUTE (L7) | Vùng rìa chịu biến dạng quang học fisheye lớn nhất dẫn đến nhầm lẫn class trọng yếu giữa ThreeWheeler và Truck; vật thể mờ ở xa dễ bị bỏ sót. | Ảnh minh chứng `submission/screenshots/frame2_threewheeler_left_edge_L3.png`, `frame2_threewheeler_L5.png`, findings dòng 5, 6, 13, 14 |
| `adasind_086220.jpg` (Vùng center & mid) | 4 ca: 1 MISSING (R4), 2 SPURIOUS (L2, L5), 1 ATTRIBUTE (L4) | Mật độ giao thông trung tâm cao, nhiều đối tượng cự ly xa sát ngưỡng H=40px dễ gây bất đồng về việc có box hay không và thuộc tính occluded. | Dòng findings 7, 8, 9, 10, 11 trong `findings.csv`, bảng xung đột `local_quality_conflicts.csv` |

**Giới hạn của kết luận từ ba frame ADASIND:**
Ba frame thuộc cùng một camera góc rộng phía trước trên xe thử nghiệm ADASIND trong điều kiện ban ngày khô ráo. Kết luận từ 3 frame chỉ phản ánh các mẫu lỗi cục bộ (local error patterns) theo góc quang học, không thể khái quát hóa cho điều kiện ánh sáng yếu, ban đêm, trời mưa hoặc góc quan sát thấp/chĩa xuống của 3 camera SVM còn lại (rear, left, right).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- **Kiểm soát độ phủ và tránh trùng lặp:** Áp dụng kỹ thuật lấy mẫu phân tầng theo thời gian (time-strided stratified sampling). Các frame được chọn phải cách nhau tối thiểu 30-50 frame (khoảng 3-5 giây di chuyển) hoặc thuộc các sequence/đoạn đường khác nhau để đảm bảo mỗi frame mang một bối cảnh độc lập, tránh hiện tượng nhiều frame liên tiếp trong cùng một cảnh dừng xe đèn đỏ bị tính thành nhiều ca lỗi lặp lại.
- **Bản chất của kế hoạch 200 frame:** Đây là kế hoạch lấy mẫu có chủ đích (targeted sampling) tập trung vào các lát cắt rủi ro cao (ca hard: mép nối thấu kính, bóng râm gắt, vật thể cự ly cực gần). Do đó, kế hoạch này chỉ đóng vai trò "soi tìm lỗi và lỗ hổng quy tắc", không được dùng để tính toán tỷ lệ lỗi tổng thể (defect rate) của toàn bộ tập dữ liệu 50.000 frame vì phân phối mẫu không ngẫu nhiên đồng đều (non-uniform distribution).
