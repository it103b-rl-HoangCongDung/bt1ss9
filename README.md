# BÁO CÁO PHÂN TÍCH VÀ SỬA LỖI TRUY CẬP THUỘC TÍNH ĐỘNG TRONG JAVASCRIPT

> 👤 **Học viên:** Hoàng Công Dũng | **Mã SV:** PTIT-HCM-009
> 🏫 **Môn học:** it103b-rl-HoangCongDung

---

## 📊 Sơ đồ thiết kế hệ thống (Flowchart)

> 💡 *Sơ đồ dưới đây được render tự động trực tiếp trên GitHub bằng Mermaid. Bạn cũng có thể tải file **`bt1.drawio`** trong repository này để mở và chỉnh sửa trực tiếp trên [Draw.io (diagrams.net)](https://app.diagrams.net).* 

```mermaid
graph TD
  Start([Bắt đầu chương trình]) --> Init[Khai báo bookingReservation & priceKey = 'roomPrice']
  Init --> WrongWay{Sử dụng Dot Notation: bookingReservation.priceKey?}
  WrongWay -->|Kết quả| Undefined[Trả về undefined do không tìm thấy khóa 'priceKey']
  Undefined --> CalWrong[basePrice = undefined, surcharge = NaN, totalAmount = NaN]
  CalWrong --> ErrorEnd([Kết quả thực tế: Lỗi NaN nghiêm trọng])
  
  Init --> RightWay{Sử dụng Bracket Notation: bookingReservation[priceKey]?}
  RightWay -->|Kết quả| GetVal[Lấy chính xác giá trị 1500000]
  GetVal --> CheckHour{checkInHour < 12?}
  CheckHour -->|Đúng| Calc[surcharge = 1500000 * 0.3 = 450000]
  CheckHour -->|Sai| Zero[surcharge = 0]
  Calc --> Total[totalAmount = 1500000 + 450000 = 1950000]
  Zero --> Total
  Total --> SuccessEnd([Kết quả đúng: Hiển thị đầy đủ thông tin hóa đơn])
```

---

## Nhiệm vụ 1: Phân tích lỗi & Lập bảng Test Cases

Trong đoạn mã nguồn ban đầu tại quầy lễ tân Grand Hotel, lập trình viên đã sử dụng cú pháp Dot Notation để truy cập thuộc tính thông qua biến động 'priceKey'. Cụ thể, câu lệnh 'bookingReservation.priceKey' yêu cầu JavaScript tìm kiếm chính xác một thuộc tính có tên định danh là chuỗi 'priceKey' bên trong đối tượng 'bookingReservation'.

Vì đối tượng này chỉ chứa các khóa thực tế như 'bookingId', 'guestName', 'roomType', 'roomPrice' và 'checkInHour', thuộc tính 'priceKey' hoàn toàn không tồn tại. Kết quả là biểu thức trả về giá trị 'undefined'. Khi thực hiện phép tính số học 'undefined * 0.3' và 'undefined + surcharge', bộ máy V8 của JavaScript tự động ép kiểu và trả về giá trị 'NaN' (Not-a-Number), làm hỏng toàn bộ hóa đơn thanh toán của khách hàng.

Để khắc phục triệt để sự cố này, chúng ta bắt buộc phải sử dụng cú pháp Bracket Notation 'bookingReservation[priceKey]'. Cú pháp này cho phép JavaScript đánh giá giá trị được lưu bên trong biến 'priceKey' (chính là chuỗi 'roomPrice') trước, từ đó truy xuất thành công vào thuộc tính 'roomPrice' của đối tượng.

- Nguyên nhân kỹ thuật: Dot Notation 'obj.key' hiểu 'key' là tên định danh tĩnh cố định, không phân giải giá trị của biến.
- Giải pháp khắc phục: Bracket Notation 'obj[variable]' cho phép truyền biểu thức hoặc biến động chứa tên thuộc tính cần truy xuất.
- Tác động nghiệp vụ: Tránh tình trạng tính toán ra giá trị NaN, đảm bảo tính chính xác tuyệt đối cho hệ thống hóa đơn khách sạn.

| Trường hợp kiểm thử | Dữ liệu đầu vào | Kết quả sai thực tế | Kết quả đúng mong đợi |
| --- | --- | --- | --- |
| TC01: Truy cập giá phòng bằng biến động (Dot Notation) | priceKey = 'roomPrice', checkInHour = 9 | basePrice: undefined, surcharge: NaN, totalAmount: NaN | Lỗi biên dịch logic, hóa đơn hỏng hoàn toàn |
| TC02: Truy cập giá phòng bằng biến động (Bracket Notation) | priceKey = 'roomPrice', checkInHour = 9 | basePrice: 1500000, surcharge: 450000, totalAmount: 1950000 | Tính toán chính xác 100%, hiển thị đúng số tiền thanh toán |

## Nhiệm vụ 2: Sửa lỗi mã nguồn

Dưới đây là mã nguồn đã được chỉnh sửa hoàn thiện, thay thế toàn bộ cú pháp Dot Notation sai lầm thành Bracket Notation an toàn. Tệp này được lưu dưới tên 'solution_ex1.js' để nộp lên repository 'it103b-rl-HoangCongDung'.

- Đã thay thế 'bookingReservation.priceKey' thành 'bookingReservation[priceKey]'.
- Đảm bảo mã nguồn chạy mượt mà trên môi trường Node.js mà không phát sinh lỗi ép kiểu NaN.
- Tuân thủ nghiêm ngặt các quy định về đặt tên file và quy chuẩn lập trình sạch.

---

## 📁 Danh sách tệp tin nộp bài trong Repository
- 📝 `bt1.docx`: Báo cáo tài liệu phân tích nghiệp vụ hoàn chỉnh.
- 🎨 `bt1.drawio`: File thiết kế sơ đồ chuẩn theo quy định đề bài (mở trực tiếp bằng [Draw.io](https://app.diagrams.net) hoặc Lucidchart).
- 💻 `solution_ex1.js`: Mã nguồn chương trình.
