# Load Balancer: Overview and Best Practices

Load Balancer giống như **quản lý siêu thị giờ cao điểm** — điều hướng khách (user request) đến nhiều cashier (server) để tránh xếp hàng dài và đảm bảo dịch vụ trơn tru. Tương tự, load balancer phân phối **internet traffic đều trên nhiều server**.

---

## Các tầng hoạt động của Load Balancer

### Layer 4 (L4)
- Hoạt động dựa trên **IP addresses và ports**, giống shipper giao hàng dựa trên địa chỉ mà không kiểm tra nội dung.
- **Ưu điểm:** tốc độ cao
- **Nhược điểm:** limited intelligence

### Layer 7 (L7)
- Thông minh hơn, inspect **nội dung data packets**.
- Ví dụ: phát hiện request video streaming và điều hướng đến **streaming server chuyên dụng** để xử lý tốt hơn.

---

## Thuật toán phân phối traffic

| Thuật toán | Mô tả | Ưu điểm |
|------------|-------|---------|
| **Round Robin** | Gửi request tuần tự đến từng server theo thứ tự, lặp lại đều | Đơn giản, công bằng, hiệu quả trong nhiều trường hợp |
| **Least Connections** | Gửi request mới tới server có ít kết nối đang hoạt động nhất | Tối ưu performance thời gian thực dựa trên load hiện tại |
| **Weighted Distribution** | Gán weight cho server dựa trên khả năng | Linh hoạt, server mạnh hơn nhận nhiều traffic hơn |

---

## Cơ chế Health Check
- Load balancer liên tục **giám sát server health**.
- Nếu server fail hoặc không phản hồi → loại server đó khỏi rotation.
- Khi server hồi phục → reintegrate.
- **Lợi ích:** tăng **fault tolerance** và **stability** của hệ thống.

---

## Ví dụ định lượng: Tác động đến khả năng hệ thống

| Scenario | Số server | Maximum Requests per Second (RPS) | Ghi chú |
|----------|------------|----------------------------------|---------|
| Single server without load balancer | 1 | 200 | System bottleneck xảy ra |
| Three servers with load balancer | 3 | Thousands | Tăng khả năng xử lý đáng kể nhờ phân phối traffic |

> Chỉ cần thêm vài server phía sau load balancer có thể nhân khả năng xử lý request của hệ thống, là bước crucial đầu tiên cho **horizontal scaling**.

---

## Bốn bài học chính về Load Balancer

1. **Phân phối traffic** để tránh server overload và system crashes.
2. Hoạt động ở hai tầng chính: **L4** cho tốc độ, **L7** cho routing thông minh dựa trên nội dung dữ liệu.
3. Sử dụng thuật toán đa dạng (**Round Robin, Least Connections, Weighted**) để phân bổ workload hiệu quả.
4. **Health check liên tục** đảm bảo traffic chỉ đi tới server khỏe mạnh, tăng fault tolerance.
