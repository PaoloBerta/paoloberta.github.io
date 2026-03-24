---
layout: page
permalink: /statistical-tricks/lln/
title: Law of Large Numbers
description: Visualizzazione interattiva della Legge dei Grandi Numeri.
nav: false
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500&display=swap');

  .lln-wrap {
    --bg: #0d0f14;
    --surface: #151820;
    --border: #222733;
    --accent: #e8c547;
    --accent2: #5b9cf6;
    --accent3: #f06292;
    --text: #c8d0e0;
    --text-dim: #5a6275;
    background: var(--bg);
    border-radius: 14px;
    padding: 28px 24px 32px;
    font-family: 'JetBrains Mono', monospace;
    color: var(--text);
    margin: 0 auto;
    max-width: 860px;
  }

  .lln-wrap * { box-sizing: border-box; }

  .lln-subtitle {
    color: var(--text-dim);
    font-size: 0.72rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 24px;
    text-align: center;
  }

  .lln-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px;
    margin-bottom: 16px;
  }

  .lln-dist-row {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }

  .lln-dist-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.68rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 7px 14px;
    border-radius: 6px;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text-dim);
    cursor: pointer;
    transition: all 0.2s;
  }

  .lln-dist-btn.active {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(232,197,71,0.08);
  }

  .lln-dist-btn:hover:not(.active) {
    border-color: var(--text-dim);
    color: var(--text);
  }

  .lln-controls-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 18px;
    margin-bottom: 8px;
  }

  @media (max-width: 600px) {
    .lln-controls-grid { grid-template-columns: 1fr; }
  }

  .lln-ctrl { display: flex; flex-direction: column; gap: 7px; }

  .lln-ctrl label {
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-dim);
  }

  .lln-val-row { display: flex; align-items: center; gap: 10px; }

  .lln-ctrl input[type=range] {
    flex: 1;
    accent-color: var(--accent);
    cursor: pointer;
    height: 3px;
  }

  .lln-val {
    font-size: 0.88rem;
    font-weight: 500;
    color: var(--accent);
    min-width: 36px;
    text-align: right;
  }

  .lln-btn-row {
    display: flex;
    gap: 10px;
    align-items: center;
    flex-wrap: wrap;
    margin-top: 14px;
  }

  .lln-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 9px 18px;
    border-radius: 6px;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text);
    cursor: pointer;
    transition: all 0.2s;
  }

  .lln-btn:hover { border-color: var(--accent); color: var(--accent); }

  .lln-btn.primary {
    background: var(--accent);
    color: #0d0f14;
    border-color: var(--accent);
    font-weight: 700;
  }

  .lln-btn.primary:hover { opacity: 0.85; color: #0d0f14; }

  .lln-canvas-wrap { width: 100%; }
  #lln-cvs { width: 100%; height: auto; display: block; border-radius: 6px; }

  .lln-legend {
    display: flex;
    gap: 18px;
    flex-wrap: wrap;
    margin-top: 12px;
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-dim);
  }

  .lln-legend span { display: flex; align-items: center; gap: 6px; }
  .lln-ldot { width: 11px; height: 11px; border-radius: 2px; flex-shrink: 0; }
  .lln-lline { width: 20px; height: 2px; flex-shrink: 0; }

  .lln-stats-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-top: 14px;
  }

  @media (max-width: 500px) {
    .lln-stats-grid { grid-template-columns: repeat(2, 1fr); }
  }

  .lln-stat {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 7px;
    padding: 10px;
    text-align: center;
  }

  .lln-stat .sl { font-size: 0.6rem; text-transform: uppercase; letter-spacing: 0.09em; color: var(--text-dim); margin-bottom: 3px; }
  .lln-stat .sv { font-size: 0.95rem; font-weight: 500; color: #fff; }

  .lln-formula-row { display: flex; gap: 12px; flex-wrap: wrap; margin-top: 4px; }

  .lln-formula {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 10px 14px;
    font-size: 0.74rem;
    color: var(--text-dim);
    line-height: 1.75;
    flex: 1;
    min-width: 160px;
  }

  .lln-formula b { color: var(--accent); }
  .lln-formula em { color: var(--accent2); font-style: normal; }

  .lln-note {
    font-size: 0.67rem;
    color: var(--text-dim);
    line-height: 1.7;
    margin-top: 10px;
    border-top: 1px solid var(--border);
    padding-top: 10px;
  }

  .lln-progress { font-size: 0.68rem; color: var(--text-dim); margin-left: auto; align-self: center; }
</style>

<div class="lln-wrap">
  <p class="lln-subtitle">X&#772;&#8345; = (X&#8321;+&hellip;+X&#8345;)/n &rarr; &mu; as n &rarr; &infin;</p>

  <div class="lln-card">
    <div class="lln-dist-row">
      <button class="lln-dist-btn active" data-dist="bernoulli">Bernoulli(p)</button>
      <button class="lln-dist-btn" data-dist="uniform">Uniforme(0, b)</button>
      <button class="lln-dist-btn" data-dist="exponential">Esponenziale(&lambda;)</button>
    </div>
    <div class="lln-controls-grid">
      <div class="lln-ctrl">
        <label id="lln-plabel">Probabilit&agrave; p</label>
        <div class="lln-val-row">
          <input type="range" id="lln-p" min="0.05" max="0.95" step="0.05" value="0.3">
          <span class="lln-val" id="lln-pval">0.30</span>
        </div>
      </div>
      <div class="lln-ctrl">
        <label>Percorsi K</label>
        <div class="lln-val-row">
          <input type="range" id="lln-k" min="1" max="15" step="1" value="8">
          <span class="lln-val" id="lln-kval">8</span>
        </div>
      </div>
      <div class="lln-ctrl">
        <label>Max n</label>
        <div class="lln-val-row">
          <input type="range" id="lln-nmax" min="100" max="2000" step="100" value="500">
          <span class="lln-val" id="lln-nmaxval">500</span>
        </div>
      </div>
    </div>
    <div class="lln-btn-row">
      <button class="lln-btn primary" id="lln-run">&#9654; Simula</button>
      <button class="lln-btn" id="lln-anim">&#10227; Anima</button>
      <button class="lln-btn" id="lln-stop" style="display:none">&#9632; Stop</button>
      <span class="lln-progress" id="lln-prog"></span>
    </div>
  </div>

  <div class="lln-card">
    <div class="lln-canvas-wrap">
      <canvas id="lln-cvs" width="820" height="420"></canvas>
    </div>
    <div class="lln-legend">
      <span><span class="lln-lline" style="background:#5b9cf6;opacity:0.7"></span>Medie campionarie X&#772;&#8345;</span>
      <span><span class="lln-lline" style="background:#e8c547;height:2px"></span>Media teorica &mu;</span>
      <span><span class="lln-ldot" style="background:rgba(91,156,246,0.2)"></span>Banda &plusmn;2&sigma;/&radic;n</span>
    </div>
    <div class="lln-stats-grid">
      <div class="lln-stat"><div class="sl">&mu; teorica</div><div class="sv" id="lln-mu">&mdash;</div></div>
      <div class="lln-stat"><div class="sl">&sigma; teorica</div><div class="sv" id="lln-sig">&mdash;</div></div>
      <div class="lln-stat"><div class="sl">X&#772; finale (media K)</div><div class="sv" id="lln-xbar">&mdash;</div></div>
      <div class="lln-stat"><div class="sl">Banda a n<sub>max</sub></div><div class="sv" id="lln-band">&mdash;</div></div>
    </div>
  </div>

  <div class="lln-card">
    <div class="lln-formula-row">
      <div class="lln-formula">
        <b>LGN debole</b><br>
        X&#772;&#8345; <em>&rarr;&#x209a;</em> &mu;<br>
        P(|X&#772;&#8345; &minus; &mu;| &gt; &epsilon;) &rarr; 0
      </div>
      <div class="lln-formula">
        <b>LGN forte</b><br>
        X&#772;&#8345; <em>&rarr; q.c.</em> &mu;<br>
        P(lim X&#772;&#8345; = &mu;) = 1
      </div>
      <div class="lln-formula">
        <b>Tasso di convergenza</b><br>
        Banda: &plusmn;<em>2&sigma;/&radic;n</em><br>
        Var(X&#772;&#8345;) = &sigma;&sup2;/n (dal TLC)
      </div>
      <div class="lln-formula">
        <b>Condizione</b><br>
        X&#8321;,...,X&#8345; i.i.d.<br>
        E[X] = &mu; &lt; <em>&infin;</em>
      </div>
    </div>
    <div class="lln-note">
      <b style="color:#f06292">Come leggere il grafico:</b>
      Ogni curva è un percorso della media campionaria X&#772;&#8345; al crescere di n.
      La linea <span style="color:#e8c547">gialla</span> è la media teorica &mu;.
      La banda <span style="color:#5b9cf6">blu</span> è &plusmn;2&sigma;/&radic;n: si restringe come 1/&radic;n.
      Tutti i percorsi convergono verso &mu; &mdash; questa è la LGN in azione.
      Prova con K=15 e <em>Esponenziale(&lambda;=0.3)</em> per vedere come distribuzioni asimmetriche convergono comunque alla stessa media.
    </div>
  </div>
</div>

<script>
(function () {
  const cvs = document.getElementById('lln-cvs');
  const ctx = cvs.getContext('2d');
  const W = 820, H = 420;
  cvs.width = W; cvs.height = H;

  let dist = 'bernoulli', param = 0.3, K = 8, Nmax = 500;
  let animH = null, animN = 0, paths = null;

  function getMu() {
    if (dist === 'bernoulli') return param;
    if (dist === 'uniform') return param / 2;
    return 1 / param;
  }

  function getSigma() {
    if (dist === 'bernoulli') return Math.sqrt(param * (1 - param));
    if (dist === 'uniform') return param / Math.sqrt(12);
    return 1 / param;
  }

  function sample() {
    if (dist === 'bernoulli') return Math.random() < param ? 1 : 0;
    if (dist === 'uniform') return Math.random() * param;
    return -Math.log(1 - Math.random()) / param;
  }

  function generatePaths() {
    paths = [];
    for (let k = 0; k < K; k++) {
      const path = new Float64Array(Nmax);
      let sum = 0;
      for (let i = 0; i < Nmax; i++) {
        sum += sample();
        path[i] = sum / (i + 1);
      }
      paths.push(path);
    }
  }

  function draw(upToN) {
    const mu = getMu(), sigma = getSigma();
    const dispN = upToN || Nmax;
    const PAD = { l: 62, r: 24, t: 30, b: 48 };
    const PW = W - PAD.l - PAD.r, PH = H - PAD.t - PAD.b;

    const ySpread = 3.2 * sigma / Math.sqrt(Math.min(5, dispN));
    const yMin = mu - Math.max(ySpread, sigma * 0.5);
    const yMax = mu + Math.max(ySpread, sigma * 0.5);

    const cx = n => PAD.l + ((n - 1) / Math.max(dispN - 1, 1)) * PW;
    const cy = v => PAD.t + PH - ((v - yMin) / (yMax - yMin)) * PH;
    const clamp = v => Math.max(PAD.t - 2, Math.min(PAD.t + PH + 2, v));

    // Background
    ctx.clearRect(0, 0, W, H);
    ctx.fillStyle = '#0d0f14';
    ctx.fillRect(0, 0, W, H);

    // Grid
    ctx.strokeStyle = '#1e2230'; ctx.lineWidth = 1;
    for (let i = 0; i <= 5; i++) {
      const y = PAD.t + (PH / 5) * i;
      ctx.beginPath(); ctx.moveTo(PAD.l, y); ctx.lineTo(W - PAD.r, y); ctx.stroke();
    }
    for (let i = 0; i <= 5; i++) {
      const x = PAD.l + (PW / 5) * i;
      ctx.beginPath(); ctx.moveTo(x, PAD.t); ctx.lineTo(x, PAD.t + PH); ctx.stroke();
    }

    // ±2σ/√n band (filled as a shape)
    ctx.beginPath();
    for (let n = 1; n <= dispN; n++) {
      const band = 2 * sigma / Math.sqrt(n);
      const x = cx(n);
      const yTop = clamp(cy(mu + band));
      if (n === 1) ctx.moveTo(x, yTop); else ctx.lineTo(x, yTop);
    }
    for (let n = dispN; n >= 1; n--) {
      const band = 2 * sigma / Math.sqrt(n);
      ctx.lineTo(cx(n), clamp(cy(mu - band)));
    }
    ctx.closePath();
    ctx.fillStyle = 'rgba(91,156,246,0.10)';
    ctx.fill();

    // Paths
    if (paths) {
      for (let k = 0; k < K; k++) {
        ctx.beginPath();
        ctx.strokeStyle = 'rgba(91,156,246,0.50)';
        ctx.lineWidth = 1.3;
        let first = true;
        for (let n = 1; n <= dispN; n++) {
          const x = cx(n), y = cy(paths[k][n - 1]);
          if (first) { ctx.moveTo(x, clamp(y)); first = false; }
          else ctx.lineTo(x, clamp(y));
        }
        ctx.stroke();
      }
    }

    // μ line
    ctx.beginPath();
    ctx.strokeStyle = '#e8c547'; ctx.lineWidth = 2;
    ctx.shadowColor = '#e8c547'; ctx.shadowBlur = 7;
    ctx.moveTo(PAD.l, cy(mu)); ctx.lineTo(W - PAD.r, cy(mu));
    ctx.stroke(); ctx.shadowBlur = 0;

    // Axes
    ctx.strokeStyle = '#3a4055'; ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.moveTo(PAD.l, PAD.t); ctx.lineTo(PAD.l, PAD.t + PH); ctx.lineTo(W - PAD.r, PAD.t + PH);
    ctx.stroke();

    // X labels
    ctx.fillStyle = '#5a6275'; ctx.font = '11px JetBrains Mono'; ctx.textAlign = 'center';
    const ticks = Math.min(6, dispN);
    for (let i = 0; i <= ticks; i++) {
      const n = Math.round(1 + (i / ticks) * (dispN - 1));
      ctx.fillText(n, cx(n), PAD.t + PH + 18);
    }

    // Y labels
    ctx.textAlign = 'right';
    for (let i = 0; i <= 4; i++) {
      const v = yMin + (i / 4) * (yMax - yMin);
      ctx.fillText(v.toFixed(2), PAD.l - 6, cy(v) + 4);
    }

    // Info
    const distLabel = dist === 'bernoulli' ? `Bernoulli(${param.toFixed(2)})`
      : dist === 'uniform' ? `Uniforme(0, ${param.toFixed(1)})`
      : `Esponenziale(\u03bb=${param.toFixed(2)})`;
    ctx.fillStyle = '#fff'; ctx.font = '500 12px JetBrains Mono'; ctx.textAlign = 'left';
    ctx.fillText(`${distLabel}   K=${K}   n=${dispN}`, PAD.l + 4, PAD.t + 18);

    const finalBand = 2 * sigma / Math.sqrt(dispN);
    ctx.fillStyle = '#5b9cf6'; ctx.textAlign = 'right';
    ctx.fillText(`\u00b12\u03c3/\u221an = \u00b1${finalBand.toFixed(3)} @ n=${dispN}`, W - PAD.r - 4, PAD.t + 18);

    // Stats
    document.getElementById('lln-mu').textContent = mu.toFixed(4);
    document.getElementById('lln-sig').textContent = sigma.toFixed(4);
    if (paths) {
      const avg = paths.reduce((s, p) => s + p[dispN - 1], 0) / K;
      document.getElementById('lln-xbar').textContent = avg.toFixed(4);
    }
    document.getElementById('lln-band').textContent = `\u00b1${finalBand.toFixed(4)}`;
  }

  function run() { generatePaths(); draw(); }

  // Distribution buttons
  document.querySelectorAll('.lln-dist-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.lln-dist-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      dist = btn.dataset.dist;
      const label = document.getElementById('lln-plabel');
      const slider = document.getElementById('lln-p');
      const valEl = document.getElementById('lln-pval');
      if (dist === 'bernoulli') {
        label.textContent = 'Probabilit\u00e0 p';
        slider.min = '0.05'; slider.max = '0.95'; slider.step = '0.05'; slider.value = '0.3';
        param = 0.3; valEl.textContent = '0.30';
      } else if (dist === 'uniform') {
        label.textContent = 'Estremo b';
        slider.min = '0.5'; slider.max = '5'; slider.step = '0.5'; slider.value = '2';
        param = 2; valEl.textContent = '2.0';
      } else {
        label.textContent = 'Tasso \u03bb';
        slider.min = '0.2'; slider.max = '2'; slider.step = '0.1'; slider.value = '1';
        param = 1; valEl.textContent = '1.00';
      }
      run();
    });
  });

  document.getElementById('lln-p').addEventListener('input', e => {
    param = +e.target.value;
    document.getElementById('lln-pval').textContent =
      dist === 'uniform' ? param.toFixed(1) : param.toFixed(2);
    run();
  });

  document.getElementById('lln-k').addEventListener('input', e => {
    K = +e.target.value;
    document.getElementById('lln-kval').textContent = K;
    run();
  });

  document.getElementById('lln-nmax').addEventListener('input', e => {
    Nmax = +e.target.value;
    document.getElementById('lln-nmaxval').textContent = Nmax.toLocaleString();
    run();
  });

  document.getElementById('lln-run').addEventListener('click', run);

  document.getElementById('lln-anim').addEventListener('click', () => {
    if (animH) return;
    generatePaths();
    document.getElementById('lln-anim').style.display = 'none';
    document.getElementById('lln-stop').style.display = '';
    animN = 2;
    const step = Math.max(1, Math.floor(Nmax / 180));
    animH = setInterval(() => {
      draw(animN);
      document.getElementById('lln-prog').textContent = `n = ${animN} / ${Nmax}`;
      animN += step;
      if (animN > Nmax) {
        draw(Nmax);
        clearInterval(animH); animH = null;
        document.getElementById('lln-anim').style.display = '';
        document.getElementById('lln-stop').style.display = 'none';
        document.getElementById('lln-prog').textContent = '';
      }
    }, 28);
  });

  document.getElementById('lln-stop').addEventListener('click', () => {
    if (animH) { clearInterval(animH); animH = null; }
    document.getElementById('lln-anim').style.display = '';
    document.getElementById('lln-stop').style.display = 'none';
    document.getElementById('lln-prog').textContent = '';
  });

  run();
})();
</script>
