Các bước:
- Dựng tx
- Kí (đã xử lí)
- Submit transaction (API)


- MeshJS -> yêu cầu node version >= 20, cần input `initior` wallet (mesh/react - thư viện UI)
- Lucid - không import được
- Emurgo/cardano-serialization-lib (https://github.com/Emurgo/cardano-serialization-lib/blob/master/doc/getting-started/generating-transactions.md)

- https://forum.cardano.org/t/how-can-i-transfer-ada-to-another-address-if-the-utxo-contains-multi-native-assets-cardano-serialization-library/115180
- https://forum.cardano.org/t/include-utxo-containing-both-ada-and-assets-in-a-transaction-sending-ada-only/106689/4
- https://github.com/Emurgo/cardano-serialization-lib/issues/390: Thảo luận về cách sử dụng construct TransactionUnspentOutput