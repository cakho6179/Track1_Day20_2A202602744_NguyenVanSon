# AI Support Log

> Học viên: Nguyễn Văn Sơn — 2A202602744 · nhóm Ca Khô
> Bài: Track 1 Day 20 — Metrics Pack · Dự án AI Notes: Personal Learning Notes (tiếp nối Day 17–18 Case B)
> Công cụ: OpenCode (`opencode`, model `muse-spark`) · dùng 2 phiên 05–06/10/2026 để brainstorm, đặt tên event và phản biện

## AI đã giúp tôi ở đâu?

AI (OpenCode) hỗ trợ hệ thống hóa các ứng viên core action/core value event từ evidence P07 Day 17, phản biện khác biệt giữa “lưu note” và “ôn được từ note”, gợi ý tên event `snake_case`, properties, acceptance criteria mẫu và kiểm tra truy vết metric → event. Tôi cũng nhờ AI đóng vai học viên khó tính để kiểm tra câu kết luận cadence có chữ “vì” hay không.

## AI sai, hời hợt hoặc đề xuất metric sai nature ở đâu?

Đề xuất ban đầu dễ mặc định cadence daily/weekly, dùng DAU/streak và tổng số note tuyệt đối, và tính câu trả lời AI sinh là value. Tôi kiểm tra lại thấy nhu cầu chốt note phụ thuộc buổi học vừa xong trong lịch môn (2–3 buổi/tuần, không đều; P07 đã bỏ ghi chú 2 tuần) — ép daily tạo số ảo. AI cũng gợi ý benchmark retention % và ngưỡng đỗ rubric khi chưa có pilot; tôi loại bỏ toàn bộ vì không có nguồn kiểm chứng.

## Tôi đã tự sửa hoặc quyết định lại điều gì?

Tôi chọn core action là chốt và lưu ghi chú có neo vị trí (`study_note_saved`), còn `study_note_review_viewed` (mở lại ở phiên ôn sau mà không xem lại toàn bộ video) là core value event — để đo đúng Hypothesis B Day 18. Tôi chọn cadence event-based theo buổi học trong `study_cycle_id` 2 tuần cố định, dùng study opportunity review value rate làm NSM với thêm tầng outcome `review_check_completed` ≥2/3 để tách khỏi engagement depth, và full video rewatch rate làm counter-metric (guardrail +5 điểm % so với baseline pilot). Metric hypothesis chỉ nêu hướng thay đổi trong 4 cycle tiếp theo; mọi target số và guardrail chốt sau pilot cùng nhóm.
