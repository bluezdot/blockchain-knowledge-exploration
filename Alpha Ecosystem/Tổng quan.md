1. Alpha Bridge (Rubicon)
	1. Bittensor <-> Subtensor EVM
		- Bản chất mạng substrate và mạng EVM này là 1, mỗi địa chỉ substrate và evm được ánh xạ và có thể convert qua nhau. Tài sản sẽ hiển thị đồng thời trên evm và substrate.
		- Để bridge thì bản chất chỉ là transfer in-chain:
			- Bittensor <-> Bittensor
			- Subtensor EVM <-> Substensor EVM
	2. Subtensor EVM <-> Base Mainnet
		- Thực hiện Crosschain thông qua CCIP của Chainlink.
2. Liquid Staking (Rubicon)
	1. Stake TAO/Alpha token để mint xTAO/xAlpha
		- TAO -> xTAO/xAlpha
		- Alpha -> xAlpha
	2. Cần hỗ trợ trên ví các tính năng: Stake, Stake more, Unstake, hiển thị balance xAlpha, xTAO.
3. Delegated Staking (TrustedStake)
	1. Cần hỗ trợ trên ví các tính năng: Delegate Stake, Undelegate Stake. Có nhiều Strategies để lựa chọn Delegate
4. (Backprop Finance)