## TÀI LIỆU KỸ THUẬT (STANDARDIZED SPEC)

### 1. Tổng quan
Bot giao dịch theo xu hướng (Trend-Following). Vào lệnh khi giá breakout khỏi vùng kháng cự/hỗ trợ quan trọng.

1.1. Xác định các vùng hỗ trợ, kháng cự quan trọng.
- Sử dụng Williams Fractals (iFractals)
	- Số nến lookback.
	- Khung thời gian chính (Entry TF).
	- Khung thời gian lớn (HTF).
- Tiêu chí chọn S/R quan trọng
	- Mức đó xuất hiện trên cả khung HTF lẫn Entry TF.
	- Mức đó từng bị test lại >= 2 lần mà không bị phá vỡ (zone strength).
- Chú ý: Nên tìm vùng kháng cự - vùng hỗ trợ sau đó xác định 1 đường làm kháng cự/hỗ trợ để kiểm tra.
1.2. Dựa vào price action và giá breakout vùng kháng cự để vào lệnh.
- Tín hiệu Breakout
	- Nến đóng cửa hoàn toàn bên trên/dưới vùng S/R, không chỉ shadow.
	- Giá đóng vượt mức S/R tối thiểu `BreakoutBuffer` pip -> Tránh false breakout
	- ~~Nến breakout: Body >= 50% tổng chiểu cao (filter doji/spining top).~~
	- Không vào lệnh nếu đã có breakout cùng hướng trong `CooldownBars` nến gần đây
1.3. Đặt stoploss dựa vào swing high/swing low.
- Buy SL: Tại Swing Low gần nhất trong `SwingLookback` nến gần đây (tính từ nến thứ 2 trở về), trừ thêm `SL_Buffer` pip. Ngược lại với Sell SL.
- Nếu SL quá nhỏ (< `MinSL` pip), lệnh không được mở (bảo vệ tài khoản).
1.4. Đặt TP với RR 1:2.


Tư duy xác định kháng S/R:
- **Tư duy vùng giá:** Không chỉ dùng 1 đường kẻ, hãy vẽ một vùng (zone) bao gồm cả râu nến (bóng nến) và thân nến để tránh nhiễu.
- **Đỉnh/Đáy quá khứ:** Các đỉnh cũ thường là kháng cự mạnh (nơi lực bán thắng), đáy cũ là hỗ trợ mạnh (nơi lực mua thắng).
- **Số tròn (Round Numbers):** Giá thường phản ứng tại các mức chẵn (ví dụ: 10, 50, 100, 1000...) do tâm lý nhà đầu tư thích đặt lệnh tại đây.
- **Quy tắc "Đảo chiều":** Một vùng hỗ trợ sau khi bị phá vỡ (breakout) thường biến thành kháng cự và ngược lại.
- **Sử dụng đường xu hướng (Trendline):** Nối các đỉnh/đáy để tìm ra rào cản di động.
- **Đường trung bình động (MA):** Sử dụng các đường MA quan trọng như MA50, MA100, MA200 để xác định hỗ trợ/kháng cự động.
- **Độ mạnh của vùng:** Vùng được chạm lại nhiều lần (chạm đi chạm lại) và tồn tại trong thời gian dài sẽ có ý nghĩa và độ tin cậy cao hơn.