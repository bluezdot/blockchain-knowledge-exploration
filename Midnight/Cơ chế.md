References: Midnight Preview Testnet Guide

# 1. Token

A. NIGHT (Currency):
- Native token.
- Sử dụng minh bạch cho payments, staking, governance.
- 1 NIGHT = 10^6 Stars.
B. DUST (Resource):
- Not a currency.
- Non-transferable.
- Sử dụng để trả phí.
- Được sản sinh do sự tồn tại của NIGHT thông qua cơ chế Designation: hold hoặc register NIGHT.
- 1 DUST = 10^15 Specks.

Lí do chia ra 2 loại token mà không dùng 1 cho vừa currency vừa fee:
- fee có thể leak metadata.
- tránh chi phí giao dịch biến động theo thị trường, cũng nhằm che dấu để tính các govern params được đưa ra không gây ảnh hưởng.
- thiết kế ưu tiên privacy yêu cầu tính dễ dàng thực hiện giao dịch cao.
=> Tách riêng 2 tokens để mỗi loại đảm nhận vai trò cụ thể của nó, tránh 1 thứ làm 2 nhiệm vụ gây phức tạp về mặt kĩ thuật.

Cơ chế Generation & Decay DUST
- Designation: Khi NIGHT được register cho 1 account, cụ thể là DUST address của account đó, DUST bắt đầu được tích luỹ tuyến tính theo thời gian cho tới maximum cap.
- Cap: Lượng DUST tối đa mà ví có thể giữ, phụ thuộc vào NIGHT đang sở hữu:
	- Cap = NIGHT x night_dust_rate
- Rate: Tốc độ generate và decay như nhau
	- Rate = NIGHT x generation_decay_rate
- Trạng thái UTXO: 
	- Nếu NIGHT UTXO chưa chi tiêu: DUST tăng dần đến khi chạm CAP thì dừng.
	- Nếu NIGHT UTXO bị chi tiêu: DUST sẽ tự giảm/tăng tới mức CAP mới theo Rate.

Example scenarios:
- Steady state: Giao dịch vừa phải, DUST luôn ở mức ổn định và tăng lên max cap trở lại.
- Burst usage: Giao dịch đột biến khiến DUST tạm cạn kiệt, sau đó tăng lên max cap lại.
- Change NIGHT (increase): Khiến Cap tăng, DUST tiếp tục tăng tuyến tính lên mức mới.
- Change NIGHT (decrease): Khiến Cap giảm, DUST giảm tuyến tính về 1 mức thấp hơn.

Query từ Node để estimate fee, các on-chains param (night_dust_rate, generation_decay_rate, dust_grace_period, ...) sẽ đảm bảo tính toán được thông số, DUST progress bar, ... chính xác.

C. MNTs (Midnight Native Tokens)
- Tương tự CNAs trên Cardano hay ERC-20 trên Ethereum. MNTs là chuẩn token được sinh từ smart-contract có thêm tính năng bảo mật nhờ công nghệ ZK.
- Có thể tồn tại cả dưới dạng Unshielded & Shielded (Custom) tokens.
- Hoạt động:
	- Minting: Đúc token, có thể không làm lộ tổng cung nếu cần
	- Transfer:
		- Public transfer -> Unshielded token.
		- Private transfer -> Shielded (Custom) token.
D. Custom tokens

# 2. Cơ chế đăng kí
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