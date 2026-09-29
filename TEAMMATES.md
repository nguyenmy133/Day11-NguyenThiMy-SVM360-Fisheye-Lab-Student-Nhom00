# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm
- Khóa/lớp: K4
- Tên nhóm: Nhom00
- Repo Public: https://github.com/banglc-vinaiinaction/K4-DAY11-Nhom00
- Máy giữ hồ sơ chính / người quản lý: Lê Chí Bằng
- Slice chung lấy từ mode.json: B2-mid
- Tên định danh vai A dùng cho --self: bang
- Kênh trao đổi nội bộ: Discord, Zalo
- Đại diện nộp (vai C): Lê Chí Bằng, 2A202602215
- Commit chốt bài: [SHA hoặc URL commit]

## 2. Ba vai chính
| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Lê Chí Bằng | 2A202602215 | bang | Parking/C0/slice, self-QC, lock, rework | submission/r1_craft/ |
| B · QA độc lập | Nguyễn Thị My | 2A202602061 | my | Review trước reference, finding QA, kiểm lại ca sửa | submission/r2_qa/qa_review.md |
| C · Chẩn đoán & điều phối | Lê Chí Bằng, Nguyễn Minh Đức | 2A202602215, 2A202602114 | bang, duc | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | submission/r3_diag/, 45_sampling_plan.csv |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha
| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | [mode.json, slice, phân vai] | [Điền] | [Điền] |
| P2 · Khóa bản đầu | A → B, C | submission/r1_craft/lock.txt, B2-mid, mã khóa 3928-E99C | Đã tự soát self-QC 9 mục | Khóa thành công (26 boxes, 10 polys) |
| P3 · Chốt QA mù | B → C, A | qa_review.md, 3 findings QA, 2 screenshots | Đã đối chiếu luật R01/R04/R05 | Chốt QA thành công (My phát hiện xe 3 bánh mờ ở Frame 2) |
| P4 · Quyết định sửa | C → A, B | [finding, decision log, commit] | [Điền] | [Điền] |
| P5 · Kiểm bản sửa | A → B → C | [v2, lock2, review kiểm lại, delta] | [Điền] | [Điền] |
| P6 · Chốt nộp | A, B → C | [manifest, commit chốt] | [Điền] | [Điền] |

## 4. Bất đồng và phối hợp
- Một ca đã phân xử: [Frame/object/rule; ý kiến A/B; bằng chứng; quyết định và link]
- Ca còn mở: [Nội dung, người theo dõi, phép kiểm tiếp theo; nếu không còn thì ghi rõ]
- Đóng góp của A/B/C vào kế hoạch và exit ticket: [Điền phần việc thực tế]
- Thay đổi phân công nếu có: [Thời điểm, lý do, người nhận; nếu không đổi thì ghi rõ]

## 5. Xác nhận trước khi nộp
- [ ] A xác nhận nhãn và export đúng phiên bản: [Tên / bằng chứng]
- [ ] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: [Tên / bằng chứng]
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
