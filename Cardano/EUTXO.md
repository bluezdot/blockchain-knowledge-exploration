- sách: https://cardano.vn/files/EUTXO_handbook_for_EC_vi.pdf
- book: https://ucarecdn.com/3da33f2f-73ac-4c9b-844b-f215dcce0628/EUTXOhandbook_for_EC.pdf

- Giao dịch thành công hay không chỉ phụ thuộc vào bản thân giao dịch và input, chứ không phụ thuộc vào bất kì state nào khác trên blockchain => Có thể validate offchain. Trái với trên Ethereum, các giao dịch có thể thất bại giữa chừng. Một giao dịch có thể không thành công trong trường hợp nhiều giao dịch sử dụng đồng thời một input. -> Đây là lợi ích của UTXOs Balance model.
- Khá tương tự giao dịch tiền mặt: Mỗi tờ tiền là 1 UTXOs, với giá trị khác nhau; Số dư của 1 người là tổng giá trị của tất cả UTXOs; Để thanh toán cần lựa chọn các UTXOs để làm đầu vào giao dịch, giá trị đầu vào phải >= giá trị cần chi trả, trong trường hợp lớp hơn thì sẽ có UTXO thừa trả về. Khác biệt giữa tiền mặt vào UTXOs:
	- Tiền mặt: Input của người gửi sẽ trở thành UTXO của người nhận. Người nhận trả 1 UTXO khác cho người gửi.
	- UTXOs: Input sẽ bị tiêu huỷ hoàn toàn. Người nhận nhận UTXOs đúng bằng giá trị giao dịch, người gửi nhận UTXOs bằng giá trị thừa so với Input vừa sử dụng.
	- Bất lợi: Trong khi mô hình giao dịch tiền mặt truyền thống luôn đảm bảo được số lượng của từng loại tiền, mô hình UTXOs sẽ có thể gây ra vấn đề UTXOs bị phân mảnh thành nhiều UTXO bụi. Lúc này có thể merge các UTXOs nhỏ thành 1 UTXO lớn nhưng sẽ tốn chi phí.
	- Lợi ích: Giao dịch sẽ luôn thực hiện được mà người nhận ko cần phải có tiền thừa trả lại. Mô hình kế toán đơn giản và giảm logic xử lí.
- Tại mỗi thời điểm, mỗi node lưu 1 bản ghi ChainState, bao gồm tất cả giao dịch mới nhất và tập hợp UTXOs.
- EUTXOs kết hợp:
	- Contract
	- Redemer
	- Datum
	- Context

	- Giúp giải quyết vấn đề mà UTXO model cũ không cung cấp:
		- Duy trì trạng thái contract; `continuity`: giúp cùng 1 contract code được sử dụng suốt chuỗi giao dịch; `datum`: script dữ liệu contract cụ thể, sử dụng trong lúc node validate transaction.
		- Phí được tính chính xác trước khi gửi
	- `EUTXO = UTXO + contract script + datum`