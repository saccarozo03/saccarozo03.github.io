---
title: "Bài 2. Biến và nhập xúât cơ bản trong C"
date: 2026-02-10
draft: false
series: ["Học lại C"]
weitght: 2
tags: ["learning-log", "c", "linux"]
---
## String way up close (Chuỗi trong ngôn ngữ C)
- `string` chỉ là mảng ký tự 
```c
s = "Shatter";
```
- `C` nhìn chuỗi theo cách sau : nó đọc như là mảng với từng ký tự rời rạc 
```c
s = {'S', 'h', 'a', 't', 'n', 'e', 'r'};
```
- Mỗi ký tự chỉ là 1 phần tử trong chuỗi 
- Đó là lý do tại sao bạn có thể tham chiếu đến từng phần tử ở trong chuỗi bằng cách dùng chỉ mục 
như `s[0]` và `s[1]`
![mô tả về chuỗi nhé](/images/bai2_Nhapxuatchuoi/string.png)
- Vậy `string` khác gì `char`