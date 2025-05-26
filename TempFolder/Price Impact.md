1. [What is Price Impact? – Uniswap Labs](https://support.uniswap.org/hc/en-us/articles/8671539602317-What-is-Price-Impact)
- Phụ thuộc liquidity pool. Khi pool có thanh khoản cao, price impact thấp
- Price impact càng cao càng ảnh hưởng tới lượng trade. 

[https://calculator.academy/price-impact-calculator/](https://calculator.academy/price-impact-calculator/) 

- PI = (V * P * D) / T

[https://stackoverflow.com/questions/74282316/uniswap-v2-pool-priceimpact](https://stackoverflow.com/questions/74282316/uniswap-v2-pool-priceimpact)  

**Short note: We apply Fomular 2.**
Base formulation:

- x token A; y token B; k = x * y; rate = x/y. x,y,k are known
- swap m token A to n token B. m is known
- x' = x + m
- y' = k / x' (Because x' * y' must also equal to k)
- n = y - y'

**Where:** 
- x: the amount of token A in Pool.
- x': the amount of token A in Pool after swappnig.
- y: the amount of token B in Pool.
- y': the amount of token B in Pool after swapping.
- k: Pool constant.
- m: Amount of token A used to swap.
- n: Amount of token B actual receive.
- Skip for fees and slippage impact.

------
- PI = n / y'

=> **Finally formula 1**: PI =  ym/k
- Detail: 
  - PI = n / y' = (y - y') / y' = (y - k / x') / (k / x') = (y - k / (x + m)) * (x + m) / k
= (y * (x + m) - k) / k = y * (x + m) / k - 1 =  y * m / k.

------
- PI = actual_rate/market_rate - 1
- m/n = actual_rate
- x/y = market_rate

=> **Finally formula 2**: PI = m/x
- Detail: 
  - m/n = m / (y - k / x') = m / (y - k / (x + m)) = m / ((xy + my - k) / (x + m)) = m * (x + m) / my = (x + m) / y.
  - PI = ((x + m) / y) /  (x / y) - 1 = (x + m) / x - 1 = m / x
 
[https://support.uniswap.org/hc/en-us/articles/8643794102669-Price-Impact-vs-Price-Slippage](https://support.uniswap.org/hc/en-us/articles/8643794102669-Price-Impact-vs-Price-Slippage)
- #### Price Impact
The change in token price directly caused by your trade. Price Impact is reflected as the difference between the current market price and how your trade impacts the total liquidity in a pool.  

- #### Price Slippage
The change in token price caused by the total movement of the entire current market. Price Slippage is reflected as the difference between the price you _expect_ to receive after swapping vs what you _actually_ receive after the swap is complete.

[https://dailydefi.org/articles/price-impact-and-how-to-calculate/](https://dailydefi.org/articles/price-impact-and-how-to-calculate/)  

- PI = (m/n) / (x/y) - 1

[https://medium.com/@arian.web3developer/everything-about-price-impact-slippage-rate-and-deadline-in-dex-swaps-explained-on-uniswap-dex-7f700ffa3948](https://medium.com/@arian.web3developer/everything-about-price-impact-slippage-rate-and-deadline-in-dex-swaps-explained-on-uniswap-dex-7f700ffa3948)  
- Tx Deadline: 

[https://blog.uniswap.org/uniswap-v3-math-primer](https://blog.uniswap.org/uniswap-v3-math-primer)  
[https://blog.uniswap.org/uniswap-v3-math-primer-2](https://blog.uniswap.org/uniswap-v3-math-primer-2)  
[https://medium.com/luchadores-chronicles/how-to-calculate-price-impact-b4c87d8b10ed](https://medium.com/luchadores-chronicles/how-to-calculate-price-impact-b4c87d8b10ed)



----
Updated: 2025.05.26

Nên sử dụng Formula 2 hiển thị cho tính năng swap của non-custodial wallet.

```
E research thì thấy 2 công thức dùng để đánh giá các ý nghĩa khác nhau. Công thức 3 để đánh giá sự tác động của swap đến price giữa 2 token trong pool sau khi swap xong -> Price pool sau so với Price pool ban đầu. Công thức 2 để đánh giá hiệu quả của tỉ lệ swap được so với Price ban đầu -> Price swap thực tế so với Price pool ban đầu.
Theo góc độ user sử dụng swap thì e thấy sử dụng công thức 2 sẽ hợp lí hơn vì nó quan tâm đến hiệu quả của swap thay vì quan tâm đến tác động của swap tới pool thanh khoản
```

https://grok.com/share/bGVnYWN5_49ea1b95-11e0-4f58-a099-8bebbcbc4e59