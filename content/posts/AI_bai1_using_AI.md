---
title: "[AI Course] Bài 1: Tổng quan và hướng dẫn AI đọc hiểu project"
date: 2026-02-11
draft: false
series: ["Học và sử dụng AI"]
weight: 2
tags: ["learning-log", "AI", "claude", "software-development", "ai-first"]
---

## Giới thiệu

Trong thời đại AI-first development, việc biết cách giao tiếp hiệu quả với AI không chỉ là một kỹ năng bổ sung mà đã trở thành yếu tố then chốt quyết định năng suất làm việc. Bài viết này sẽ giúp bạn hiểu rõ cách AI hoạt động và làm thế nào để định hướng AI đọc hiểu project một cách chuyên nghiệp.

## Phần 1: Các khái niệm cơ bản về AI

### 1.1. Context Window - Bộ nhớ suy luận của AI

Context Window là không gian bộ nhớ mà AI sử dụng để suy luận và trả lời câu hỏi. Đối với Claude AI, con số này là **1,000,000 tokens** - một con số khổng lồ nhưng vẫn có giới hạn.

**Token là gì?**
- Token có thể hiểu đơn giản là một đơn vị ngữ nghĩa trong câu
- Ví dụ: "I love you" = 3 tokens (I, love, you)
- "Trái đất" tuy là 2 từ nhưng chỉ tính là 1 token
- **1 triệu tokens ≈ 7 quyển Harry Potter**

**Vấn đề khi vượt quá giới hạn:**
- ChatGPT: Trở nên lag, không mượt, phải xóa bớt token đằng trước
- Claude: Nén lại dữ liệu hoặc tạo cuộc hội thoại mới

### 1.2. Attention - Độ tập trung của AI

Trong không gian suy luận, không phải tất cả thông tin đều được AI chú ý như nhau. Đây là một điểm cực kỳ quan trọng khi thiết kế cấu trúc project.

**Nguyên tắc vàng:**
- Thông tin ở **đầu** và **cuối** context window được chú ý nhiều hơn
- Thông tin ở giữa có thể bị "lu mờ" khi context quá dài

### 1.3. Không gian tri thức và quá trình ghép nối

AI hoạt động dựa trên việc ghép nối các trường thông tin với nhau. Hiệu quả của AI phụ thuộc vào:

**Cách bạn dẫn dắt AI:**
- Tri thức phổ biến (hàng triệu người đã hỏi) → AI trả lời rất nhanh
- Tri thức hẹp và hiếm → Cần đặt câu hỏi chính xác và dẫn dắt AI vào không gian tri thức đó

**Kỹ thuật ghép nối:**
- Cần biết ghép nối các trường thông tin với nhau nhiều nhất có thể
- Phụ thuộc vào cách chúng ta đặt câu hỏi và cung cấp context

## Phần 2: Hai cách tiếp cận khi làm việc với AI

### 2.1. Người dùng Amateur (Không chuyên nghiệp)

**Cách làm:**
- Vứt nguyên cả source code cho AI
- Bắt AI tự đọc và tự hiểu

**Hệ quả nguy hiểm:**

#### ⚠️ AI tự tạo ra thông tin ngầm định (Implicit Information)
- AI tự giả định mục đích của project
- AI tự giả định về flow code
- AI tự giả định về cách biên dịch, gỡ lỗi

> **Rất nguy hiểm nếu chúng ta để AI tự giả định thông tin!**

#### ⚠️ AI tuân theo quy tắc mặc định (Default Behavior)
- AI sẽ ưu tiên đọc flow từ **code** hơn là **tài liệu**
- Có thể dẫn đến hiểu sai về ý đồ thiết kế

### 2.2. Người dùng Professional (Chuyên nghiệp)

**Triết lý:**
- AI đọc hiểu thông qua **explicit rules** (quy tắc rõ ràng)
- Hạn chế tối đa việc AI "ngầm hiểu" về project
- Đưa ra chỉ dẫn rõ ràng, cụ thể

**Công cụ: File CLAUDE.md - AI Control Panel**

File này có khả năng **ghi đè behavior mặc định** của AI và luôn nằm trong vùng attention cao nhất.

## Phần 3: CLAUDE.md - Entry Point cho AI

### 3.1. Tại sao cần CLAUDE.md?

File `CLAUDE.md` là điểm vào (entry point) đầu tiên mà AI phải đọc trước khi tiếp cận bất kỳ thứ gì khác trong project. Đây là tài liệu cốt lõi nhất với những đặc điểm:

- **Độ ưu tiên cao nhất**: Dữ liệu luôn nằm trong vùng attention
- **Persistent**: Kể cả khi cuộc hội thoại bị reload, vượt quá 1 triệu token, hoặc bị compressed, dữ liệu vẫn được truyền sang cuộc hội thoại mới
- **Độ dài tối ưu**: Nên dài nhưng không quá 150 dòng

### 3.2. Cấu trúc Project theo chuẩn AI-First

```
ai_mindset/
├── CLAUDE.md           # AI entry point (file này)
├── README.md           # Human entry point
├── docs/
│   ├── architecture.md # Kiến trúc tổng quan
│   ├── api.md          # API specification
│   ├── data-format.md  # Event formats
│   ├── use-cases.md    # Use cases
│   ├── deployment.md   # Deployment guide
│   └── decisions/      # ADRs
├── conversations/      # AI chat logs
├── src/                # Source code
└── tests/              # Test cases
```

### 3.3. Nội dung quan trọng trong CLAUDE.md

#### 1. Project Overview
```markdown
## Project Overview

- **Tên**: ai_mindset
- **Mục đích**: Project mẫu (teaching example) minh họa quy trình phát triển phần mềm theo triết lý AI-first
- **Tech stack**: C / Linux Kernel Module
- **Ngôn ngữ tài liệu**: Tiếng Việt
```

#### 2. Thứ tự đọc (Learning Order)

Đây là phần **CỰC KỲ QUAN TRỌNG** - định hướng AI đọc theo trình tự logic:

```markdown
## Thứ tự đọc (Learning Order)

Khi tiếp cận project này, AI PHẢI đọc theo thứ tự:

1. `CLAUDE.md` (file này) — rules, constraints, context
2. `docs/architecture.md` — kiến trúc tổng quan
3. `src/` — code (ground truth)
4. `tests/` — test cases
5. `docs/decisions/` — ADRs (Architecture Decision Records)
6. `docs/api.md` — API specification
7. `docs/data-format.md` — event formats
8. `docs/use-cases.md` — use cases chi tiết
9. `docs/deployment.md` — deployment guide
10. `conversations/` — lịch sử thảo luận (nếu cần thêm context)
```

> **Nguyên tắc vàng**: Code là source of truth. Khi document và code mâu thuẫn → tin code.

#### 3. Rules - Quy tắc chung

**Nguyên tắc cốt lõi:**
- **Explicit > Implicit**: Không đoán. Nếu không rõ, hỏi lại.
- **Rationale matters**: Mọi quyết định phải kèm lý do (WHY, không chỉ WHAT)
- **Text-first**: Dùng Markdown, Mermaid cho tài liệu. Không dùng binary formats
- **Không tự ý thêm feature** ngoài scope được yêu cầu
- **Commit messages**: Viết bằng tiếng Việt

#### 4. Quy tắc code (C / Kernel)

Ví dụ cụ thể cho project C/Linux Kernel:

```markdown
### Quy tắc code (C / Kernel)

- Tuân thủ [Linux Kernel Coding Style]
- Indent bằng TAB (không dùng spaces)
- Tên biến, hàm: `snake_case`
- Tên macro: `UPPER_SNAKE_CASE`
- Mỗi function tối đa ~50 dòng. Nếu dài hơn, tách ra.
- Comment giải thích WHY, không giải thích WHAT (code phải tự giải thích WHAT)
- Luôn kiểm tra return value của các hàm có thể fail
```

#### 5. Constraints đặc thù (System Programming)

Phần này cực kỳ quan trọng khi làm việc với các domain đặc biệt:

```markdown
### Constraints đặc thù System Programming

AI **KHÔNG THỂ** compile hay chạy kernel code. Workflow bắt buộc:

1. **Human** chạy/debug trên máy thật → cung cấp output
2. **AI** phân tích output → đề xuất fix/giải pháp
3. **Human** verify và apply

Khi cần debug, AI phải yêu cầu human cung cấp:
- `dmesg` output
- Stack trace (nếu có)
- Steps to reproduce
- Kernel version (`uname -r`)
```

### 3.4. Conversation Logs - Lưu trữ tri thức

AI phải lưu lại cuộc hội thoại để tạo ra "knowledge base" cho project:

**Quy tắc đặt tên:**
```
conversations/YYYY-MM-DD_mô-tả-ngắn.md
```

**Thời điểm lưu (tối thiểu):**
- Cuối session (trước khi kết thúc)
- Khi có quyết định quan trọng (architecture, thay đổi rules)
- Khi chuyển sang phase mới trong workflow

**Nội dung:**
- Tóm tắt bối cảnh
- Các quyết định đã đưa ra
- Lý do đằng sau quyết định
- Commits liên quan

## Phần 4: Workflow AI-First Development

Quy trình 5 bước làm việc với AI:

### 1. Brainstorm
- Thảo luận requirement với AI
- Lưu vào `conversations/`

### 2. Vibe Coding
- Prototype nhanh với AI
- Chất lượng thấp, tốc độ cao
- Mục đích: Khám phá ý tưởng

### 3. Review
- Demo và review sequence diagram
- Kiểm tra logic flow

### 4. Production Coding
- Code lại chuẩn
- **Developer PHẢI hiểu từng dòng**
- Áp dụng coding conventions

### 5. Testing
- Bàn giao docs + conversation logs cho tester
- Tester có đầy đủ context để test

## Phần 5: Best Practices

### DO's ✅

1. **Luôn tạo CLAUDE.md trước khi bắt đầu project**
2. **Định nghĩa rõ ràng thứ tự đọc** cho AI
3. **Viết explicit rules**, không để AI tự đoán
4. **Document the WHY**, không chỉ WHAT
5. **Lưu conversation logs** thường xuyên
6. **Code là source of truth** - khi có mâu thuẫn, tin code

### DON'Ts ❌

1. **Không** vứt nguyên source code cho AI
2. **Không** để AI tự giả định về project
3. **Không** dùng binary formats cho documentation
4. **Không** bỏ qua việc review AI-generated code
5. **Không** quên cập nhật CLAUDE.md khi có thay đổi lớn

## Kết luận

Làm việc hiệu quả với AI không chỉ là việc biết prompt hay, mà là việc xây dựng một hệ thống **explicit rules** và **structured information** giúp AI hiểu đúng ý đồ của bạn.

File `CLAUDE.md` không chỉ là một tài liệu - nó là **giao diện điều khiển** (control panel) giúp bạn định hướng AI theo đúng cách bạn muốn. Đầu tư thời gian xây dựng file này sẽ giúp bạn tiết kiệm hàng giờ đồng hồ giao tiếp với AI sau này.

Trong các bài tiếp theo, chúng ta sẽ đi sâu vào:
- Kỹ thuật viết prompts hiệu quả
- Cách thiết kế architecture documents cho AI
- Workflow debugging với AI
- Case studies thực tế

---

**Tài liệu tham khảo:**
- [Linux Kernel Coding Style](https://www.kernel.org/doc/html/latest/process/coding-style.html)
- [Markdown Guide](https://www.markdownguide.org/)
- [Mermaid Documentation](https://mermaid.js.org/)

**Tags**: #AI #ClaudeAI #AIFirst #SoftwareDevelopment #BestPractices #LearningLog