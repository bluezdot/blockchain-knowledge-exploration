https://github.com/paritytech/polkadot-sdk/issues?q=state%3Aopen%20label%3A%22T6-XCM%22
- https://github.com/paritytech/polkadot-sdk/issues/6774
- https://github.com/paritytech/polkadot-sdk/issues/6126 (rcm)
- https://github.com/paritytech/polkadot-sdk/issues/5160
- https://github.com/paritytech/polkadot-sdk/pull/7641

ExcutionFee, DeliveryFee, otherFee?
Hiện tại chỉ có cách Dry-run mới biết fee.
Dry-run dở ở chỗ là nếu 1 step nào đấy ko đủ fee thì nó failed luôn và ko biết cần bao nhiêu fee. 
-> Việc kết hợp xcmPaymenApi và Dry-run khá là cồng kềnh.

- https://github.com/paritytech/polkadot-sdk/pull/4834
- https://github.com/paritytech/polkadot-sdk/pull/3872

- https://docs.moonbeam.network/builders/interoperability/xcm/send-execute-xcm/#dry-run-xcm-api-method