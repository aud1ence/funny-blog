---
title: Cấu trúc Kim Tự Tháp trong Code Review
date: 2024-01-09 12:00:00 +0700
categories: [Development, Best Practices]
tags: [code-review, best-practices, development]
---

![Kim Tu Thap Review Structure](https://res.cloudinary.com/dbkg7owwc/image/upload/v1743500063/kim-tu-thap-review_kx4euh.png)


Trong suốt quá trình làm việc với vai trò reviewer trong team, tôi luôn tự hỏi: Liệu mình đã review code thực sự hiệu quả chưa? Trước đây, tôi thường tiếp cận việc review một cách khá tùy hứng - không theo quy trình rõ ràng nào cả. Cho đến một ngày, tôi tình cờ phát hiện ra một mô hình đã thay đổi hoàn toàn cách tôi nhìn nhận về quy trình review code.

# Cấu trúc Kim Tự Tháp

Điều tuyệt vời của mô hình kim tự tháp này là nó thể hiện một nguyên lý đơn giản nhưng sâu sắc: càng lên cao trong kim tự tháp, những thay đổi càng dễ thực hiện và càng dễ tự động hóa. Ngược lại, càng xuống thấp, những thay đổi càng phức tạp và đòi hỏi sự tập trung cao độ khi review.

## Phần trên cùng (Dễ thay đổi, nên tự động hóa)
Ở đỉnh kim tự tháp, chúng ta thấy những yếu tố như code style và tests - những yếu tố có thể (nên) được tự động hóa thông qua các công cụ linting và quy trình CI/CD. Tại sao chúng ta lại lãng phí thời gian của mình vào những việc mà máy móc có thể làm tốt hơn?

## Phần dưới cùng (Khó thay đổi, cần tập trung nhiều hơn khi review)
Càng xuống đáy kim tự tháp, chúng ta đối mặt với những thách thức phức tạp hơn như thiết kế API và logic hệ thống - những yếu tố có ảnh hưởng mạnh mẽ và cần được xem xét cẩn thận bởi đôi mắt tinh tường của reviewer có kinh nghiệm.

## Code Style

### Mục tiêu
Đảm bảo code có định dạng chuẩn, tuân theo quy ước đặt tên, và dễ đọc

### Tại sao quan trọng?
- Code dễ đọc = dễ bảo trì và phát triển
- Sự nhất quán giữa các thành viên tạo nên ngôn ngữ chung
- Tự động hóa qua linting giúp tiết kiệm thời gian review

### Câu hỏi cần đặt ra khi review
- Code có tuân thủ định dạng chuẩn của dự án không?
- Các tên biến, hàm có ý nghĩa và theo quy ước không?
- Code có tuân thủ nguyên tắc DRY (Don't Repeat Yourself) không?
- Người đọc có hiểu được mục đích của code chỉ bằng cách đọc không?

### Công cụ đề xuất
ESLint, Prettier, Black, Checkstyle

## Tests

### Mục tiêu
Đảm bảo code hoạt động chính xác thông qua unit tests và integration tests

### Tại sao quan trọng?
- Phát hiện lỗi từ sớm, tiết kiệm chi phí sửa lỗi
- Hình thành hệ thống cảnh báo an toàn khi có thay đổi trong tương lai

### Câu hỏi cần đặt ra khi review
- Tất cả tests đều pass không?
- Các edge cases đã được xử lý chưa?
- Tests có đủ độ bao phủ không?
- Unit tests và integration tests có được sử dụng đúng mục đích không?

### Công cụ đề xuất
Xây dựng CI/CD pipeline (Jenkins, GitHub Actions) để tự động hóa việc chạy tests sau mỗi lần commit

## Documentation

### Mục tiêu
Đảm bảo code mới được tài liệu hóa đầy đủ, giúp dễ hiểu và dễ bảo trì

### Tại sao quan trọng?
- Dự án thiếu tài liệu như ngôi nhà không có bản thiết kế
- Giúp các developer mới onboard nhanh chóng

### Câu hỏi cần đặt ra khi review
- Code mới có được ghi chép lại đầy đủ không?
- Các tài liệu quan trọng (README, API docs) có được cập nhật không?
- Tài liệu có rõ ràng và không mắc lỗi chính tả không?

### Đề xuất
- Yêu cầu mỗi PR phải có mô tả và tài liệu đi kèm
- Viết commit message rõ ràng, có ý nghĩa

## Implementation Semantics

### Mục tiêu
Đảm bảo logic và cấu trúc code chính xác, hiệu quả.

### Tại sao quan trọng?
Đây là nơi các lỗi nghiêm trọng thường tiềm ẩn, ảnh hưởng đến bảo mật và hiệu suất của hệ thống

### Câu hỏi cần đặt ra khi review
- Code có đáp ứng đúng yêu cầu business không?
- Logic xử lý có lỗ hổng không?
- Code có quá phức tạp và khó bảo trì không?
- Đã xử lý đúng concurrency và error handling chưa?
- Các vấn đề bảo mật (SQL injection, XSS) đã được giải quyết chưa?
- Có đầy đủ hệ thống giám sát (logging, metrics, tracing) không?

### Công cụ đề xuất
Copilot, Cursor

## API Semantics

### Mục tiêu
Đảm bảo API được thiết kế trực quan, dễ sử dụng và tuân thủ tiêu chuẩn

### Tại sao quan trọng?
API không chuẩn sẽ khó sửa đổi và ảnh hưởng đến nhiều hệ thống khác

### Câu hỏi cần đặt ra khi review
- API có đơn giản và dễ dùng không?
- Có hoạt động theo cách mà người dùng mong đợi không
- Có tuân theo nguyên tắc only one way không (tránh trùng lặp API)?
- Các API classes, endpoints, and methods trực quan không?
- Có versioning để tránh làm hỏng API cũ không?
- API có cung cấp các tính năng hữu ích và cần thiết không?

### Recommendation
- Use API linting tools (e.g., Spectral for OpenAPI)
- Define API guidelines for REST, GraphQL, or gRPC

## Key Takeaways

1. Tập trung review nhiều nhất vào API Semantics & Implementation Semantics vì đây là phần quan trọng nhất
2. Tự động hóa Code Style và Tests
3. Sử dụng linting & CI/CD
4. Enforce documentation guidelines
5. Improve maintainability

Với cách áp dụng cấu trúc kim tự tháp này, tôi đã thay đổi hoàn toàn cách tiếp cận review code và nhận thấy chất lượng code trong team được cải thiện đáng kể. Hy vọng nó cũng sẽ giúp ích cho hành trình coding của bạn.
