Document: https://docs.rubiconbridge.io/
dApp: https://www.rubiconbridge.io/
Explorer: https://evm.taostats.io/tokens
Convert address tool: https://snow-address-converter.netlify.app/

- Mạng triển khai: Subtensor EVM
- Công thức chuyển đổi: Khởi tạo với tỉ lệ 1:1. Sau đó: `xAlphaPrice = alphaBalance / xAlphaMinted * alphaPrice`
- xAlpha để làm gì? -> Bridge sang các mạng khác, tham gia DEFI.
- Các functionalities:
	- Mint: TAO,  Alpha -> xTAO, xAlpha.
	- Redeem: xTAO, xAlpha -> TAO,  Alpha.
	- Automatic Rebasing: xAlpha price càng ngày càng tăng.
	- CCIP Bridging: Xử lí bởi Chainlink CCIP
- Kiến trúc:
	![[Pasted image 20251223161926.png]]
	![[Pasted image 20251223161943.png]]

