![[Pasted image 20241023173815.png]]
substrate:
- Key: `account:publicKey`
- address: subtrateAddress
- encoded: logic này dùng chung
- encoding và meta: 2 phần này không cần xử lí
evm:
- Key: `account:evmAddress`
- address: publicKey
còn lại:
- Key: `account:publicKey` hoặc `account:rawKey`. Check phụ thuộc vào meta.noPubKey
- address: publicKey hoặc rawKey.
