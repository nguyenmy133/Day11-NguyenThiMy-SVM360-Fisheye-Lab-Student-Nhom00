# QA review · B2-mid

- Reviewer (Vai B): Nguyễn Thị My (my)
- Annotator (Vai A): Lê Chí Bằng (bang)
- Slice: B2-mid
- Mã khóa: 3928-E99C

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_102750.jpg | L5 | R01 | Thiếu 1 xe ba bánh ThreeWheeler ở cự ly xa (x=252, y=900, H=44.8px), xe hơi mờ và bị che một phần. Đo chiều cao đạt >40px nên đủ điều kiện gán nhãn theo R01. |
| adasind_102750.jpg | L3 | R04 | Xe ở mép trái ngoài vành kính (x=0, y=840): Nhãn hiện tại là ThreeWheeler, cần kiểm tra đối chiếu kỹ với Rule R04 xem có phải là Truck hay xe ba gác chở hàng. |
| adasind_086220.jpg | L5 | R01 | Xe máy hậu cảnh L5 có chiều cao H=37px sát ngưỡng 40px của Rule R01, cần kiểm tra xem có cần giữ hay loại bỏ theo quy định kích thước tối thiểu. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
