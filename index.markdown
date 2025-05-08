---
layout: home
---
<style>
  /* Custom styles for header enhancement */
  .site-header {
    border-top: 5px solid #4056A1;
    border-bottom: 1px solid #e8e8e8;
    min-height: 55px;
  }
  .site-nav .page-link:hover {
    color: #4056A1;
  }

  .hero-section {
    text-align: center;
    margin-bottom: 3rem;
  }

  .profile-photo {
    border-radius: 50%;
    width: 200px;
    height: 200px;
    object-fit: cover;
    border: 3px solid #4056A1;
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    margin-bottom: 1.5rem;
  }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
    margin: 2rem 0;
  }

  .skill-card {
    background-color: #f8f9fa;
    padding: 1.5rem;
    border-radius: 8px;
    border-left: 4px solid #4056A1;
    transition: transform 0.2s ease;
  }

  .skill-card:hover {
    transform: translateY(-5px);
  }

  .skill-card h3 {
    color: #4056A1;
    margin-top: 0;
    margin-bottom: 0.5rem;
  }

  .connect-links {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin-top: 2rem;
  }

  .connect-link {
    color: #4056A1;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border: 2px solid #4056A1;
    border-radius: 4px;
    transition: all 0.2s ease;
  }

  .connect-link:hover {
    background-color: #4056A1;
    color: white;
  }

  @media (max-width: 768px) {
    .skills-grid {
      grid-template-columns: 1fr;
    }
  }
</style>

<div class="hero-section">
  <img src="/assets/images/aquib-photo.jpg" alt="Aquib Shaikh" class="profile-photo">
  <h1>Hey There!</h1>
  <p class="lead">I'm a Backend Engineer from Mumbai, India, passionate about crafting scalable, high-performance systems. With expertise in <strong>.NET Core</strong>, <strong>Go</strong>, and <strong>Python</strong>, I build microservices, optimize APIs, and tackle financial infrastructure challenges—like SWIFT messaging and low-latency transactions.</p>
</div>

## What I Do

<div class="skills-grid">
  <div class="skill-card">
    <h3>Financial Systems Architecture</h3>
    <p>Building robust financial infrastructure with SWIFT messaging integration, bond market systems, and cross-border payment solutions.</p>
  </div>
  <div class="skill-card">
    <h3>Backend Engineering</h3>
    <p>Crafting high-performance APIs and microservices using .NET Core, Go, and Python, with a focus on financial data processing and real-time systems.</p>
  </div>
  <div class="skill-card">
    <h3>Data & Integration</h3>
    <p>Designing secure multi-tenant systems, implementing MongoDB and PostgreSQL solutions, and building efficient ETL pipelines for financial data.</p>
  </div>
  <div class="skill-card">
    <h3>DevOps & Infrastructure</h3>
    <p>Managing AWS cloud infrastructure, implementing CI/CD pipelines, and orchestrating containerized applications with Kubernetes.</p>
  </div>
</div>

## Connect
<div class="connect-links">
  <a href="https://linkedin.com/in/aquib-sh" class="connect-link" target="_blank">LinkedIn</a>
  <a href="https://github.com/aquib-sh" class="connect-link" target="_blank">GitHub</a>
  <a href="mailto:shaikhaquib394@gmail.com" class="connect-link">Email</a>
</div>

Want to know more? [Check out my full profile](/about/) or explore my [blog posts](/blog/).
