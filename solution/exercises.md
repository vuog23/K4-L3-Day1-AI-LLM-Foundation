# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, câu trả lời thường ít lặp lại và đa dạng hơn về cách diễn đạt, ví dụ và cấu trúc. Ở mức thấp, nội dung có xu hướng ổn định, trực tiếp và nhất quán hơn; ở mức cao, model có thể sáng tạo hơn nhưng cũng dễ lan man hoặc đưa ra chi tiết kém chắc chắn hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2–0.4 cho chatbot hỗ trợ khách hàng. Các câu trả lời cần chính xác, nhất quán và tuân theo quy trình hơn là sáng tạo; mức này vẫn đủ tự nhiên nhưng giảm nguy cơ trả lời khác nhau cho cùng một vấn đề.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Mỗi ngày có 10.000 × 3 × 350 = 10,5 triệu output token. Với đơn giá output trong lab, GPT-4o ($0,010/1K token) đắt hơn GPT-4o-mini ($0,0006/1K token) khoảng 16,7 lần. GPT-4o đáng dùng khi cần suy luận phức tạp hoặc nội dung quan trọng cần chất lượng cao; mini phù hợp cho phân loại, tóm tắt đơn giản và chatbot có lượng truy cập lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, câu trả lời thường ngắn hơn, dùng từ phổ thông, có ví dụ gần gũi và giải thích từng bước. Với persona chuyên gia tài chính, câu trả lời thường dùng thuật ngữ như sổ cái phân tán, đồng thuận và tính bất biến, đồng thời phân tích kỹ hơn. System prompt định hướng vai trò, giọng điệu, độ sâu và đối tượng người đọc nên làm thay đổi rõ rệt cách model trình bày cùng một kiến thức.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Kết quả tiktoken có thể chênh lệch đáng kể so với ước lượng số từ / 0,75; phần trăm chênh lệch được tính bằng |token_tiktoken − token_ước_lượng| / token_tiktoken × 100%. Tiếng Việt thường tốn nhiều token hơn vì dấu thanh, ký tự Unicode và cách tokenizer tách các chuỗi ít phổ biến hơn tiếng Anh. Vì vậy, đếm từ chỉ phù hợp để ước lượng thô, còn tiktoken đáng tin cậy hơn khi tính chi phí.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi người dùng chờ câu trả lời dài hoặc tác vụ có độ trễ cao, vì họ thấy nội dung xuất hiện sớm thay vì nghĩ ứng dụng bị treo. Non-streaming phù hợp hơn khi cần nhận toàn bộ kết quả trước để kiểm tra định dạng, lưu vào cơ sở dữ liệu, hiển thị một kết quả nguyên vẹn hoặc xử lý theo lô.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giảm dần tần suất gửi lại yêu cầu khi dịch vụ đang quá tải, cho server có thời gian phục hồi và tránh làm lỗi nặng hơn. Nếu hàng nghìn client đều retry sau đúng một giây, chúng có thể đồng loạt tạo một đợt tải mới (thundering herd), khiến server tiếp tục quá tải. Trong hệ thống thật, nên thêm jitter ngẫu nhiên vào thời gian chờ để phân tán các lần retry.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona: “Bạn là trợ giảng lập trình thân thiện, trả lời bằng tiếng Việt, giải thích từng bước và ưu tiên ví dụ ngắn có thể chạy được.” Cụm “bằng tiếng Việt” bảo đảm phù hợp với người học; “từng bước” giúp người mới theo dõi được tư duy; còn “ví dụ ngắn có thể chạy được” giúp câu trả lời thực hành và không quá dài.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn là chatbot chỉ giữ ba lượt hội thoại gần nhất nên dễ quên thông tin quan trọng từ đầu phiên và không có bộ nhớ dài hạn. Có thể cải thiện bằng cách lưu lịch sử vào cơ sở dữ liệu, tóm tắt các lượt cũ rồi gửi summary cùng các lượt gần nhất trong mỗi request. Cách này giữ được ngữ cảnh quan trọng nhưng vẫn kiểm soát số token và chi phí.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
