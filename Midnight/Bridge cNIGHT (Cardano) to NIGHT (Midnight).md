Việc "bridge" **cNIGHT** (Cardano NIGHT) từ Cardano sang Midnight thực tế không phải là một giao dịch chuyển đổi tài sản thông thường như qua các cầu nối (bridge) giữa Ethereum và Layer 2. Trong hệ sinh thái Midnight, đây là một quy trình **chuyển đổi trạng thái (State Transition)**.

Note: **Mapping Validator** đóng vai trò cực kỳ quan trọng, nhưng nó không phải là cái cầu để tài sản "đi qua", mà là **điều kiện cần** để tài sản được công nhận bên mạng Midnight.

### 1. Quy trình Bridge cNIGHT (Về mặt kỹ thuật)

Để đưa cNIGHT từ Cardano sang Midnight, bạn thực hiện qua các bước sau
#### Bước 1: Shielding (Khóa tài sản trên Cardano)
Bạn không gửi NIGHT vào một địa chỉ ví Midnight thông thường. Thay vào đó, bạn gửi cNIGHT vào một **Shielding Contract** trên mạng Cardano.
- Khi giao dịch này được xác nhận, số cNIGHT đó bị "khóa" lại trên Cardano.
- Hệ thống **Observability Layer** của Midnight sẽ quét thấy sự kiện này.    

#### Bước 2: Đúc (Mint) NIGHT trên Midnight
Sau khi vượt qua thời gian chờ, mạng lưới Midnight sẽ cho phép bạn "mint" (đúc) một lượng NIGHT tương ứng dưới dạng **Private Token** trên mạng Midnight.
- Lúc này, số NIGHT đó sẽ nằm ở **Shielded Address** mà bạn sở hữu.

### 2. Mapping Validator có liên quan như thế nào?

Câu trả lời là: **Có liên quan trực tiếp đến việc trả phí (Gas) để thực hiện lệnh Bridge.**

Hãy tưởng tượng Mapping Validator là bản đăng ký "hộ khẩu" để bạn được cấp "năng lượng" (DUST):

1. **Để "Mint" NIGHT trên Midnight:** Bạn cần thực hiện một giao dịch trên mạng Midnight. Để thực hiện giao dịch này, bạn cần có **DUST**.
2. **Để có DUST:** Bạn phải có một bản đăng ký hợp lệ tại **Mapping Validator** trên Cardano.
3. **Mối liên kết:** Nếu bạn chưa đăng ký tại Mapping Validator, NIGHT của bạn dù đã được Shielded thành công sang Midnight cũng sẽ bị "đóng băng" vì bạn không có DUST để làm phí gas thực hiện bất kỳ thao tác nào tiếp theo (như chuyển tiền hoặc tương tác dApp).

> **Tóm lại:** Mapping Validator không trực tiếp chuyển NIGHT đi, nhưng nó là bộ phận **kích hoạt bình năng lượng (DUST)** để bạn có thể sử dụng số NIGHT đó sau khi đã bridge sang Midnight.

---

### Luồng UX khuyến nghị

1. **Check Registration:** Kiểm tra xem địa chỉ ví Cardano của user đã có UTxO tại **Mapping Validator** chưa.
2. **If No:** Yêu cầu user ký 1 giao dịch có 2 hành động (outputs):
    - **Output 1:** Gửi NIGHT vào Shielding Contract.
    - **Output 2:** Gửi Mapping Datum vào Mapping Validator.
3. **Result:** User chỉ cần ký 1 lần, NIGHT vừa được bridge, vừa đảm bảo sau 2.5 giờ sẽ có DUST để sử dụng số NIGHT đó bên phía Midnight.
Note: 
- Nếu user đã bridge NIGHT sang Midnight trước khi đăng ký Mapping Validator:
	- Số NIGHT đó vẫn an toàn nhưng **không sinh ra DUST**.
	- Bạn cần hướng dẫn user thực hiện thao tác **"Respend to Self"** (như đã đề cập trong tài liệu) sau khi đã đăng ký Mapping Validator để kích hoạt dòng tiền DUST cho số NIGHT cũ này.