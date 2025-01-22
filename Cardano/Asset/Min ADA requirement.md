- max No. UTxOs <= (total ADA in circulation) / (min-ada-value)
- max UTxO size <= (max No. UTxOs) * (max UTxO entry size) + overhead
- `minUTxOValue`, `adaOnlyUTxOSize`, `sizeBound(u)`, `minAda(u)`, `minUTxOValue / adaOnlyUTxOSize = minAda(u) / sizeBound(u)`

Ref:
- https://docs.cardano.org/developer-resources/native-tokens
- https://cardano-ledger.readthedocs.io/en/latest/explanations/min-utxo-alonzo.html
- https://www.reddit.com/r/CryptoCurrency/comments/1dl9zzw/the_ugly_parts_of_using_cardano_utxo_limitations/
- https://forum.cardano.org/t/minimum-ada-per-transaction/55109
- https://github.com/StricaHQ/typhonjs/blob/master/src/transaction/transactionBuilder.ts: `minUtxo`, `calculateMinUtxoAmountBabbage`
- https://cardano.stackexchange.com/questions/507/is-there-a-minimum-amount-of-ada-i-can-send-in-a-transaction: `min = 1ADA + fees`.
- https://www.reddit.com/r/CardanoDevelopers/comments/tjirid/comment/i1kmeza/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button: UTxO containing an output `txout`=> `min = utxoEntrySize (txout) * coinsPerUTxOWord = [utxoEntrySizeWithoutVal + size (v) + dataHashSize (dh)] * coinsPerUTxOWord`
- https://emurgohelpdesk.zendesk.com/hc/en-us/articles/5008187102351-What-is-the-locked-assets-deposit: EMURGO giải thích về locked ADA

- send CNA on typhoon:
![[Pasted image 20241217153558.png]] ![[Pasted image 20241217153726.png]]
- Send CNA trên Nami: Có thay đổi minADA theo amount CNA
![[Pasted image 20241217153918.png]]
![[Pasted image 20241217154047.png]]


- Mục đích:
	- Tránh UTxOs bloat
	- minADA được yêu cầu lock vào mỗi UTxO dựa theo storage size, giúp hạn chế lãng phí UTxO size trên sổ cái.
	- Khuyến khích giải phóng ADA locked = cách merge UTXOs trong ví.
	- Hydra - layer 2 solution cho Cardano cho phép send ADA < 1.
	- 