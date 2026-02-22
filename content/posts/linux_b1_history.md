---
title: "[Linux programing Course] Bài 1: Linux -Câu chuyện về một hệ điều hành thay đổi cả thế giới"
date: 2026-02-21
draft: false
series: ["Linux programing Course"]
weight: 2
tags: ["learning-log", "linux"]
---
Có thể bạn chưa biết :  Bạn đang dùng điện thoại Android , Netflix , Google. Tất cả đều chạy trên linux, nhưng Linux đến từ đâu.
## Phần 1 : Trước khi có Linux - Multics và cú vấp ngã 
- Năm 1965 , những bộ não thiên tài ở Bell Labs , MIT và General Eletric ngồi lại với nhau để tạo ra một hệ điều hành hoàn hảo , họ đặt tên là `Multics (Multiplexed Information and Computing Service)`.
- Nhưng `Multics` quá tham vọng , quá phức tạp và cuối cùng thất bại 

- Bell Labs rút lui , một vài kỹ sư ở lại ở đó và không chịu bỏ cuộc 
## Phần 2 : Unix ra đời - Năm 1969 
- Hai người đàn ông ngồi trong 1 góc của Bell Labs : Ken Thompson và Danis Ritchie 
- Họ đã tạo ra cái tên Unix (Uniplexed Information and Computing System), chơi chữ của Multics. Nếu Multics là "nhiều và phức tạp", thì UNIX là "một và đơn giản ".
### Unix có triết lý rất hay : 
- Mọi thứ đều là file 
- Mỗi chương trình chỉ làm một việc nhưng làm thật tốt
- Kết hợp các chương trình nhỏ để làm chương trình lớn 
Triết lý hay đến mức nó vẫn còn giữ nguyên vẹn đến tận ngày nay trong Linux 
## Phần 3 UNIX bị "đóng" - Và một người nổi giận 
- Trong những năm đầu , UNIX được chia sẻ tự do cho các trường đại học , Các sinh viên ,giáo sư , học hỏi , cải tiến , chia sẻ với nhau thoải mái.
- Nhưng đến thập niên 80s , AT&T - công ty sở hữu Bell Labs - nhận ra UNIX có thể kiếm tiền . Họ đóng cửa lại , bán bản quyền và cấm chia sẻ.
- Một trình viên tài năng tại MIT tên Richard Stallman đã đích thân trải nghiệm sự bực bội này . Ông muốn sửa phần mềm máy in Xerox hay bị kẹt giấy , chỉ để thêm tính năng , thông báo khi giấy bị kẹt . Rất đơn giản nhưng Xerox từ chối cung cấp mã nguồn.
- Stallman nghĩ : "Tại sao lại cấm sửa 1 thứ mà tôi dùng hàng ngày".
- Câu hỏi đó đã thay đổi lịch sử hiện nay.
## Phần 4 GNU và GPL - Cuộc chiến pháp lý vì tự do
- Năm 1983 , Stallman tuyên bố dự án GNU ( GNU's Not UNIX ) một cái tên nhái theo UNIX  . Mục tiêu : Viết lại toàn bộ UNIX từ đầu , hoàn toàn miễn phí và mã nguồn mở
