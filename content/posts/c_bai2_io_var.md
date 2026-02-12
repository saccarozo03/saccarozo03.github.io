---
title: "[C Course] Bài 2. Biến và nhập xúât cơ bản trong C"
date: 2026-02-10
draft: false
series: ["Học lại C"]
weitght: 3
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
- Coi `char` là viên gạch , còn `string` là bức tường 
- Chiếm đúng `1 byte (8 bit)` . Ví dụ ký tự `A` thực chất là số `65`
- `string` là tập hợp mảng nhiều `char` đứng liên tiếp nhau , kích thước của nó thay đổi tùy vào độ dài từng chuỗi 
## Quy tắc "dấu nháy" , đừng bao giờ nhầm lẫn :
### Dấu nháy đơn chỉ dành cho `char` 
- Đúng : 
```c
char c = 'A';
```
- <span style="color:red"><strong>Sai :</strong></span>
```c
char c = "A";
```
### Dấu nháy kép dành cho `string`  
- Đúng :
```c
string c = "I love u !"; 
```
- Đúng :
```c
string c =  "A";
```
### Bí mật của ký tự `\0`
- Khi khai báo `char c = 'A';` , bộ tốn chỉ chứa 1 ô duy nhất chứa số `65`
- Khi bạn khai báo `char c[] = "A";` , bộ nhớ tốn 2 ô , 1 ô chứa ký tự `A` và ô chứa ký tự đặc biệt `\0 (null terminal)`
### Tại sao máy tính cần ký tự `\0`
- Vì máy tính của bạn không biết chuỗi dài bao nhiêu , nó sẽ đọc vào ô đầu tiên chỉ số `c[0]` và chỉ dừng lại tới ô có ký tự `c[end] = '\0'`
- Nếu bạn quên hoặc không có ký tự này , máy tính sẽ không biết dừng ở đâu , đọc tràn sang các bộ nhớ khác 
### Toán tử với chuỗi và mảng ký tự 
- Với mảng ký tự , nó sẽ xử lý ký tự như 1 số học đơn thuần , ví dụ :
```c
char c = 'A';
c =  c + 1 ;
```
- Nó sẽ thực hiện bằng cách : biến ký tự `A` thành `65` , và bắt đầu cộng thêm `1` 
- `c = 65 + 1 = 66`
- Sau đó nó sẽ biến đổi ngược lại từ `66` thành ký tự `B`
- Kết quả `c = 'B'`
- `String` thì không thể  lấy `"Hello" - 1` , Phép cộng thường là Phép nối chuỗi 
- Ví dụ : `"Hello" + "World!"`
### Ví dụ về char 
```c
char *c = "Hello";
```
- Biến `S` không chứa chữ `"Hello"` . nó chỉ chứa tọa độ địa chỉ của ô nhớ đầu tiên nơi chữ `"H"` tọa lạc 
- Vì chữ `"Hello"` nằm trong vùng nhớ chỉ đọc , nên bạn không thể sửa nó  
- Đó là lý do vì sao Gọi `"Hello"` là `hằng số chuỗi ( string literal)` , còn `char a = 'C'` , bạn có thể đổi `c = 'B'` thoải mái vì giá trị có nó nằm ngay `Stack` của hàm đang chạy 

## Lỗi xảy ra với char có thể gặp 
### Nhầm `char` với chuỗi 
```c
char c = 'A' ; // <------ Đúng 
char c = "B" ; // <------ Sai 
```
- <span style="color:red"><strong>Nguyên nhân</strong></span>
- `"A"` là chuỗi `(char[2])`
- `'A'` là chuỗi `char[1]` 
### Gán vượt quá phạm vị của `char`
```c
char c = 300;
```
- char c thường nhận ký tự từ `-128 đến 127` hoặc `0 -> 255`
- Giá trị bị tràn 



 
