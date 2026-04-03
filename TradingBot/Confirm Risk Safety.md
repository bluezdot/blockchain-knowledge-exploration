- SL/TP
Kiểm tra thông qua `SYMBOL_TRADE_STOPS_LEVEL`, là một thuộc tính cho biết khoảng cách tối thiểu (tính bằng Point) mà bạn phải tuân thủ khi đặt các mức dừng SL/TP hoặc các lệnh Pending Orders so với giá hiện tại.
=> Đánh giá: Hiện tại chỉ có AlphaTrend check tham số này. Các Bot khác các cấu hình hiện tại cũng chưa bị ảnh hưởng bởi giá trị Min này.

- Lot validation
Min, Max lot: Phụ thuộc sàn và tiền tệ, có thể check ở Specification. Cụ thể với XAUUSD trên Exness: Min: 0.01 lot; Max: 200 lot.
=> Đánh giá: Max lot khá lớn nên an toàn với các thuật toán hiện tại. Tuy nhiên để đảm bảo thì có thể áp dụng dần validate max lot size với constant `SYMBOL_VOLUME_MAX`, `SYMBOL_VOLUME_MIN`, `SYMBOL_VOLUME_STEP`, hiện đã có AlphaTrend áp dụng.

- Max orders & Account Limits
Sử dụng `ACCOUNT_LIMIT_ORDERS` để xác định giới hạn cứng của Broker. Tham số này bao gồm Pending Orders và Positions hiện tại. Nếu giá trị này là 0 nghĩa là ko có giới hạn.
	- Exness: Thường 1000 với Standard/Standard Cent và Unlimited với Pro/Raw/Zero.
	- XM/Vantage: Thường là 200.

- Order fill mode: check thông qua SYMBOL_FILL_MODE
Đây là hố tử thần khiến EA chạy được trên Exness nhưng có thể báo lỗi trên sàn khác.
	- **Exness:** Thường hỗ trợ `SYMBOL_FILL_IOC` hoặc `SYMBOL_FILL_FOK`.
	- **XM/Vantage:** Có thể chỉ hỗ trợ `SYMBOL_FILL_RETURN` (đặc biệt là tài khoản ECN). -> Cần kiểm tra cụ thể hơn.
	- **Giải pháp:** Trong code gửi lệnh, phải check `SYMBOL_FILLING_MODE` để gán đúng kiểu `ENUM_ORDER_TYPE_FILLING`.

- Rate Limit: Nếu gửi quá nhiều Order trong 1 giấy có thể bị khoá quyền giao dịch vì nghi ngờ Spam. Hiện chưa rõ có hay không hoặc thông số cụ thể.

Ngoài ra 1 số sàn có thể cho đóng lệnh vào cuối tuần, không cho mở lệnh. Khác với Exness không cho đóng và mở lệnh vào cuối tuần.