1. Tạo keypair gồm PrivateKey và PublicKey từ Mnemonic
- Logic chính: keyring > createFromUri.
- Flow: xác định derivedPath, tạo seed, tạo derivedKeyPair (HD)
- Lưu ý: derivePath nên nghiên cứu các ví khác để dễ tương thích.
   
2. Sinh địa chỉ từ public key
- Logic chính: pair > index
- Lưu ý: 
	- Xử lí case read only.
	- Check xem địa chỉ testnet/mainnet có thể sinh ra địa chỉ của nhau ko, hoặc từ raw address. Tương tự với các địa chỉ mạng khác nhau trên cùng 1 blockchain.

3. Xử lí encode/decode address. Encode xử lí address được thêm mới address/raw address -> address. Decode mã hoá address -> address/raw address
- Logic chính: utils > address > encode/decode.
- Lưu ý: Nếu có nhiều mạng trên cùng 1 loại, xem xét sử dụng `ss58Format` để xác định mạng.

5. encode/decode pairs. Tương tự address, encode/decode pairs giúp mã hoá thông tin liên quan đến keypairs và tái tạo keypairs từ đoạn mã hoá.
- Logic chính: pair > decodePair, encodePair
- Lưu ý: Check độ dài của keypair để xem thuật toán có mã hoá và giải mã chính xác cặp keypair ko.

6. sign
- Logic chính: pair > index > viết thêm 1 con signer
- Lưu ý:
	- Cần nghiên cứu logic kí của mạng. Thông thường: (message, key) => signedMessageHex

7. derive: auto, custom
- Logic chính: pair > index
- Lưu ý: Nên xem logic derive account/address của các ví khác hiện có để dễ tương thích

8. exportJson
- Test lại toJson

9. exportMnemonic
- Test lại trong pair > index > exportMnemonic