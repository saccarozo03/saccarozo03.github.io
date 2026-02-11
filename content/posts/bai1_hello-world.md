---
title: "Bài 1. Hello World"
date: 2026-02-08
draft: false
series: ["Học lại C"]
weight: 2
tags: ["learning-log", "c", "linux"]
---

## C is a language for small, fast programs.

- Tại sao lại cần phải sử dụng C mà không phải bất kỳ ngôn ngữ nào khác ?
- Ngôn ngữ C được dùng trong việc thiết kế các chương trình mà yêu cầu thực thi nhanh, nhỏ gọn. 
- Hơn nữa nó cũng là một ngôn ngữ bậc thấp so với các ngôn ngữ lập trình bậc cao khác, điều này có nghĩa rằng, nó tạo ra các đoạn mã, cái mà gần với mã máy nhất có thể .
## The way C works.
- Máy tính thật sự chỉ hiểu 1 ngôn ngữ duy nhất : **mã máy** , một ngôn ngữ mà chỉ toàn **0** và **1**.
- Việc của bạn cần làm là chuyển mã chương trình ngôn ngữ C sang mã máy.
- Để thực hiện điều này , bạn cần 1 cái gọi là **trình biên dịch** - **compiler**
![Hình ảnh mô tả cách trình biên dịch hoạt động](/images/Bai1_helloworld/code_to_binary.png)
- Việc đầu tiên của bạn cần làm là viết 1 chương trình có đuôi **.c**, với mọi ngôn ngữ bạn viết được chương trình hello world là đã bắt đầu bước chân vào cánh cửa của ngôn ngữ đó. 
- Bạn có thể thử copy đoạn code này vào tất cả các công cụ phát triển tích hợp nào (IDE) để chạy. 
---
```c 
#include <stdio.h>
// Dòng này là các thư viện , các hàm đã được viết sẵn
// Họ đã viết sẵn bằng mã máy 1 số hàm như nhập xuất ,
// Việc của chúng ta là sử  dụng mà thôi 
int main()
{
    printf("Hello world!");
    return 0;
}
```

## Hàm trong toán học và hàm trong ngôn ngữ lập trình 
- Ví dụ bạn đang có hàm f(x) = 2*x +1 , thì x ở đây là biến số , nếu thay x=2 , thì 2 là tham số , ta được hàm mới như sau : f(2) = 2.2+1 
- Tương tự trong ngôn ngữ lập trình, hàm cũng như vậy, 
```c
printf("Hello World!");
```
- Đây là hàm in ra màn hình 1 ký tự hay đoạn văn bản bất kỳ 
- Công việc của bạn chỉ cần truyền tham số nhưng phải đúng theo cú pháp mã ngôn ngữ lập trình đề ra 
- Ở đây mình truyền tham số `hello world!` để in ra màn hình
- Hàm `printf()` đã được người sáng lập ngôn ngữ lập trình viết trước đó rồi, công việc của bạn là tạo ra các hàm khác,Tính căn bậc 2 của 1 số chẳng hạn :v 
- Tiếp đó là giá trị trả về  của 1 hàm số , ví dụ ở hàm `f(x)=2*x+2` với tham số là `2` , giá trị cần trả về là `4` .
```c
return 0;
```
- Ngôn ngữ lập trình cũng vậy, với hàm đặc biệt mà bạn nhìn trên đầu là `main()`, giá trị trả về là `0` , hoặc `1` để báo rằng hàm hoặc chương trình đã kết thúc. 

### Khái niệm hàm trong hàm : 
- hàm `printf()` trong hàm `main()`
- Tưởng tượng giống hàm toán học `f(g(x))` ta coi `g(x)` giống `x` , thì từ đó ta có `f(x)` 