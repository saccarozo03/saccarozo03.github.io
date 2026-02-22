---
title: "[Linux programing Course] Bài 2: Cốt lõi của hệ điều hành - KERNEL"
date: 2026-02-21
draft: false
series: ["Linux programing Course"]
weight: 2
tags: ["learning-log", "linux"]
---
## Định nghĩa cơ bản :
Thuật ngữ hệ điều hành được sử dụng với hai ngữ nghĩa khác nhau 
- **Nghĩa rộng** : Là chương trình lớn bao gồm tất cả các phần mềm : phần mềm điều khiển tài nguyên hệ thống , các phần mềm cơ bản như Command Line , phần mềm đồ họa ,phần mềm chỉnh sửa ...
- **Nghĩa hẹp** : Là phần mềm trung tâm quản lý và phân bổ tài nguyên máy tính 
Từ kernel thường được sử dụng với ý nghĩa thứ 2 , và cũng chính là thứ chúng ta sẽ đi xuyên suốt khóa này 
- Mặc dù có thể chạy chương trình mà không cần kernel , sự hiện diện của nó giúp chúng ta đơn giản hóa rất nhiều việc viết và ghi chương trình khác.
Tệp thực thi kernel thường được nằm trong đường dẫn `/boot/vmlinuz`
## Các công việc mà được thực hiện bởi Kernel
### `(Process Scheduling) Lập lịch quy trình` 
Một máy tính có nhiều hơn 1 bộ xử lý trung tâm ( CPU ) , cái mà thực thi các lệnh của chương trình . Giống như các hệ thống UNIX khác , LINUX là một hệ điều hành `đa nhiệm ưu tiên (preemptive multitasking)`
- Trong đó, `Multitasking (Đa nhiệm)` tức là nhiều tiến trình ( các chương trình đang chạy ) có thể đồng thời nằm trong bộ nhớ và nhận quyền sử dụng (các ) CPU
- `Preemptive (Ưu tiên) ` : Các quy tắc quản lý việc sử dụng CPU của các chương trình trong khoảng thời gian bao lâu bằng kernel process scheduler .
### (Memory Management) Quản lý bộ nhớ :

