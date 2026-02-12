---
title: "[AI Course] Bài 1. Tổng quan và hướng dẫn AI đọc hiểu project"
date: 2026-02-11
draft: false
series: ["Học và sử dụng AI "]
weitght: 2
tags: ["learning-log", "AI", "claude"]
---
## Các khái niệm cơ bản 
### Context Window 
- AI có bộ nhớ suy luận hữu hạn 
- 1,000,000 tokens đối với ClaudeAI 
- Tokens có thể hiểu là 1 từ có ngữ nghĩa trong câu 
- Ví dụ : I love u , trong đó I là 1 token , u là 1 token , love là 1 token , trái đất nghe là 2 từ nhưng cũng chỉ là 1 token 
- 1 triệu tokens tương đương 7 quyển Harry Potter 
- Chẳng hạn : Khi cửa sửa context window vượt quá 1 triệu tokens , -> rất lag , chatGPT không mượt , phải xóa bớt token đằng trước 
đối với Claude , nén lại dữ liệu , tạo cuộc hội thoại mới ... 

### Attention ( Độ tập trung )
- Trong không gian suy luận , không phải tất cả các thông tin dữ liệu đều được AI chú ý , tập trung như nhau 
- Thường các thông tin ở đầu , cuối của context window sẽ được chú ý hơn 
### Không gian tri thức và quá trình ghép nối 
- Cần biết ghép nối các trường thông tin với nhau nhiều nhất có thể 
- Cần phải dẫn dắt AI vào tri thức mà chúng ta cần (phụ thuộc vào cách chúng ta đặt câu hỏi )
- Tri thức phổ biến , có nhiều người hỏi -> trả lời rất nhanh (hàng triệu người đã hỏi)
- Tri thức hẹp và hiếm , càng phải đặt câu hỏi , và dẫn dắt AI vào không gian tri thức đó 

## Quá trình AI đọc hiểu Project 
- Phụ thuộc vào cách thiết kế con AI , Claude , Copilot hoạt động khác nhau 
- Cách người dùng định hướng cho AI , cho AI đọc như nào , định hướng ra sao , cái gì trước cái gì sau , cái gì đúng , cái gì sai 
### Người dùng Amater 
- Vất nguyên cả source code , bắt AI đọc 
- Sẽ có 1 số vấn đề nảy sinh nếu AI tự đọc 
#### Tự tạo ra thông tin ngầm định (Implicit Information)
- AI tự giả định mục đích của project 
- AI tự giả định về flow code 
- AI tự giả định về cách biên dịch , gỡ lỗi 
- <span style="color:red"><strong>Rất nguy hiểm nếu chúng ta để AI tự giả định thông tin</strong></span>
#### Để AI tự đọc ,sẽ tuân theo quy tắc mặc định (default behavior)
- AI sẽ ưu tiên đọc flow từ code hơn là tài liệu 
- Claude.md > default behavior (code > doc) 
### Người dùng chuyên nghiệp 
- AI đọc hiểu thông qua explicit rule , hạn chế tối đa AI ngầm hiểu về project , cần phải đưa ra chỉ dẫn rõ ràng 
- Các rule trong file Claude.md có khả năng ghi đè behavior default của AI .
- Ưu tiên đọc file Claude.md , dữ liệu luôn nằm trong vùng attention , kể cả cuộc hội thoại có bị reload lại , bị quá 1 triệu token , hoặc bị compressed , dữ liệu vẫn được truyền sang cuộc hội thoại mới . 
- Dữ liệu cốt lõi nhất luôn để ở trong file này 
- Nên để file này càng dài càng tốt , (dưới 150 dòng )
- Nội dung bao gồm : Mục đích của project , thứ tự đọc , nguyên tắc chung (không đoán , nếu không rõ hỏi lại , mọi thứ khi quyết định phải đi kèm lý do (WHY , không chỉ WHAT)), khi đọc document mà mâu thuẫn , tin code hay document hơn -> tin code 
- Khi làm tài liệu cho AI , Dùng markdown , mermaid , không dùng binary format 
- Commit message : Viết bằng tiếng việt 
- Quy tắc code : Tuân thủ theo coding convention : ví dụ Linux Kernel Coding style 
- Indent không dùng tab ...





