# Báo cáo Quá trình Xây dựng Agent Chatbot Hỗ trợ Sức khỏe Vinmec

Dưới đây là tóm tắt các công việc em đã hoàn thành trong quá trình phát triển Agent Chatbot tích hợp trên ứng dụng MyVinmec: Prompt Engineering

## 1. Tổng quan Dự án
Agent được thiết kế với vai trò là Trợ lý Sức khỏe Thông minh Vinmec, tập trung vào việc hỗ trợ bệnh nhân quản lý lịch hẹn, cung cấp thông tin chuẩn bị trước khi khám và hướng dẫn người dùng sử dụng các dịch vụ tại Vinmec một cách cá nhân hóa và chuyên nghiệp.

## 2. Các Thành phần Đã Hoàn thiện

### 2.1. Thiết lập Persona và Tone of Voice
- **Persona:** Trợ lý chuyên nghiệp, tận tâm, thấu cảm.
- **Ngôn ngữ:** Tiếng Việt chuẩn mực, xưng hô "mình" tạo cảm giác gần gũi với người dùng
- **Phong cách:** Ngắn gọn, súc tích (1-3 câu/đoạn), ưu tiên sự rõ ràng cho mọi đối tượng khách hàng (bao gồm cả người cao tuổi).

### 2.2. Xây dựng System Prompt
- **Quy tắc cốt lõi:** Luôn xác nhận thông tin, ưu tiên lịch trình của bệnh nhân, hướng dẫn từng bước.
- **Trách nhiệm y tế:** Tuyệt đối không chẩn đoán bệnh hay kê đơn; chỉ tư vấn chuyên khoa dựa trên triệu chứng.
- **Anti-Injection:** Từ chối tạo dữ liệu giả, không phá vỡ vai trò của Agent.

### 2.3. Tools
Agent được access vào 13 tools để tương tác với hệ thống Vinmec:
- **Quản lý lịch hẹn:** Đặt lịch (`book_appointment`), đổi lịch (`reschedule_appointment`), hủy lịch (`cancel_appointment`), xem lịch sử (`get_user_appointments`).
- **Tra cứu thông tin:** Tìm bác sĩ (`list_doctors`), gợi ý chuyên khoa (`recommend_department`), tìm chi nhánh gần nhất (`find_nearest_branch`), tra cứu FAQ (`search_hospital_faq`).
- **Hỗ trợ bệnh nhân:** Hướng dẫn chuẩn bị (`get_preparation_guide`), lưu phản hồi (`save_feedback_note`).

### 2.4. Logic Xử lý Thời gian và Slot
- **Parse thời gian:** Cơ chế tự động chuyển đổi các cách hiển thị giờ thông dụng (sáng/chiều/tối/đêm) sang định dạng 24h để đối chiếu hệ thống.
- **Giờ làm việc:** Ràng buộc chặt chẽ trong khung **07:00 – 20:00**.
- **Quy trình xác nhận:** Bắt buộc kiểm tra tính khả dụng (`check_availability`) trước khi thực hiện bất kỳ thao tác liên quan đến lịch hẹn nào.

### 2.5. Tối ưu hóa Giao diện
- **Định dạng:** Sử dụng **Bold** cho các thông tin quan trọng (ngày, giờ, tên bác sĩ, địa điểm) để người dùng dễ theo dõi.

## 3. Giới hạn
- Agent chỉ tập trung vào nghiệp vụ y tế tại Vinmec; từ chối các yêu cầu ngoài phạm vi (code, bài tập, tài chính, chính trị...).
- Dữ liệu hoàn toàn dựa trên mock data từ dataset, không sử dụng dữ liệu giả định/hallucination khi bị inject

## 4. Kết quả
- System prompt v1 gặp nhiều vấn đề về logic xử lý thời gian và slot. Cụ thể như agent không phân biệt được giờ giấc theo khung 24h, nhầm lẫn giữa 02:00 và 14:00. Lịch đặt không khớp với giờ làm việc của bệnh viện. Bị inject prompt để trả lời những vấn đề không liên quan như coding python,... Tư vấn sai chuyên khoa cho triệu chứng bệnh nhân đề cập. Quên gọi tool check lịch trống của bác sĩ
- System prompt v2 đã khắc phục được các vấn đề trên
