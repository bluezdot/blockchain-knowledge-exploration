- CIP2: https://cips.cardano.org/cip/CIP-2 (Không gồm thuật toán adjust coin selections for network fee và minUTxO).
- MeshJS: default xài `experiment`, ngoài ra có `LargeFirst, LargeFirstMultiAsset, KeepRelevant`
- csl: xài `LargestFirstMultiAsset`

- Problem:
	- Size limitation
	- Dust
		- Nhỏ so với payment thông thường của user
		- Nhỏ so với phí tx

- Algorithm có thể failed khi:
	- Không đủ balance
	- UTxO không phân mảnh đủ để chi tiêu cho số lượng payment
	- UTxO Fully Depleted??
	- Max input count#

Experiment trong thực tế thường tốt hơn trong việc giảm thiểu dust và duy trì UTxO có lợi. Tuy nhiên trong 1 số trường hợp hiếm, Experiment lại bị fail. -> nên xử lí: Experiment và fall-back Largest-First

Cơ sở lí luận:
- Khả năng chọn trúng utxos dust tỉ lệ với lượng utxos dust trong ví.
- Tạo ra change utxos tương đồng nhất với các lần sử dụng. Giúp tăng khả năng tối ưu việc chọn utxos cho những lần sau.

Thảo luận về vấn đề này: https://discord.com/channels/907191435864977459/1322166040662380585. Team MeshJS đang nghiên cứu để đưa ra 1 thuật toán tốt hơn, các thuật toán hiện tại đang để 1 ngưỡng mặc định là UTxOs đầu vào phải có value >= 5. Có thể giảm ngưỡng này xuống nhưng sẽ không đảm bảo được việc tx được build và gửi thành công.

Coin selection là 1 topic lớn và rất phức tạp với nhiều vấn đề khó để giải quyết. Mỗi sản phẩm hỗ trợ việc chọn utxo tối thiểu cần giải quyết được 2 vấn đề này:
- Có thuật toán chọn utxos từ utxo set để chi trả cho các output
- Có thuật toán để thay đổi utxos để chi trả network fee.
- Đặc biệt với Cardano: Thuật toán để thay đổi utxos đáp ứng minUTxO. Cái khó ở đây là việc tính toán minUTxO xảy ra sau 2 thuật toán chọn utxos trước, dẫn đến nhiều case và hành vi kì quặc và cần đưa ra thuật toán phù hợp giải quyết vấn đề đó.