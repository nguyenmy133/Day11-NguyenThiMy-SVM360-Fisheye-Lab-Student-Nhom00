# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 1 | 3 | 5 | 8 | SPURIOUS (3) |
| mid | 8 | 0 | 1 | 4 | 10 | ATTRIBUTE (2) |
| edge | 3 | 1 | 1 | 2 | 0 | WRONG_CLASS (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên:
  - Đối với người (L): Vùng center có số lỗi phát sinh tuyệt đối nhiều nhất với 3 ca SPURIOUS và 1 ca MISSING trên 9 vật reference. Tuy nhiên, xét theo tỷ lệ sai lệch nghiêm trọng về bản chất vật thể, vùng edge là nơi nhạy cảm nhất khi có 1 ca WRONG_CLASS (chiếm 33.3% trên tổng số 3 vật reference ở rìa) do vật thể bị méo hình quang học nặng.
  - Đối với model (M): Gãy nghiêm trọng nhất ở vùng mid (10 ca M thừa, 4 ca M missing) và vùng center (8 ca M thừa, 5 ca M missing). Model có xu hướng gãy nhiều ở vùng mid và center do phát hiện nhầm các chi tiết nền/bóng đổ thành xe và người (false positives cao), đồng thời bỏ sót các phương tiện bị khuất một phần.

- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  - Ở vùng edge: Hiệu ứng quang học mắt cá làm cong và nén dẹt tỷ lệ chiều rộng/cao của phương tiện, khiến người gán nhãn dễ nhầm lẫn giữa ThreeWheeler và Truck/Car (vi phạm R04). Model phẳng (YOLO26m) chưa được huấn luyện trên miền dữ liệu mắt cá nên hoàn toàn mất khả năng nhận diện chính xác các góc rìa cong.
  - Ở vùng center và mid: Mật độ giao thông cao, nhiều vật thể xa có chiều cao sát ngưỡng H=40px dẫn đến ranh giới mờ giữa việc box hay bỏ qua, gây ra lỗi SPURIOUS do vẽ box cho vật thể quá nhỏ hoặc không đủ tin cậy.
  - Giới hạn: Slice chỉ gồm 3 frame trên một camera (B2-mid: adasind_060000.jpg, adasind_086220.jpg, adasind_102750.jpg) với tổng số n_ref = 20 đối tượng. Cỡ mẫu này mang tính khảo sát định hướng cục bộ (error patterns) theo từng vùng bán kính quang học, chưa đại diện đầy đủ cho phân bố 50.000 frame hay hệ thống 4 camera SVM.
