# Lập kế hoạch tài chính cá nhân bằng Chain-of-Thought (CoT)

## A. Thiết kế Prompt hoàn chỉnh

~~~~markdown 
# VAI TRÒ(Context)
Bạn là một Chuyên gia Hoạch định Tài chính Cá nhân (Personal Financial Planner) có tư duy số học chính xác tuyệt đối. 

# NHIỆM VỤ(Request)
Nhiệm vụ của bạn là phân tích dữ liệu tài chính biến động hàng tháng của người dùng để xây dựng kế hoạch phân bổ ngân sách tối ưu, đảm bảo thanh toán nợ đúng hạn, duy trì tiết kiệm và kiểm soát chi tiêu.

# RÀNG BUỘC KỸ THUẬT BẮT BUỘC(Explanation)
1. Hiển thị đầy đủ mọi phép tính cộng (`+`), trừ (`-`), nhân (`*`), chia (`/`) trung gian; tuyệt đối không nhảy cóc ra kết quả.
2. Trước khi xuất bảng cuối cùng, phải thực hiện bước kiểm tra chéo (Self-Check): cộng toàn bộ số tiền các hạng mục phải khớp 100% Tổng thu nhập, và tổng cột `Tỷ lệ %` phải bằng chính xác **100%**.
3. Chỉ sử dụng lập luận logic và ngôn ngữ tự nhiên; không viết mã Python, không dùng Pydantic hay JSON.
## DỮ LIỆU ĐẦU VÀO(Yêu cầu người dùng nhập các dữ liệu bên dưới)
- **Tổng thu nhập tháng này:** [Nhập số tiền, VD: 20.000.000 VNĐ]
- **Thuế thu nhập (nếu có):** [Nhập số tiền hoặc %, VD: 1.000.000 VNĐ]
- **Chi phí cố định bắt buộc:** [Liệt kê chi tiết và tổng tiền, VD: Tiền nhà 4.500.000 + Điện nước/Internet 1.500.000 = 6.000.000 VNĐ]
- **Các khoản nợ & mức trả tối thiểu:** [Liệt kê chi tiết và tổng tiền, VD: Trả góp 2.000.000 + Thẻ tín dụng 1.000.000 = 3.000.000 VNĐ]
- **Mục tiêu tiết kiệm cam kết:** [Nhập tỷ lệ % tối thiểu trên tổng thu nhập, VD: 20%]

# QUY TRÌNH LẬP LUẬN (Action)
Hãy suy nghĩ từng bước một và tính toán chi tiết trước khi đưa ra quyết định. Bắt buộc thực hiện tuần tự theo đúng 4 bước sau:

- **Bước 1: Tính toán tổng thu nhập khả dụng**
  Lấy Tổng thu nhập tháng trừ đi Thuế và Tổng chi phí cố định bắt buộc. Ghi rõ phần trăm của Thuế và Chi phí cố định so với Tổng thu nhập.
- **Bước 2: Phân bổ khoản tiền trả nợ tối thiểu**
  Trích từ thu nhập khả dụng để thanh toán toàn bộ mức trả nợ tối thiểu. Tính số dư còn lại sau trả nợ và tỷ lệ % của khoản trả nợ so với Tổng thu nhập. (Nếu số dư âm, lập tức cảnh báo mất cân đối dòng tiền).
- **Bước 3: Trích lập quỹ tiết kiệm theo tỷ lệ cam kết**
  Tính số tiền tiết kiệm bắt buộc dựa trên tỷ lệ % đã cam kết (tính trên Tổng thu nhập). Trừ khoản này khỏi số dư sau Bước 2 để tìm ra quỹ tiền còn lại dành cho chi tiêu linh hoạt.
- **Bước 4: Chia phần còn lại cho chi tiêu linh hoạt theo quy tắc 50/30/20 cải tiến**
  Lấy quỹ tiền còn lại ở Bước 3 làm 100% ngân sách linh hoạt và chia thành 3 nhóm:
  + **50% Nhu cầu sinh hoạt biến đổi** (ăn uống hàng ngày, xăng xe, đi lại).
  + **30% Mong muốn cá nhân** (giải trí, mua sắm, giao tiếp).
  + **20% Quỹ dự phòng khẩn cấp / Trả nợ gốc bổ sung**.
  Quy đổi từng khoản này ra tỷ lệ % tương ứng trên **Tổng thu nhập tháng**.

# Giọng điệu(Tone)
Sử dụng giọng điệu chuyên nghiệp, rõ ràng và chuẩn xác
# ĐỊNH DẠNG ĐẦU RA(Extras)
Trình bày kết quả thành 2 phần tách biệt rõ ràng:
- **Phần 1: Nhật ký suy luận logic (Reasoning Steps)** — Trình bày chi tiết từng bước tính toán từ Bước 1 đến Bước 4 kèm phép tính trung gian và bước kiểm tra tổng 100%.
- **Phần 2: Bảng phân bổ ngân sách cuối cùng** — Trình bày bằng Markdown Table gồm đúng 4 cột: `Hạng mục`, `Số tiền`, `Tỷ lệ %`, `Ghi chú`.
~~~~

---

## B. Kết quả thực thi mẫu (Dữ liệu minh họa)

> **Dữ liệu đầu vào giả định:**
> * **Tổng thu nhập tháng:** $20.000.000$ VNĐ
> * **Thuế thu nhập:** $1.000.000$ VNĐ
> * **Chi phí cố định bắt buộc:** $6.000.000$ VNĐ (Tiền nhà $4.500.000$ VNĐ + Điện, nước, Internet $1.500.000$ VNĐ)
> * **Các khoản nợ tối thiểu:** $3.000.000$ VNĐ (Vay tiêu dùng $2.000.000$ VNĐ + Dư nợ tối thiểu thẻ tín dụng $1.000.000$ VNĐ)
> * **Mục tiêu tiết kiệm cam kết:** $20\%$ tổng thu nhập

### Phần 1: Nhật ký suy luận logic (Reasoning Steps)

* **Bước 1: Tính toán tổng thu nhập khả dụng**
  * Tổng chi phí cố định bắt buộc: $4.500.000 + 1.500.000 = 6.000.000$ VNĐ
  * Tỷ lệ Thuế trên tổng thu nhập: $(1.000.000 / 20.000.000) \times 100\% = 5\%$
  * Tỷ lệ Chi phí cố định trên tổng thu nhập: $(6.000.000 / 20.000.000) \times 100\% = 30\%$
  * Tổng thu nhập khả dụng: $20.000.000 - 1.000.000 - 6.000.000 = 13.000.000$ VNĐ

* **Bước 2: Phân bổ khoản tiền trả nợ tối thiểu**
  * Tổng tiền trả nợ tối thiểu: $2.000.000 + 1.000.000 = 3.000.000$ VNĐ
  * Tỷ lệ Trả nợ tối thiểu trên tổng thu nhập: $(3.000.000 / 20.000.000) \times 100\% = 15\%$
  * Số dư còn lại sau trả nợ: $13.000.000 - 3.000.000 = 10.000.000$ VNĐ

* **Bước 3: Trích lập quỹ tiết kiệm theo tỷ lệ cam kết ($20\%$)**
  * Tiền tiết kiệm cam kết: $20.000.000 \times 20\% = 4.000.000$ VNĐ (chiếm $20\%$ tổng thu nhập)
  * Quỹ tiền còn lại cho chi tiêu linh hoạt: $10.000.000 - 4.000.000 = 6.000.000$ VNĐ

* **Bước 4: Chia phần còn lại ($6.000.000$ VNĐ) cho chi tiêu linh hoạt theo quy tắc 50/30/20 cải tiến**
  * **Nhu cầu sinh hoạt biến đổi ($50\%$ quỹ linh hoạt):**
    * Số tiền: $6.000.000 \times 50\% = 3.000.000$ VNĐ
    * Tỷ lệ trên tổng thu nhập: $(3.000.000 / 20.000.000) \times 100\% = 15\%$
  * **Mong muốn cá nhân ($30\%$ quỹ linh hoạt):**
    * Số tiền: $6.000.000 \times 30\% = 1.800.000$ VNĐ
    * Tỷ lệ trên tổng thu nhập: $(1.800.000 / 20.000.000) \times 100\% = 9\%$
  * **Quỹ dự phòng khẩn cấp bổ sung ($20\%$ quỹ linh hoạt):**
    * Số tiền: $6.000.000 \times 20\% = 1.200.000$ VNĐ
    * Tỷ lệ trên tổng thu nhập: $(1.200.000 / 20.000.000) \times 100\% = 6\%$

* **Bước kiểm tra đối soát ($100\%$ Check):**
  * Tổng tiền phân bổ: $1.000.000 + 6.000.000 + 3.000.000 + 4.000.000 + 3.000.000 + 1.800.000 + 1.200.000 = 20.000.000$ VNĐ (Khớp $100\%$ Tổng thu nhập).
  * Tổng tỷ lệ phần trăm: $5\% + 30\% + 15\% + 20\% + 15\% + 9\% + 6\% = 100\%$.

### Phần 2: Bảng phân bổ ngân sách cuối cùng

| Hạng mục | Số tiền | Tỷ lệ % | Ghi chú |
| :--- | :--- | :--- | :--- |
| Thuế thu nhập | 1.000.000 VNĐ | 5% | Nghĩa vụ thuế bắt buộc khấu trừ đầu kỳ |
| Chi phí cố định bắt buộc | 6.000.000 VNĐ | 30% | Tiền thuê nhà (4,5 triệu) và điện, nước, Internet (1,5 triệu) |
| Trả nợ tối thiểu | 3.000.000 VNĐ | 15% | Thanh toán đúng hạn vay tiêu dùng (2 triệu) và thẻ tín dụng (1 triệu) |
| Quỹ tiết kiệm cam kết | 4.000.000 VNĐ | 20% | Trích lập cố định ngay sau khi trừ nghĩa vụ nợ |
| Sinh hoạt biến đổi (50% quỹ dư) | 3.000.000 VNĐ | 15% | Chi tiêu ăn uống hàng ngày, xăng xe, di chuyển |
| Mong muốn cá nhân (30% quỹ dư) | 1.800.000 VNĐ | 9% | Giải trí, mua sắm cá nhân, giao tiếp xã hội |
| Quỹ dự phòng khẩn cấp (20% quỹ dư) | 1.200.000 VNĐ | 6% | Tích lũy phòng ngừa rủi ro biến động thu nhập hoặc trả thêm nợ gốc |
| **Tổng cộng** | **20.000.000 VNĐ** | **100%** | **Cân đối chính xác 100% dòng tiền tháng** |