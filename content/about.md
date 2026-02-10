---
title: "About"
date: 2026-02-10
draft: false
---

<style>
  :root {
    --primary: #007aff;
    --primary-dark: #0051d5;
    --accent: #34c759;
    --bg-card: rgba(255, 255, 255, 0.95);
    --text-main: #1d1d1f;
    --text-sub: #424245;
    --text-light: #86868b;
    --radius: 24px;
    --border: 1px solid rgba(0, 122, 255, 0.08);
    --shadow-sm: 0 4px 20px rgba(0,0,0,0.04);
    --shadow-lg: 0 20px 60px rgba(0,0,0,0.12);
  }

  /* Container tổng */
  .about-container {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "SF Pro Display", sans-serif;
    color: var(--text-main);
    line-height: 1.7;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Header Greeting */
  .hero-section {
    padding: 60px 0 40px;
    text-align: left;
  }

  .status-badge {
    display: inline-flex;
    align-items: center;
    background: linear-gradient(135deg, rgba(52, 199, 89, 0.12) 0%, rgba(52, 199, 89, 0.08) 100%);
    color: #1d7a3a;
    padding: 8px 18px;
    border-radius: 100px;
    font-size: 0.875rem;
    font-weight: 600;
    margin-bottom: 20px;
    border: 1px solid rgba(52, 199, 89, 0.2);
  }

  .status-dot {
    height: 8px;
    width: 8px;
    background: linear-gradient(135deg, #34c759 0%, #30d158 100%);
    border-radius: 50%;
    margin-right: 10px;
    box-shadow: 0 0 12px rgba(52, 199, 89, 0.6);
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.95); }
  }

  .hero-section h1 {
    font-size: 3.5rem;
    font-weight: 800;
    margin: 15px 0;
    background: linear-gradient(135deg, var(--text-main) 0%, var(--primary) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-subtitle {
    font-size: 1.25rem;
    color: var(--text-sub);
    margin-top: 15px;
    line-height: 1.6;
  }

  /* Grid Layout */
  .modern-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 24px;
    margin: 40px 0;
  }

  /* Modern Flat Card */
  .flat-card {
    background: var(--bg-card);
    border: var(--border);
    border-radius: var(--radius);
    padding: 32px;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(20px);
    box-shadow: var(--shadow-sm);
    position: relative;
    overflow: hidden;
  }

  .flat-card::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--primary) 0%, var(--accent) 100%);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .flat-card:hover {
    background: #ffffff;
    border-color: var(--primary);
    transform: translateY(-8px);
    box-shadow: var(--shadow-lg);
  }

  .flat-card:hover::before {
    transform: scaleX(1);
  }

  .flat-card h3 {
    margin: 0 0 20px 0;
    display: flex;
    align-items: center;
    gap: 12px;
    color: var(--text-main);
    font-size: 1.35rem;
    font-weight: 700;
  }

  .flat-card h3 span {
    font-size: 1.5rem;
    filter: grayscale(0.3);
    transition: filter 0.3s;
  }

  .flat-card:hover h3 span {
    filter: grayscale(0);
  }

  .flat-card ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .flat-card li {
    padding: 14px 0 14px 28px;
    border-bottom: 1px solid rgba(0,0,0,0.04);
    color: var(--text-sub);
    font-size: 0.975rem;
    position: relative;
    transition: all 0.3s;
  }

  .flat-card li::before {
    content: "→";
    position: absolute;
    left: 0;
    color: var(--primary);
    font-weight: 700;
    opacity: 0;
    transform: translateX(-5px);
    transition: all 0.3s;
  }

  .flat-card li:hover {
    padding-left: 32px;
    color: var(--primary);
  }

  .flat-card li:hover::before {
    opacity: 1;
    transform: translateX(0);
  }

  .flat-card li:last-child { 
    border-bottom: none; 
  }

  /* Section Titles */
  .section-title {
    font-size: 2rem;
    font-weight: 800;
    margin: 60px 0 30px;
    position: relative;
    padding-left: 20px;
    color: var(--text-main);
  }

  .section-title::before {
    content: "";
    position: absolute;
    left: 0;
    top: 15%;
    height: 70%;
    width: 5px;
    background: linear-gradient(180deg, var(--primary) 0%, var(--accent) 100%);
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 122, 255, 0.3);
  }

  /* Workflow Section */
  .workflow-container {
    background: var(--bg-card);
    border-radius: var(--radius);
    padding: 32px;
    margin-top: 20px;
    border: var(--border);
    box-shadow: var(--shadow-sm);
  }

  .workflow-step {
    display: flex;
    align-items: flex-start;
    margin-bottom: 24px;
    padding: 20px;
    border-radius: 16px;
    transition: all 0.3s;
  }

  .workflow-step:hover {
    background: rgba(0, 122, 255, 0.03);
  }

  .workflow-number {
    flex-shrink: 0;
    width: 48px;
    height: 48px;
    background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
    color: white;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 700;
    font-size: 1.2rem;
    margin-right: 20px;
    box-shadow: 0 4px 12px rgba(0, 122, 255, 0.3);
  }

  .workflow-content {
    flex: 1;
  }

  .workflow-content strong {
    color: var(--text-main);
    font-size: 1.1rem;
    display: block;
    margin-bottom: 6px;
  }

  .workflow-content p {
    margin: 0;
    color: var(--text-sub);
    line-height: 1.6;
  }

  /* Contact Pills */
  .contact-wrapper {
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    margin-top: 24px;
  }

  .contact-pill {
    padding: 14px 28px;
    border-radius: 100px;
    background: linear-gradient(135deg, #f5f5f7 0%, #e8e8ed 100%);
    text-decoration: none;
    color: var(--text-main);
    font-weight: 600;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    border: 1px solid transparent;
    box-shadow: var(--shadow-sm);
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.95rem;
  }

  .contact-pill:hover {
    background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
    color: white;
    transform: translateY(-3px);
    box-shadow: 0 8px 24px rgba(0, 122, 255, 0.35);
  }

  /* Responsive */
  @media (max-width: 768px) {
    .hero-section h1 {
      font-size: 2.5rem;
    }
    
    .hero-subtitle {
      font-size: 1.1rem;
    }
    
    .modern-grid {
      grid-template-columns: 1fr;
    }
    
    .section-title {
      font-size: 1.6rem;
    }
  }

  /* Dark Mode support */
  @media (prefers-color-scheme: dark) {
    :root {
      --bg-card: rgba(30, 30, 32, 0.95);
      --text-main: #f5f5f7;
      --text-sub: #a1a1a6;
      --text-light: #86868b;
      --border: 1px solid rgba(255, 255, 255, 0.08);
      --shadow-sm: 0 4px 20px rgba(0,0,0,0.3);
      --shadow-lg: 0 20px 60px rgba(0,0,0,0.5);
    }
    
    .contact-pill { 
      background: linear-gradient(135deg, #2c2c2e 0%, #1c1c1e 100%);
      color: var(--text-main);
    }
    
    .flat-card {
      background: rgba(30, 30, 32, 0.8);
    }
    
    .flat-card:hover {
      background: rgba(40, 40, 42, 0.95);
    }
    
    .workflow-container {
      background: rgba(30, 30, 32, 0.8);
    }
    
    .workflow-step:hover {
      background: rgba(255, 255, 255, 0.05);
    }
  }

  /* Smooth scroll */
  html {
    scroll-behavior: smooth;
  }
</style>

<div class="about-container">
  <div class="hero-section">
    <div class="status-badge">
      <span class="status-dot"></span> Available for Research & Collaboration
    </div>
    <h1>Chào, mình là Đức 👋</h1>
    <p class="hero-subtitle">
      Kỹ sư Embedded & Computer Vision. Đam mê tối ưu hóa thuật toán trên thiết bị biên (Edge Learning).
    </p>
  </div>

  <div class="modern-grid">
    <div class="flat-card">
      <h3><span>🎯</span>Trọng tâm</h3>
      <ul>
        <li>Firmware/Embedded Software</li>
        <li>Computer Vision (YOLO, OCR)</li>
        <li>Hệ thống Linux & Debugging</li>
      </ul>
    </div>

    <div class="flat-card">
      <h3><span>🛠</span>Công cụ</h3>
      <ul>
        <li>C/C++, GCC, CMake, Makefile</li>
        <li>GDB, Valgrind, Perf</li>
        <li>Git, Docker, Github Actions</li>
      </ul>
    </div>

    <div class="flat-card">
      <h3><span>🚀</span>Project hiện tại</h3>
      <ul>
        <li>License Plate Recognition (YOLO + PaddleOCR)</li>
        <li>Thermal Imaging Processing (Halcon)</li>
        <li>Edge AI on Raspberry Pi 5</li>
      </ul>
    </div>
  </div>

  <h2 class="section-title">🧪 Quy trình làm việc</h2>
  <div class="workflow-container">
    <div class="workflow-step">
      <div class="workflow-number">1</div>
      <div class="workflow-content">
        <strong>Hiểu bài toán</strong>
        <p>Xác định rõ input/output và giới hạn phần cứng.</p>
      </div>
    </div>
    
    <div class="workflow-step">
      <div class="workflow-number">2</div>
      <div class="workflow-content">
        <strong>Thiết kế & Prototype</strong>
        <p>Ưu tiên module hóa và đo lường hiệu năng sớm.</p>
      </div>
    </div>
    
    <div class="workflow-step">
      <div class="workflow-number">3</div>
      <div class="workflow-content">
        <strong>Tài liệu hóa</strong>
        <p>Ghi lại pitfalls và giải pháp để tái sử dụng.</p>
      </div>
    </div>
  </div>

  <h2 class="section-title">📬 Kết nối với mình</h2>
  <div class="contact-wrapper">
    <a href="https://github.com/saccarozo03" class="contact-pill">
      GitHub
    </a>
    <a href="mailto:saccarozo04@gmail.com" class="contact-pill">
      Email
    </a>
    <a href="#" class="contact-pill">
      LinkedIn
    </a>
  </div>
</div>