
Vấn đề:
1. Tiếp tục DCA liên tục mặc dù ngược Trend -> Quá nguy hiểm. Chỉ sống sót được trong Trend yếu.
2. Bắt đầu rải lệnh theo Tín hiệu hiện có khi idle -> Rủi ro vì có thể đang ở giữa/cuối trend
3. Sau khi đóng dải Position, Bot lập tức vào lệnh với hướng cũ chứ không vào lệnh dựa trên tín hiệu AlphaTrend.

**Giải quyết bài toán 1:**
- Cắt rổ lệnh khi đảo chiều? Không! Xử lí như này chỉ khiến cho thuật toán bị thua lỗ liên tục trong thị trường sideway range vừa/to. Lí do là vì sideway range to để đủ khiến cho AlphaTrend đảo tín hiệu liên tục. (Sideway bé AlphaTrend chỉ cho tín hiệu đi ngang).

1st: Bổ sung kĩ thuật chống sập (Hedge mechanism)
- Mô tả
2nd: Filter false signal
- Mô tả
	- ATR Filter: Chỉ cắt rổ khi giá đóng cửa dưới AlphaTrend 1 khoảng 1 * ATR
	- Thời gian xác nhận: Chỉ cắt rổ khi AlphaTrend trong trend đảo chiều 2-3 nến

3rd:
- Mô tả
	- Lúc ngược trend tạm dừng DCA, chờ đợi bắt đầu đảo chiều về thuận trend thì DCA.


**Giải quyết bài toán 2:**
- Đợi kết thúc hẳn trend hiện tại và sang 1 trend mới mới bắt đầu lại.

--- Mở rộng ---
Đổi hẳn sang chiến thuật Breakout + Trailing SL
Chiến thuật: Pyramiding.
- DCA thuận hướng kèm theo dời SL các lệnh cũ lên level mới
Ưu điểm: Khuếch đại lợi nhuận nếu có Trend. Vẫn giữ được rủi ro thấp.

--- AlphaTrendCustom ---
-> Chỉ có tín hiệu Buy/Sell, sideway ngay sau tín hiệu nào thì chuyển thành tín hiệu đó