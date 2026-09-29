# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `3928e99cfb8ce70e4e5009f7410f80339db7aad24f9fa2273d9510b326f67ee8`; slice `B2-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_060000.jpg, adasind_086220.jpg, adasind_102750.jpg. Frame thiếu trong export: không.
TP=18; FP=5; FN=2; số lần đối chiếu=24; mean IoU của TP=0.884.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.750 | 0.942 | 0.875 |
| precision | 0.783 | 0.750 | 0.000 |
| recall | 0.900 | 0.683 | 0.000 |
| jaccard | 0.720 | 0.633 | 0.000 |
| dice | 0.837 | 0.703 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 1 | 0.958 | 1.000 | 0.750 | 0.750 | 0.857 |
| Car | 0 | 2 | 0 | 0.917 | 0.000 | 0.000 | 0.000 | 0.000 |
| Pedestrian | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 9 | 3 | 0 | 0.875 | 0.750 | 1.000 | 0.750 | 0.857 |
| Truck | 2 | 0 | 1 | 0.958 | 1.000 | 0.667 | 0.667 | 0.800 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_060000.jpg | 10 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_086220.jpg | 4 | 2 | 1 | 0.571 | 0.667 | 0.800 |
| adasind_102750.jpg | 4 | 3 | 1 | 0.571 | 0.571 | 0.800 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 0 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 4 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 9 | 0 | 0 |
| Truck | 0 | 0 | 0 | 1 | 2 | 0 |
| <extra> | 0 | 2 | 0 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
