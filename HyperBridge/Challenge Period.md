1. Khái niệm:
- Là một khoảng thời gian chờ bắt buộc để đảm bảo tính xác thực của dữ liệu trước khi nó được chấp nhận hoàn toàn.

2. Giải thích chi tiết:
- Thay vì kiểm tra mọi giao dịch ngay lập tức (vốn rất tốn kém và chậm), hệ thống sẽ tạm thời "tin tưởng" rằng dữ liệu gửi lên là đúng. Tuy nhiên, nó sẽ mở ra một cửa sổ thời gian (ví dụ: 1 giờ, vài giờ hoặc thậm chí 7 ngày) để bất kỳ ai cũng có thể kiểm tra và đưa ra bằng chứng chứng minh dữ liệu đó là giả mạo.
	- **Nếu không có ai phản đối (Challenge):** Sau khi hết thời gian này, dữ liệu/giao dịch được coi là cuối cùng (Finalized) và tiền có thể được rút ra hoặc thực thi.
	- **Nếu có người phát hiện sai sót:** Họ sẽ gửi một **Fraud Proof** (Bằng chứng gian lận). Nếu bằng chứng đúng, kẻ gian lận sẽ bị phạt (mất tiền deposit/stake), và giao dịch sai trái sẽ bị hủy bỏ.

3. Feedback tích hợp
- Khi bạn sử dụng SDK để Bridge tài sản, bạn sẽ thấy trạng thái của tin nhắn có thể là `Pending` hoặc `In Challenge Period`.
	- **Thời gian cụ thể:** Tùy thuộc vào cấu hình của từng cặp chuỗi (mạng Testnet thường rất nhanh, nhưng Mainnet sẽ lâu hơn để đảm bảo an toàn).
	- **Lời khuyên:** Trong UI/UX của ví bạn đang phát triển, bạn nên hiển thị một thanh tiến trình (Progress bar) kèm theo thông báo: _"Tài sản đang trong giai đoạn xác thực bảo mật (Challenge Period), dự kiến hoàn tất trong X phút/giờ"_ để người dùng không lo lắng.