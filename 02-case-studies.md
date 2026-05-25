# Claude — Case Studies

## Case Study 1: Hệ thống hỗ trợ khách hàng tự động

**Bối cảnh:**  
Công ty thương mại điện tử ~500 nhân viên, xử lý 3.000 ticket hỗ trợ/ngày. Team CS gồm 20 người thường xuyên quá tải, thời gian phản hồi trung bình 4 tiếng.

**Vấn đề:**  
80% câu hỏi lặp lại (trạng thái đơn hàng, chính sách đổi trả, hướng dẫn sử dụng). Nhân viên mất thời gian cho câu hỏi đơn giản thay vì xử lý trường hợp phức tạp.

**Giải pháp với Claude:**  
- Tích hợp Claude API với hệ thống ticket (Zendesk)
- System prompt định nghĩa vai trò CS, tone và chính sách công ty
- Tool use để tra cứu trạng thái đơn hàng, lịch sử mua hàng
- Claude tự động trả lời ticket đơn giản, escalate ticket phức tạp cho nhân viên
- RAG với knowledge base chính sách công ty

**Kết quả:**  
- 65% ticket được xử lý tự động không cần nhân viên
- Thời gian phản hồi giảm từ 4 tiếng xuống 2 phút
- CSAT tăng từ 3.2 lên 4.1/5
- Team CS tập trung vào 35% ticket phức tạp, chất lượng xử lý cao hơn

---

## Case Study 2: Trợ lý phân tích hợp đồng pháp lý

**Bối cảnh:**  
Công ty luật với 50 luật sư, xử lý hàng trăm hợp đồng mỗi tháng. Mỗi hợp đồng cần review để tìm điều khoản rủi ro, so sánh với template chuẩn.

**Vấn đề:**  
Review một hợp đồng phức tạp mất 2-4 giờ/luật sư. Bottleneck ở bước sơ bộ — tìm điều khoản bất thường trong tài liệu 50-100 trang.

**Giải pháp với Claude:**  
- Upload hợp đồng PDF, Claude đọc toàn bộ nhờ context window 200K token
- Prompt yêu cầu Claude extract: điều khoản phạt, điều kiện chấm dứt, nghĩa vụ bất thường
- So sánh với checklist điều khoản rủi ro được định nghĩa trong system prompt
- Output có cấu trúc: JSON với từng điều khoản, mức độ rủi ro (low/medium/high), giải thích

**Kết quả:**  
- Bước sơ bộ giảm từ 2 giờ xuống 15 phút
- Luật sư tập trung vào phân tích chuyên sâu thay vì đọc toàn văn
- Không bỏ sót điều khoản ẩn do mệt mỏi
- ROI dương sau 2 tháng triển khai

---

## Case Study 3: Code review và documentation tự động

**Bối cảnh:**  
Startup fintech 30 developer, release cycle 2 tuần. Code review là bottleneck — senior dev dành 30% thời gian review thay vì build feature.

**Vấn đề:**  
PR nhỏ chờ review 1-2 ngày. Nhiều comment lặp lại về style, naming, thiếu error handling. Senior dev kiệt sức với review routine.

**Giải pháp với Claude:**  
- Tích hợp Claude vào GitHub Actions, tự động trigger khi có PR
- Claude review theo checklist: security, performance, error handling, naming convention
- Comment trực tiếp vào từng dòng code trên GitHub qua API
- Generate draft documentation từ code mới
- Chỉ escalate cho senior khi Claude phát hiện vấn đề architecture

**Kết quả:**  
- 70% PR được review sơ bộ trong vòng 5 phút sau khi push
- Senior dev giảm 50% thời gian review routine
- Bug rate giảm 30% nhờ consistent checklist
- Documentation coverage tăng từ 40% lên 85%

---

## Bài học chung

- **System prompt rõ ràng là chìa khóa:** Đầu tư thời gian viết system prompt tốt mang lại ROI cao hơn nhiều so với fine-tuning
- **Tool use mở rộng khả năng đáng kể:** Claude không chỉ generate text mà có thể thực sự tương tác với hệ thống
- **Human-in-the-loop cho trường hợp edge case:** Không nên để Claude xử lý 100% tự động với tác vụ quan trọng; luôn có cơ chế escalate
- **Structured output (JSON) dễ tích hợp hơn free text:** Khi output cần xử lý programmatically, yêu cầu Claude trả về JSON
- **Đo lường và iterate:** A/B test prompt, theo dõi quality metrics, cải thiện liên tục
