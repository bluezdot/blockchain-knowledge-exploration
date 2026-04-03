1. Bot bị restart trong những tình huống sau:
- Biên dịch lại code: Khi file code được biên dịch, Mt5 tự động update và restart bot đang chạy trên biểu đồ với code `REASON_RECOMPILE`(2).
- Thay đổi khung thời gian - TF: Lập tức DeInit và Init lại ngay lập tức với code `REASON_CHARTCHANGE`(3).
- Thay đổi cặp tiền - Symbol: Lập tức DeInit và Init lại ngay lập tức với code `REASON_CHARTCHANGE`(3).
- Thay đổi tham số đầu vào - Input parameters: Lập tức DeInit và Init để áp dụng thông số mới với code `REASON_PARAMETERS`(5).
- Khi switch account khác: Lập tức DeInint và Init với code `REASON_ACCOUNT`(6).
- Áp dụng Chart Template: Bot cũ bị gỡ (DeInit) và Init lại (nếu có trong tpl) theo template với code `REASON_TEMPLATE`(7).

2. Bot bị stop trong những tình huống sau:
- Khi gọi hàm ExpertRemove(): `REASON_PROGRAM` (0).
- Remove expert khỏi Chart: `REASON_REMOVE` (1).
- Đóng Chart: `REASON_CHARTCLOSE` (4).
- Khi OnInit không return `INIT_SUCCEEDED`: `REASON_INITFAILED` (8).
- Tắt máy hoặc terminal: `REASON_CLOSE` (9).

3. Cơ chế Restart diễn ra
- Gọi hàm OnDeinit(const int reason): Thường xoá Objects, Indicators, bộ nhớ đệm.
- Giải phòng tài nguyên: Xoá global variables trong code.
- Gọi hàm OnInit(): Bot bắt đầu chạy lại từ OnInit.
Với các trường hợp Restart thì mình có thể bắt được reason để xử lí tuỳ theo expect.

Lưu ý: Nút "Algo Trading" không restart bot. Nó chỉ chặn bot thực hiện lệnh (Trade Operations), các logic trong hàm OnTick vẫn liên tục chạy.

Discuss:
- Trường hợp Bot stop:
	- Opt1: Không xử lí gì thêm (Đề xuất) -> Giúp tránh đóng mất lệnh đang chạy khi có phát sinh khiến bot stop.
	- Opt2: Đóng hết lệnh.
	- Comment: Cái này còn phụ thuộc thuật toán, với các bot đặt SL, TP từ đầu cho Order thì theo Opt1 luôn là ok. Với các bot liên tục theo dõi quản lí lệnh thì Opt1 có thể khiến các position trôi nổi không được quản lí.
- Trường hợp Bot restart: Phụ thuộc vào restart reason, thường thì theo quy trình deploy của mình thì có thể sẽ gặp trường hợp (5) và (7).
	- Restart bot với tham số cũ:
		- Opt1: Tiếp tục quản lý Order/Position cũ -> Tuỳ theo logic của từng EA để xử lí kiểm tra trạng thái và khôi phục khác nhau.
		- Opt2: Đóng hết lệnh cũ.
	- Restart bot với tham số mới
		- Thay đổi Magic number => Đề xuất: Không cho phép.
		- Thay đổi các tham số khác => Đề xuất: Yêu cầu đóng hết lệnh cũ.
- Trường hợp start Bot mà Acc đang có position với magic number trùng với MN của bot
	- Hỏi user có muốn đóng lệnh cũ trước khi cài đặt bot không (Đề xuất)
	- EA tự xử lí để đóng position - tuỳ thuật toán => Khó scale vì phải sửa từng thuật toán.

FAQ
1. Có muốn đóng hết position trong các trường hợp bot bị stop không?
2. Restart -> Code = 7
3. Trường hợp: Khi khởi tạo bot, có position cùng magic number nhưng không nằm trong điều kiện đóng lệnh. => Đề xuất: Hỏi user có muốn đóng lệnh không.
4. Testing Code   
   ```
input int fake_parameter=3;      // useless parameter  
//+------------------------------------------------------------------+  
//| Expert initialization function                                   |  
//+------------------------------------------------------------------+  
int OnInit()  
  {  
//--- Get the number of a build where the program is compiled  
   Print(__FUNCTION__," Build #",__MQLBUILD__);  
//--- Reset reason code can also be obtained in OnInit()  
   Print(__FUNCTION__," Deinitialization reason code can be received during the EA reset");  
//--- The first way to get a deinitialization reason code  
   Print(__FUNCTION__," _UninitReason = ",getUninitReasonText(_UninitReason));  
//--- The second way to get a deinitialization reason code    
   Print(__FUNCTION__," UninitializeReason() = ",getUninitReasonText(UninitializeReason()));  
//---  
   return(INIT_SUCCEEDED);  
  }  
//+------------------------------------------------------------------+  
//| Expert deinitialization function                                 |  
//+------------------------------------------------------------------+  
void OnDeinit(const int reason)  
  {  
//--- The first way to get a deinitialization reason code  
   Print(__FUNCTION__," Deinitialization reason code = ",reason);  
//--- The second way to get a deinitialization reason code  
   Print(__FUNCTION__," _UninitReason = ",getUninitReasonText(_UninitReason));  
//--- The third way to get a deinitialization reason code    
   Print(__FUNCTION__," UninitializeReason() = ",getUninitReasonText(UninitializeReason()));  
  }  
//+------------------------------------------------------------------+  
//| Return a textual description of the deinitialization reason code |  
//+------------------------------------------------------------------+  
string getUninitReasonText(int reasonCode)  
  {  
   string text="";  
//---  
   switch(reasonCode)  
     {  
      case REASON_ACCOUNT:  
         text="Account was changed";break;  
      case REASON_CHARTCHANGE:  
         text="Symbol or timeframe was changed";break;  
      case REASON_CHARTCLOSE:  
         text="Chart was closed";break;  
      case REASON_PARAMETERS:  
         text="Input-parameter was changed";break;  
      case REASON_RECOMPILE:  
         text="Program "+__FILE__+" was recompiled";break;  
      case REASON_REMOVE:  
         text="Program "+__FILE__+" was removed from chart";break;  
      case REASON_TEMPLATE:  
         text="New template was applied to chart";break;  
      default:text="Another reason";  
     }  
//---  
   return text;  
  }
   ```