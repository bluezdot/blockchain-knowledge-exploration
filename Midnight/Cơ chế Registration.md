2. Cơ chế đăng kí
Tổng quan: Midnight có cơ chế đăng kí để việc nắm giữ/liên kết NIGHT có thể giúp 1 địa chỉ sản sinh ra được DUST. Phần này mô tả quy trình từ token cNIGHT nắm giữ (là token CNA trên mạng Cardano), cho tới việc sản sinh DUST trên mạng Midnight.

2.1. Đăng kí DUST generation
	- Trong Midnight, việc có NIGHT trong ví là chưa đủ. Cần phải thực hiện một bước gọi là destinate trên chuỗi Cardano để kích hoạt sinh DUST.
	- Logic: `Cardano Wallet Address` (Nguồn tạo) $\rightarrow$ `Midnight DUST Address` (Đích nhận). Cần xác định 2 thông tin địa chỉ này. Cần đảm bảo không có registration UTxO nào đã gắn với Cardano wallet. Nếu có thì cần deregister trước.
	- Cơ chế lưu trữ: Tạo 1 transaction để gửi thông tin mapping này với inline datum (dữ liệu đính vào tx). Thông tin ánh xạ này được lưu trong một **UTxO đặc biệt** tại "Mapping Validator" trên Cardano với dữ liệu đính kèm (Inline Datum - chuẩn CIP-0032).
	- Độ trễ: Sau khi gửi giao dịch trên Cardano, cần khoảng **432 block Midnight (~2.5 giờ)** để hệ thống quan sát (Observability Layer) ghi nhận và bắt đầu sinh DUST.
	- Khi relay thông tin đăng kí tới mạng Midnight thành công. UTxOs tới wallet gốc cũng được tính vào việc sinh DUST cho mapped address.
2.2. Quản lí trạng thái
	Giúp check trạng thái Cardano wallet address.
	- **Unregistered:** Chưa có UTxO đăng ký nào. Không sinh DUST.
	- **Pending:** Giao dịch đăng ký đã thấy trên Cardano nhưng đang trong quá trình "Relay" sang Midnight.
	- **Registered:** Đăng ký thành công và hợp lệ. Mọi NIGHT UTxO mới nhận sẽ sinh DUST.
	- **Invalid:** Có đăng ký nhưng không được node chấp nhận (sai định dạng hoặc bị thay thế).
	
2.3.  Quy tắc "Generating" vs "Non-generating" UTxOs
	- **Generating:** Chỉ những NIGHT UTxO được tạo ra sau thời điểm đăng ký (Registration Time) mới tự động sinh DUST.
	- **Non-generating:** Những NIGHT UTxO đã nằm sẵn trong ví trước khi đăng ký sẽ không sinh DUST.
	- **Giải pháp:** Để kích hoạt DUST cho các token cũ, bạn phải thực hiện một giao dịch tự gửi cho chính mình trên Cardano để biến chúng thành UTxO mới (Post-registration).

2.4. Các tính năng nâng cao
- Update địa chỉ DUST: 
	- Yêu cầu hiện đang register và đã relayed. Sở hữu account Cardano và xác định địa chỉ DUST address mới.
	- Thực hiện: Deregister + new register trong cùng 1 giao dịch Cardano.
- Huỷ đăng kí: Sử dụng UTxO đăng ký tại Mapping Validator mà không tạo output mới.
- Dừng sinh DUST: Chi tiêu các NIGHT UTxO Generating
- Xử lý đăng ký thừa: Hệ thống ưu tiên timestamp/slot mới nhất. Nên clean các UTxO thừa.

Note:
- Mapping Validator: Đóng vai trò như 1 Smart Contract gác cổng trên mạng Cardano. Quản lí việc tạo, cập nhật, huỷ bỏ các bản ghi ánh xạ địa chỉ Cardano.  và địa chỉ DUST Midnight. Khi đăng kí, thực tế đang gửi 1 lượng ADA nhỏ - UTxO mapping để làm storage deposit cho Script này.
	- Liên quan đến CIP32
	- Dữ liệu thường cóc format là:
		{
		  "cardano_address": "addr1...", // Địa chỉ sở hữu NIGHT
		  "midnight_dust_address": "0x..." // Địa chỉ sẽ nhận DUST
		}