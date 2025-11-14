Đặc điểm:
- Members (Signatories)
- Threshold
Tác dụng:
- 2FA mechanism. Slow downs interactions but improve secure.
- board decisions: businesses use multisigs to govern the treasury.
- group participation in governance: multisig acc can do everything a regular acc can do.
Hành động với transactions:
- asMulti
- approveAsMulti
- cancelAsMulti
Note:
- Multi-sig acc không thể thay đổi members hoặc threshold sau khi tạo.
- Do đó, multi-sig acc address là deterministic, có thể sinh nếu biết trước các member và threshold. Từ đó việc send tokens tới địa chỉ chưa được tạo là khả thi. Lúc nào multi-sig acc được khởi tạo thì sẽ ngay lập tức có thể sử dụng những tokens đó.
- Khi 1 người approve multisig tx thì không thể nào revoke approve
- Signatures phải được xếp theo thứ tự alphabet

![[Pasted image 20250924165600.png]]

![[Pasted image 20250930155933.png]]
Sao chỗ này bảo signatories khác reject được.

- Chú ý case tạo multisig account rồi remove 1 account signatory
	- Cảnh báo liên quan tới multisig account?

Q?
1. Lí do mất phí:
- Thực hiện giao dịch để remark 1 thông tin trên on-chain (Việc tạo ra multi-sig account). Lý do remark?
	- 

2. Vì sao thằng cuối không luôn luôn approve cho giao dịch mà phải phụ thuộc vào th gọi asMulti cuối?

3. Cách setup multisig account pure proxy (puppet)
- ![[Pasted image 20251009145457.png]]

- A,B,C -> MultisigAccount -> PureProxy

References: 
- https://wiki.polkadot.com/learn/learn-account-multisig/
	- https://wiki.polkadot.com/general/multisig-apps/
	- https://wiki.polkadot.com/learn/learn-guides-accounts-multisig/
		- https://support.polkadot.network/support/solutions/articles/65000181826-how-to-create-and-use-a-multisig-account
		- https://www.youtube.com/watch?v=Qv_nJVcvQr8&t=2109s