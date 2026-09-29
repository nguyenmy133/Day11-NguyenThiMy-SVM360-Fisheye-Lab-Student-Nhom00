# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm
- Khóa/lớp: K4
- Tên nhóm: Nhom00
- Repo Public: https://github.com/iotech-vietnam/K4-L2-DAY11-NguyenMinhDuc-2A202602114
- Repo nhóm tham chiếu: https://github.com/banglc-vinaiinaction/K4-DAY11-Nhom00
- Máy giữ hồ sơ chính / người quản lý: Lê Chí Bằng
- Slice chung lấy từ mode.json: B2-mid
- Tên định danh vai A dùng cho --self: bang
- Kênh trao đổi nội bộ: Discord, Zalo
- Đại diện nộp (vai C): Nguyễn Minh Đức (2A202602114), Lê Chí Bằng (2A202602215)
- Commit chốt bài: 0db7982

## 2. Ba vai chính
| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Lê Chí Bằng | 2A202602215 | bang | Parking/C0/slice, self-QC, lock, rework | submission/r1_craft/, submission/rework/ |
| B · QA độc lập | Nguyễn Thị My | 2A202602061 | my | Review trước reference, finding QA, kiểm lại ca sửa | submission/r2_qa/qa_review.md, screenshots/ |
| C · Chẩn đoán & điều phối | Nguyễn Minh Đức, Lê Chí Bằng | 2A202602114, 2A202602215 | duc, bang | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | submission/r3_diag/, 45_sampling_plan.csv, tickets |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha
| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | mode.json, slice B2-mid, doctor.txt, parking task | Môi trường CVAT & repo chạy tốt, phân vai A, B, C | Hoàn thành P0 (task parking đã nạp và khóa) |
| P2 · Khóa bản đầu | A → B, C | submission/r1_craft/lock.txt, B2-mid, mã khóa 3928-E99C | Đã tự soát self-QC 9 mục | Khóa thành công (26 boxes, 10 polys) |
| P3 · Chốt QA mù | B → C, A | qa_review.md, 3 findings QA, 2 screenshots | Đã đối chiếu luật R01/R04/R05 | Chốt QA thành công (My phát hiện xe 3 bánh mờ ở Frame 2) |
| P4 · Quyết định sửa | C → A, B | findings.csv (48 dòng chẩn đoán), zone_table.md, 10_error_card.md, decision_log | Đối chiếu so sánh 3 nguồn L-R-M, phân loại WHAT/WHY | Chốt danh sách ca cần sửa và ca cần escalate |
| P5 · Kiểm bản sửa | A → B → C | annotations-v2.xml, lock2.txt (mã khóa A436-6712), delta.md | Đã sửa thuộc tính occluded/truncated và rà soát box | Rework thành công, delta ghi nhận thay đổi |
| P6 · Chốt nộp | A, B → C | manifest.json (failed_gates rỗng), check exit 0, hồ sơ đủ | Kiểm tra toàn bộ 37 file required trong submission/ | Hoàn tất 100%, sẵn sàng commit và push |

## 4. Bất đồng và phối hợp
- Một ca đã phân xử: Frame adasind_102750.jpg, đối tượng L3 (ThreeWheeler) vs R2 (Truck). Thống nhất giữ ThreeWheeler theo nhận diện buồng lái thực tế, đồng thời lập Escalation Ticket 1 và Rule Patch R04b để làm rõ tiêu chí thấu kính méo rìa, ghi nhận quyết định D4 trong decision log.
- Ca còn mở: Không còn ca tranh chấp tồn đọng; toàn bộ các bất đồng nhãn đã được phân loại action trong findings.csv (rework, keep_with_reason, escalate).
- Đóng góp của A/B/C vào kế hoạch và exit ticket:
  - Vai A (Bằng): Gán nhãn, xuất file, tự soát self-QC và thực hiện rework.
  - Vai B (My): Thực hiện QA mù độc lập, bắt 3 lỗi phát hiện thực tế kèm ảnh chụp màn hình chứng cứ.
  - Vai C (Đức, Bằng): Phân tích chẩn đoán lỗi trong zone_table và error card, xây dựng kế hoạch phân bổ 200 frame sampling, kế hoạch Gold Set 4 camera, hoàn thiện exit ticket và kiểm tra gate validation.
- Thay đổi phân công nếu có: Giữ nguyên phân công ban đầu.

## 5. Xác nhận trước khi nộp
- [x] A xác nhận nhãn và export đúng phiên bản: Lê Chí Bằng (bản khóa r1_craft: 3928-E99C, rework: A436-6712).
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Thị My (báo cáo qa_review.md).
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Nguyễn Minh Đức, Lê Chí Bằng (python3 lab11.py check báo exit 0).
- [x] manifest.json tại commit chốt có failed_gates rỗng.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [x] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
