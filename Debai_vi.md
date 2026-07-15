# Bài tập nhóm — "Kiến trúc hóa Agent"

> **Tài liệu đi kèm:** `agentic-turn-seminar.html` + `agentic-turn-speaker-notes.md` · Học phần **CTT526 – Kiến trúc phần mềm**
> **Hình thức:** xưởng thiết kế trên lớp, thực hiện **ngay sau** seminar · **~45–60 phút** · nhóm **4–5 người**
> **Câu giới thiệu ngắn cho lớp:** *"Các em vừa nghe phần lý thuyết. Bây giờ nhóm của em chính là nhóm kiến trúc — hãy lấy một hệ thống bình thường, biến nó thành agentic, rồi bảo vệ các ranh giới thiết kế trước một nhóm đối thủ đang cố phá vỡ nó."*

---

## Vì sao có bài tập này

Bài tập buộc sinh viên phải *sử dụng* bốn góc nhìn trong bài seminar, thay vì chỉ nhận diện chúng:

1. **Một phong cách mới** → chọn các mẫu thiết kế agent.
2. **Một middleware mới** → quyết định agent sẽ kết nối với gì (MCP / công cụ & dữ liệu).
3. **Các thuộc tính chất lượng** → nêu cách giới hạn tính phi tất định.
4. **Vai trò của kiến trúc sư** → thiết kế guardrail và các cổng human-in-the-loop.

Vòng **red-team** cuối bài biến thông điệp chính của seminar thành trải nghiệm thực tế: phần bền vững của một thiết kế là các *ranh giới*, và cách để biết một ranh giới có đứng vững hay không là để người khác tấn công nó.

### Mục tiêu học tập

Khi kết thúc, mỗi sinh viên có thể:
- Ánh xạ một hệ thống thực tế vào các mẫu agent và biện minh cho lựa chọn đó.
- Xác định nơi tính phi tất định tạo ra rủi ro kiến trúc và đề xuất cách giới hạn rủi ro đó.
- Đặt các cổng human-in-the-loop tại những hành động *không thể đảo ngược / có phạm vi ảnh hưởng lớn*, và tránh đặt cổng ở nơi gây lãng phí.
- Phê bình kiến trúc của nhóm khác bằng từ vựng về thuộc tính chất lượng và guardrail.

---

## Tổ chức thực hiện

|                       |                                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Quy mô nhóm**       | 4–5 người (phân vai bên dưới, hoặc để nhóm tự tổ chức)                                                             |
| **Số lượng nhóm**     | Bất kỳ; bài tập có thể mở rộng — xem phần *Mở rộng quy mô* ở cuối                                                  |
| **Thời lượng**        | Tối thiểu 45 phút, thoải mái hơn nếu có 60 phút                                                                    |
| **Tài liệu/vật dụng** | Mỗi nhóm một bản in **Architecture Canvas** (mẫu bên dưới), hoặc một slide/bảng trắng dùng chung; bút lông hoặc laptop |
| **Sản phẩm nộp**      | Một canvas đã điền + phần trình bày 3 phút                                                                         |

**Vai trò tùy chọn trong nhóm** (gắn từ vựng với từng người):
- **Kiến trúc sư trưởng** — chịu trách nhiệm canvas, đưa ra quyết định cuối cùng về trade-off.
- **Tích hợp** — phụ trách cột công cụ/dữ liệu (MCP).
- **Chất lượng & đánh giá** — phụ trách tính phi tất định và kiểm thử.
- **An toàn / Guardrails** — phụ trách các cổng human-in-the-loop và giới hạn.
- **Người hoài nghi (thành viên thứ 5)** — tấn công trước thiết kế của chính nhóm mình trước vòng red-team.

---

## Lịch trình cho người điều phối (phiên bản 45 phút)

| Thời gian | Giai đoạn                | Hoạt động                                                                                                 |
| --------- | ------------------------ | ---------------------------------------------------------------------------------------------------------- |
| 0–5 phút  | **Giới thiệu & chia nhóm** | Phát một kịch bản cho mỗi nhóm + một canvas trống. Đọc to nhiệm vụ.                                       |
| 5–25 phút | **Design sprint**        | Các nhóm điền canvas. Người điều phối đi quanh lớp; gợi ý bằng các câu hỏi trong phần *Gợi ý cho người điều phối*. |
| 25–30 phút | **Chuẩn bị trình bày**  | Mỗi nhóm cô đọng thành phần trình bày 3 phút: hệ thống, mẫu thiết kế, guardrail, "cái gì hỏng trước".      |
| 30–40 phút | **Trình bày + red team** | Các nhóm trình bày theo cặp; nhóm ghép cặp dùng **checklist red-team** và hỏi một câu tấn công.            |
| 40–45 phút | **Tổng kết**            | Thảo luận toàn lớp bằng các câu hỏi tổng kết.                                                              |

*(Phiên bản 60 phút: cho design sprint 30 phút và để mọi nhóm trình bày, không chỉ trình bày theo cặp.)*

---

## Nhiệm vụ (đọc phần này cho lớp)

> "Nhóm của em sở hữu một hệ thống hiện nay hoàn toàn do con người vận hành. Ban lãnh đạo muốn làm cho hệ thống đó trở nên **agentic** — một AI agent thực hiện các công việc thường lệ. Việc của nhóm không phải là xây dựng nó; việc của nhóm là **kiến trúc hóa** nó một cách có trách nhiệm trên một trang giấy.
>
> Hãy điền Architecture Canvas. Các em có 20 phút. Sau đó nhóm sẽ trình bày trong 3 phút, và một nhóm đối thủ sẽ cố tìm ra ranh giới mà nhóm em đã bỏ sót. Hãy thiết kế với giả định rằng agent của em là một *tác nhân có năng lực nhưng không đáng tin hoàn toàn* — vì đúng là như vậy."

---

## Kịch bản (phân một kịch bản cho mỗi nhóm)

Mỗi kịch bản cố ý chứa ít nhất một **hành động không thể đảo ngược hoặc có rủi ro cao** để phần suy nghĩ về guardrail trở nên thực chất. Có thể dùng cùng một kịch bản cho hai nhóm nếu muốn có đối đầu trực tiếp khi trình bày.

1. **Trợ lý đăng ký học phần (đại học).** Giúp sinh viên chọn môn, xử lý trùng lịch, và *nộp đăng ký học phần*. Rủi ro: hạn đăng ký, quy định tiên quyết, số chỗ giới hạn.
2. **Trợ lý phân loại ban đầu tại phòng khám (y tế).** Đọc triệu chứng và tiền sử bệnh do bệnh nhân mô tả, đề xuất mức độ khẩn cấp và bước tiếp theo. Rủi ro: an toàn, phân loại sai, quyền riêng tư.
3. **Agent hoàn tiền & hỗ trợ (thương mại điện tử).** Trả lời khách hàng và *phát hành hoàn tiền* đến một mức tiền nhất định. Rủi ro: tiền thất thoát, gian lận, giọng điệu giao tiếp.
4. **Agent ứng phó sự cố (DevOps).** Theo dõi cảnh báo, chẩn đoán sự cố, và *có thể khởi động lại dịch vụ hoặc rollback một bản triển khai*. Rủi ro: downtime production, lỗi lan truyền.
5. **Agent tiền thẩm định khoản vay / gian lận (ngân hàng).** Sàng lọc hồ sơ hoặc gắn cờ giao dịch đáng ngờ cho nhân viên phụ trách. Rủi ro: tiền, thiên lệch/công bằng, false positive.
6. **Trợ lý nghiên cứu/thư viện (lĩnh vực của nhóm).** Tìm kiếm tài liệu nội bộ và soạn tóm tắt tổng quan tài liệu với trích dẫn. Rủi ro: nguồn bịa đặt, đạo văn.

*(Có thể thay bằng một kịch bản từ chính đồ án học kỳ của sinh viên — điều đó làm phần tổng kết tác động mạnh hơn.)*

---

## Architecture Canvas (một trang cho mỗi nhóm)

> Các nhóm sao chép mẫu này và điền đầy đủ từng ô. Giữ câu trả lời ở dạng gạch đầu dòng — đây là kiến trúc phác thảo trên giấy, không phải một tài liệu dài.

```text
┌────────────────────────────────────────────────────────────────────┐
│  NHÓM: __________________     KỊCH BẢN: ___________________________ │
├────────────────────────────────────────────────────────────────────┤
│ 1. HỆ THỐNG & MỤC TIÊU                                              │
│    Agent làm gì, và thế nào được xem là "hoàn thành":               │
│    ________________________________________________________________ │
│                                                                      │
│ 2. MẪU AGENT  (chọn 2–4 mẫu, nêu LÝ DO cho từng mẫu)                 │
│    ☐ Reflection  ☐ ReAct  ☐ Plan-and-Execute  ☐ Tool Use            │
│    ☐ Multi-Agent ☐ Memory ☐ Human-in-the-Loop                       │
│    Vì sao chọn: __________________________________________________  │
│                                                                      │
│ 3. CÔNG CỤ & DỮ LIỆU  (lớp MCP — agent kết nối với gì)              │
│    Công cụ/API agent gọi: ________________________________________  │
│    Dữ liệu agent đọc/ghi: ________________________________________  │
│    Phần nào READ-ONLY và phần nào WRITE: _________________________  │
│                                                                      │
│ 4. GIỚI HẠN TÍNH PHI TẤT ĐỊNH  (agent đôi khi sẽ sai)               │
│    Cách kiểm thử (semantic? rollout? ngưỡng?): ___________________  │
│    Những gì được cố định / giới hạn (phiên bản, temperature): _____ │
│    Thuộc tính chất lượng mới quan trọng nhất: ____________________  │
│                                                                      │
│ 5. GUARDRAILS & HUMAN-IN-THE-LOOP                                    │
│    Hành động agent được làm KHÔNG CẦN GIÁM SÁT: __________________  │
│    Hành động BẮT BUỘC cần con người bấm xác nhận: ________________  │
│    Giới hạn cứng (phạm vi, trần chi phí, quyền): _________________  │
│                                                                      │
│ 6. CHẾ ĐỘ LỖI                                                        │
│    "Cái gì hỏng TRƯỚC, và ai bị ảnh hưởng?": _____________________  │
│                                                                      │
│ 7. PHẦN BỀN VỮNG                                                     │
│    Một câu: nếu ngày mai toàn bộ mã agent bị bỏ đi,                  │
│    ranh giới nào trong thiết kế này phải được giữ lại? ___________  │
└────────────────────────────────────────────────────────────────────┘
```

Các ô **5, 6, 7** là những ô nối trực tiếp với luận điểm chính của seminar — hãy đánh giá cao các nhóm xử lý nghiêm túc những ô này, không chỉ chọn mẫu ở ô 2.

---

## Phần trình bày 3 phút (mỗi nhóm trình bày gì)

Giữ phần trình bày có kỷ luật — một slide canvas trên màn hình hoặc giơ tờ giấy lên:

1. **Hệ thống** trong một câu. *(20 giây)*
2. **Mẫu thiết kế & lý do** — hai hoặc ba mẫu định hình thiết kế. *(40 giây)*
3. **Hành động nguy hiểm** và **guardrail** được đặt quanh hành động đó. *(60 giây)*
4. **Cái gì hỏng trước** — nêu lỗi khiến nhóm lo nhất. *(40 giây)*
5. **Ranh giới bền vững** — một dòng từ ô 7. *(20 giây)*

---

## Vòng red-team (phần tạo hiệu quả chính)

Sau mỗi phần trình bày, **nhóm đối thủ được ghép cặp** có 90 giây để tấn công bằng checklist này. Họ phải đưa ra **một** câu hỏi cụ thể để nhóm trình bày trả lời ngay tại chỗ.

**Checklist red-team — cố tìm ranh giới bị thiếu:**
- [ ] Nêu một **hành động không thể đảo ngược** mà agent có thể làm khi không có cổng con người.
- [ ] Mô tả một đầu vào khiến agent **tự tin nhưng sai** — chuyện gì xảy ra tiếp theo?
- [ ] Tìm một công cụ/quyền đáng lẽ phải là **read-only** nhưng lại không phải.
- [ ] Hỏi: *làm sao nhóm biết được* nó đang lỗi trong production? (observability)
- [ ] Chỉ ra một guardrail chỉ mang tính **hình thức** — tạo ma sát nhưng không bảo vệ thật.
- [ ] Nếu phiên bản model thay đổi qua đêm, **cái gì sẽ âm thầm hỏng?**

**Luật cho nhóm trình bày:** nhóm có thể sửa thiết kế *bằng lời nói* — "bắt được hay, chỗ đó nhóm sẽ thêm một cổng xác nhận." Cải thiện khi bị tấn công mới là mục tiêu, không phải bảo vệ một câu trả lời hoàn hảo. Có thể thưởng điểm nhỏ cho câu tấn công sắc nhất, không chỉ cho thiết kế tốt nhất — điều đó khuyến khích tư duy guardrail mà seminar đã lập luận.

---

## Tổng kết (5 phút, toàn lớp)

Chọn 2–3 câu hỏi sau:

1. **Hành động nguy hiểm của mọi nhóm đều cần cổng con người — có nhóm nào đặt cổng cho thứ *không cần* đặt cổng không?** (Làm nổi bật nguyên tắc "đặt cổng cho thứ không thể đảo ngược, giải phóng thứ có thể đảo ngược" từ slide 9.)
2. **"Cái gì hỏng trước" của nhóm nào đáng sợ nhất — và đó là lỗi code hay khoảng trống kiến trúc?** (Gần như luôn là vế sau — đó là luận điểm chính.)
3. **Có hai nhóm nào cùng kịch bản nhưng vẽ ranh giới khác nhau không? Ai đúng?** (Hiếm khi chỉ có một đáp án — kiến trúc là trade-off.)
4. **Cả lớp điền vào chỗ trống:** *"Khi mã của agent có thể bị thay thế, thứ đáng tranh luận trong buổi review là ______."* (Hướng về: các ranh giới ở ô 7.)

Kết thúc bằng cách chỉ lại bảng ánh xạ trong seminar: *"Các em vừa làm design patterns, middleware, quality attributes, và governance — toàn bộ học phần — cho một loại hệ thống chưa tồn tại khi sách giáo khoa được viết."*

---

## Rubric đánh giá (tùy chọn, nếu có chấm điểm)

| Tiêu chí                 | 0 — thiếu                 | 1 — có mặt                      | 2 — mạnh                                                   |
| ------------------------ | ------------------------- | -------------------------------- | ---------------------------------------------------------- |
| **Độ phù hợp của mẫu**   | Chọn mẫu ngẫu nhiên       | Mẫu tương đối hợp lý             | Mẫu được biện minh rõ bằng nhiệm vụ                        |
| **Độ rõ của tích hợp**   | Công cụ/dữ liệu mơ hồ     | Nêu được công cụ & đọc/ghi       | Lập luận least-privilege (mặc định read-only)              |
| **Kế hoạch xử lý phi tất định** | Bỏ qua             | Có ý tưởng kiểm thử/giới hạn     | Chấp nhận theo thống kê + cố định phiên bản + nêu QA       |
| **Guardrails**           | Không có, hoặc chỉ hình thức | Có cổng con người cho hành động rủi ro | Chỉ đặt cổng ở nơi không thể đảo ngược; có giới hạn cứng |
| **Nhận diện lỗi**        | "Nó sẽ không lỗi"         | Nêu một lỗi hợp lý               | Truy lỗi về một ranh giới cụ thể + ai bị ảnh hưởng         |
| **Hiệu quả red-team**    | Im lặng                   | Một câu tấn công hợp lệ          | Câu tấn công phơi bày một ranh giới thật sự bị thiếu       |

Tổng /12. Bài tập thưởng cho *phán đoán về ranh giới* hơn là độ bóng bẩy kỹ thuật — đúng với kỹ năng mà seminar cho rằng sẽ trở nên khan hiếm và có giá trị.

---

## Gợi ý cho người điều phối (để gỡ bí cho các nhóm trong sprint)

- Bí khi chọn mẫu? *"Agent có cần kiểm tra lại công việc của chính nó không? Đó là Reflection. Agent có cần dữ liệu trực tiếp không? Đó là Tool Use."*
- Nói chung chung về chất lượng? *"Cùng một yêu cầu chạy hai lần và cho hai câu trả lời khác nhau — ở đây có chấp nhận được không? Nếu không, em xử lý thế nào?"*
- Đặt cổng quá nhiều? *"Hành động nào trong số này luôn có thể hoàn tác? Những hành động đó có lẽ không cần con người xác nhận."*
- Đặt cổng quá ít? *"Chỉ vào hành động tiêu tiền hoặc không thể thu hồi. Ai phê duyệt nó?"*
- Ô 7 trống? *"Nếu tối nay ta xóa toàn bộ mã agent và xây lại, quy tắc nào phiên bản mới vẫn phải tuân theo?"*

---

## Mở rộng quy mô & biến thể

- **Lớp đông / ít thời gian:** bỏ phần trình bày trực tiếp; làm **gallery walk** — dán canvas lên tường, mỗi nhóm để lại một sticky note red-team cho nhóm bên cạnh, rồi tổng kết 5 phút.
- **Hai kịch bản đối đầu:** giao *cùng một* kịch bản cho hai nhóm và để cả lớp bình chọn ranh giới của nhóm nào đáng tin hơn với tiền/dữ liệu của chính họ.
- **Bài về nhà / cá nhân:** canvas có thể dùng như bài viết một trang cá nhân; bỏ vòng red-team và yêu cầu thêm một đoạn viết "đối thủ sẽ tấn công thiết kế này như thế nào".
- **Lab tiếp nối:** canvas mạnh nhất trở thành đặc tả cho một bài build nhỏ sau này trong học phần — kiến trúc là artifact bền vững, code đến sau.

---

### Checklist in nhanh trước giờ học

- [ ] In **N ÷ 2** … thật ra là **N** bản Architecture Canvas (mỗi nhóm một bản) + vài bản dự phòng.
- [ ] Quyết định cách phân kịch bản (hoặc để nhóm bốc thăm ngẫu nhiên).
- [ ] Ghép cặp trước cho vòng red-team (Nhóm 1↔2, 3↔4, …).
- [ ] Đưa **checklist red-team** lên một slide hoặc bảng để nhóm tấn công nhìn thấy.
- [ ] Giữ sẵn slide 9 (guardrails) và slide 11 (luận điểm chính) của seminar — phần tổng kết sẽ cần chỉ lại chúng.
