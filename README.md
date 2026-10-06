# Track 1 · Day 20 — Metrics Pack

## Thông tin học viên

- **Họ và tên:** Nguyễn Văn Sơn
- **Mã học viên:** 2A202602744
- **Nhóm:** Ca Khô
- **Dự án chọn làm:** AI Notes: Personal Learning Notes trong VLearn (Case B — tiếp nối Day 17–18)
- **Evidence gốc:** 1 practice interview P07 Day 17; prototype 18 tổ hợp URL + logger CSV Day 18
- **Metrics Pack trong repository:** [Mở bản Markdown](./metrics-pack.md)

> Trước khi nộp: mở link ở cửa sổ ẩn danh để chắc chắn giảng viên có quyền xem (repo public hoặc đã cấp quyền).

## Điều tôi mang về áp dụng cho dự án thật

Tôi áp dụng core action “chốt và lưu ghi chú có neo vị trí” cho luồng cuối buổi học trong VLearn; `study_note_review_viewed` là event xác nhận người học có cơ hội ôn từ note mà không cần xem lại toàn bộ video. Cadence event-based theo từng buổi học vừa xong trong `study_cycle_id` 2 tuần cố định, thay vì ép daily. NSM là study opportunity review value rate — thêm tầng outcome `review_check_completed` ≥2/3 (kế thừa 3 câu Day 18) để tách khỏi engagement depth; full video rewatch rate là guardrail (không tăng quá 5 điểm % so với baseline pilot). Progress loop qua hai chu kỳ và bộ tracking 7 events kiểm chứng lịch sử tích lũy có làm tăng value rate hay không.

Tiếp nối Day 18, câu hỏi lớn nhất vẫn là Hypothesis B: người học vốn không ôn bằng note mà xem lại video. Metrics Pack này không tuyên bố đã giải được — nó đặt NSM và counter để đo đúng câu đó trong pilot.

## Cấu trúc bài nộp

- [Metrics Pack 00–07](./metrics-pack.md)
- [AI Support Log](./ai-support-log.md)

```text
Track1_Day20_2A202602744_NguyenVanSon/
├── README.md
├── metrics-pack.md
└── ai-support-log.md
```

## Quy tắc sử dụng AI

AI chỉ được dùng để brainstorm ứng viên core action và câu hỏi phản biện, gợi ý tên event dạng `object_action` và acceptance criteria mẫu, hoặc đóng vai khách hàng khó tính để kiểm tra core job/cadence.

Các quyết định chọn core action, kết luận cadence, metric hypothesis, rationale và reflection phải do học viên tự thực hiện. Không sử dụng benchmark hoặc số liệu retention do AI tạo ra khi không có nguồn kiểm chứng.

## Kiểm tra trước khi nộp

- [x] Repository đúng tên `Track1_Day20_2A202602744_NguyenVanSon`.
- [x] README có tên, MHV, dự án và link Metrics Pack.
- [x] Metrics Pack đủ mục 00–06 (+07 self-check) và không còn placeholder.
- [x] Core Action qua đủ 5 tiêu chí tự kiểm.
- [x] Cadence suy ra từ action nature và có câu kết luận theo template (có “vì”).
- [x] Retention đủ 6 thành phần, khớp cadence.
- [x] NSM là rate/ratio có mẫu số và có counter-metric.
- [x] Product loop chạy qua ít nhất 2 chu kỳ và có metric hypothesis trỏ về Phase 3.
- [x] Tracking có 4–8 events (bài này 7); mọi event map về metric và chỉ bắn khi hoàn tất.
- [x] Có ít nhất 2 acceptance criteria kiểm thử được (bài này 4).
- [x] Revision log ghi lý do cho mọi thay đổi quan trọng.
- [x] AI Support Log trả lời ngắn, trung thực đủ 3 câu hỏi bắt buộc.
