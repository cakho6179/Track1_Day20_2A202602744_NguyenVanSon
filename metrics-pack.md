# Metrics Pack — AI Notes: Personal Learning Notes (VLearn)

> Tiếp nối Day 17–18, Case B. Core action → action nature → cadence → metric & retention → product loop → tracking.
> Mọi ngưỡng hiệu quả chỉ chốt sau pilot. Không dùng benchmark retention, không dùng số liệu do AI tạo.

## 00 — Phạm vi

- **Dự án:** AI Notes: Personal Learning Notes trong VLearn — công cụ gộp dấu vết học tập rời rạc thành ghi chú ôn tập dùng được ngay.
- **Persona:** Học viên đại học học video VLearn 30–40 phút/bài, vừa học xong cần ôn lại sau vài ngày (đại diện: P07 — bạn cùng lớp, học Pipeline CPU, Cache & CPI).
- **Core job (lời người học, Day 17 nguyên văn ý):** “Khi vừa học xong một bài, tôi muốn gộp các điểm chính và chỗ chưa hiểu thành một bản ghi chú dùng được ngay, để vài ngày sau ôn lại mà không phải mở lại toàn bộ video 35 phút.”
- **Phạm vi đo:** Luồng cuối buổi học → lưu ghi chú → mở lại ghi chú ở phiên ôn sau. Không đo toàn bộ VLearn.

### Thông tin chi tiết

| Trường | Nội dung |
|---|---|
| Tên sản phẩm | AI Notes trong VLearn |
| Mô tả | Gộp 7 dấu vết rời rạc (highlight, câu hỏi, note điện thoại, 2 ảnh chụp không số slide/phút, điểm chưa hiểu) thành ghi chú 4 mục, mỗi mục có neo vị trí trong bài |
| Giai đoạn | Prototype Day 18 đã có (18 tổ hợp URL, logger CSV); chưa có baseline sản phẩm thật |
| Phạm vi | Một buổi học vừa xong (`study_session_id`) → một ghi chú (`study_note_id`) → một phiên ôn sau (`review_session_id`) |
| Persona | Học viên ôn thi, học 2–3 buổi/tuần theo lịch môn, đã từng bỏ ghi chú 2 tuần vì “ghi xong cũng chẳng mở lại được” (P07) |
| Bối cảnh | Cuối buổi học, đang có 7 dấu vết rời rạc, chưa biết ưu tiên gì, không chắc ảnh nào thuộc slide nào |
| Nỗi đau | Dấu vết không mang theo vị trí trong bài; khi ôn không định vị được, phải tua lại cả video hoặc bỏ ôn |
| Thay thế hiện tại | Highlight trong app + ảnh chụp màn hình + note điện thoại + hỏi lại ChatGPT |
| Ràng buộc | Ảnh chụp có thể thiếu số slide/phút; AI chỉ được gắn tạm và phải ghi rõ độ tin cậy; phiên ôn sau cách vài ngày, không phải ngay |

**Bằng chứng nhu cầu:** 1 practice interview P07 Day 17 (consent ghi âm có): 3 cách ghi song song, pause video liên tục, sáng hôm sau mở note “chịu, mấy tấm ảnh không biết thuộc khúc nào”, phải xem lại cả video 35 phút; quiz nhỏ lộn công thức CPI dù tưởng đã nhớ.

---

## 01 — Core Action

### Phân biệt bốn khái niệm

| Khái niệm | Câu trả lời cho AI Notes |
|---|---|
| Core job | Ôn lại một bài đã học mà không phải xem lại toàn bộ video |
| Core action | Chốt và lưu ghi chú ôn tập có neo vị trí cho một buổi học vừa xong |
| Core value | Hiểu lại bài từ ghi chú và định vị được mỗi ý thuộc đoạn nào trong bài gốc |
| Core value event | `study_note_review_viewed` — ghi chú của một buổi học được mở lại ở phiên ôn sau, render thành công và người học có cơ hội ôn từ đó |

`study_note_saved` xác nhận người học đã hoàn tất core action và dữ liệu đã lưu. Nhưng event này chưa chứng minh value — đúng Still Unproven Day 18 (Hypothesis B): người học vốn không ôn bằng note mà xem lại video. Vì vậy `study_note_review_viewed` mới là core value event.

### Core Action Card

| Thành phần | Câu trả lời |
|---|---|
| Target user | Học viên VLearn vừa học xong một bài video/Lab 30–40 phút |
| Core job | Ôn lại bài đã học mà không phải xem lại toàn bộ video |
| Core action | Chốt và lưu ghi chú ôn tập có neo vị trí cho buổi học vừa xong |
| Object | Một ghi chú ôn tập (`study_note_id`), gắn với một buổi học (`study_session_id`) |
| Preconditions | Buổi học đã kết thúc; có ít nhất 1 dấu vết (highlight/note/ảnh/câu hỏi); danh sách slide/lab của bài đã có để neo |
| Completion rule | Backend xác nhận đã lưu thành công `study_note_id` với ít nhất 3 mục, mỗi mục có `anchor` (slide_id hoặc lab_step_id + `anchor_source`: có số phút / bạn xác nhận / AI gắn tạm) và note chuyển lần đầu sang trạng thái `saved` |
| Core value | Ôn được từ ghi chú, biết mỗi mục thuộc đoạn nào, không cần tua lại cả video |
| Evidence of value | Ghi chú của buổi học cũ được mở ở phiên ôn sau, render đủ mục + neo, và phiên đó không kèm xem lại toàn bộ video |
| Candidate event | Core action: `study_note_saved`; core value: `study_note_review_viewed` |

### Tự kiểm 5 tiêu chí

| # | Tiêu chí | Đạt? | Bằng chứng/lý giải |
|---:|---|:---:|---|
| 1 | Gần core value | ✓ | Ghi chú có neo là đầu vào trực tiếp để ôn; không có nó thì phiên ôn sau không thể bắt đầu từ note |
| 2 | Có thể lặp lại | ✓ | Mỗi buổi học vừa xong là một cơ hội lưu note mới (2–3 lần/tuần theo lịch môn) |
| 3 | Có thể quan sát | ✓ | Hệ thống biết chính xác khi backend lưu đủ 3 mục có anchor cho một `study_note_id` |
| 4 | Có ý nghĩa | ✓ | Tỷ lệ buổi học có note dùng được tăng nghĩa là lịch sử ôn đầy đủ hơn; không dùng tổng số note tuyệt đối để ép lưu note rỗng |
| 5 | Có thể tác động | ✓ | Team có thể giảm bước chốt, làm rõ badge tin cậy neo, cho phép sửa anchor trước khi lưu |

**Vì sao không phải “mở app”, “highlight”, “hỏi AI”:** Mở app và highlight chỉ tạo dấu vết rời rạc — đúng hiện trạng P07 đang kẹt. Câu trả lời do AI sinh chỉ là output hệ thống. Value chỉ xuất hiện khi 7 dấu vết được chốt thành một artifact có neo và được lưu.

**Kết quả GATE 1:** Đạt 5/5; có actor, object và completion rule rõ ràng. Core action được giữ nguyên.

---

## 02 — Nature & cadence

### Action Nature Card

| Thành phần | Câu trả lời |
|---|---|
| Actor | Học viên VLearn vừa học xong một bài |
| Intent | Gộp dấu vết rời rạc thành một bản dùng được ngay để vài ngày sau ôn |
| Trigger | Chủ yếu do buổi học vừa kết thúc trong lịch môn hoặc nhu cầu ôn trước quiz/thi; reminder chỉ nhắc lại trigger, không tạo ra nhu cầu học bài mới |
| Effort | Vừa: rà 7 dấu vết, quyết định vị trí 2 ảnh thiếu thông tin, chốt 4 mục rồi lưu; prototype Day 18 đo 1–60 giây tùy option |
| Value timing | Một phần tức thời khi có bản chốt; phần lớn tích lũy khi mở lại ở phiên ôn sau vài ngày |
| State | `study_note_id` với các mục + `anchor` + `anchor_source` được lưu theo `study_session_id` trong timeline môn học |
| Dependency | Phụ thuộc việc buổi học đã diễn ra và có dấu vết; không phụ thuộc user khác. Chất lượng neo phụ thuộc rule/badge đã duyệt |
| Repeat condition | Một buổi học mới vừa xong và có nội dung cần ôn, hoặc một bài cũ đến hạn ôn lại |

### Kết luận cadence

- **Dạng hành vi được chọn:** Theo sự kiện (event-based), không phải daily.

> **Đối với học viên VLearn vừa học xong một bài 30–40 phút, core action chốt và lưu ghi chú có neo vị trí thường xuất hiện khi một buổi học vừa kết thúc và có dấu vết cần gộp, vì nhu cầu gộp chỉ sinh ra sau khi đã học và lịch học tuân theo lịch môn (2–3 buổi/tuần, không đều) chứ không do ứng dụng tạo ra. Do đó, nhịp đo phù hợp là event-based theo từng buổi học vừa xong ở cấp từng học viên trong từng môn.**

- **Nature trước, nurture sau:** Nếu bỏ notification, học viên vẫn có lý do chốt note khi vừa học xong bài khó. Reminder, template 4 mục và nút “AI gắn tạm” chỉ hỗ trợ hành vi tự nhiên này.
- **Không chọn daily/weekly cố định:** Ép streak đăng nhập hằng ngày sẽ tạo số liệu ảo — đúng bài học Day 18: P07 đã bỏ ghi chú 2 tuần, ép daily không kéo được hành vi quay lại.
- **Frequency cao hơn không mặc nhiên tốt hơn:** Value được đánh giá bằng tỷ lệ buổi học có note được ôn lại thành công, không phải tổng số note đã lưu.
- **Cửa sổ 7 ngày:** Chỉ dùng để tổng hợp vận hành; không định nghĩa cadence tự nhiên.
- **Giả định cần kiểm chứng:** Mỗi buổi học xác định được `study_session_id` đáng tin và học viên hiểu đúng 3 loại badge neo (có số phút / bạn xác nhận / AI gắn tạm).

**Kết quả GATE 2:** Đạt — đã chọn một dạng hành vi, cadence xuất phát từ trigger thực tế, giải thích được “vì” và không bị quyết định bởi cửa sổ dashboard.

---

## 03 — Metric System

### Activation metric

| Thành phần | Định nghĩa |
|---|---|
| Start event | `study_session_finished`: buổi học đầu tiên trong kỳ được ghi nhận hoàn tất với ít nhất 1 dấu vết |
| Activation event | `study_note_review_viewed` hợp lệ lần thứ hai, gắn với hai `study_session_id` khác nhau |
| Time window | Từ `study_session_finished` đầu tiên đến hết 14 ngày sau đó |
| Chu kỳ đo | `study_cycle_id`: khối 2 tuần cố định Thứ 2 00:00 – Chủ Nhật tuần sau 23:59, timezone `Asia/Ho_Chi_Minh` (vd `2026-W41/2`). Mọi NSM/retention chốt theo cycle này, lát cắt 7 ngày chỉ để vận hành |

**Activation rate** = số learner đạt activation event trong 14 ngày đầu / số learner có `study_session_finished` đầu tiên trong cùng kỳ (không lọc mẫu số bằng hành vi tương lai).

Learner chỉ có 0–1 buổi học trong 14 ngày đầu được tách thành segment riêng “chưa đủ cơ hội”, không tính vào mẫu số chính và không tính là churn — tránh look-ahead bias làm phồng rate.

Một `study_note_saved` đầu tiên chỉ cho thấy user **active**. Một `study_note_review_viewed` đầu tiên chứng minh first value. Lần thứ hai trên buổi học khác mới là giả thuyết activation — cho thấy hành vi chốt → ôn lại đã lặp. Giả thuyết “hai lần” phải kiểm chứng bằng phân tích retention theo số lần review, không xem là benchmark đã biết.

### Engagement metric — tối đa hai góc đo

| Góc đo | Metric và công thức | Cửa sổ |
|---|---|---|
| Frequency | **Qualified notes per eligible session** = số `study_note_saved` hợp lệ (đủ 3 mục có anchor) / số `study_session_finished` đủ điều kiện | Một `study_cycle_id` 2 tuần; báo cáo có thể tổng hợp theo 7 ngày |
| Depth | **Review rate** = số note hợp lệ có `study_note_review_viewed` (mọi lượt mở đạt viewport) / tổng số `study_note_saved` hợp lệ | Một `study_cycle_id` 2 tuần; báo cáo có thể tổng hợp theo 7 ngày |

Depth chỉ đo “có mở lại hay không”. NSM bên dưới đo thêm tầng outcome (không rewatch + checkpoint đạt) nên không trùng depth. Không dùng breadth vì phạm vi chỉ có một workflow ghi chú — ôn tập. Frequency cao hơn không mặc nhiên tốt hơn; luôn đọc cùng số buổi học đến hạn ôn.

### North Star Metric

**NSM: Study opportunity review value rate**

| Thành phần công thức NSM | Định nghĩa |
|---|---|
| Unit of value | Một buổi học đã có ghi chú hợp lệ được ôn thành công từ note |
| Quality threshold | Note đủ 3 mục có anchor hợp lệ; phiên review render đủ mục + neo, không kèm `full_video_rewatch` (xem lại ≥80% video gốc), và `review_check_completed` đạt ≥2/3 checkpoint ôn tập (kế thừa 3 câu kiểm tra Day 18: Pipeline CPU / Cache & CPI, đáp án nhiễu từ lỗi sai thật) |
| Frequency | Được tính ở mỗi buổi học đến hạn ôn và tổng hợp trong từng `study_cycle_id` 2 tuần |

```text
NSM = Số buổi học đến hạn ôn có note hợp lệ được review thành công
      (viewed + không rewatch full + check ≥2/3)
      / Tổng số buổi học có note hợp lệ đến hạn ôn trong cùng study_cycle_id
```

NSM này đo trực tiếp Hypothesis B Day 17–18 — câu hỏi cuối phiên test: “Sáng mai mở note lên, bạn có cần mở lại video không?”.

### Leading indicators

| Leading indicator | Công thức | Vì sao tin rằng dự báo core action lặp lại |
|---|---|---|
| Note completion rate | `study_note_saved` / `study_note_started` hợp lệ | Ít bỏ dở khi chốt nghĩa là ma sát thấp hơn, tăng khả năng chốt ở buổi hiện tại và quay lại ở buổi sau |
| Median time to save | Trung vị `saved_at - started_at` của note hoàn tất (kỳ vọng giảm sau khi tối ưu template; đọc cùng anchor confirm rate — nếu time giảm mà correction tăng thì chốt ẩu, fail) | Thời gian chốt ngắn hơn phản ánh effort thấp hơn; cần kiểm chứng tương quan với activation/retention bằng dữ liệu pilot |
| Anchor confirm rate | Số anchor “AI gắn tạm” được user xác nhận/sửa trước khi lưu / tổng số anchor “AI gắn tạm” | User chịu quyết định vị trí ảnh (trục Day 18) thì note đáng tin hơn, tạo lý do mở lại ở phiên sau |

### Counter-metric

**Full video rewatch rate** = số phiên ôn có `full_video_rewatch_started` (xem ≥80% video gốc) / tổng số phiên ôn có note trong cùng `study_cycle_id`; đồng thời theo dõi rolling 28 ngày khi mẫu nhỏ. Guardrail kiểm thử được: rewatch rate chu kỳ hiện tại không tăng quá 5 điểm % so với baseline 2 tuần pilot đầu; người chốt là nhóm Ca Khô sau buổi pilot đầu, ghi vào revision log. NSM tăng mà counter vượt guardrail thì tăng trưởng là ảo.

Thứ cấp: **Anchor correction rate** = số note bị sửa toàn bộ anchor sau khi lưu / tổng số note đã lưu. Dùng để bắt failure mode AI đoán sai vị trí ảnh hàng loạt.

**Quy tắc tính**

- Eligible session: buổi học đã `finished`, thuộc môn đang hiệu lực; mỗi `study_session_id` chỉ có một outcome.
- Một phiên ôn (`review_session_id`) gắn đúng một `study_session_id` (một note); ôn nhiều note thì mỗi note một row. `watch_coverage` đo bằng server-clock, snapshot tại lúc đóng phiên.
- Loại trừ: internal/test/bot, event trùng, buổi học hủy, note thiếu mục/anchor, dữ liệu ngoài cửa sổ.
- Nguồn: event stream, study session, study note, review session.
- Timezone: `Asia/Ho_Chi_Minh`; `study_cycle_id` là khối 2 tuần cố định, không trôi.
- Chống trùng: theo `event_id` + idempotency key `user_id + study_session_id + study_note_id`; metric note chỉ lấy lần `saved` hợp lệ đầu tiên (FSM lần đầu chuyển sang `saved`).

---

## 04 — Retention Definition

> **Retention là tỷ lệ learner đã activated trong chu kỳ 2 tuần đầu và có buổi học mới ở chu kỳ kế tiếp, quay lại chốt và ôn lại thành công ít nhất một buổi học mới trong chu kỳ 2 tuần kế tiếp.**

| Thành phần | Định nghĩa |
|---|---|
| Unit | Học viên VLearn trong một môn đang học (`user_id` ổn định) |
| Cohort entry | User gia nhập cohort tại `study_note_review_viewed` hợp lệ lần thứ hai trên hai buổi học khác nhau trong `study_cycle_id` đầu tiên |
| Return event | Trong `study_cycle_id` kế tiếp, user có `study_note_review_viewed` cho buổi học mới; `study_note_saved` cho buổi đó được phép lưu cuối cycle trước nhưng phải có viewed trong window + 7 ngày grace (vì value timing vài ngày sau, tránh false negative) |
| Window | Custom bracket: `study_cycle_id` kế tiếp + 7 ngày grace cho viewed; không dùng D7 cố định |
| Threshold | Ít nhất một return event trong window và chỉ tính user một lần vào tử số |
| Segment | Học viên đã activated, môn còn hiệu lực và có ít nhất một buổi học mới trong return window; chưa có buổi mới/hủy/no-show = chưa vào mẫu số, không tính churn |

```text
Retention = Số learner trong cohort có ≥1 return event đúng window
            / Số learner trong cohort có ≥1 buổi học mới đúng window
```

**Measurement rule và exclusions:** Bracket retention theo `study_cycle_id`, timezone `Asia/Ho_Chi_Minh`; deduplicate event; loại internal/test/bot, môn hết hiệu lực trước window, user không có buổi học mới, note thiếu anchor và phiên review kèm xem lại toàn bộ video.

**Vì sao khớp cadence:** Core action xuất hiện theo buổi học trong chu kỳ môn, nên return window là chu kỳ tự nhiên kế tiếp và denominator chỉ gồm learner thật sự có trigger. Báo cáo 7 ngày chỉ là lát cắt vận hành, không thay thế natural cycle.

**Ba mốc so sánh:** (1) so các chu kỳ 2 tuần liên tiếp; (2) so đúng segment/môn có số buổi học tương đương; (3) chỉ so benchmark category khi có nguồn kiểm chứng phù hợp — hiện chưa có nên không đưa con số cứng.

**Kết quả GATE 3:** Đạt — activation có start event, activation event và time window; engagement có hai góc đo; retention đủ sáu thành phần và khớp cadence; NSM đủ unit of value, quality threshold, frequency; có counter-metric.

---

## 05 — Product Loop

### Loại loop chính

**Progress loop** — mỗi note hợp lệ vừa tạo value tức thời (có bản chốt), vừa làm giàu lịch sử môn học để lần ôn sau có thêm bối cảnh và ôn nhanh hơn.

### Hai chu kỳ liên tiếp

#### Chu kỳ 1

```text
Natural trigger: buổi học thứ nhất vừa xong, có 7 dấu vết
→ Core action: chốt 4 mục có neo vị trí và lưu study_note_id
→ Immediate value: có bản chốt dùng được ngay
→ Saved state/investment: note chuẩn hóa được lưu vào timeline môn học
```

#### Chu kỳ 2

```text
Next natural trigger: buổi học tiếp theo vừa xong hoặc bài cũ đến hạn ôn
→ Core action tiếp theo: chốt note mới, đồng thời mở lại note cũ để ôn
→ Repeat value: hiểu bài mới và so được với lịch sử của chính mình, không cần xem lại toàn bộ video
→ Saved state tăng: timeline đầy đủ hơn cho chu kỳ kế tiếp
```

- **Reason to return khi bỏ notification:** Buổi học mới trong lịch môn hoặc quiz sắp tới vẫn xuất hiện; user quay lại để chốt bài mới và ôn bài cũ từ lịch sử đã tích lũy. Notification chỉ chuyển tiếp trigger.
- **Metric hypothesis:** Nếu loop này hoạt động, metric **Study opportunity review value rate** sẽ thay đổi theo hướng **tăng so với baseline 2 tuần pilot đầu** trong **bốn `study_cycle_id` tiếp theo**, vì mỗi note hợp lệ làm lịch sử môn học đầy đủ hơn và lần ôn sau có thêm neo vị trí; đồng thời **Full video rewatch rate** không tăng quá 5 điểm % so với baseline (guardrail nhóm Ca Khô chốt sau buổi pilot đầu).
- **Dữ liệu kiểm chứng:** `study_session_finished`, `study_note_saved`, `study_note_review_viewed`, `review_check_completed`, `history_context_used`, `full_video_rewatch_started`.

---

## 06 — Tracking nhanh

Mọi event có `event_id`, `user_id`, `occurred_at`, `source`; completion chỉ phát sau xác nhận server. Properties bắt buộc được nêu trong cột ý nghĩa.

| Tên event | Ý nghĩa — điều đã xảy ra | Thời điểm ghi nhận | Metric sử dụng |
|---|---|---|---|
| `study_session_finished` | Buổi học `study_session_id`, `course_id`, `session_type` (lý thuyết/lab) đã hoàn tất với ít nhất 1 dấu vết | Sau khi backend chuyển session sang `finished` thành công | Activation denominator, NSM denominator, retention denominator |
| `study_note_started` | User đã vào luồng chốt cho `study_session_id`, tạo `study_note_id` và chọn `entry_method` | Khi draft note đầu tiên được server tạo thành công; không bắn chỉ vì mở màn hình note | Leading: completion rate và time to save |
| `study_note_saved` | Note đã lưu đủ ít nhất 3 mục có `anchor` + `anchor_source` cho `study_note_id` | Sau transaction lưu thành công và note chuyển lần đầu sang `saved` | Engagement frequency, leading completion/time, NSM numerator candidate |
| `study_note_review_viewed` | Note `study_note_id` của buổi học cũ đã qua quality check và được xem ở phiên ôn; kèm `review_session_id`, `anchor_coverage`, `history_context_used` | Khi note render thành công và đạt điều kiện viewport (server-clock + snapshot); không bắn lúc hệ thống mới tạo note | Activation, engagement depth, NSM numerator, retention numerator |
| `review_check_completed` | Learner làm 3 checkpoint ôn tập của buổi học (`review_session_id`, `correct_count`, `check_version` kế thừa Day 18) sau khi viewed | Sau khi backend chấm xong lần đầu cho `review_session_id`; retry cùng phiên không ghi thêm | NSM quality (≥2/3 mới tính value), phân biệt NSM với depth |
| `full_video_rewatch_started` | Phiên ôn đã mở lại ≥80% thời lượng video gốc của buổi học (`study_session_id`, `watch_coverage`) | Khi player xác nhận ngưỡng coverage bằng server-clock; mở lướt vài giây không tính | Counter-metric và quality filter của NSM |
| `anchor_correction_submitted` | User đã sửa/xác nhận anchor (`study_note_id`, `corrected_anchors`, `correction_scope`) | Sau khi bản sửa được lưu thành công | Counter thứ cấp và quality filter |

### Acceptance criteria

1. **Completion:** Với mỗi `study_note_id`, chỉ ghi `study_note_saved` khi backend chuyển note lần đầu (FSM) từ chưa hoàn tất sang `saved` sau khi lưu đủ 3 mục có anchor. Bấm nút nhưng request lỗi, bị hủy hoặc thiếu anchor không phát event.
2. **Không ghi trùng:** Với cùng `user_id + study_session_id + study_note_id`, reload, retry, autosave hoặc mở lại note đã lưu không tạo thêm `study_note_saved`; consumer deduplicate theo `event_id` và idempotency key của lần chuyển trạng thái. On-time tính bằng server-clock + snapshot, reschedule sau start không hồi tố.
3. **Value event:** `study_note_review_viewed` chỉ phát một lần cho cùng `user_id + study_note_id + review_session_id` khi note đã qua quality check và render đạt điều kiện viewport; backend mới tạo note hoặc user rời trang trước khi render không được tính.
4. **Retention:** User chỉ được tính một lần vào tử số khi có viewed cho buổi học mới trong `study_cycle_id` kế tiếp + 7 ngày grace; loại event ngoài window+grace, trùng và tài khoản test/internal/bot. Chưa có buổi mới/hủy/no-show thì chưa vào mẫu số.

| Metric | Numerator | Denominator | Đủ? |
|---|---|---|:---:|
| Activation | User có viewed hợp lệ lần 2 trên buổi học khác trong 14 ngày đầu | Mọi user có `study_session_finished` đầu tiên (segment <2 buổi tách riêng, không tính churn) | ✓ |
| Engagement — frequency | `study_note_saved` hợp lệ | `study_session_finished` đủ điều kiện | ✓ |
| Engagement — depth | Note có `study_note_review_viewed` | `study_note_saved` hợp lệ | ✓ |
| NSM | Buổi học có saved + viewed + `review_check_completed` ≥2/3, không rewatch full | Buổi học có note đến hạn ôn trong cùng `study_cycle_id` | ✓ |
| Leading — completion | `study_note_saved` | `study_note_started` | ✓ |
| Leading — time | `saved.occurred_at - started.occurred_at` cùng `study_note_id` | Note hoàn tất hợp lệ | ✓ |
| Leading — anchor | Anchor được xác nhận/sửa | Anchor “AI gắn tạm” | ✓ |
| Counter | Phiên có `full_video_rewatch_started` | Mọi phiên ôn có note | ✓ |
| Retention | Cohort user có viewed cho buổi mới trong window+grace | Cohort user có buổi học mới trong window | ✓ |

**Kết quả GATE 4:** Đạt — progress loop chạy qua hai chu kỳ, metric hypothesis trỏ về NSM Phase 3, có 7 core events và mọi event đều map tới ít nhất một metric.

---

## 07 — Self-check & Revision

### Tự soi 7 lỗi kinh điển

| # | Câu tự soi | Kết quả | Đối chiếu và rationale |
|---:|---|:---:|---|
| 1 | Core action không phải thao tác giao diện hay output hệ thống? | ✓ | Core action là chốt và lưu note có neo, chỉ hoàn tất khi backend lưu đủ mục; “mở app”, highlight rời rạc và câu AI sinh không được tính |
| 2 | Activation không phải xem hướng dẫn hay đăng nhập? | ✓ | Activation là review lần thứ hai trên buổi học khác trong 14 ngày đầu; login/onboarding không tham gia tử số |
| 3 | Frequency không cao hơn nhu cầu thật? | ✓ | Frequency được chuẩn hóa theo buổi học đến hạn ôn, không tối ưu tổng số note và không khuyến khích lưu note rỗng |
| 4 | Loop có reason to return ngoài notification? | ✓ | Buổi học mới và quiz sắp tới tạo trigger; lịch sử tích lũy làm tăng bối cảnh cho lần sau. Notification chỉ chuyển trigger |
| 5 | Retention không dùng chung một window cho mọi cadence? | ✓ | Dùng custom bracket theo `study_cycle_id` 2 tuần; lát cắt 7 ngày không thay thế natural cycle |
| 6 | Mọi event đều map về một metric? | ✓ | Cả 7 event trong mục 06 đều có cột Metric sử dụng và phục vụ ít nhất một metric Phase 3/4 |
| 7 | Metric nào cũng có event để tính? | ✓ | Bảng metric → numerator/denominator tại mục 06 truy được activation, hai engagement metrics, NSM, ba leading indicators, counter và retention về event cụ thể |

Không giữ lựa chọn nào “phá rule”. Sau self-check, các định nghĩa vẫn nhất quán từ Core Action → Nature & cadence → Metric/Retention → Loop → Tracking.

**Kết quả GATE 5:** Đạt — đã đối chiếu đủ 7 câu, sửa các điểm chưa nhất quán và ghi rationale cho mọi thay đổi lớn.

### Revision log

| Ngày | Thay đổi | Trước → Sau | Lý do | Đã rà soát |
|---|---|---|---|---|
| 2026-10-04 | Core action/value event | Gộp “lưu note” và “ôn từ note” → `study_note_saved` xác nhận action, `study_note_review_viewed` xác nhận cơ hội nhận value | Output được lưu chưa chứng minh user đã ôn được; Hypothesis B Day 18 yêu cầu đo việc mở lại | Tất cả phần sau |
| 2026-10-05 | Cadence | Daily/weekly → event-based theo buổi học và `study_cycle_id` 2 tuần cố định | Lịch môn 2–3 buổi/tuần không đều, P07 đã bỏ ghi chú 2 tuần nên ép daily tạo số ảo | Metrics đến tracking |
| 2026-10-05 | NSM tách khỏi depth | Viewed-không-rewatch → viewed + không rewatch + check ≥2/3 (`review_check_completed`) | Depth và NSM cũ trùng tử/mẫu; NSM cần thêm tầng outcome từ 3 câu Day 18 | Loops và tracking |
| 2026-10-06 | Activation/retention | Mẫu số lọc ≥2 buổi → mọi newcomer (segment <2 buổi riêng); retention saved+viewed cùng cycle → viewed trong window+7 ngày grace | Bỏ look-ahead bias; tránh false negative khi note cuối cycle chưa kịp đến hạn ôn vài ngày sau | Retention, loop, tracking |

## Final gate review

| Gate | Trạng thái | Bằng chứng |
|---|:---:|---|
| Core Action | ✓ | Actor/object/completion và 5/5 tiêu chí, mục 01 |
| Cadence | ✓ | Event-based theo buổi học, mục 02 |
| Metric & Retention | ✓ | Công thức mục 03 và 6 thành phần mục 04 |
| Loop | ✓ | Progress loop qua hai chu kỳ và metric hypothesis, mục 05 |
| Tracking | ✓ | 7 events, 4 acceptance criteria, bảng truy vết |

**Trạng thái:** READY TO SUBMIT. Các ngưỡng số chi tiết và guardrail chốt sau pilot 2 tuần đầu cùng nhóm Ca Khô.
