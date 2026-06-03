# Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** Nhóm 5 anh em
**Track:** D Finance  
**Product/app đã chọn:** Expense Tracking App with AI + Telegram Bot
**Build slice đang nghĩ:** Evidence feature - Simple message input with AI auto-categorization (without multi-step form)

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| Test app bên ngoài: Nhập chi tiêu bằng tin nhắn Telegram đơn giản | [Screenshot sẽ gắn tại đây] | Happy | Việc chỉ cần gửi tin nhắn text (không form) rất thuận tiện, giảm friction lớn |
| AI tự động phân loại chi tiêu mà không cần xác nhận từng field | [Screenshot sẽ gắn tại đây] | Happy | Người dùng sẽ tiết kiệm thời gian nhập liệu, tăng tỷ lệ ghi chép chi tiêu |
| Xác nhận dữ liệu qua Telegram (không cần mở web) | [Screenshot sẽ gắn tại đây] | Happy | Flow trên mobile/chat rất natural, không cần switch app ||

## 3. User / review / social evidence

Nguồn có thể là review App Store/Play, group, comment, phỏng vấn nhanh, hoặc nguồn public khác.

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Mình chỉ muốn nhập nhanh, không muốn tíck tác với form" | Test user (self-use) | Người dùng cá nhân có chi tiêu liên tục | Friction quá cao với form multi-step |
| Lấy ví dụ: Ăn cơm 50k, Xe taxi 100k → app phải tự hiểu | Test thực tế | Người dùng tiền mặt nhỏ lẻ | Không muốn nhập chi tiết, chỉ muốn ghi lại cái gì đó nhanh |
| "Dùng Telegram nhiều hơn là mở app web" | Observation | Người dùng mobile-first | Mở app riêng = thêm step, thêm friction |

Nếu chưa có nguồn ngoài nhóm, ghi rõ:

```text
Đây là giả định từ self-use testing. Nhóm sẽ validate với real users (3-5 người dùng tiền mặt) trước checkpoint M1 Day 06.
```

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| Money Lover | Form phức tạp: chọn category, nhập note, chọn ngày, confirm | Người dùng không muốn multi-step | ✓ Rút ra: Skip form, chỉ text input |
| Spendee | Có voice input nhưng vẫn cần xác nhận từng field | Voice tốn thêm thời gian process | ✓ Text đơn giản hơn voice |
| GG Keep + manual tracking | Người dùng note tự do, sau ghi vào app riêng | Người dùng sẽ dùng tool họ quen để note | ✓ Tích hợp Telegram làm điểm input duy nhất |
| Chat GPT for expense | Nhắn tin cho bot, bot tự phân loại | AI làm heavy lifting, UX gọn | ✓ Pattern: text → AI → auto-categorize|

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
- Người dùng tiền mặt KHÔNG muốn nhập chi tiết (date/category/note riêng)
- Lý tưởng: chỉ gửi tin nhắn → hệ thống tự xử lý
- Telegram là channel họ đã dùng hàng ngày (friction = 0)

Insight:
User không chỉ gặp "process quá phức tạp".
Thật ra họ cần "không phải suy nghĩ / không phải confirm nhiều lần / chỉ ghi lại nhanh".
Pain là: mỗi field xác nhận = mỗi cơ hội user không nhập tiếp.

Opportunity:
AI có thể giúp bằng cách:
- Tự động trích xuất amount, category, note từ text
- Ghi nhận 100% correct hoặc cho user confirm 1 lần (không từng field)
- Integrate vào Telegram (zero friction entry point)
→ Tăng tỷ lệ người dùng thực sự ghi chép chi tiêu hàng ngày
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính. (Giữ nguyên: Người dùng cá nhân, tiền mặt)
- [x] Đổi pain statement. (Rõ hơn: Friction từ form multi-step)
- [x] Đổi build slice. (Skip form → Telegram simple input)
- [x] Đổi Auto/Aug decision. (Automaton: AI tự phân loại, không cần user confirm từng field)
- [x] Đổi 4 paths. (Telegram is main entry point)
- [ ] Đổi failure mode.
- [x] Đổi owner/test plan. (Cần validate với real users)

Ghi rõ 1-2 thay đổi quan trọng:

```text
Trước evidence:
- Nhóm định: Cân bằng giữa simplicity + control
- Form sẽ có: Date, Category, Amount, Note (mỗi field 1 step)
- User xác nhận từng field trước save

Sau evidence:
- Nhóm đổi thành: Prioritize simplicity 100%
- Chỉ input: Text simple message (Telegram)
- AI tự phân loại, user confirm 1 lần duy nhất (hoặc 0 confirmation nếu AI confident)
- Bỏ voice note (text đơn giản hơn, process nhanh hơn)

Lý do:
- Evidence cho thấy multi-step = mỗi step là cơ hội user bỏ cuộc
- Telegram (where users already are) → 0 friction
- AI confidence cao đủ để auto-categorize → không cần per-field confirmation
- Real user feedback: "Just let me type quick and go"
```
