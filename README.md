# Day 21 Lab — Thiết kế Test Inputs cho AI Evals

Tài liệu này tóm tắt toàn bộ yêu cầu bài Lab Day 21: **Thiết kế Test Inputs cho AI Evals**.

---

## 🎯 Mục tiêu bài Lab
Mỗi học viên tự thiết kế một bộ **test inputs** có coverage (độ phủ) rõ ràng cho use case đã làm ở Day 18/19. Sau đó, nhóm sẽ gom lại, kiểm tra coverage và chốt một **Scenario Dataset v1** chung để phục vụ cho các bước chạy agent và đọc trace về sau.

---

## 📦 Quy trình nộp bài & Cấu trúc Report

### 1. Quy trình nộp bài (2 Pha)
1. **Cá nhân:** Mỗi học viên tự tạo một **Scenario Dataset v0** cá nhân.
2. **Nhóm:** Các thành viên trong nhóm gom các dataset cá nhân lại, đánh giá coverage và chốt một **Scenario Dataset v1** chung.

Nhóm nộp **một liên kết duy nhất** tới report hoặc workspace đã cấp quyền xem.

### 2. Cấu trúc Report bắt buộc
* Phần cá nhân của từng thành viên.
* Phần group merge (quá trình gom và chuẩn hóa).
* **Scenario Dataset v1** cuối cùng.
* Coverage review và các known gaps (khoảng trống kiểm thử đã biết).

*Lưu ý:* 
* Tên thư mục/repository (nếu cần tạo riêng): `Day21-MãHV-Họ Và Tên`.
* **Không** đưa API key, token hoặc credential vào bài nộp.

---

## 🔍 Bài Lab này thực sự đánh giá điều gì?

### Bài Lab này CHƯA yêu cầu:
* Chạy agent.
* Đọc trace.
* Viết trace codes.
* Tạo reference dataset.
* Build automated evaluator.
* Viết LLM judge.
* Tối ưu prompt để đạt score cao.

### Bài Lab này ĐÁNH GIÁ năng lực của Product Manager (PM) trong việc:
1. Chọn đúng lát cắt use case cần đánh giá.
2. Biến câu hỏi chất lượng mơ hồ thành câu hỏi chất lượng cụ thể (**Quality Question**).
3. Thiết kế **User Input Grid** để kiểm soát coverage.
4. Chọn các tổ hợp input đáng test, không tổ hợp máy móc.
5. Dùng AI để sinh natural-language inputs nhưng không giao quyền chọn coverage cho AI.
6. Lọc lại inputs do AI sinh ra (loại bỏ case generic, sai intent hoặc mất ambiguity).
7. Gom bộ dữ liệu cá nhân thành **Scenario Dataset v1** có độ phủ tốt hơn.

### 🔄 Logic thực hiện bài Lab:
```
Use case từ Day 18/19
      ↓
Quality Question
      ↓
User Input Grid
      ↓
Meaningful Combinations
      ↓
AI-assisted Input Generation
      ↓
Human Filtering
      ↓
Individual Scenario Dataset v0
      ↓
Group Coverage Review
      ↓
Group Scenario Dataset v1
```
> **[!IMPORTANT]**
> **Điểm cốt lõi:** Con người (Human) quyết định coverage. AI chỉ đóng vai trò hỗ trợ viết nhiều cách diễn đạt tự nhiên hơn.

---

## 🛠️ Hướng dẫn chi tiết từng bước thực hiện

### Bài 1 — Chọn Use Case và Quality Question

#### 1. Chọn Unit of AI Work
* **Định nghĩa:** Là đơn vị nhỏ nhất mà team muốn đánh giá (một lần AI nhận input và tạo ra output/action có thể review được). Không chọn toàn bộ sản phẩm.
* **Ví dụ:**
  * *Customer Support:* Một tin nhắn của khách → agent phân loại intent, hỏi thêm hoặc đề xuất hướng xử lý.
  * *Travel Planner:* Một yêu cầu du lịch → agent tạo lịch trình hoặc hỏi thêm constraint.
  * *Student Assistant:* Một yêu cầu học tập → agent giải thích, lên kế hoạch hoặc tạo bài tập nhỏ.
  * *MVP của nhóm:* Một request cụ thể → agent trả lời, phân loại, trích xuất hoặc đề xuất action.
* **Bảng cần hoàn thành:**
  | Thành phần | Câu trả lời |
  | :--- | :--- |
  | Use case từ Day 18/19 | |
  | Persona chính | |
  | Unit of AI Work | |
  | Input user đưa vào | |
  | Output agent cần tạo | |
  | Agent được phép làm gì? | |
  | Agent không được phép làm gì? | |

#### 2. Viết Quality Question
* **Nguyên tắc:** Không hỏi chung chung như *"Agent có tốt không?"*. Hãy hỏi cụ thể: *"Trong lát cắt này, behavior nào nếu sai sẽ làm user mất trust hoặc không hoàn thành task?"*.
* **Bảng cần hoàn thành:**
  | Câu hỏi | Câu trả lời |
  | :--- | :--- |
  | Quality question chính | |
  | Vì sao câu hỏi này quan trọng với user? | |
  | Nếu agent fail ở đây, hậu quả là gì? | |
  | Behavior nào là bắt buộc? | |
  | Behavior nào bị cấm? | |

---

### Bài 2 — Thiết kế User Input Grid

#### 1. Chọn ít nhất 3 dimensions chính
* **Định nghĩa:** Dimension là một biến làm expected behavior của agent thay đổi (khi đổi value của dimension này, câu trả lời hoặc hành động đúng của agent cũng phải đổi theo).
* **Ví dụ mẫu:**
  * *Customer Support:* `User intent`, `Context completeness`, `Risk level`.
  * *Travel Planner:* `Trip goal`, `Constraint`, `Planning complexity`.
  * *Student Assistant:* `Student goal`, `Context completeness`, `Support type`.
* **Template cần hoàn thành:**
  | Dimension | Values | Vì sao làm agent phải đổi behavior? |
  | :--- | :--- | :--- |
  | | | |
  | | | |
  | | | |

#### 2. Kiểm tra tính hợp lệ của Dimension
Với mỗi dimension, tự trả lời các câu hỏi:
* Nếu đổi value, expected behavior có đổi không?
* Dimension này có gắn với risk hoặc user outcome không?
* Dimension này có giúp tìm failure mà happy path không thấy không?
* Có value nào quá generic hoặc khó quan sát không?
* *Lưu ý:* Bỏ qua các dimension yếu (ví dụ: Độ dài câu hỏi, User vui/buồn không ảnh hưởng chính sách, Giao diện đẹp/xấu).

---

### Bài 3 — Chọn Meaningful Combinations

#### 1. Nguyên tắc: Không tổ hợp mọi thứ
* Nếu có 3 dimensions, mỗi dimension có 4 values $\rightarrow 4 \times 4 \times 4 = 64$ combinations. Không cần test hết.
* Mỗi học viên chọn **ít nhất 10 scenarios/combinations đáng test nhất**.
* **Tiêu chí giữ lại:** Tình huống thường gặp, dễ sai, failure cost cao, thiếu context/mơ hồ, giúp phân biệt behavior tốt/xấu, từng xuất hiện trong prototype, hoặc ranh giới đúng/sai chưa rõ.
* **Tiêu chí loại bỏ:** Vô nghĩa trong thực tế, quá giống combination khác, không làm thay đổi expected behavior, quá xa use case hiện tại.

#### 2. Bảng combinations cá nhân (Tối thiểu 10 rows)
| Combination ID | Dimension values | Expected behavior | Vì sao đáng test? | Loại (representative/challenge/high-risk) |
| :--- | :--- | :--- | :--- | :--- |
| C01 | | | | |
| C02 | | | | |
| ... | | | | |
| C10 | | | | |

---

### Bài 4 — Dùng AI Generate Natural-Language Inputs

#### 1. Nguyên tắc
* Chỉ dùng AI để **paraphrase** hoặc viết thành câu hỏi tự nhiên từ các combinations đã thiết kế.
* AI không được tự chọn: use case, quality question, dimensions, combinations, risk priority, coverage strategy.

#### 2. Prompt mẫu
```text
Bạn là người dùng thật đang nhắn cho một AI assistant.
Tôi đang thiết kế test inputs cho use case: [mô tả use case]
Quality question: [quality question]

Tôi đã chọn các combinations sau. Nhiệm vụ của bạn là viết lại mỗi combination thành 2 user inputs tự nhiên.

Yêu cầu:
- Không tự thêm combination mới.
- Không thay đổi intent, risk hoặc context completeness đã cho.
- Viết như user thật, không quá sạch.
- Có cả câu ngắn, câu dài, thiếu context hoặc hơi vòng vo.
- Không giải thích cách agent nên trả lời.
- Output dạng bảng gồm: combination_id, user_input, style, notes.

Combinations:
[dán bảng combinations ở đây]
```
*Lưu ý sau khi chạy prompt:* Lưu lại **prompt đã dùng**, **output thô của AI**, và **phiên bản đã lọc cuối cùng**.

#### 3. Human Filter (Lọc dữ liệu)
Không lấy nguyên output của AI. Tiến hành loại bỏ input nếu:
* AI đổi intent hoặc làm case quá sạch (mất đi tính thực tế/lỗi).
* AI tự thêm thông tin không có trong combination.
* Nhiều câu chỉ khác từ ngữ nhưng kiểm thử chung một behavior.
* Input không giúp trả lời quality question.

---

### Bài 5 — Tạo Individual Scenario Dataset v0

#### 1. Yêu cầu số lượng cá nhân
* Ít nhất **10 scenarios/combinations**.
* Ít nhất **20 natural-language user inputs** sau khi lọc.
* Mỗi input map rõ về combination và có expected behavior ở mức high-level.

#### 2. Schema cho Scenario Dataset v0
| Field | Ý nghĩa |
| :--- | :--- |
| `scenario_id` | ID duy nhất (ví dụ: A01, A02) |
| `owner` | Tên hoặc mã học viên |
| `use_case` | Use case đang test |
| `quality_question` | Quality question chính |
| `combination_id` | Combination gốc |
| `dimension_values` | Values của các dimensions đã chọn |
| `user_input` | Câu user thật có thể nói |
| `style` | Ngắn, dài, mơ hồ, angry, polite, mixed language... |
| `expected_behavior` | Agent nên làm gì ở mức high-level |
| `why_included` | Case này test gap/risk nào |
| `set_type` | representative/challenge/high-risk |

#### 3. Coverage note cá nhân (Viết ngắn 5-7 dòng)
* Dataset cá nhân đang cover tốt slice nào?
* Slice nào chưa cover?
* Có combination nào cố tình chưa chọn? Vì sao?
* Input nào là high-risk nhất?
* Input nào là boundary case khó nhất?

---

### Bài 6 — Group Merge và Coverage Review

#### 1. Quy trình merge nhóm
* **Step 1 - Trình bày nhanh:** Mỗi thành viên có 3 phút trình bày use case, quality question, dimensions, 2 combinations tốt nhất, 1 input high-risk, và 1 coverage gap còn thiếu.
* **Step 2 - Chuẩn hóa dimensions:** Gom các dimension tương đương của các thành viên thành ít nhất 3 dimensions chính chung.
* **Step 3 - Deduplicate inputs:** Loại bỏ các input trùng lặp hoặc quá giống nhau. Chỉ giữ lại nếu kiểm thử các style/persona khác nhau thực sự.
* **Step 4 - Kiểm tra coverage:** Tạo bảng coverage matrix để kiểm tra số lượng row cho từng slice.
* **Step 5 - Chốt Scenario Dataset v1:** Chọn **ít nhất 30 rows cuối cùng** đáp ứng:
  * Đủ representative, challenge, high-risk cases.
  * Ít nhất 2 cases ambiguous hoặc missing-context.
  * Ít nhất 2 cases có risk cao.
  * Ít nhất 2 cases dễ làm agent chọn sai action.
  * Đa dạng cách diễn đạt (ngắn, dài, thiếu context, cảm xúc, mixed language).

#### 2. Schema cho Scenario Dataset v1
| Field | Ý nghĩa |
| :--- | :--- |
| `scenario_id` | ID cuối cùng (ví dụ: G01) |
| `source_owner` | Row đến từ thành viên nào |
| `use_case` | Use case chung của nhóm |
| `quality_question` | Quality question nhóm chốt |
| `dimension_values` | Values theo dimension chuẩn hóa |
| `user_input` | Input cuối cùng sẽ dùng để chạy agent |
| `expected_behavior` | Agent nên làm gì ở mức high-level |
| `risk_if_fail` | Nếu agent fail thì hậu quả là gì |
| `why_included` | Row này bổ sung coverage gì |
| `set_type` | representative/challenge/high-risk |
| `merge_decision` | kept/merged/rewritten |

#### 3. Group Coverage Review (Trả lời ngắn các câu hỏi)
* Dataset v1 đang cover tốt những slice nào?
* Slice nào còn thiếu hoặc yếu?
* Có đang over-sample happy path không?
* Có row nào high-risk nhưng chưa đủ rõ expected behavior không?
* AI generation đã làm sai hoặc bóp méo combination ở đâu?
* Nếu chỉ được chạy agent trên một batch nhỏ đầu tiên, nhóm chọn rows nào? Vì sao?

---

### Bài 7 — Handoff cho bước chạy agent sau này
* Bài này dừng ở **Scenario Dataset v1**, chưa cần chạy agent hay đọc trace.
* **Handoff note (5-7 dòng):**
  * Khi chạy agent, nhóm muốn quan sát behavior nào đầu tiên?
  * Rows nào nên chạy trước?
  * Rows nào là critical regression candidates?
  * Dự đoán vị trí fail của agent (hiểu intent, thiếu context, policy/tool/action, hay output clarity)?
  * Tiêu chí nào có thể trở thành trace code sau này?

---

## ⏱️ Timeline gợi ý (Tổng cộng 150 phút)
| Giai đoạn | Hoạt động | Thời gian | Đầu ra |
| :---: | :--- | :---: | :--- |
| **0** | Đọc đề, chọn lại use case Day 18/19 | 10 phút | Use case + Unit of AI Work |
| **1** | Viết quality question | 15 phút | Quality question |
| **2** | Tạo User Input Grid cá nhân | 20 phút | Ít nhất 3 dimensions |
| **3** | Chọn scenarios/combinations cá nhân | 20 phút | Ít nhất 10 scenarios |
| **4** | Dùng AI generate inputs + human filter | 25 phút | 20+ inputs |
| **5** | Hoàn thiện Scenario Dataset v0 | 15 phút | Individual dataset |
| **6** | Nhóm trình bày và merge | 25 phút | Dimensions chuẩn hóa + dedup |
| **7** | Coverage review và chốt Dataset v1 | 15 phút | 30+ final rows |
| **8** | Handoff note + nộp bài | 5 phút | Final report |

---

## 📊 Rubric chấm nhanh (Thang điểm 100)
| Tiêu chí | Điểm tối đa |
| :--- | :---: |
| Quality question cụ thể, không quá rộng | 15 |
| Dimensions làm agent behavior thay đổi thật | 20 |
| Combinations có lý do chọn rõ | 20 |
| AI-generated inputs tự nhiên nhưng vẫn giữ đúng coverage | 15 |
| Scenario Dataset v0 cá nhân đủ rõ và usable | 10 |
| Group merge có dedup, chuẩn hóa và coverage review thật | 15 |
| Handoff note chuẩn bị tốt cho bước chạy agent | 5 |
| **Tổng điểm** | **100** |

---

## ✅ Checklist trước khi nộp bài

### Cá nhân
* [ ] Có use case từ Day 18/19.
* [ ] Có Unit of AI Work.
* [ ] Có một quality question rõ.
* [ ] Có ít nhất 3 dimensions và values.
* [ ] Có tối thiểu 10 scenarios/combinations.
* [ ] Có prompt đã dùng để generate inputs.
* [ ] Có tối thiểu 20 user inputs sau khi lọc.
* [ ] Có Scenario Dataset v0 cá nhân.
* [ ] Có coverage note cá nhân.

### Nhóm
* [ ] Có bảng chuẩn hóa dimensions.
* [ ] Có coverage matrix.
* [ ] Có danh sách merge/dedup decisions.
* [ ] Có Scenario Dataset v1 gồm ít nhất 30 rows.
* [ ] Có known gaps.
* [ ] Có handoff note cho bước chạy agent và đọc trace sau này.