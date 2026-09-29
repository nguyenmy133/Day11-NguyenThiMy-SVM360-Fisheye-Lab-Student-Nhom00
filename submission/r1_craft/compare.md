# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_060000.jpg
## adasind_086220.jpg
- L4+R1 mid ATTRIBUTE
- L2 center SPURIOUS
- L5 center SPURIOUS
- R4 center MISSING
## adasind_102750.jpg
- L7+R1 mid ATTRIBUTE
- L2 center SPURIOUS
- L3+R2 edge WRONG_CLASS
- L5 mid SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 9 | 8 | 1 | 3 |
| mid | 8 | 8 | 0 | 1 |
| edge | 3 | 2 | 1 | 1 |
