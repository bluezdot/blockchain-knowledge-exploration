- Quy trình truyền tham số khi thực hiện multisig
    - Initiator:
        - `maybeTimepoint`: `null`.
        - `maxWeight`: `0` (Vì người đầu tiên chỉ tốn phí lưu trữ - deposit, chưa thực thi hàm nội bộ nên weight thực thi chưa cần thiết).
    - Approver
        - `maybeTimepoint`: Phải lấy từ block khởi tạo.
        - `maxWeight`: `0`.
    - Executor
        - `maybeTimepoint`: Phải lấy từ block khởi tạo.
            
        - `maxWeight`: **Phải là giá trị thực tế** (như cách dùng `paymentInfo` ở trên) để node có đủ gas thực thi hàm `extrinsic`.
  
   ```tsx
 			const sortedOthers = sortAddresses(otherSignatories);
 			const { weight } = await extrinsic.paymentInfo(sortedOthers[0]);
			```

