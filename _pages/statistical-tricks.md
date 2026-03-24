---
layout: page
permalink: /statistical-tricks/
title: Statistical Tricks
description: Interactive visualizations of key statistical concepts.
nav: true
nav_order: 7
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap');

  .st-wrap {
    --bg: #0d0f14;
    --surface: #151820;
    --border: #222733;
    --accent: #e8c547;
    --accent2: #5b9cf6;
    --accent3: #f06292;
    --text: #c8d0e0;
    --text-dim: #5a6275;
    font-family: 'JetBrains Mono', monospace;
    color: var(--text);
    max-width: 860px;
    margin: 0 auto;
  }

  .st-intro {
    color: var(--text-dim);
    font-size: 0.78rem;
    line-height: 1.75;
    margin-bottom: 36px;
    letter-spacing: 0.02em;
    border-left: 2px solid var(--border);
    padding-left: 16px;
  }

  .st-intro b { color: var(--accent); }

  .st-section-title {
    font-size: 0.65rem;
    text-transform: uppercase;
    letter-spacing: 0.15em;
    color: var(--text-dim);
    margin-bottom: 14px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }

  .st-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 16px;
    margin-bottom: 40px;
  }

  .st-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 20px;
    text-decoration: none;
    color: var(--text);
    display: flex;
    flex-direction: column;
    gap: 10px;
    transition: border-color 0.2s, transform 0.2s, box-shadow 0.2s;
    position: relative;
    overflow: hidden;
  }

  .st-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--card-color, var(--accent));
    opacity: 0;
    transition: opacity 0.2s;
  }

  .st-card:hover {
    border-color: var(--card-color, var(--accent));
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.4);
    text-decoration: none;
    color: var(--text);
  }

  .st-card:hover::before { opacity: 1; }

  .st-card-icon {
    font-size: 1.6rem;
    line-height: 1;
  }

  .st-card-title {
    font-family: 'Playfair Display', serif;
    font-size: 1.05rem;
    color: #fff;
    font-weight: 700;
  }

  .st-card-desc {
    font-size: 0.7rem;
    color: var(--text-dim);
    line-height: 1.65;
  }

  .st-card-tag {
    display: inline-block;
    font-size: 0.6rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 3px 8px;
    border-radius: 4px;
    background: var(--bg);
    border: 1px solid var(--border);
    color: var(--card-color, var(--accent));
    margin-top: auto;
    align-self: flex-start;
  }

  .st-coming-soon {
    background: var(--surface);
    border: 1px dashed var(--border);
    border-radius: 12px;
    padding: 22px 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    opacity: 0.5;
    cursor: default;
  }

  .st-coming-soon .st-card-title { color: var(--text-dim); }

  .st-coming-badge {
    font-size: 0.6rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 3px 8px;
    border-radius: 4px;
    background: var(--bg);
    border: 1px solid var(--border);
    color: var(--text-dim);
    align-self: flex-start;
    margin-top: auto;
  }
</style>

<div class="st-wrap">

  <p class="st-intro">
    A collection of <b>interactive visualizations</b> for key statistical and econometric concepts.
    Each tool is designed to build intuition — move the sliders, run simulations, and watch the theory come alive.
  </p>

  <div class="st-section-title">Probability &amp; Distributions</div>

  <div class="st-grid">

    <a href="/statistical-tricks/clt/" class="st-card" style="--card-color: #e8c547;">
      <div class="st-card-icon">📊</div>
      <div class="st-card-title">Central Limit Theorem</div>
      <div class="st-card-desc">
        Watch a Bernoulli(p) distribution converge to a Gaussian as the number of repetitions grows.
        Adjust p, n, and sample size in real time.
      </div>
      <span class="st-card-tag">Interactive · Simulation</span>
    </a>

    <a href="/statistical-tricks/lln/" class="st-card" style="--card-color: #5b9cf6;">
      <div class="st-card-icon">🎲</div>
      <div class="st-card-title">Law of Large Numbers</div>
      <div class="st-card-desc">
        Watch K independent running averages converge to μ as n grows.
        Choose Bernoulli, Uniform, or Exponential and animate the convergence.
      </div>
      <span class="st-card-tag">Interactive · Simulation</span>
    </a>

    <div class="st-coming-soon">
      <div class="st-card-icon">🔔</div>
      <div class="st-card-title">Common Distributions</div>
      <div class="st-card-desc">
        Explore the shape, moments, and relationships of key probability distributions.
      </div>
      <span class="st-coming-badge">Coming soon</span>
    </div>

  </div>

  <div class="st-section-title">Causal Inference &amp; Quasi-Experiments</div>

  <div class="st-grid">

    <div class="st-coming-soon">
      <div class="st-card-icon">📈</div>
      <div class="st-card-title">Difference-in-Differences</div>
      <div class="st-card-desc">
        Visualize the parallel trends assumption, treatment effects, and what violations look like.
      </div>
      <span class="st-coming-badge">Coming soon</span>
    </div>

    <div class="st-coming-soon">
      <div class="st-card-icon">📐</div>
      <div class="st-card-title">Regression Discontinuity</div>
      <div class="st-card-desc">
        Explore how RDD identifies causal effects at a threshold, with bandwidth and smoothing controls.
      </div>
      <span class="st-coming-badge">Coming soon</span>
    </div>

    <div class="st-coming-soon">
      <div class="st-card-icon">🔀</div>
      <div class="st-card-title">Instrumental Variables</div>
      <div class="st-card-desc">
        See how a valid instrument isolates exogenous variation and what weak instruments do to estimates.
      </div>
      <span class="st-coming-badge">Coming soon</span>
    </div>

  </div>

</div>
