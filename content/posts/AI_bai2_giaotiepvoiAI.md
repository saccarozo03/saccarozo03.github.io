---
title: "[AI Course] Bài 2: Kỹ năng giao tiếp với AI  "
date: 2026-03-04
draft: false
series: ["Học và sử dụng AI"]
weight: 3

tags: ["learning-log", "AI", "claude", "software-development", "ai-first"]
---
> Giúp giao tiếp thông tin giữa người dùng và AI được truyền đi 2 chiều 
## Phần 0 - Hiểu AI trước khi dùng 
AI "nghĩ như thế nào "
AI ngôn ngữ (Claude , ChatGPT ) không có não thật , nó hoạt động theo nguyên lý :
- Bạn gõ văn bản vào  
- AI nhìn vào toàn bộ văn bản đó 
- Dự đoán từ tiếp theo có xác xuất cao nhất 
- Lặp lại cho đến khi thành câu hoàn chỉnh 
---
### Hệ quả bạn phải nhớ :
- Ai không "hiểu " - nó dự đoán . Câu hỏi rõ - dự đoán đúng hơn 
- AI không biết bạn là ai , bạn muốn gì nếu bạn không nói rõ 
- AI có thể bịa thông tin trông rất thật - gọi là hallunication 
- AI không có bộ nhớ giữa các cuộc trò chuyện , mỗi chat là trang giấy trắng 
- Garbage in , Garbage out : Prompt tệ -> Câu trả lời tệ , dù AI giỏi đến đâu 
## Phần 1 : Prompt là gì ?
***Prompt*** = ***toàn bộ nội dung bạn gửi cho AI*** , không chỉ ***câu hỏi*** - mà là ***bối cảnh + yêu cầu + ràng buộc***
### Giải phẫu một ***prompt*** hoàn chỉnh :
- [Vai trò / Bối cảnh ]
- [Nhiệm vụ yêu cầu cụ thể ]
- [Dữ liệu / Thông tin đầu vào ]
- [Định dạng đầu ra mong muốn ]
- [Ràng buộc /Giới hạn ]
> ví dụ thực tế 
- <span style="color:red"><strong>Prompt tệ : "giải thích pointer"</strong></span>
- <span style="color:green"><strong>Prompt tốt : "Tôi là sinh viên năm 2 mới học C++, chưa hiểu khái niệm pointer.
Hãy giải thích pointer là gì, tại sao cần dùng, kèm ví dụ code
đơn giản nhất có thể. Giải thích từng dòng code. Không dùng
thuật ngữ khó."</strong></span>
## Phần 2 : Các kỹ thuật cơ bản 
### Kỹ thuật 1 : Specificity ( Tính cụ thể ) 
> Là gì , càng nói cụ thể , AI trả lời càng đúng ý .

> Tại sao ? AI phải đoán ý bạn , Bạn nói mơ hồ -> AI đoán theo hướng phổ biển nhất , chưa chắc đúng với ý bạn 
> 4 chiều cụ thể cần có:
<div style="overflow-x: auto;">
  <table style="width: 100%; border-collapse: collapse; table-layout: fixed; border: 1px solid var(--border);">
    <thead>
      <tr style="background-color: var(--code-bg);">
        <th style="border: 1px solid var(--border); padding: 12px; text-align: left; width: 25%;"> Tiêu chí</th>
        <th style="border: 1px solid var(--border); padding: 12px; text-align: left; width: 35%;"> Mơ hồ (Vague)</th>
        <th style="border: 1px solid var(--border); padding: 12px; text-align: left; width: 40%;"> Cụ thể (Specific)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="border: 1px solid var(--border); padding: 12px;"><b>Đối tượng</b></td>
        <td style="border: 1px solid var(--border); padding: 12px;">"Giải thích"</td>
        <td style="border: 1px solid var(--border); padding: 12px;">"Giải thích cho <b>người mới học</b>"</td>
      </tr>
      <tr>
        <td style="border: 1px solid var(--border); padding: 12px;"><b>Phạm vi</b></td>
        <td style="border: 1px solid var(--border); padding: 12px;">"Về Python"</td>
        <td style="border: 1px solid var(--border); padding: 12px;">"Về <b>List Comprehension</b> trong Python"</td>
      </tr>
      <tr>
        <td style="border: 1px solid var(--border); padding: 12px;"><b>Định dạng</b></td>
        <td style="border: 1px solid var(--border); padding: 12px;"><i>(Không yêu cầu)</i></td>
        <td style="border: 1px solid var(--border); padding: 12px;">"Dùng ví dụ, <b>tối đa 5 dòng</b>"</td>
      </tr>
    </tbody>
  </table>
</div>