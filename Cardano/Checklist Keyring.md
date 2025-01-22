- CreateFromUri
	- 1. derivePath: `m / 1852' / 1815' / account' / role / index`
		- index: thứ tự account
		- type: 
			- 0 - payment address
			- 1 - change address
			- 2 - staking address
		- type theo CIP-1852: 
			- 0 - External chain
			- 1 - Internal chain
			- 2 - Staking key
			- 3 - DRep Key
			- 4 - Cold Key
			- 5 - Hot Key
	- 2. Xử lí mnemonic + derivePath -> seed (xử lí tương tự Daedalous)
	- 3. Seed -> Keypair (Hd derivation)
	- 4. Keypair -> address
	- 5. mnemonic -> entropy


![[Pasted image 20241017170042.png]]

Mn hay nói về Shelley address. Không biết có sự khác nhau gì giữa việc derive ra địa chỉ của các Era ko.

https://cips.cardano.org/cip/CIP-19: Cardano Addresses
- Shelley address
- Legacy address: Byron era (Introduce before Bech32)

- Bech32, Base58 encoding
- Hiện có 11 loại địa chỉ:
	- 8 loại Shelly
		- Payment part
		- Delegation part
	- 2 loại stake
	- 1 loại Byron

- ABNF: https://raw.githubusercontent.com/cardano-foundation/CIPs/master/CIP-0019/CIP-0019-cardano-addresses.abnf

- Lý do thay đổi 1852' thay vì 44':
	- Phân biệt với Byron-era address
	- Gây chú ý rằng Cardano dùng BIP32-Ed25519 thay vì BIP32 tiêu chuẩn
	- Gia tăng khả năng mở rộng
	- 1852 và 1815 là ngày mất và ngày sinh của Ada Lovelace.

- Đoán: address = Bech32(blake2b-224(pubKey))


path: "m/1852'/1815'/{index}'/{role}/0"
- chốt: ko support derive địa chỉ.
- import/export: seedphrase, json, (privateKey?).
- check privacy derive address.
- tìm index và kiểu địa chỉ của các ví khác.
	- Nami tương tự mình
	- Yoroi có derive address thì mình hiện chốt là k support derive address
- giữa các era mà có các địa chỉ khác nhau thì có bị giới hạn tính năng k.
- Kiểm tra tính năng derive.
	- Kiểm tra trên ví Nami thì thấy derive account mới tương ứng với tăng index.
- export json -> check lại xem đáp ứng được cả export only cardano và all account chưa (tên, metadata, ...).
	- Ko ảnh hưởng.

![[Pasted image 20241024100257.png]]


----
Các phần trọng tâm cần sửa: Keyring:

1. createFromUri -> Xử lí để tạo keypairs, tương ứng việc tạo account (*)
2. encode/decode address (*)
3. encode/decode pairs

Create Pair:

4. convert public key -> address (*)
5. sign (*)
	1. https://github.com/Emurgo/cardano-serialization-lib/blob/4792b1b121e728a51686d5fcbffd33489d65c903/rust/src/crypto.rs#L279
	2. https://github.com/erdtman/cose-js
	3. https://github.com/Emurgo/message-signing
6. derive: auto, custom
7. exportJson
8. exportMnemonic

Comment:
- Flow khá ổn, nhưng vẫn còn khó theo dõi vì các step có thể phụ thuộc nhau
- Cần làm rõ hơn input output phục vụ cho testing từng step.
- 2,3 cần note thêm những case nhỏ cần xử lí: accountReadOnly, import json, ...


Submit transaction:
- https://ogmios.dev/mini-protocols/local-tx-submission/