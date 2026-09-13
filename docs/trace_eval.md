# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Phạm Hương Giang  
> **Mã Sinh Viên / Mã Học viên:** 2A02359  
> **Chủ đề Lựa chọn:** Trợ lý Tác tử Học vụ Thông minh Đại học VinUni (VinUni Academic ReAct Agent)  

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | 5 / 5 | Bài toán đòi hỏi suy luận đa bước rõ rệt (minh chứng ở TC04): Agent không thể đặt lịch ngay mà phải chia nhỏ thành 2 bước logic tuần tự: (1) Tra cứu hồ sơ học vụ để xác định tên Cố vấn học tập của sinh viên -> (2) Dùng dữ liệu cố vấn vừa tìm được làm đầu vào để tiến hành đặt lịch hẹn tư vấn. |
| **2. Tool Interaction** | 5 / 5 | Hệ thống bắt buộc phải tương tác với công cụ bên ngoài thông qua MCP Server (JSON-RPC 2.0). Cần gọi `academic_query` để đọc dữ liệu sinh viên thời gian thực và `schedule_appointment` để ghi nhận đặt lịch thành công, tránh hoàn toàn việc LLM bịa đặt dữ liệu học vụ. |
| **3. Dynamic Decision** | 5 / 5 | Quyết định của Agent ở bước tiếp theo phụ thuộc 100% vào kết quả Observation trả về: nếu tra cứu ra cố vấn thì tiếp tục gọi tool đặt lịch; nếu sinh viên không tồn tại (status `NOT_FOUND` ở TC05) thì lập tức dừng chuỗi tác vụ và đưa ra cảnh báo chính xác, không gọi bừa bãi. |
| **4. Long Horizon Goal** | 4 / 5 | Agent phải duy trì mục tiêu tối hậu (hoàn tất buổi hẹn tư vấn cho sinh viên vào đúng khung giờ mong muốn) xuyên suốt qua nhiều bước của vòng lặp ReAct, không bị phân tâm hay quên ngữ cảnh ban đầu khi nhận phản hồi từ MCP Server. |
| **TỔNG ĐIỂM AGENTIC FIT** | **19 / 20** | *Nếu tổng điểm > 12/20: Bài toán rất phù hợp triển khai Agentic System.* |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Dán 1 đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` sinh ra từ phản hồi LLM API thật:

```json
[
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026001",
      "data": {
        "full_name": "Nguyễn Văn An",
        "class": "AI-K4",
        "gpa": 3.85,
        "email": "an.nv@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    },
    "latency_ms": 1350.2
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "FINAL_ANSWER",
    "thought": "Gemini phản hồi trực tiếp bằng văn bản (không cần gọi công cụ).",
    "output": "KẾT QUẢ TRA CỨU THÔNG TIN HỌC VỤ:\n- Họ và tên: Nguyễn Văn An\n- Mã sinh viên: SV2026001\n- Lớp: AI-K4\n- GPA: 3.85 / 4.0\n- Cố vấn học tập: PGS.TS Nguyễn Văn A",
    "latency_ms": 890.4
  }
]

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [x] Đã điền API Key thật trong `.env` và xác nhận Agent chạy mượt mà trên LLM API thật (Gemini).
- **Tổng số Test Cases đã chạy thành công:** 5 / 5 test cases.
- **Số lượt gọi Tool qua MCP Server chính xác:** 5 lượt.
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!