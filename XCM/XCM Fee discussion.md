- Reference
https://github.com/paritytech/polkadot-sdk/issues/690

https://github.com/paritytech/polkadot-sdk/issues/3434 -> Expect to have a `BuyTransport` for deliveryFee to pay once and ensure the rest of execution will succeed. -> Discuss 1 hồi thì chuyển sang `DepositFee`
![[Pasted image 20250325105150.png]]

https://github.com/paritytech/polkadot-sdk/pull/3607

https://github.com/polkadot-fellows/xcm-format/pull/53 -> Acala cần additional storage deposit cho EVM calls. Có ý kiến cho răng có thể Estimate trước fee để chắc chắn đủ hoặc thừa, thừa có thể đẩy vào AssetTrap và Claim sau.

https://github.com/paritytech/polkadot-sdk/pull/5420: XCM v5

https://github.com/polkadot-fellows/RFCs/pull/105: Improve fee mechanism

https://polkadot-fellows.github.io/RFCs/approved/0105-xcm-improved-fee-mechanism.html: RFC-0105: Improve fee mechanism. 
ExecutionFee, DeliveryFee, AdditionalFee.
holding register -> dedicated fees register. BuyExecution -> PayFees. PayFees sẽ xử lí nhiều loại fee hơn so với BuyExecution hiện tại.


https://forum.polkadot.network/t/xcm-rfc-better-fee-handling/6547
https://forum.polkadot.network/t/improve-xcm-development-and-release-process/6497/6

https://forum.polkadot.network/t/xcm-user-and-developer-experience-improvements/4511/21
Proposal for improving XCM Fees painpoint:
1. Sử dụng token khác trả phí chỉ xử lí được cho ExecutionFee -> Improve để có thể trả mọi phí = token khác
2. Hợp nhất XCM executionFee và deliveryFee -> XCMv5
3. Enable complex action: Gửi nhiều loại token và xuyên nhiều chain.
4. Fix UX asset trap
5. Custom XCM

- XCM - Bridge Roadmap: https://github.com/orgs/paritytech/projects/145
- XCM Format: https://github.com/polkadot-fellows/xcm-format/blob/master/README.md
- High-level lib (wrap pallet-xcm & xtokens): https://github.com/paritytech/asset-transfer-api
- High-level lib: https://github.com/paraspell/xcm-tools/tree/main
- High-level lib: https://moonbeam-foundation.github.io/xcm-sdk/latest/
