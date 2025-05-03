---
layout: page
title: About Me
permalink: /about/
---

<div class="about-container">
  <div class="about-header">
    <h1>About Aquib Shaikh</h1>
    <p class="subtitle">Backend Engineer | Financial Systems Specialist | Cloud Architect</p>
  </div>

  <div class="about-content">
    <div class="about-section">
      <h2>Who I Am</h2>
      <p>I'm a Backend Engineer based in Mumbai, India, with a passion for architecting scalable, high-performance systems. My work revolves around financial systems, enterprise applications, and distributed architectures, leveraging tools like <strong>.NET Core (C#)</strong>, <strong>Go</strong>, and <strong>Python</strong>.</p>
    </div>

    <div class="about-section">
      <h2>What I Do</h2>
      <div class="skills-grid">
        <div class="skill-card">
          <h3>Low-Latency APIs</h3>
          <p>Optimizing performance for financial transactions and real-time systems.</p>
        </div>
        <div class="skill-card">
          <h3>Microservices</h3>
          <p>Designing resilient, distributed systems that scale with your needs.</p>
        </div>
        <div class="skill-card">
          <h3>Database Tuning</h3>
          <p>Speeding up queries with PostgreSQL and MongoDB optimizations.</p>
        </div>
        <div class="skill-card">
          <h3>Cloud & Infra</h3>
          <p>Deploying systems on AWS and Kubernetes for maximum reliability.</p>
        </div>
      </div>
    </div>

    <div class="about-section">
      <h2>Experience</h2>
      <div class="timeline">
        <div class="timeline-item">
          <div class="timeline-header">
            <h3>Software Engineer</h3>
            <span class="company">Oneture Technologies</span>
            <span class="date">Nov 2024 – Present</span>
          </div>
          <div class="timeline-content">
            <ul>
              <li>Modernizing Mirae Asset Capital Markets' financial infrastructure by migrating legacy JSP apps to .NET Core Web APIs.</li>
              <li>Building SWIFT messaging solutions with Alliance Cloud for secure cross-border payments (MT to MX migration).</li>
              <li>Developing a zero-footprint SWIFT SDK for better maintainability.</li>
              <li>Integrating bond market data and transactions seamlessly.</li>
            </ul>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-header">
            <h3>Associate Backend Developer</h3>
            <span class="company">Softgen Solutions LLP</span>
            <span class="date">Sep 2023 – Sep 2024</span>
          </div>
          <div class="timeline-content">
            <ul>
              <li>Created backend services to sync financial data from Tally using .NET and Python Qt.</li>
              <li>Built a secure, multitenant authentication system with PostgreSQL for data isolation.</li>
              <li>Developed APIs and MongoDB services for analytics and sales tracking.</li>
              <li>Designed a PDF generation service and API gateway for routing and security.</li>
            </ul>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-header">
            <h3>Software Developer</h3>
            <span class="company">Furation Tech Solutions Pvt. Ltd.</span>
            <span class="date">Mar 2023 – May 2023</span>
          </div>
          <div class="timeline-content">
            <ul>
              <li>Enhanced Flutter web apps, improving functionality and user experience.</li>
            </ul>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-header">
            <h3>Cyber Security Intern</h3>
            <span class="company">Maharashtra Cyber</span>
            <span class="date">Dec 2022 – Jan 2023</span>
          </div>
          <div class="timeline-content">
            <ul>
              <li>Reverse-engineered malicious apps to uncover vulnerabilities and fraud mechanisms, aiding law enforcement.</li>
            </ul>
          </div>
        </div>

        <div class="timeline-item">
          <div class="timeline-header">
            <h3>Independent Software Developer</h3>
            <span class="company">Self-Employed</span>
            <span class="date">Aug 2020 – Jan 2023</span>
          </div>
          <div class="timeline-content">
            <ul>
              <li>Delivered 150+ projects across 23+ countries with 100% client satisfaction.</li>
              <li>Focused on Python, API integrations, and automation solutions.</li>
              <li>Managed full project lifecycles, from requirements to deployment.</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <div class="about-section">
      <h2>Tech Stack</h2>
      <div class="tech-stack">
        <div class="tech-category">
          <h3>Languages</h3>
          <div class="tech-tags">
            <span class="tech-tag">.NET Core (C#)</span>
            <span class="tech-tag">Go</span>
            <span class="tech-tag">Python</span>
          </div>
        </div>
        <div class="tech-category">
          <h3>Databases</h3>
          <div class="tech-tags">
            <span class="tech-tag">PostgreSQL</span>
            <span class="tech-tag">MongoDB</span>
            <span class="tech-tag">Oracle Database</span>
          </div>
        </div>
        <div class="tech-category">
          <h3>Infrastructure</h3>
          <div class="tech-tags">
            <span class="tech-tag">AWS</span>
            <span class="tech-tag">Kubernetes</span>
            <span class="tech-tag">Linux</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .about-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }

  .about-header {
    text-align: center;
    margin-bottom: 3rem;
  }

  .about-header h1 {
    color: #4056A1;
    margin-bottom: 0.5rem;
  }

  .subtitle {
    color: #666;
    font-size: 1.2rem;
    margin-top: 0;
  }

  .about-section {
    margin-bottom: 3rem;
  }

  .about-section h2 {
    color: #4056A1;
    border-bottom: 2px solid #4056A1;
    padding-bottom: 0.5rem;
    margin-bottom: 1.5rem;
  }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
  }

  .skill-card {
    background-color: #f8f9fa;
    padding: 1.5rem;
    border-radius: 8px;
    border-left: 4px solid #4056A1;
  }

  .skill-card h3 {
    color: #4056A1;
    margin-top: 0;
    margin-bottom: 0.5rem;
  }

  .timeline {
    position: relative;
    padding-left: 2rem;
  }

  .timeline::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 2px;
    background-color: #4056A1;
  }

  .timeline-item {
    position: relative;
    margin-bottom: 2rem;
    padding-left: 2rem;
  }

  .timeline-item::before {
    content: '';
    position: absolute;
    left: -0.5rem;
    top: 0;
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    background-color: #4056A1;
  }

  .timeline-header {
    margin-bottom: 1rem;
  }

  .timeline-header h3 {
    margin: 0;
    color: #4056A1;
  }

  .company {
    display: block;
    color: #666;
    font-weight: 500;
  }

  .date {
    display: block;
    color: #888;
    font-size: 0.9rem;
  }

  .timeline-content ul {
    margin: 0;
    padding-left: 1.5rem;
  }

  .timeline-content li {
    margin-bottom: 0.5rem;
  }

  .tech-stack {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }

  .tech-category h3 {
    color: #4056A1;
    margin-top: 0;
    margin-bottom: 1rem;
  }

  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .tech-tag {
    background-color: #f8f9fa;
    color: #4056A1;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    font-size: 0.9rem;
  }

  @media (max-width: 768px) {
    .skills-grid,
    .tech-stack {
      grid-template-columns: 1fr;
    }

    .timeline {
      padding-left: 1rem;
    }

    .timeline-item {
      padding-left: 1rem;
    }
  }
</style>

## Certifications
- Cross Platform Mobile App Development
- Google IT Automation with Python Professional Certificate
- Web Scraping and API Fundamentals in Python
- Programming, Data Structures, and Algorithms Using Python
- IBM Full Stack Software Developer Specialization

## Let's Connect!
I'm always up for a chat about backend engineering, fintech, or scalable systems. Reach me at [shaikhaquib394@gmail.com](mailto:shaikhaquib394@gmail.com) or connect on [LinkedIn](https://linkedin.com/in/aquib-sh).
