# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 6 |
| center | B2 | SPURIOUS | 13 |
| edge | B2 | MISSING | 3 |
| edge | B2 | SPURIOUS | 1 |
| edge | B2 | WRONG_CLASS | 1 |
| edge | C0 | ATTRIBUTE | 1 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | MISSING | 4 |
| mid | B2 | SPURIOUS | 12 |
| mid | C0 | BOX_GEOMETRY | 1 |
| unknown | B2 | BOX_GEOMETRY | 1 |
| unknown | B2 | MISSING | 1 |
| unknown | B2 | WRONG_CLASS | 1 |
| unknown | C0 | MISSING | 1 |

## Top defects
- SPURIOUS: 26 (ví dụ frame adasind_086220.jpg)
- MISSING: 15 (ví dụ frame adasind_019560.jpg)
- ATTRIBUTE: 3 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**
  - Đối với lỗi SPURIOUS cao (26 ca) và MISSING (15 ca) từ mô hình: Nguyên nhân cốt lõi là `E4_model_domain`. Mô hình YOLO26m được huấn luyện trên không gian ảnh phối cảnh thông thường (pinhole) nên khi áp dụng trực tiếp lên ảnh fisheye bị biến dạng phi tuyến tính ở vùng rìa cong (edge zone) và nén mật độ ở tâm/trung gian (center/mid), mô hình sinh ra rất nhiều box trùng lặp hoặc nhận diện nhầm các bóng đổ, vệt mặt đường thành xe/người.
  - Đối với lỗi người gán nhãn ở frame `adasind_102750.jpg` (vật thể L5 và L3): Lỗi bỏ sót L5 bắt nguồn từ `E1_annotator_error` do vật thể ThreeWheeler ở xa có độ tương phản thấp và bị che một phần dù chiều cao đo được là H=44.8px (đạt ngưỡng R01). Đối với ca L3 (WRONG_CLASS), nguyên nhân là `E0_reference_defect` kết hợp `E2_guideline_gap`: vật thể sát mép trái bị méo hình học nặng, người gán nhãn nhận diện ThreeWheeler dựa trên kết cấu buồng lái nhỏ phía trước nhưng reference gán nhãn Truck.

- **Cách sửa và ai nhận việc (`owner`):**
  - `annotator`: Thực hiện rework trong vòng P5, bổ sung box cho xe ThreeWheeler L5 tại frame `adasind_102750.jpg` và hiệu chỉnh nhãn thuộc tính.
  - `ai_team`: Cần thu thập dữ liệu fisheye thực tế để huấn luyện lại (fine-tune) mô hình với hàm suy giảm độ cong (fisheye distortion augmentation), loại bỏ ngưỡng tin cậy thấp gây ra lượng box SPURIOUS lớn.
  - `guideline`: Cập nhật Rule Patch (bổ sung tiêu chí nhận diện phần đầu xe ba bánh vs xe tải nhỏ khi bị biến dạng ở mép thấu kính).

- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**
  - Ảnh chụp minh chứng: `submission/screenshots/frame2_threewheeler_left_edge_L3.png` và `submission/screenshots/frame2_threewheeler_L5.png`.
  - Dòng findings liên quan: Dòng 5, dòng 6, dòng 14 trong `submission/findings.csv`.
  - Căn cứ quy tắc: Rule R01 (ngưỡng chiều cao vật thể H ≥ 40px) và Rule R04 (phân loại chính xác phương tiện đặc thù ThreeWheeler).
