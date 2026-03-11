---
title: "[Kotlin  Course] Bài 1. Biến và nhập xúât cơ bản trong Kotlin"
date: 2026-02-10
draft: false
series: ["Kotlin"]
weitght: 3
tags: ["learning-log", "c", "linux"]
---
### Hàm trong Kotlin 
- trả về kiểu unit , giống void trong ngôn ngữ C 
- Có thể truyền tham số với tên của nó cho dễ đọc 
- Compact function : viết gọn khi có 1 dòng trong hàm chính 
```kotlin 
fun double(x: Int):Int = x * 2
```
### Lamda và higher order function 
- Lamda : Hàm rút gọn tối đa , viết hàm không cần tên , không có từ khóa `fun` 
- Lamda cấu trúc 
```kotlin
//{Bên trái tham số 1 : kiểu  , tham số 2 : kiểu ->  lệnh thực thi } 
val waterFilter = {level :Int -> level /2  }
``` 
