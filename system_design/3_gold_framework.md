# System Design Interviews: Five-Step Framework

Video này đề cập đến một thách thức phổ biến mà nhiều kỹ sư gặp phải trong **system design interviews**: ban đầu cảm thấy tự tin nhưng nhanh chóng bị bế tắc, không biết bắt đầu từ đâu.  

**Thống kê quan trọng:** khoảng **60% ứng viên trượt phỏng vấn thiết kế hệ thống**, không phải do thiếu kiến thức kỹ thuật, mà vì **thiếu một phương pháp hoặc framework có cấu trúc**.

Diễn giả giới thiệu một **framework năm bước** giúp ứng viên tiếp cận vấn đề một cách hệ thống, áp dụng không chỉ trong phỏng vấn mà còn trong thực tế tại các công ty công nghệ hàng đầu.

---

## Five-Step Framework cho System Design Interviews

| Step | Mô tả | Trọng tâm chính |
|------|-------|----------------|
| 1 | **Clarify Requirements** | Hiểu chính xác hệ thống phải làm gì, phân biệt giữa functional và non-functional requirements |
| 2 | **Estimate Scale** | Thực hiện các phép tính nhanh ("back of the envelope") để hiểu quy mô và tải hệ thống |
| 3 | **High-Level Design** | Phác thảo kiến trúc tổng thể với các thành phần chính mà không đi vào chi tiết |
| 4 | **Deep Dive into Key Components** | Phân tích 2–4 thành phần phức tạp hoặc quan trọng để thể hiện chiều sâu kỹ thuật |
| 5 | **Analyze Trade-offs** | Thảo luận ưu nhược điểm của các lựa chọn thiết kế, thể hiện khả năng biện minh dựa trên yêu cầu |

---

## Những hiểu biết và chi tiết quan trọng

### Common Pitfall
- Nhiều ứng viên vội vã vẽ **diagram kiến trúc** ngay sau khi nghe đề bài → đây là **sai lầm nghiêm trọng**, dẫn đến thất bại sớm.

### Step 1: Clarify Requirements
- Luôn bắt đầu bằng **hỏi câu hỏi** thay vì nhảy ngay vào giải pháp.
- Phân biệt:
  - **Functional requirements:** Hệ thống phải làm gì.
  - **Non-functional requirements:** Performance, availability, latency.
- **Ví dụ: URL shortener**
```text
Functional requirement:
  - Tạo link ngắn
  - Redirect về URL gốc

Non-functional requirement:
  - Latency < 100ms
  - Xử lý 1 tỷ request/ngày
  - High availability
```

### Step 2: Estimate Scale
- Chuyển nhanh các yêu cầu mơ hồ thành các con số cụ thể để **tránh thiết kế quá hoặc thiếu**.
- **Ví dụ URL shortener:**
```text
Write requests: 12 req/s
Read requests: 1,200 req/s
Read-to-write ratio: 100:1
```
- Tỉ lệ này ảnh hưởng trực tiếp đến kiến trúc, ví dụ: **caching bắt buộc để xử lý read traffic cao**.

### Step 3: High-Level Design
- Phác thảo kiến trúc tổng thể gồm các thành phần chính:
  - Clients
  - Load balancers
  - Web services
  - Databases
  - Caches
  - Object storage
- **Lưu ý:** tránh đi vào chi tiết implementation ở bước này.

### Step 4: Deep Dive into Key Components
- Chọn **2–4 thành phần phức tạp hoặc rủi ro nhất** để thảo luận chi tiết.
- **Ví dụ URL shortener:**
  - Sinh ID ngắn duy nhất ở quy mô lớn
  - Lựa chọn giữa SQL và NoSQL
  - Thiết kế caching layer đạt >99% cache hit ratio
  - Quyết định kỹ thuật phân tích, có thể dùng message queues

### Step 5: Analyze Trade-offs
- **System design là về trade-offs thông minh**, không phải tìm giải pháp hoàn hảo.
- **Ví dụ:**
  - SQL: consistency mạnh nhưng hạn chế scalability
  - NoSQL: scale tốt nhưng chỉ eventual consistency
- Lựa chọn tốt phụ thuộc vào yêu cầu ở **Step 1**
- **Khả năng biện minh và giải thích quyết định thiết kế** được đánh giá cao bởi interviewer.

---

## Điểm nổi bật khác
- Framework này bao phủ khoảng **70% vấn đề thường gặp** trong phỏng vấn và thực tế.
- Phương pháp này tương tự cách **senior engineers và architects** thiết kế hệ thống lớn tại:
  - Facebook
  - Google
  - Netflix
- Video hứa hẹn phần tiếp theo tập trung vào **Step 1**: nghệ thuật hỏi đúng câu hỏi để xác định rõ functional và non-functional requirements – nền tảng cho system design thành công.

---

## Khái niệm cốt lõi
- **Structured Framework:** Phương pháp tuần tự, có kỷ luật trong thiết kế hệ thống.
- **Requirements Clarification:** Bước quan trọng để tránh sai hướng.
- **Back-of-the-envelope Calculations:** Toán học đơn giản để ước lượng scale và load.
- **High-Level vs. Deep Dive:** Cân bằng giữa tổng quan và phân tích chi tiết.
- **Trade-offs:** Nhận ra không có thiết kế nào hoàn hảo; phù hợp phụ thuộc vào bối cảnh.
- **Communication:** Giải thích và bảo vệ lựa chọn quan trọng ngang với kiến thức kỹ thuật.

---

## Keywords
`System Design Interview`, `Framework`, `Requirements Clarification`, `Functional vs. Non-functional Requirements`, `Back-of-the-envelope Estimation`, `High-Level Architecture`, `Deep Dive Analysis`, `Trade-offs`, `SQL vs. NoSQL`, `Caching`, `Scalability`, `Consistency Models`

---

## Kết luận
- Thành công trong **system design interviews** đến từ việc **áp dụng framework rõ ràng, có cấu trúc**, thay vì chỉ dựa vào kiến thức kỹ thuật.
- **Five-step process** hướng dẫn ứng viên từ việc hiểu yêu cầu đến đưa ra trade-offs hợp lý, trang bị tư duy như các kỹ sư cao cấp toàn cầu.
- Thành thạo phương pháp này không chỉ cải thiện kết quả phỏng vấn mà còn nâng cao **kỹ năng thiết kế hệ thống thực tế**.
