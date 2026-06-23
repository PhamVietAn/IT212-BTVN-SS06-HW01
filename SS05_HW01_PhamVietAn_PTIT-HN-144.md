# BÀI 1: Phân tích & Lựa chọn (Kỹ thuật Prompt suy luận theo các bước - Chain-of-thought)

## 1. Đáp án lựa chọn
**Đáp án: Phương án B**

---

## 2. Phân tích chi tiết tại sao Phương án B là tối ưu nhất
Phương án B áp dụng một cách hoàn hảo kỹ thuật **Chain-of-thought (CoT) - Suy luận theo các bước** và tích hợp đầy đủ **cấu trúc 5 thành phần của một Prompt chuyên nghiệp**:

### A. Phân tích dựa trên kỹ thuật Chain-of-thought (CoT)
* **Yêu cầu trì hoãn viết code ("Đừng viết code ngay"):** Đây là mệnh lệnh cốt lõi kích hoạt chế độ suy luận chậm của LLM. Thay vì nhảy ngay đến việc sinh code (Zero-shot) dễ dẫn đến bỏ sót điều kiện biên hoặc tính sai logic, AI bị buộc phải phân bổ năng lượng tính toán (attention mechanism) để giải quyết từng chặng logic trung gian.
* **Chia nhỏ bài toán thành 4 bước suy luận rõ ràng:**
  1. *Bước 1 (Phân tích đầu vào và giảm trừ):* Làm rõ cách xác định thu nhập tính thuế. Bước này giúp AI không bị nhầm lẫn giữa "Tổng thu nhập", "Thu nhập chịu thuế" và "Thu nhập tính thuế".
  2. *Bước 2 (Liệt kê công thức):* Ép AI ghi nhận công thức toán học cho từng bậc thuế riêng biệt, tạo tiền đề để ánh xạ chính xác vào code mà không bị nhảy vọt logic.
  3. *Bước 3 (Dry-run bằng tay):* Đây là kỹ thuật kiểm thử logic cực kỳ hiệu quả. Bằng cách thực hiện tính toán trên một ví dụ cụ thể (30 triệu VND, 1 người phụ thuộc, không bảo hiểm), AI tự kiểm chứng tính đúng đắn của các công thức đã liệt kê trước khi lập trình. Nếu tính toán bằng tay ra kết quả đúng, code sinh ra ở bước sau sẽ có độ chính xác rất cao.
  4. *Bước 4 (Sinh mã nguồn):* Chặng cuối cùng khi mọi phân tích logic và dữ liệu đã được làm sáng tỏ trên "bảng nháp" văn bản. Code Java được sinh ra dựa trên nền tảng lập luận vững chắc.

### B. Cấu trúc 5 thành phần của Prompt trong Phương án B
1. **Vai trò (Role):** *"Hãy đóng vai trò là một Java Developer chuyên nghiệp kiêm chuyên gia tài chính."* -> Định vị AI có cả kiến thức kỹ thuật lập trình lẫn nghiệp vụ kế toán/tài chính.
2. **Mục tiêu/Nhiệm vụ (Goal):** *"viết một class Java tính thuế thu nhập cá nhân (TNCN) lũy tiến dựa trên thu nhập chịu thuế."* -> Xác định rõ ràng sản phẩm đầu ra mong muốn.
3. **Ngữ cảnh (Context):** Cung cấp cụ thể biểu thuế lũy tiến từng phần giả định gồm 7 bậc từ 5% đến 35% kèm theo các mốc ranh giới rõ ràng. Điều này tránh việc AI tự lấy thông tin biểu thuế cũ hoặc sai lệch.
4. **Ràng buộc (Constraint):** *"Sử dụng Java 17, kiểu dữ liệu BigDecimal để tránh sai số làm tròn tiền tệ."* -> Đây là ràng buộc kỹ thuật tối quan trọng trong tài chính. Kiểu `double` hay `float` thông thường sẽ gây ra sai số dấu phẩy động (floating-point error). Việc ép dùng `BigDecimal` đảm bảo tính chính xác tuyệt đối.
5. **Định dạng & Quy trình thực hiện (Format & Process):** Yêu cầu in ra màn hình quá trình phân tích 4 bước (CoT) trước khi viết code.

---

## 3. Phân tích lý do loại trừ các phương án còn lại
### Phương án A:
* **Nhược điểm:** Đây là một dạng prompt thô, quá chung chung (Zero-shot).
* **Nguy cơ lỗi logic:** 
  - Không cung cấp biểu thuế cụ thể mà yêu cầu "theo biểu thuế Việt Nam hiện hành". AI có nguy cơ lấy nhầm biểu thuế cũ (trước năm 2020) hoặc tự bịa ra biểu thuế (hallucination).
  - Không có ràng buộc về kiểu dữ liệu (`BigDecimal`). AI rất có thể sẽ sử dụng kiểu `double`, dẫn đến sai số làm tròn tiền tệ - một lỗi nghiêm trọng trong các hệ thống Core HR và Tài chính.
  - Không có quy trình CoT, AI sẽ lập tức sinh code và dễ tính sai các mốc ranh giới (ví dụ: nhầm lẫn giữa lớn hơn hoặc bằng `>=` và lớn hơn `>`).

### Phương án C:
* **Nhược điểm:** Yêu cầu tối ưu hiệu năng không thực tế và lạm dụng công nghệ (Over-engineering).
* **Nguy cơ lỗi logic:**
  - Việc tính thuế TNCN cho một nhân viên là một phép toán rất nhẹ, không cần sử dụng Parallel Stream. Sử dụng Java Stream API song song (`parallelStream()`) cho bài toán tính lũy tiến (vốn có tính tuần tự phụ thuộc lẫn nhau) là một phản hoạt động (anti-pattern).
  - Parallel Stream tạo ra các luồng xử lý song song, dẫn đến overhead quản lý Thread Pool (`ForkJoinPool`) lớn, làm giảm hiệu năng thực tế thay vì tăng tốc.
  - Hơn nữa, việc tính toán song song các bậc thuế lũy tiến nếu không được thiết kế cực kỳ khéo léo sẽ dẫn đến lỗi Race Condition hoặc sai thứ tự cộng dồn các bậc thuế.
  - Prompt cũng không chỉ định rõ biểu thuế và thiếu ràng buộc dữ liệu `BigDecimal` cũng như quy trình CoT.

---

## 4. Minh chứng thực tế: Nội dung Prompt & Phản hồi của AI
Dưới đây là đoạn hội thoại thực tế được chạy nghiệm thu trực tiếp với AI theo đúng Prompt của Phương án B:

### Lượt Prompt gửi AI:
```text
Hãy đóng vai trò là một Java Developer chuyên nghiệp kiêm chuyên gia tài chính. Nhiệm vụ của bạn là viết một class Java tính thuế thu nhập cá nhân (TNCN) lũy tiến dựa trên thu nhập chịu thuế. Ràng buộc: Sử dụng Java 17, kiểu dữ liệu BigDecimal để tránh sai số làm tròn tiền tệ. Các bậc thuế lũy tiến giả định:
- Bậc 1: Đến 5 triệu VND (5%)
- Bậc 2: Trên 5 triệu đến 10 triệu VND (10%)
- Bậc 3: Trên 10 triệu đến 18 triệu VND (15%)
- Bậc 4: Trên 18 triệu đến 32 triệu VND (20%)
- Bậc 5: Trên 32 triệu đến 52 triệu VND (25%)
- Bậc 6: Trên 52 triệu đến 80 triệu VND (30%)
- Bậc 7: Trên 80 triệu VND (35%)

Định dạng & Quy trình thực hiện (CoT):
Đừng viết code ngay. Hãy phân tích và suy luận từng bước sau ra màn hình trước:
1. Phân tích cách xác định thu nhập tính thuế sau khi trừ các khoản giảm trừ gia cảnh (bản thân 11M, người phụ thuộc 4.4M/người) và bảo hiểm (10.5% tổng thu nhập).
2. Liệt kê công thức tính thuế cho từng bậc riêng lẻ.
3. Dry-run (chạy thử bằng tay) với ví dụ cụ thể: tổng thu nhập 30 triệu VND, 1 người phụ thuộc, không bảo hiểm. Tính ra số tiền cụ thể bằng văn bản.
4. Cuối cùng, sinh ra mã nguồn Java hoàn chỉnh tuân thủ các phân tích trên.
```

### Phản hồi từ AI:
```text
Chào bạn, dưới đây là phân tích chi tiết từng bước (Chain-of-thought) và mã nguồn Java hoàn chỉnh đáp ứng các yêu cầu và ràng buộc của bạn.

---

### BƯỚC 1: PHÂN TÍCH CÁCH XÁC ĐỊNH THU NHẬP TÍNH THUẾ
Để tính thuế TNCN lũy tiến, trước hết chúng ta cần xác định "Thu nhập tính thuế" từ "Tổng thu nhập" (Gross Income) theo công thức sau:
1. Xác định các khoản giảm trừ và bảo hiểm bắt buộc:
   - Bảo hiểm bắt buộc: 10.5% * Tổng thu nhập.
   - Giảm trừ gia cảnh bản thân: 11,000,000 VND.
   - Giảm trừ người phụ thuộc: 4,400,000 VND * Số người phụ thuộc.
2. Công thức tính:
   Thu nhập tính thuế = Tổng thu nhập - Bảo hiểm bắt buộc - Giảm trừ bản thân - Giảm trừ người phụ thuộc.
   *Lưu ý:* Nếu Thu nhập tính thuế tính ra nhỏ hơn hoặc bằng 0, thì người đó không phải nộp thuế TNCN (Thu nhập tính thuế = 0).

---

### BƯỚC 2: LIỆT KÊ CÔNG THỨC TÍNH THUẾ CHO TỪNG BẬC RIÊNG LẺ
Gọi TNTT là Thu nhập tính thuế (đơn vị: VND). Số thuế phải nộp được tính lũy tiến theo từng bậc như sau:
- Bậc 1: TNTT từ 0 đến 5,000,000 VND
  -> Thuế bậc 1 = TNTT * 5% (Tối đa 250,000 VND)
- Bậc 2: TNTT trên 5,000,000 đến 10,000,000 VND
  -> Thuế bậc 2 = (TNTT - 5,000,000) * 10% (Tối đa 500,000 VND)
- Bậc 3: TNTT trên 10,000,000 đến 18,000,000 VND
  -> Thuế bậc 3 = (TNTT - 10,000,000) * 15% (Tối đa 1,200,000 VND)
- Bậc 4: TNTT trên 18,000,000 đến 32,000,000 VND
  -> Thuế bậc 4 = (TNTT - 18,000,000) * 20% (Tối đa 2,800,000 VND)
- Bậc 5: TNTT trên 32,000,000 đến 52,000,000 VND
  -> Thuế bậc 5 = (TNTT - 32,000,000) * 25% (Tối đa 5,000,000 VND)
- Bậc 6: TNTT trên 52,000,000 đến 80,000,000 VND
  -> Thuế bậc 6 = (TNTT - 52,000,000) * 30% (Tối đa 8,400,000 VND)
- Bậc 7: TNTT trên 80,000,000 VND
  -> Thuế bậc 7 = (TNTT - 80,000,000) * 35%

Tổng thuế TNCN = Thuế bậc 1 + Thuế bậc 2 + ... + Thuế bậc 7.

---

### BƯỚC 3: DRY-RUN (CHẠY THỬ BẰNG TAY) VỚI VÍ DỤ CỤ THỂ
*Thông số đầu vào:*
- Tổng thu nhập = 30,000,000 VND.
- Số người phụ thuộc = 1 người.
- Không bảo hiểm (Tỷ lệ bảo hiểm = 0%).

*Tiến hành tính toán:*
1. Tính các khoản giảm trừ:
   - Bảo hiểm: 0 VND.
   - Giảm trừ bản thân: 11,000,000 VND.
   - Giảm trừ người phụ thuộc: 1 * 4,400,000 = 4,400,000 VND.
   - Tổng giảm trừ = 11,000,000 + 4,400,000 = 15,400,000 VND.
2. Tính thu nhập tính thuế (TNTT):
   TNTT = 30,000,000 - 15,400,000 = 14,600,000 VND.
3. Tính thuế lũy tiến cho TNTT = 14,600,000 VND:
   - Bậc 1: Tính trên 5,000,000 VND đầu tiên:
     Thuế bậc 1 = 5,000,000 * 5% = 250,000 VND.
   - Bậc 2: Tính trên phần thu nhập từ 5,000,000 đến 10,000,000 VND (tương đương 5,000,000 VND tiếp theo):
     Thuế bậc 2 = 5,000,000 * 10% = 500,000 VND.
   - Bậc 3: Tính trên phần thu nhập còn lại vượt trên 10,000,000 VND:
     Thu nhập chịu thuế ở bậc này = 14,600,000 - 10,000,000 = 4,600,000 VND.
     Thuế bậc 3 = 4,600,000 * 15% = 690,000 VND.
   - Các bậc 4, 5, 6, 7: Không phát sinh do thu nhập tính thuế không vượt quá 18,000,000 VND.
4. Tổng thuế TNCN phải nộp:
   Tổng thuế = 250,000 (Bậc 1) + 500,000 (Bậc 2) + 690,000 (Bậc 3) = 1,440,000 VND.

---

### BƯỚC 4: MÃ NGUỒN JAVA HOÀN CHỈNH (JAVA 17 + BIGDECIMAL)
Dưới đây là code Java hoàn chỉnh được thiết kế sạch sẽ, sử dụng BigDecimal để đảm bảo độ chính xác tài chính và tuân thủ các bước phân tích trên.
```java
import java.math.BigDecimal;
import java.math.RoundingMode;

public class PersonalIncomeTaxCalculator {

    // Định nghĩa các hằng số giảm trừ gia cảnh
    private static final BigDecimal SELF_DEDUCTION = new BigDecimal("11000000");
    private static final BigDecimal DEPENDENT_DEDUCTION_UNIT = new BigDecimal("4400000");
    private static final BigDecimal INSURANCE_RATE = new BigDecimal("0.105"); // 10.5%

    // Định nghĩa các mốc giới hạn bậc thuế (đơn vị: VND)
    private static final BigDecimal LIMIT_LEVEL_1 = new BigDecimal("5000000");
    private static final BigDecimal LIMIT_LEVEL_2 = new BigDecimal("10000000");
    private static final BigDecimal LIMIT_LEVEL_3 = new BigDecimal("18000000");
    private static final BigDecimal LIMIT_LEVEL_4 = new BigDecimal("32000000");
    private static final BigDecimal LIMIT_LEVEL_5 = new BigDecimal("52000000");
    private static final BigDecimal LIMIT_LEVEL_6 = new BigDecimal("80000000");

    // Định nghĩa thuế suất tương ứng từng bậc
    private static final BigDecimal RATE_LEVEL_1 = new BigDecimal("0.05"); // 5%
    private static final BigDecimal RATE_LEVEL_2 = new BigDecimal("0.10"); // 10%
    private static final BigDecimal RATE_LEVEL_3 = new BigDecimal("0.15"); // 15%
    private static final BigDecimal RATE_LEVEL_4 = new BigDecimal("0.20"); // 20%
    private static final BigDecimal RATE_LEVEL_5 = new BigDecimal("0.25"); // 25%
    private static final BigDecimal RATE_LEVEL_6 = new BigDecimal("0.30"); // 30%
    private static final BigDecimal RATE_LEVEL_7 = new BigDecimal("0.35"); // 35%

    /**
     * Hàm tính Thuế TNCN từ Tổng thu nhập (Gross Income)
     *
     * @param totalIncome        Tổng thu nhập (Gross Income)
     * @param numberOfDependents Số người phụ thuộc
     * @param hasInsurance       Có tính bảo hiểm bắt buộc hay không
     * @return Số tiền thuế TNCN phải nộp (đã làm tròn về đơn vị Đồng)
     */
    public static BigDecimal calculateTax(BigDecimal totalIncome, int numberOfDependents, boolean hasInsurance) {
        if (totalIncome == null || totalIncome.compareTo(BigDecimal.ZERO) <= 0) {
            return BigDecimal.ZERO.setScale(0, RoundingMode.HALF_UP);
        }

        // 1. Tính bảo hiểm bắt buộc
        BigDecimal insurance = BigDecimal.ZERO;
        if (hasInsurance) {
            insurance = totalIncome.multiply(INSURANCE_RATE);
        }

        // 2. Tính các khoản giảm trừ gia cảnh
        BigDecimal dependentDeduction = DEPENDENT_DEDUCTION_UNIT.multiply(BigDecimal.valueOf(numberOfDependents));
        BigDecimal totalDeductions = SELF_DEDUCTION.add(dependentDeduction).add(insurance);

        // 3. Tính thu nhập tính thuế (Taxable Income)
        BigDecimal taxableIncome = totalIncome.subtract(totalDeductions);

        // Nếu thu nhập tính thuế <= 0 thì không phải nộp thuế
        if (taxableIncome.compareTo(BigDecimal.ZERO) <= 0) {
            return BigDecimal.ZERO.setScale(0, RoundingMode.HALF_UP);
        }

        // 4. Tính thuế lũy tiến từng phần
        BigDecimal totalTax = BigDecimal.ZERO;
        BigDecimal remainingIncome = taxableIncome;

        // Bậc 1: Đến 5 triệu
        BigDecimal incomeInLevel1 = remainingIncome.min(LIMIT_LEVEL_1);
        totalTax = totalTax.add(incomeInLevel1.multiply(RATE_LEVEL_1));
        remainingIncome = remainingIncome.subtract(incomeInLevel1);

        // Bậc 2: Trên 5 triệu đến 10 triệu (Dải 5 triệu tiếp theo)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            BigDecimal level2Range = LIMIT_LEVEL_2.subtract(LIMIT_LEVEL_1);
            BigDecimal incomeInLevel2 = remainingIncome.min(level2Range);
            totalTax = totalTax.add(incomeInLevel2.multiply(RATE_LEVEL_2));
            remainingIncome = remainingIncome.subtract(incomeInLevel2);
        }

        // Bậc 3: Trên 10 triệu đến 18 triệu (Dải 8 triệu tiếp theo)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            BigDecimal level3Range = LIMIT_LEVEL_3.subtract(LIMIT_LEVEL_2);
            BigDecimal incomeInLevel3 = remainingIncome.min(level3Range);
            totalTax = totalTax.add(incomeInLevel3.multiply(RATE_LEVEL_3));
            remainingIncome = remainingIncome.subtract(incomeInLevel3);
        }

        // Bậc 4: Trên 18 triệu đến 32 triệu (Dải 14 triệu tiếp theo)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            BigDecimal level4Range = LIMIT_LEVEL_4.subtract(LIMIT_LEVEL_3);
            BigDecimal incomeInLevel4 = remainingIncome.min(level4Range);
            totalTax = totalTax.add(incomeInLevel4.multiply(RATE_LEVEL_4));
            remainingIncome = remainingIncome.subtract(incomeInLevel4);
        }

        // Bậc 5: Trên 32 triệu đến 52 triệu (Dải 20 triệu tiếp theo)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            BigDecimal level5Range = LIMIT_LEVEL_5.subtract(LIMIT_LEVEL_4);
            BigDecimal incomeInLevel5 = remainingIncome.min(level5Range);
            totalTax = totalTax.add(incomeInLevel5.multiply(RATE_LEVEL_5));
            remainingIncome = remainingIncome.subtract(incomeInLevel5);
        }

        // Bậc 6: Trên 52 triệu đến 80 triệu (Dải 28 triệu tiếp theo)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            BigDecimal level6Range = LIMIT_LEVEL_6.subtract(LIMIT_LEVEL_5);
            BigDecimal incomeInLevel6 = remainingIncome.min(level6Range);
            totalTax = totalTax.add(incomeInLevel6.multiply(RATE_LEVEL_6));
            remainingIncome = remainingIncome.subtract(incomeInLevel6);
        }

        // Bậc 7: Trên 80 triệu (Phần còn lại)
        if (remainingIncome.compareTo(BigDecimal.ZERO) > 0) {
            totalTax = totalTax.add(remainingIncome.multiply(RATE_LEVEL_7));
        }

        // Làm tròn kết quả thuế về số nguyên Đồng
        return totalTax.setScale(0, RoundingMode.HALF_UP);
    }

    public static void main(String[] args) {
        // Chạy thử với ví dụ dry-run: 30 triệu VND, 1 người phụ thuộc, không bảo hiểm
        BigDecimal totalIncome = new BigDecimal("30000000");
        int dependents = 1;
        boolean hasInsurance = false;

        BigDecimal tax = calculateTax(totalIncome, dependents, hasInsurance);

        System.out.println("--- KẾT QUẢ TÍNH THUẾ TNCN ---");
        System.out.println("Tổng thu nhập: " + totalIncome + " VND");
        System.out.println("Số người phụ thuộc: " + dependents);
        System.out.println("Thuế TNCN phải nộp: " + tax + " VND");
        
        // Output mong đợi: 1440000
    }
}
```
```
