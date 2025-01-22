- Để tạo được masteraccount chứa account mạng mới:
	- From source: `packages/extension-base/src/services/keyring-service/context/handlers/Mnemonic.ts`.
	- Check functions:
		- mnemonicCreateV2
		- accountsCreateSuriV2
		
	- From source: `packages/extension-base/src/utils/account/transform.ts`
	- Check function: getAccountChainType

- Để hiển thị được địa chỉ khi copy address từ account
	- ![[Pasted image 20241127141206.png]]
	- From source: `packages/extension-koni-ui/src/hooks/account/useGetAccountChainAddresses.tsx`.
	- Check functions:
		- getChainsByAccountType
			- isChainCompatibleWithAccountChainTypes
				- isChainInfoAccordantAccountChainType
		- getReformatedAddressRelatedToChain
			- getKeypairTypeByAddress (keyring)
			- networkPrefix
			- encodeAddress (keyring)
			- decodeAddress (keyring)

- Để thực hiện được transaction trên blockchain mới cần xử lí list action cho phép của loại account đó:
	- From source: `packages/extension-base/src/utils/account/transform.ts`
	- Check functions: 
		- `getAccountTransactionActions`
		- có thể check thêm `getAccountActions`


- UI để tạo giao dịch gửi Cardano 