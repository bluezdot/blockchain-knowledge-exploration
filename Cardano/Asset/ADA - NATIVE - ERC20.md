
|                                                | Ada | Native tokens | Comment                                                                                   |
| ---------------------------------------------- | --- | ------------- | ----------------------------------------------------------------------------------------- |
| Can be sent in transactions                    | Y   | Y             |                                                                                           |
| Can be kept in UTXO outputs                    | Y   | Y             |                                                                                           |
| Can be locked with script outputs              | Y   | Y             |                                                                                           |
| Can be sent to an exchange address             | Y   | Y             |                                                                                           |
| Can be minted/burned                           | N   | Y             | Ada cannot be created or destroyed,<br><br>its policy ID does not correspond to a script. |
| Can be used to pay fees, receive rewards, etc. | Y   | N             | Ada is the only currency which can be used for<br><br>paying fees and earning rewards.    |
| Can be used to cover the minimum UTXO value    | Y   | N             | Ada is the only currency which can be used for deposits.                                  |

|                                                               | ERC20                                                                  | Native tokens                                                           |
| ------------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Blockchain                                                    | Ethereum                                                               | Cardano                                                                 |
| Relationship to the blockchain                                | A contract standard, users copy-paste the standard code and modify it. | Not a standard. Most functionalityis built into the ledger itself.      |
| Controlled by                                                 | A Solidity smart contract                                              | A minting policy script in any scripting. language supported by Cardano |
| Requires a smart contract to mint/burn?                       | Y                                                                      | Y                                                                       |
| Minting logic can be customized?                              | Y                                                                      | Y                                                                       |
| Requires a smart contract to transfer?                        | Y                                                                      | N                                                                       |
| Can be used by other smart contracts without special support? | N                                                                      | Y                                                                       |
| Can be transferred alongside other tokens?                    | N                                                                      | Y                                                                       |
| Transfer logic provided by                                    | Copy-pasting from the ERC20 template                                   | The Cardano ledger itself                                               |
| Transfer logic can be customized                              | Y                                                                      | N                                                                       |
| Requires special fees to transfer                             | Y                                                                      | N                                                                       |
| Requires additional event-handling logic to track transfers   | Y                                                                      | N                                                                       |
| Supports non-fungible tokens                                  | N                                                                      | Y                                                                       |
| Readable metadata                                             | Provided by the operating smart<br><br>contract                        | Provided by the off-chain metadata server                               |

- Hơn 10k native token.

- asset:
	- asset ID:
		- policy ID
		- asset name (immutable)
		- 


ADA:
- Không thể tạo outputs chỉ chứa mỗi custom token
- Số lượng của custom token ko ảnh hưởng tới min-ada-value của output. Số loại custom token làm tăng min-ada-value. -> Vì làm tăng storage.
- Send custom tokens luôn bao gồm send min-ada-value tới địa chỉ nhận vào cùng output
- Lưu ý: Trước khi gửi custom tokens, người dùng có thể chọn off-chain communication để đàm phán ai cung cấp ada để cover min-ada-value.