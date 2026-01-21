### 1. Tổng quan về Hyperbridge

Hyperbridge là một **interoperability coprocessor** (bộ vi xử lý đồng hành về khả năng tương tác) được xây dựng dưới dạng một Parachain trên Polkadot. Thay vì dựa vào các nhóm đa chữ ký (multisig) hay các bên trung gian tập trung, Hyperbridge sử dụng bằng chứng mật mã (cryptographic proofs) để xác thực trạng thái giữa các chuỗi.

- **Mô hình bảo mật:** Thừa hưởng bảo mật kinh tế (crypto-economic security) từ Polkadot.
    
- **Cơ chế cốt lõi:** Sử dụng giao thức **ISMP (Interoperable State Machine Protocol)** – một giao thức nhắn tin tương tự HTTP nhưng dành cho blockchain, cho phép gửi các yêu cầu `POST` (gửi dữ liệu) và `GET` (đọc dữ liệu) xuyên chuỗi một cách phi tập trung.
    
- **Điểm khác biệt:** Hyperbridge đóng vai trò như một "máy xác thực" tập trung các phép tính toán phức tạp (như kiểm tra bằng chứng đồng thuận ZK-proofs) ra khỏi chuỗi đích để tiết kiệm chi phí gas và tăng tốc độ.
    

### 2. Kiến trúc và Thành phần chính (Github & Docs)

Khi tích hợp, bạn cần chú ý đến các thành phần trong Repository của Polytope Labs:

- **Hyperbridge Parachain:** Đóng vai trò là hub trung tâm để xác thực các bằng chứng trạng thái từ các chuỗi kết nối (như Ethereum, Optimism, Base, BSC...).
    
- **Relayers:** Hệ thống chuyển tiếp tin nhắn không cần cấp phép (permissionless). Bất kỳ ai cũng có thể chạy relayer để chuyển tin nhắn mà không cần staking hay vào whitelist.
    
- **Gateway Contracts:** Các hợp đồng thông minh được triển khai trên EVM-based chains để xử lý logic Bridge:
    
    - **TokenGateway:** Hỗ trợ cơ chế _burn-and-mint_ (cho các token hyper-fungible) hoặc _lock-and-mint_ (cho ERC-20 thông thường).
        
    - **Handler:** Xử lý việc nhận và thực thi các thông điệp ISMP từ Hyperbridge.
        

### 3. Tích hợp SDK (`@hyperbridge/sdk`)

SDK này được thiết kế để đơn giản hóa việc tương tác giữa Polkadot và EVM. Các tính năng chính bao gồm:

- **Hỗ trợ đa chuỗi:** Cung cấp các class `EvmChain` và `SubstrateChain` để khởi tạo kết nối thông qua RPC/WS.
    
- **Ước tính phí (Fee Estimation):** Phương thức `quoteNative` trong `TokenGateway` cho phép tính toán phí relayer và phí giao thức trước khi thực hiện Bridge.
    
- **Quản lý tài sản:**
    
    - `getErc20Address(assetId)`: Lấy địa chỉ hợp đồng ERC-20 của tài sản trên chuỗi đích.
        
    - `getErc6160Address(assetId)`: Hỗ trợ chuẩn ERC-6160 (Hyper-fungible tokens) cho phép chuyển tài sản nguyên bản (native-like) giữa các chuỗi mà không cần các pool thanh khoản tập trung dễ bị hack.
        
- **Indexer Client:** Đi kèm với `@hyperbridge/indexer` (dựa trên SubQuery) giúp theo dõi trạng thái của các tin nhắn xuyên chuỗi theo thời gian thực.
    

### 4. Quy trình Bridge tài sản (Flow)

Để tích hợp thành công giữa Polkadot và EVM, quy trình thường diễn ra như sau:

1. **Khởi tạo:** Sử dụng SDK để kết nối với ví của người dùng và các node RPC của cả hai chuỗi.
    
2. **Yêu cầu chuyển tài sản:** Gọi hàm từ `TokenGateway` (trên EVM) hoặc pallet tương ứng (trên Polkadot).
    
3. **Xác thực:** Hyperbridge sẽ nhận bằng chứng về giao dịch này, xác thực tính đúng đắn dựa trên bằng chứng đồng thuận (như BEEFY cho Polkadot hoặc Sync Committee cho Ethereum).
    
4. **Thực thi:** Sau khi xác thực thành công, tin nhắn sẽ được gửi đến chuỗi đích để giải phóng/đúc tài sản tương ứng.
    

### 5. Tài liệu tham khảo quan trọng cho lập trình viên

- **Đọc dữ liệu xuyên chuỗi:** Hyperbridge cho phép thực hiện "Cross-chain GET requests", nghĩa là một hợp đồng trên EVM có thể đọc trạng thái lưu trữ của một Parachain trên Polkadot một cách có thể xác thực.
    
- **Môi trường phát triển:** SDK yêu cầu các tệp WebAssembly (WASM) để hoạt động, bạn nên sử dụng các plugin hỗ trợ (như Vite plugin đi kèm trong SDK) để đảm bảo các tệp này được tải đúng cách.