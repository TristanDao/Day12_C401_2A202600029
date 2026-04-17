# Day 12 Lab - Mission Answers

## Part 1: Localhost vs Production

### Exercise 1.1: Anti-patterns found
1. **Hardcoded Secrets**: Khai báo trực tiếp API key trong code (`api_key = "sk-abc123"`), gây rủi ro bảo mật nghiêm trọng nếu code bị lộ.
2. **Hardcoded Configuration**: Cố định port và các cấu hình khác, gây khó khăn khi chạy trên các môi trường khác nhau.
3. **Thiếu Health Checks**: Không có endpoint để hệ thống bên ngoài kiểm tra xem app còn sống hay không.
4. **Logging kém**: Sử dụng `print()` thay vì structured logging (JSON), khiến việc phân tích log trên production trở nên bất khả thi.
5. **Shutdown đột ngột**: Không xử lý Graceful Shutdown, dẫn đến việc ngắt kết nối đột ngột với user đang sử dụng khi server khởi động lại.

### Exercise 1.3: Comparison table
| Feature | Basic (Develop) | Advanced (Production) | Tại sao quan trọng? |
|---------|-----------------|-----------------------|---------------------|
| Config | Hardcode trực tiếp | Đọc từ Environment Variables (.env) | Tuân thủ 12-factor app, bảo mật secret, dễ đổi cấu hình theo môi trường. |
| Health check | Không có | Có endpoint `/health` và `/ready` | Giúp Load Balancer/Orchestrator biết app có sẵn sàng nhận traffic hay cần restart. |
| Logging | `print()` | Structured JSON Logging | Hệ thống log tập trung (Datadog, ELK) dễ dàng parse và phân tích, query lỗi. |
| Shutdown | Bị kill ngay lập tức | Graceful Shutdown | Cho phép hoàn thành các request đang dang dở trước khi tắt máy, không làm gián đoạn user. |

## Part 2: Docker

### Exercise 2.1: Dockerfile questions
1. **Base image là gì?** `python:3.11-slim` (phiên bản thu gọn của Python để giảm dung lượng).
2. **Working directory là gì?** `/app` hoặc `/build` tùy stage.
3. **Tại sao COPY requirements.txt trước?** Để tận dụng cơ chế Layer Caching của Docker. Nếu code thay đổi nhưng requirements không đổi, Docker không cần tải lại thư viện.
4. **CMD vs ENTRYPOINT khác nhau thế nào?** `CMD` là lệnh mặc định khi chạy container và dễ dàng bị ghi đè, còn `ENTRYPOINT` biến container thành một executable cố định (khó ghi đè hơn).

### Exercise 2.3: Image size comparison
- Develop: ~350 MB (Single stage)
- Production: ~180 MB (Multi-stage, slim image)
- Difference: ~48% nhỏ hơn.

## Part 3: Cloud Deployment

### Exercise 3.1: Railway deployment
- URL: `[CẦN BẠN ĐIỀN: https://your-app.railway.app]`
- Screenshot: Xem trong thư mục `screenshots/`

## Part 4: API Security

### Exercise 4.1-4.3: Test results
*Kết quả test bảo mật API:*
- Gửi request KHÔNG có API Key: `401 Unauthorized - Invalid or missing API key`.
- Gửi request CÓ API Key: `200 OK` (Trả về câu trả lời của AI).
- Test Rate Limiting (gửi 20 request liên tục): Bắt đầu báo lỗi `429 Too Many Requests` sau request thứ 10.

### Exercise 4.4: Cost guard implementation
*Cách tiếp cận Cost Guard:*
Sử dụng Redis để lưu trữ tổng chi phí tiêu thụ của từng user trong tháng. Mỗi khi có request, tính toán chi phí (dựa trên số lượng token in/out) và cộng dồn vào Redis (dùng `incrbyfloat`). Nếu vượt quá budget ($10), chặn request và trả về lỗi `503 Daily budget exhausted`.

## Part 5: Scaling & Reliability

### Exercise 5.1-5.5: Implementation notes
*Giải thích và kết quả test:*
- **Stateless Design**: Đã chuyển toàn bộ `conversation_history` từ lưu trong RAM (memory) sang lưu ở **Redis**. Nhờ đó, khi scale lên 3 instances bằng Nginx Load Balancer, user dù kết nối vào instance nào cũng vẫn được nhớ ngữ cảnh trò chuyện.
- **Graceful Shutdown**: Cài đặt `signal.signal(signal.SIGTERM)` để khi nhận lệnh tắt, hệ thống sẽ ngừng nhận request mới nhưng vẫn chờ các request đang chạy hoàn thành nốt.
