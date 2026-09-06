# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `Câu trả lời của bạn` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature càng thấp (0.0) thì phản hồi càng ổn định, máy móc và ít sáng tạo. Ngược lại, temperature càng cao (1.5) thì phản hồi càng đa dạng, sáng tạo, nhưng có thể sinh ra văn bản vô nghĩa hoặc ảo giác (hallucination).

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature ở mức thấp (khoảng 0.0 đến 0.2). Lý do là chatbot hỗ trợ khách hàng cần sự chính xác, nhất quán và đáng tin cậy cao, không cần sáng tạo để tránh đưa ra thông tin sai lệch cho khách hàng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá, GPT-4o đắt hơn GPT-4o-mini khoảng 16.6 lần (cả input và output). GPT-4o xứng đáng khi cần xử lý các tác vụ suy luận logic phức tạp, viết code khó hoặc phân tích đa phương thức. Nên dùng GPT-4o-mini cho các tác vụ đơn giản, lặp đi lặp lại như phân loại văn bản, tóm tắt ngắn, hoặc khi ngân sách hạn hẹp.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên, phản hồi ngắn gọn, dùng từ ngữ đơn giản, gần gũi và ví dụ minh họa dễ hiểu (như sổ liên lạc hoặc đồ chơi). Với persona chuyên gia, phản hồi dài hơn, sử dụng nhiều thuật ngữ chuyên ngành (như phi tập trung, mã hóa cryptography, node) và ví dụ phức tạp. System prompt giúp định hướng rõ ràng giọng điệu, phong cách và mức độ chuyên sâu của câu trả lời từ model.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số token đếm bằng tiktoken thường cao hơn so với ước lượng thô khoảng 1.5 đến 2 lần đối với tiếng Việt. Tiếng Việt tốn nhiều token hơn tiếng Anh vì các mô hình ngôn ngữ như GPT chủ yếu được huấn luyện bằng dữ liệu tiếng Anh, nên bộ tokenizer được tối ưu cho tiếng Anh (1 từ ~ 1 token), trong khi từ tiếng Việt thường bị chia nhỏ thành nhiều ký tự hoặc âm tiết, làm tăng số lượng token.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng chatbot giao tiếp thời gian thực, nơi người dùng cần xem phản hồi ngay lập tức để giảm cảm giác chờ đợi (giảm độ trễ cảm nhận - TTFB). Non-streaming phù hợp hơn cho các tác vụ xử lý hàng loạt (batch processing), phân tích dữ liệu, hoặc khi hệ thống cần nhận toàn bộ kết quả để định dạng/lưu trữ trước khi hiển thị cho người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên server bằng cách giãn cách thời gian giữa các lần thử lại, thay vì dồn dập gửi yêu cầu. Nếu hàng nghìn client cùng retry với delay cố định, server vừa phục hồi sẽ lại bị quá tải ngay lập tức bởi một lượng lớn yêu cầu cùng một lúc (hiệu ứng Thundering Herd), dẫn đến sập hệ thống liên tục.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> System prompt: "Bạn là trợ giảng lập trình thân thiện, hãy giải thích code thật ngắn gọn, dễ hiểu bằng tiếng Việt". Tôi yêu cầu "ngắn gọn" để tiết kiệm token và thời gian đọc, "bằng tiếng Việt" để đảm bảo phản hồi luôn ở ngôn ngữ mong muốn thay vì tự động chuyển sang tiếng Anh.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ lưu 3 lượt gần nhất, nên trợ lý không thể nhớ ngữ cảnh từ đầu buổi chat. Cải thiện: Tích hợp cơ sở dữ liệu vector (như ChromaDB hoặc Pinecone) để lưu trữ toàn bộ lịch sử hội thoại, và dùng kỹ thuật RAG để trích xuất các đoạn chat liên quan nhất làm context đưa vào prompt trước khi gọi API.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
