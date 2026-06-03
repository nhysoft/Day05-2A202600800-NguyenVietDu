# Workshop — Mổ App AI Thật

**Thời gian:** 35-45 phút  
**Hình thức:** cá nhân  
**Output:** finding note + sketch `as-is / to-be`

---

## 1. Sản phẩm được chọn

**Vietnam Airlines — NEO**  
AI feature: Chatbot hỗ trợ tra cứu chuyến bay, vé, hành lý, khiếu nại  
Cách truy cập: Website vietnamairlines.com / Zalo OA Vietnam Airlines

---

## 2. Dùng thử: promise vs reality

**Product hứa gì?**  
NEO được giới thiệu là trợ lý ảo thông minh, hỗ trợ 24/7, có thể giải đáp mọi thắc mắc về đặt vé, tra cứu chuyến bay, hành lý, thủ tục, đổi/hoàn vé và tiếp nhận khiếu nại — không cần chờ tổng đài.

**User nào được hứa sẽ được giúp?**  
Hành khách cần tra cứu thông tin chuyến bay hoặc hỗ trợ nhanh, đặc biệt ngoài giờ hành chính khi hotline đông.

**Kỳ vọng AI làm được task nào?**  
Hỏi: "Tìm tôi chuyến bay hà nội về phú yên"  
Hỏi: "Tìm tôi chuyến bay gần nhất ngày hôm nay"  
Hỏi: "Giá chuyến bay nếu muốn bay ngay lúc này về phú yên"  
Hỏi: "Cho tôi gặp người trực tiếp bán vé"

**Điểm gãy quan sát được:**

| Lần thử | Input thực tế | Kết quả thực tế | Điểm gãy |
|---|---|---|---|
| 1 | "Tìm tôi chuyến bay hà nội về phú yên" | Trả về đúng: 3 chuyến/ngày, 1h40p, 1.260km, sân bay khởi hành/đến, kèm link lịch bay | Happy path hoạt động tốt với câu hỏi thông tin cố định |
| 2 | "Tìm tôi chuyến bay gần nhất ngày hôm nay" | Không trả được lịch realtime — yêu cầu vào website hoặc gọi hotline; đưa 2 quick reply (Mã đặt chỗ / Số hiệu chuyến bay) không liên quan | Không có dữ liệu realtime, nhưng không giải thích giới hạn này — user không biết tại sao bot không trả lời thẳng |
| 3 | "Giá chuyến bay nếu muốn bay ngay lúc này về phú yên" | NEO thu thập đúng slots (một chiều, phổ thông, HAN→TBB, 2026-06-03), nhưng flag "ngày khởi hành đã qua" và hỏi thêm số lượng hành khách — không trả được giá | Biết slot filling nhưng không phân loại được "bay ngay lúc này" = hôm nay = ngày hiện tại; hỏi thêm thông tin thừa thay vì báo không hỗ trợ tìm chuyến realtime |
| 4 | "Cho tôi gặp người trực tiếp bán vé" | Chờ ngắn → nhân viên hỗ trợ kết nối, hỏi thêm thông tin cần giúp | Escalation hoạt động, nhưng không có phân loại case trước đó — mọi yêu cầu đều được đẩy thẳng sang người thật, kể cả các case AI có thể tự xử lý |

**Evidence kèm theo:**
- Screenshot 1 (17.25.59): NEO trả lời đúng thông tin tuyến HAN–TBB → happy path rõ ràng
- Screenshot 2 (17.26.45): NEO không trả được chuyến gần nhất hôm nay, đẩy sang website + hotline
- Screenshot 3 (17.27.45): NEO thu slots nhưng flag lỗi ngày và hỏi thừa số hành khách
- Screenshot 4 (17.28.35): Escalation sang nhân viên thật hoạt động, nhưng không có pre-screening

---

## 3. Bốn paths

| Path | Quan sát trong NEO |
|---|---|
| **Happy** | User hỏi thông tin cố định (tuyến bay, giờ bay, hành lý tiêu chuẩn): NEO trả lời nhanh, đúng, có link hành động. |
| **Low-confidence** | **Không tồn tại rõ ràng.** Khi NEO không có dữ liệu realtime (chuyến gần nhất hôm nay, giá bay ngay lúc này), NEO không thông báo giới hạn của mình — chỉ chuyển sang hotline hoặc trả lời lệch chủ đề mà không giải thích lý do. |
| **Failure** | Khi AI không xử lý được intent (ví dụ: slot "ngay lúc này" bị hiểu sai thành ngày cụ thể đã qua), user không biết. Không có cảnh báo "thông tin này nằm ngoài khả năng hỗ trợ của tôi lúc này". |
| **Correction** | Không có. User không thể sửa output hoặc nói "Câu đó sai rồi". Mỗi lần sửa = bắt đầu lại từ đầu. Không có feedback loop nào được ghi lại để cải thiện. |

---

## 4. Finding thành quyết định

**Finding 1 — Thiếu low-confidence path:**

```
Khi user hỏi "chuyến bay gần nhất hôm nay" hoặc "giá bay ngay lúc này",
NEO không có dữ liệu realtime nhưng không nói rõ giới hạn đó,
hậu quả là user không hiểu tại sao không được trả lời, phải tự tìm sang kênh khác.
Lỗi thuộc layer: Intent (thiếu giới hạn năng lực) + UX Recovery (không cảnh báo rõ).
Nên sửa bằng: khi nhận intent realtime, NEO trả lời "Tôi không có dữ liệu chuyến bay
theo giờ thực — bạn có thể kiểm tra tại [link] hoặc gọi 1900 1100" thay vì đưa quick reply không liên quan.
```

**Finding 2 — Slot filling hiểu sai context thời gian:**

```
Khi user nói "bay ngay lúc này", NEO dịch thành ngày hôm nay (đúng)
nhưng sau đó flag "ngày đã qua" và yêu cầu nhập lại,
hậu quả là user bị vòng lặp nhập liệu vô ích vì NEO không hỗ trợ tìm chuyến realtime.
Lỗi thuộc layer: Slot extraction (hiểu sai scope thời gian tương đối) + Intent classification (không phân biệt được "xem lịch cố định" vs "tìm chuyến ngay hôm nay").
Nên sửa bằng: phát hiện intent "realtime/ngay lập tức" sớm và báo giới hạn trước khi hỏi slot.
```

**Finding 3 — Escalation không có pre-screening:**

```
Khi user yêu cầu gặp người thật,
NEO chuyển thẳng sang nhân viên mà không hỏi lý do hoặc phân loại case,
hậu quả là nhân viên nhận case không có context, phải hỏi lại từ đầu — thêm thời gian chờ.
Lỗi thuộc layer: Human escalation logic + Context handoff.
Nên sửa bằng: trước khi kết nối, hỏi 1 câu "Bạn cần hỗ trợ về vấn đề gì?" và truyền thông tin này cho nhân viên khi kết nối.
```

---

## 5. Sketch as-is / to-be

**As-is — Flow hiện tại (điểm gãy đánh dấu ⚠):**

```
User gõ câu hỏi
    └─> NEO nhận input
            └─> Match keyword/intent
                    ├─> [Match tốt, dữ liệu cố định] → Trả lời đúng → END ✓
                    ├─> [Match yếu hoặc cần realtime] → Đưa quick reply không liên quan ⚠ → User bối rối
                    ├─> [Slot sai/thiếu] → Hỏi lại slot nhưng không báo giới hạn ⚠ → Vòng lặp vô ích
                    └─> [User yêu cầu người thật] → Chuyển thẳng sang nhân viên ⚠ → Không có context handoff

Không có:
- Thông báo rõ ràng khi NEO không đủ năng lực xử lý intent ⚠
- Phân loại intent "realtime" vs "thông tin cố định" sớm trong flow ⚠
- Pre-screening trước khi escalate ⚠
- Feedback từ user ⚠
```

**To-be — Flow đề xuất (path đã sửa):**

```
User gõ câu hỏi
    └─> NEO nhận input
            └─> Intent classification (static info vs realtime vs escalate)
                    ├─> [Static info, confident] → Trả lời + "Thông tin này có hữu ích không?" ✓
                    ├─> [Realtime intent] → "Tôi không có dữ liệu theo giờ thực.
                    │                        Kiểm tra tại [link] hoặc gọi 1900 1100." ✓
                    ├─> [Thiếu slot, có thể tự xử lý] → Hỏi lại 1 câu → Trả lời sau khi đủ context ✓
                    └─> [Cần escalate] → Hỏi "Bạn cần hỗ trợ về vấn đề gì?"
                                         → Truyền context cho nhân viên khi kết nối ✓

Bổ sung:
- Thông báo giới hạn khi NEO gặp intent nằm ngoài năng lực ✓
- Feedback loop sau mỗi response ✓
- Context handoff khi chuyển sang nhân viên ✓
```

---

## 6. Finding này đổi gì trong SPEC nhóm

Low-confidence path và context handoff khi escalate là hai điểm gãy nguy hiểm nhất quan sát được. Prototype trong Day 06 phải ưu tiên: (1) phân loại sớm intent nằm ngoài năng lực AI và thông báo rõ ràng thay vì trả lời vòng vo; (2) thu thập context tối thiểu trước khi chuyển sang người thật để tránh user phải giải thích lại từ đầu.

---

## 7. Checklist tự kiểm

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể (4 screenshot thực tế từ NEO).
- [x] Có đủ 4 paths — nói rõ Low-confidence và Correction đang thiếu trong product.
- [x] Finding được viết thành product decision, không chỉ là nhận xét.
- [x] Sketch có as-is và to-be.
- [x] Có câu nói rõ finding này sẽ đổi gì trong SPEC.
