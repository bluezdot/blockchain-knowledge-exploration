
Main Logic:
``` C++
void OnTick() {
    // Kiểm tra và quản lý chuỗi lệnh
    CheckAndManageChain(buyChain);
    CheckAndManageChain(sellChain);

    // Kiểm tra và thiết lập chuỗi lệnh nếu chưa có
    if(!buyChain.active) ScanForTrend(true);
    if(!sellChain.active) ScanForTrend(false);
    
    // Liên tục update TP dựa theo tick mới
    UpdateTP(1);
    UpdateTP(-1);
```

Giải thích nhanh:
- Với mỗi Tick:
	- Kiểm tra buyChain, sellChain còn hợp lệ không, nếu không thì cần đóng

Detail logic:
1. ScanForTrend
	``` C++
 void ScanForTrend(bool isBuy) {
    MqlRates rates[];
    ArraySetAsSeries(rates, true);
    int barsNeeded = MathMax(InpBaseTrendCandles + 2, InpMaxFindSwingCandles + 2);
    if(CopyRates(_Symbol, _Period, 0, barsNeeded, rates) < barsNeeded) return;

    // Assume trend is true
    bool isTrend = true;
    // To sum up range
    double range = 0;

    for(int i=1; i <= InpBaseTrendCandles; i++) {
        if(isBuy) {
            if(rates[i].close <= rates[i+1].close) isTrend = false;
        } else {
            if(rates[i].close >= rates[i+1].close) isTrend = false;
        }
        range += MathAbs(rates[i].high - rates[i].low);
    }

    if(isTrend && range >= InpBaseTrendRange * _Point) {
        bool pullback = false;
        if(isBuy) {
            // Lấy HIGH cao nhất bao gồm cả nến hiện tại (rates[0]) đến hết trend candles
            double currentSwingHigh = rates[0].high;
            for(int k = 1; k <= InpBaseTrendCandles; k++)
                currentSwingHigh = MathMax(currentSwingHigh, rates[k].high);

            if(rates[0].close < rates[1].low) pullback = true;
            if(pullback) SetupDCAChain(1, currentSwingHigh, FindSwingLow(rates));
        } else {
            // Lấy LOW thấp nhất bao gồm cả nến hiện tại (rates[0]) đến hết trend candles
            double currentSwingLow = rates[0].low;
            for(int k = 1; k <= InpBaseTrendCandles; k++)
                currentSwingLow = MathMin(currentSwingLow, rates[k].low);

            if(rates[0].close > rates[1].high) pullback = true;
            if(pullback) SetupDCAChain(-1, currentSwingLow, FindSwingHigh(rates));
        }
    }
}
	```
2. CheckAndManageChain
``` C++
void CheckAndManageChain(TrendState &state) {
    if(!state.active) return;
    double currentPrice = SymbolInfoDouble(_Symbol, SYMBOL_BID);
    bool hasPos = HasPositions(state.direction);
    bool hasPendingOrders = HasPendingLimits(state.direction);

    // Ghi nhận từng có vị thế khi có lệnh đang mở
    if(hasPos) state.hadPositions = true;

    if(!hasPos) {
        // Trường hợp 1: Chưa khớp lệnh nào, giá phá swing → xu hướng sai
        if((state.direction == 1  && currentPrice >= state.swingHigh) ||
           (state.direction == -1 && currentPrice <= state.swingHigh)) {
            CancelAllLimits(state.direction);
            state.active = false;
            state.hadPositions = false;
            return;
        }

        // Trường hợp 2: Đã từng có vị thế, tất cả đã đóng (hit TP/SL)
        //               Còn lệnh limit treo → hủy hết và reset chain
        if(state.hadPositions) {
            CancelAllLimits(state.direction);
            state.active = false;
            state.hadPositions = false;
            return;
        }

        // Trường hợp 3: Không có vị thế VÀ không còn lệnh limit nào
        //               (chain còn active nhưng không còn gì để quản lý)
        if (!hasPendingOrders) {
            CancelAllLimits(state.direction);
            state.active = false;
            state.hadPositions = false;
            return;
        }
    }
}
```
3. UpdateTP
``` C++
void UpdateTP(int dir) {
    double lowestFiboPrice = (dir == 1) ? 999999 : 0;
    double currentTP = 0;
    int count = 0;
    
    // Tìm lệnh đã khớp sâu nhất
    for(int i=PositionsTotal()-1; i>=0; i--) {
        ulong ticket = PositionGetTicket(i);
        if(PositionSelectByTicket(ticket) && PositionGetInteger(POSITION_MAGIC) == InpMagicNumber) {
            if((dir == 1 && PositionGetInteger(POSITION_TYPE) == POSITION_TYPE_BUY) ||
               (dir == -1 && PositionGetInteger(POSITION_TYPE) == POSITION_TYPE_SELL)) {
                double openPrice = PositionGetDouble(POSITION_PRICE_OPEN);
                if(dir == 1) lowestFiboPrice = MathMin(lowestFiboPrice, openPrice);
                else lowestFiboPrice = MathMax(lowestFiboPrice, openPrice);
                count++;
            }
        }
    }

    if(count == 0) return;

    // Xác định mức TP theo logic Fibo
    TrendState active = (dir == 1) ? buyChain : sellChain;
    double diff = MathAbs(active.swingHigh - active.swingLow);
    double hitLevel = MathAbs(active.swingHigh - lowestFiboPrice) / diff;

    double targetFibo = 0;
    if(hitLevel <= 0.382) targetFibo = 0;
    else if(hitLevel <= 0.618) targetFibo = 0.236;
    else if(hitLevel <= 0.786) targetFibo = 0.5;
    else if(hitLevel <= 1.0)   targetFibo = 0.618;
    else if(hitLevel <= 1.618) targetFibo = 0.786;
    else if(hitLevel <= 2.618) targetFibo = 1.0;
    else if(hitLevel <= 3.618) targetFibo = 1.618;
    else targetFibo = 2.618;

    currentTP = (dir == 1) ? active.swingHigh - (targetFibo * diff) : active.swingHigh + (targetFibo * diff);

    // Áp dụng TP cho toàn bộ vị thế
    for(int i=PositionsTotal()-1; i>=0; i--) {
        if(PositionSelectByTicket(PositionGetTicket(i)) && PositionGetInteger(POSITION_MAGIC) == InpMagicNumber) {
            if(MathAbs(PositionGetDouble(POSITION_TP) - currentTP) > _Point) {
                trade.PositionModify(PositionGetTicket(i), 0, currentTP);
            }
        }
    }
}
```

Full code: ...

Note:
- Kiểm tra lại điều kiện pullback, hiện tại chỉ xác định pullback dựa trên close price của nến hiện tại < low price của cây nến ngay trước.
- Fix Việc lấy Swing High. Hiện tại đang lấy theo rates[1].
- InpBaseTrendRange tính theo Point. Cẩn thận khi Point ở từng sàn có giá trị khác nhau.
- DCA 10 cấp hơi nhiều, nên giảm xuống còn 5.
- Con Bot này nên tránh giờ tin tức mạnh:
	- NFP (Thứ 6 đầu tháng, 21:30 GMT+7)
	- CPI/Lãi suất Fed
	- Căng thẳng địa chính trị
	- Phiên tốt nhất: London - NY -- 15:00 - 22:00 (GMT+7)