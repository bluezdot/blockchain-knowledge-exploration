1. [ ]  Idempotent methods: GET, HEAD, PUT, DELETE, OPTIONS and TRACE. “the side-effects of N > 0 identical requests is the same as for a single request”. Nghĩa là không trigger thay đổi resource khi sử dụng các method trên. Ví dụ: GET /customers/1 luôn là thông tin customer/404 cho N request
2. [ ]  Authentication
3. [ ]  201 Created : status được response khi 1 request tạo ra 1 resource mới thành công
4. [ ]  202 Status : status được response khi 1 request hợp lệ và được thực thi, nhưng chưa hoàn thành ngay. Ví dụ: send email
5. [ ]  4xx Status: Client Error (something wrong)
6. [ ]  5xx Status: Server Side Error (something broken)
7. [ ]  410 Gone : resource đã tồn tại ở URL này nhưng hiện tại ko còn. Ví dụ: resource bị deleted, archived, hoặc expired time(download link)
8. [ ]  Expect: 100-continue. Ví dụ: phía app chuẩn bị upload 1 file có kích cỡ lớn, có thể sử dụng meta này + độ lớn của file + loại file để server kiểm tra trước xem liệu loại file/độ lớn có hợp lệ không thay vì gửi lên toàn bộ nội dung file rồi mới kiểm tra.
9. [ ]  Connection Keep-Alive: Cấu hình để thông số này là hợp lí nhất, vì nếu maintain nhiều connection, sẽ dẫn đến vấn đề lớn về mặt performance
10. [ ]  HTTP Compression : Để tăng network performance, chúng ta có thể nén body ở cả phía client lẫn server. response bodies (Accept-Encoding: gzip) and for request bodies (Content-Encoding: gzip)
11. [ ]  HTTP Caching: Cache-Control header
12. [ ]  Cache Validation: Last-Modified or ETag headers của response
13. [ ]  Conditional Modifications
14. [ ]  Absolute Redirects: Đối với các 201, 301, 302, 303, 307 response codes, trong header nên chứa Location để hỗ trợ redirect
15. [ ]  Canonical URLs: đối với các resource có nhiều URL, sử dụng 1 phương thức cố định để chỉ định đau là URL gốc. Ví dụ: link sản phẩm: products/1 và products/web-chuan-seo-mau-001
16. [ ]  Chunked Transfer Encoding: sử dụng Transfer-Encoding: Chunked, nếu content có kích thước lớn để stream dữ liệu cho client
17. [ ]  Error Handling in Chunked Transfer Encoding : suy nghĩ về việc handle lỗi khi stream data, vì khi bắt đầu stream response, chúng ta ko thể thay đổi http status code. Tuy nhiên có thể sử dụng Content-Type để làm điều này.
18. [ ]  X-HTTP-Method-Override: Một số http clients ko hỗ trợ gì khác ngoài POST/GET, có thể dùng cách này để bypass
19. [ ]  URL Length : Nếu API support filter, lưu ý đảm bảo URL ko vượt quá 2000 kí tự
20. [ ]  Statelessness
21. [ ]  Content Negotiation : hỗ trợ response ở nhiều format thông qua Accept-Headers hoặc URLs khác nhau
22. [ ]  URI Template: tạo ra template mẫu cho URI, đảm bảo tính nhất quán trong ứng dụng. [Tham khảo tại](https://www.rfc-editor.org/rfc/rfc6570)
23. [ ]  Design for Intent: Đừng chỉ forward toàn bộ business object đang lưu trữ phía server về client side. Luôn nhớ rằng cần design các API theo ý nghĩa của nó và thỏa mãn use-cases trong thực tế.
24. [ ]  Versioning : Ví dụ: /v1 tương ứng với phiên bản 1 của API
25. [ ]  Authorization : Cần đảm bảo API luôn kiểm tra quyền hạn của user khi truy cập vào các tài nguyên
26. [ ]  Bulk Operations: Đôi lúc thay vì gửi quá nhiều request thì gửi 1 bulk operations sẽ giảm tải rất nhiều thời gian chờ đợi cho phía client.
27. [ ]  Pagination : Đảm bảo API của bạn hỗ trợ pagination 1 cách nhất quán, tránh việc hiển thị quá nhiều thông tin dư thừa trên client hoặc dữ liệu quá nhiều ko thể tải hết.
28. [ ]  Unicode : luôn lưu ý, cũng cần hỗ trợ cả unicode. Ví dụ: customers/search/Sơn
29. [ ]  Error Logging: luôn chắc rằng bạn cần áp dụng log các lỗi cần thiết. Đặc biệt nên phân biệt lỗi client và server. Có thể chia làm 2 file log riêng.
30. [ ]  Content Types : support các dạng content type phổ biến
31. [ ]  HATEOAS : liên kết tới các resource liên quan
32. [ ]  Date/time: lưu ý cần cung cấp cả thông tin timezone đối với dữ liệu dạng date/time
33. [ ]  SSL : nên xài SSL cho cả client lẫn server
34. [ ]  Cross-site Request Forgery (CSRF)
35. [ ]  Throttling: Đôi khi client side request quá nhiều vào 1 endpoint trong 1 thời điểm. Ví dụ: gửi request search liên tục khi người dùng gõ. Áp dụng API request limits - 503 cùng với Retry-After header để đảm bảo API vẫn hoạt động tốt.
36. [ ]  Subtle Denial of Service: Kĩ thuật Throttling có thể chống quá tải cơ bản, tuy nhiên khi gặp các vấn đề liên quan đến Dos attack, cần đảm bảo hãy sử dụng các công cụ tương ứng.
37. [ ]  Connection Keep-Alive
38. [ ]  401 before Authorization: sử dụng Authorization khi cần thiết
39. [ ]  Documentation : làm ơn hãy ghi tài liệu hướng dẫn sử dụng API. Tiêu biểu như : swagger
40. [ ]  Design with a Customer!: kiếm người xài nha bạn
41. [ ]  Feedback : bình tĩnh nhận feedback
42. [ ]  Automated Testing: viết khi có thể, nó sẽ giúp bạn tiết kiệm rất nhiều thời gian

Source: https://nextjsvietnam.com/post/rest-api-design-checklist/