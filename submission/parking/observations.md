# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  1. Vạch sơn trắng ở tiền cảnh đáy giữa (`523.80,718.50 -> 408.56,651.97`): phân chia ranh giới giữa hai ô đỗ xe liên tiếp ở hàng trước.
  2. Vạch sơn trắng ở tiền cảnh góc dưới bên trái (`27.24,720.00 -> 28.85,679.55`): phân chia ranh giới cạnh ngoài của ô đỗ xe tiền cảnh.
  (Các vạch đều dừng chính xác tại điểm kết thúc vết sơn nhìn thấy được, không giả định kéo dài vào vùng bị khuất).
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  Bỏ qua dải vạch dài chỉ dẫn lối xe chạy ở giữa bãi đỗ và mép gờ bó vỉa (curb). Lý do: Theo quy tắc, `parking_line` chỉ dành riêng cho các đoạn vạch sơn phân chia ranh giới trực tiếp của ô đỗ xe (parking slot); các vạch phân luồng giao thông nội bộ, chỉ hướng lối đi hoặc mép vỉa hè vật lý không thuộc định nghĩa này.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  Polygon `free_space` bám sát mặt đường bê tông/nhựa trống thực tế nhìn thấy được của lối xe chạy và khoảng trống các ô đỗ trống. Ranh giới dừng ngay tại mép tiếp xúc mặt đất của bánh xe/thân xe đang đỗ, mép gờ vỉa hè (curb), bồn cây và rìa khung ảnh. Hoàn toàn không phủ xuyên qua thân xe, chướng ngại vật hoặc vùng bị che khuất tầm nhìn.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  Các vạch sơn mờ bị mài mòn và biến dạng do góc phối cảnh ở hậu cảnh xa phía sau bãi đỗ, nơi ranh giới ô đỗ và lối quay đầu xe khó phân định bằng mắt thường ở độ phân giải hiện tại.

