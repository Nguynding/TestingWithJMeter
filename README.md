# Kiểm thử hiệu suất hệ thống với Apache JMeter

## 1. Cài đặt công cụ
Tải và cài đặt Apache JMeter từ trang chủ: [JMeter Official Site](https://jmeter.apache.org/)

## 2. Thiết lập kịch bản kiểm thử hiệu suất
- Trang web kiểm thử: **[saucedemo](https://www.saucedemo.com/)**
- Mô phỏng **50 người dùng đồng thời** truy cập trang web trong **5 phút**.
- Gửi **yêu cầu HTTP** đến trang chủ của website.
- Đo thời gian phản hồi và xác định bottleneck nếu có.

### **Các bước thiết lập trong JMeter**
1. **Tạo Test Plan** trong JMeter.
2. **Thêm Thread Group** và cấu hình:
   - Number of Threads (Users): **50**
   - Ramp-up Period: **30** giây
   - Loop Count:  Đặt thời gian **10 phút**
3. **Thêm HTTP Request**:
   - Server Name: `saucedemo.com`
   - Method: `GET`
   - Path: `/`
4. **Thêm Listener**:
   - View Results Tree
   - Summary Report
   - Aggregate Report
   - Graph Results
     
## 3. Thực hiện kiểm thử tải (Load Testing)
- Chạy kiểm thử với số lượng người dùng tăng dần từ **10 → 50**.
- Ghi nhận **thời gian phản hồi trung bình** và **tỷ lệ lỗi (Error Rate)**.

## 4. Kiểm thử khả năng chịu tải (Stress Testing)
- Tiếp tục tăng số người dùng lên **100** để xác định giới hạn chịu tải của hệ thống.
- Quan sát thời điểm hệ thống bắt đầu **giảm hiệu suất đáng kể**.

## 5. Phân tích kết quả
### **Kết quả kiểm thử hiệu suất**
#### **Số lượng mẫu (Samples)**
- HTTP Request: **500** mẫu  
- Tổng cộng: **500** mẫu  
→ Hệ thống đã xử lý gần **500 yêu cầu**, cho thấy mức độ tải thử nghiệm  ở quy mô nhỏ.

#### **Thời gian phản hồi (Response Time - Average, Min, Max)**
- **Trung bình (Average)**: ~208ms  
- **Nhỏ nhất (Min)**: 148ms  
- **Lớn nhất (Max)**: 1,572ms  
→ Hệ thống có thời gian phản hồi trung bình khoảng **208ms**, khá ổn định. Tuy nhiên, thời gian phản hồi cao nhất lên đến **1,572ms**, điều này có thể cho thấy hệ thống có một số điểm nghẽn hoặc bị quá tải tại một số thời điểm.

#### **Độ lệch chuẩn (Standard Deviation - Std. Dev.)**
- **Khoảng 109.04ms**  
→ Độ lệch chuẩn cho thấy có sự biến động đáng kể trong thời gian phản hồi giữa các yêu cầu.

#### **Tỷ lệ lỗi (Error Rate)**
- Mức lỗi dao động từ **0.000%**
→ Không phát hiện lỗi trong quá trình kiểm tra, cho thấy hệ thống xử lý yêu cầu tốt.
#### **Thông lượng (Throughput)**
- **15.9 requests/sec** (trung bình tổng)
→ Hệ thống có thể xử lý khoảng 16 yêu cầu mỗi giây, một con số phù hợp tùy thuộc vào mục tiêu hiệu suất.

#### **Dữ liệu truyền tải**
- **Received KB/sec**: 61.75 KB/s  
- **Sent KB/sec**: 3.65 KB/s  
→ Hệ thống có thể tiếp nhận **~61.75 KB/s** dữ liệu từ server và gửi **~3.65 KB/s** dữ liệu, phản ánh lưu lượng mạng của hệ thống.

### **Kết luận về hiệu suất hệ thống**
- **Hệ thống hoạt động ổn định** với mức tải nhẹ (500 yêu cầu).
- **Thời gian phản hồi trung bình tốt** (~208ms), tuy nhiên độ lệch chuẩn khá cao, cho thấy có một số biến động.
- **Tỷ lệ lỗi  bằng 0%**, cho thấy hệ thống xử lý yêu cầu ổn định.
- **Thông lượng đạt ~16 request/sec**, phản ánh khả năng xử lý tải mức trung bình.
- **Thời gian phản hồi tối đa khá cao (1,572ms)**, có thể cần tối ưu thêm để giảm độ trễ cao nhất.


## 6. Câu hỏi thảo luận
1. **Tại sao kiểm thử phi chức năng lại quan trọng trong phần mềm?**  
   - Kiểm thử phi chức năng giúp đảm bảo hệ thống có thể hoạt động ổn định, đáp ứng yêu cầu về hiệu suất, bảo mật và khả năng mở rộng.
2. **Các thông số quan trọng cần theo dõi trong kiểm thử hiệu suất là gì?**  
   - Thời gian phản hồi (Response Time), Thông lượng (Throughput), Tỷ lệ lỗi (Error Rate), Độ lệch chuẩn (Standard Deviation).
3. **Nếu hệ thống không đáp ứng yêu cầu hiệu suất, bạn sẽ đề xuất giải pháp gì?**  
   - Cải thiện phần cứng (CPU, RAM, Network).
   - Tối ưu hóa truy vấn Database.
   - Sử dụng Load Balancer hoặc Scale hệ thống theo chiều ngang.
   - Triển khai caching để giảm tải cho server.
