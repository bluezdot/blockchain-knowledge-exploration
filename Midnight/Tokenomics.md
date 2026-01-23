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

- References: Midnight Preview Testnet Guide