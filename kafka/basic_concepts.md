# Apache Kafka — Topic, Partition, Offset & Broker  

## 1. Tổng quan về Kafka  
Kafka là một *event streaming platform* — không chỉ đơn thuần là message broker. Một số đặc tính nổi bật của Kafka:  
- **Scalable**: có thể mở rộng dễ dàng (thêm broker, thêm partition) mà không downtime. :contentReference[oaicite:1]{index=1}  
- **Durable**: message được lưu trên đĩa (disk), bảo toàn dữ liệu ngay cả khi server gặp sự cố. :contentReference[oaicite:2]{index=2}  
- **Reliable**: hỗ trợ replication — nhiều bản sao dữ liệu trên các broker khác nhau, giúp hệ thống chịu lỗi tốt. :contentReference[oaicite:3]{index=3}  
- **High performance**: throughput cao, phù hợp xử lý lượng lớn dữ liệu. :contentReference[oaicite:4]{index=4}  

Kafka dùng để xử lý luồng dữ liệu (streaming data) rất hiệu quả — phù hợp với các hệ thống real-time, xử lý log, event, hay streaming dữ liệu lớn. :contentReference[oaicite:5]{index=5}  

---

## 2. Topic, Partition và Offset  

### 2.1 Topic  
- **Topic** là một “luồng dữ liệu” — nơi các messages được publish (từ Producer) và consumer có thể subscribe để đọc. :contentReference[oaicite:6]{index=6}  
- Có thể tưởng tượng Topic giống như một bảng (table) trong cơ sở dữ liệu:  
  - `topic name` ~ tên table.  
  - `message` ~ một bản ghi (row). :contentReference[oaicite:7]{index=7}  

Topic được lưu trữ dưới dạng log file, chia thành các segment. :contentReference[oaicite:8]{index=8}  

### 2.2 Partition & Offset  
- Một topic có thể được chia thành một hoặc nhiều **partition**. Khi tạo topic, ta xác định số lượng partition. :contentReference[oaicite:9]{index=9}  
- Mỗi partition là một log liên tục, chứa các messages theo thứ tự append (thứ tự từ cũ → mới).  
- Mỗi message trong partition được gán một chỉ số thứ tự gọi là **offset**. Offset bắt đầu từ 0 và tăng dần. :contentReference[oaicite:10]{index=10}  
- Vì thế, để định danh chính xác một message trong Kafka, cần 3 thông tin:  
  1. `topic name`  
  2. `partition number`  
  3. `offset` trong partition :contentReference[oaicite:11]{index=11}  

#### Lưu ý quan trọng  
- Hai message ở cùng offset **nhưng** ở **partition khác nhau** là **hai message khác nhau**. Offset chỉ có ý nghĩa trong phạm vi partition. :contentReference[oaicite:12]{index=12}  
- Giá trị offset có thứ tự — offset tăng dần khi message mới đến — **nhưng chỉ đảm bảo thứ tự trong cùng partition**. Giữa các partition khác nhau, không có thứ tự toàn cục. :contentReference[oaicite:13]{index=13}  
- Message sau khi ghi vào partition là **immutable** (bất biến). Không thể sửa đổi, di chuyển hoặc thay đổi offset. :contentReference[oaicite:14]{index=14}  
- Mặc định, Kafka giữ message trong một khoảng thời gian (ví dụ 7 ngày — tuy có thể cấu hình). Sau thời gian đó dữ liệu có thể bị xóa, nhưng offset **không reset**; offset luôn tăng. :contentReference[oaicite:15]{index=15}  

### 2.3 Ví dụ minh họa  
Ví dụ: bạn có một hệ thống tracking vị trí tài xế, mỗi 20s mỗi tài xế gửi vị trí → bạn có thể tạo một `topic = driver_gps`. Tất cả các vị trí được publish vào topic này. Mỗi message gồm `{ driver_id, position }`. :contentReference[oaicite:16]{index=16}  

Sau đó nhiều consumer có thể đọc topic `driver_gps`:  
- Consumer để hiển thị vị trí thời gian thực (map)  
- Consumer để thông báo driver đi/đến  
- Consumer để phân tích dữ liệu (lịch sử, hiệu suất, v.v.) :contentReference[oaicite:17]{index=17}  

Như vậy, Kafka cho phép nhiều luồng xử lý dữ liệu khác nhau từ cùng một nguồn dữ liệu (topic), đảm bảo tính bất biến, độ bền và khả năng mở rộng.  

---

## 3. Broker (Kafka Broker và Cluster)  

### 3.1 Kafka Broker là gì?  
- **Broker** = một server Kafka, chịu trách nhiệm lưu trữ dữ liệu — log file — của các partition. :contentReference[oaicite:18]{index=18}  
- Một broker có thể chứa nhiều partition từ nhiều topic khác nhau. Ngược lại, một partition cũng có thể nằm trên bất kì broker nào trong cluster. :contentReference[oaicite:19]{index=19}  

### 3.2 Cluster & Replication  
- Vì nếu chỉ có 1 broker — nếu server đó chết → dữ liệu và service mất → không ổn. Do đó Kafka thường được triển khai ở dạng cluster (nhiều broker). Trong cluster, mỗi broker là một node. :contentReference[oaicite:20]{index=20}  
- Khi phân phối partition giữa các broker, Kafka cố gắng “phân tán đều” — không đưa tất cả partition vào cùng broker nếu có thể. :contentReference[oaicite:21]{index=21}  
- Để đảm bảo độ bền và chịu lỗi, Kafka hỗ trợ **replication**: mỗi partition có thể có nhiều bản sao (replica) nằm trên các broker khác nhau. Nếu broker chứa partition chính (leader) chết, một replica sẽ trở thành leader thay thế — đảm bảo không mất dữ liệu và hệ thống vẫn hoạt động. :contentReference[oaicite:22]{index=22}  

### 3.3 Nhiệm vụ của Broker  
Mỗi broker trong cluster có 3 nhiệm vụ chính:  
1. Nhận message từ producer và ack (xác nhận). :contentReference[oaicite:23]{index=23}  
2. Ghi message vào log file (ở partition tương ứng). :contentReference[oaicite:24]{index=24}  
3. Cung cấp message cho consumer khi được yêu cầu. :contentReference[oaicite:25]{index=25}  

### 3.4 Ưu điểm khi sử dụng Broker/Cluster  
- Hệ thống phân tán — có thể mở rộng dễ dàng chỉ bằng cách thêm broker/partition. :contentReference[oaicite:26]{index=26}  
- Khả năng chịu lỗi cao nhờ replication — nếu một broker hỏng vẫn có thể tiếp tục hoạt động. :contentReference[oaicite:27]{index=27}  
- Dữ liệu được ghi xuống đĩa, bảo đảm durability. :contentReference[oaicite:28]{index=28}  
- Cho phép nhiều producer và nhiều consumer xử lý song song mà vẫn giữ thứ tự trong mỗi partition. :contentReference[oaicite:29]{index=29}  

---

## 4. Tổng kết  

- Một message trong Kafka được định danh bởi: **Topic name + Partition + Offset**. :contentReference[oaicite:30]{index=30}  
- Partition giúp Kafka phân tán dữ liệu, tăng khả năng xử lý song song và mở rộng. Nhưng thứ tự chỉ được bảo đảm **trong cùng partition**, không phải toàn topic. :contentReference[oaicite:31]{index=31}  
- Broker là đơn vị server lưu trữ dữ liệu; Cluster + Replication giúp Kafka trở nên đáng tin cậy, bền vững và scalable. :contentReference[oaicite:32]{index=32}  
- Một khi message được ghi → bất biến (immutable), offset tăng dần, dữ liệu có thể được lưu lại trong thời gian cấu hình → phù hợp cho streaming, log, event, xử lý dữ liệu lớn, real-time. :contentReference[oaicite:33]{index=33}  

---

Nếu bạn muốn — mình có thể bổ sung thêm **sơ đồ minh họa** (ASCI/diagram) cho kiến trúc Kafka tương ứng với các khái niệm trên (topic/partition/broker/cluster) — rất hữu ích nếu bạn định lưu lại làm notes. Muốn mình tạo luôn cho bạn không?  
