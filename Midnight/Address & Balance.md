1. Public Address:
	Địa chỉ thông thường, dùng để gửi nhận các tài sản public (Night, MNTs).
2. Shielded Address:
	Địa chỉ bí mật: nơi các giao dịch ẩn danh được diễn ra, dựa theo cơ chế `Zcash-style shileded addresses`. Địa chỉ này ẩn danh tính của người gửi nhận với bên thứ 3. Chỉ có những người có `Viewing Keys` tương ứng mới thấy được nội dung gửi.
	Lưu trữ và quản lí tất cả token 1 cách riêng tư.
3. DUST Address:
   Cũng thuộc kiểu Shielded Address nhưng có vai trò đặc biệt. Sử dụng để liên kết designate từ NIGHT để nhận DUST như energy của tài khoản. Mục đích của DUST chỉ là để trả phí gas từ ví đó.
4. Viewing Keys (ivk, ovk)?
	Read-only address gồm:
		- Incoming viewing key (ivk): Cho phép xem chi tiết các thanh toán đến.
		- Outcoming viewing key (ovk): Cho phép xem chi tiết các thanh toán đi.

	Balance trên các địa chỉ sẽ nằm tách biệt, không overlap nhau như các địa chỉ khác nhau trên Bitcoin. 
	Để xem được balance shielded cần kết hợp địa chỉ với ivk. ivk key này dùng để giải mã các giao dịch private trên mạng và kiểm tra balance private trên tài khoản.
	Total Balance là tổng balance trên các địa chỉ.

THÔNG THƯỜNG PHÍA VÍ SẼ CHỈ SHOW 1, 2, 3. CHỈ DÙNG 4 BÊN DƯỚI APP ĐỂ XỬ LÍ CÁC MẶT KĨ THUẬT KHÁC: QUERY HISTORY, TÍNH TOÁN UTXOs, QUERY SHIELDED BALANCE ...

ĐỂ SUPPORT MẠNG MIDNIGHT CẦN TÍCH HỢP THÊM KEYRING MIDNIGHT; HỖ TRỢ ĐƯỢC CÁC ĐỊA CHỈ 1,2,3 TRÊN NHẰM MỤC ĐÍCH HIỂN THỊ VÀ THỰC HIỆN GIAO DỊCH; XỬ LÍ VÀ TỔNG HỢP ĐƯỢC BALANCE RIÊNG VÀ BALANCE TỔNG. 

KHI XỬ LÍ CHI TIẾT PHẦN NÀY CẦN XEM KĨ HƠN VÀ PHÂN LOẠI CÁC THÔNG TIN PUBLIC/PRIVATE/PUBLIC 1 PHẦN ĐỂ HIỂN THỊ CHO USERS!


