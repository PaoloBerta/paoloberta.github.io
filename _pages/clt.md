---
layout: page
permalink: /statistical-tricks/clt/
title: Central Limit Theorem
description: Visualizzazione interattiva del Teorema del Limite Centrale con scelta della distribuzione di partenza.
nav: false
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500&display=swap');

  .clt-wrap {
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

  .clt-wrap * { box-sizing: border-box; }

  .clt-subtitle {
    color: var(--text-dim);
    font-size: 0.72rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 24px;
    text-align: center;
  }

  .clt-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px;
    margin-bottom: 16px;
  }

  .clt-dist-row {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }

  .clt-dist-btn {
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

  .clt-dist-btn.active {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(232,197,71,0.08);
  }

  .clt-dist-btn:hover:not(.active) {
    border-color: var(--text-dim);
    color: var(--text);
  }

  .clt-controls-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 18px;
    margin-bottom: 8px;
  }

  @media (max-width: 600px) {
    .clt-controls-grid { grid-template-columns: 1fr; }
  }

  .clt-ctrl { display: flex; flex-direction: column; gap: 7px; }

  .clt-ctrl label {
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text-dim);
  }

  .clt-val-row { display: flex; align-items: center; gap: 10px; }

  .clt-ctrl input[type=range] {
    flex: 1;
    accent-color: var(--accent);
    cursor: pointer;
    height: 3px;
  }

  .clt-val {
    font-size: 0.88rem;
    font-weight: 500;
    color: var(--accent);
    min-width: 36px;
    text-align: right;
  }

  .clt-btn-row {
    display: flex;
    gap: 10px;
    align-items: center;
    flex-wrap: wrap;
    margin-top: 14px;
  }

  .clt-btn {
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

  .clt-btn:hover { border-color: var(--accent); color: var(--accent); }

  .clt-btn.primary {
    background: var(--accent);
    color: #0d0f14;
    border-color: var(--accent);
    font-weight: 700;
  }

  .clt-btn.primary:hover { opacity: 0.85; color: #0d0f14; }

  .clt-canvas-wrap { width: 100%; }
  #clt-cvs { width: 100%; height: auto; display: block; border-radius: 6px; }

  .clt-legend {
    display: flex;
    gap: 18px;
    flex-wrap: wrap;
    margin-top: 12px;
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-dim);
  }

  .clt-legend span { display: flex; align-items: center; gap: 6px; }
  .clt-ldot { width: 11px; height: 11px; border-radius: 2px; flex-shrink: 0; }

  .clt-stats-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-top: 14px;
  }

  @media (max-width: 500px) {
    .clt-stats-grid { grid-template-columns: repeat(2, 1fr); }
  }

  .clt-stat {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 7px;
    padding: 10px;
    text-align: center;
  }

  .clt-stat .sl { font-size: 0.6rem; text-transform: uppercase; letter-spacing: 0.09em; color: var(--text-dim); margin-bottom: 3px; }
  .clt-stat .sv { font-size: 0.95rem; font-weight: 500; color: #fff; }

  .clt-formula-row { display: flex; gap: 12px; flex-wrap: wrap; margin-top: 4px; }

  .clt-formula {
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

  .clt-formula b { color: var(--accent); }
  .clt-formula em { color: var(--accent2); font-style: normal; }

  .clt-note {
    font-size: 0.67rem;
    color: var(--text-dim);
    line-height: 1.7;
    margin-top: 10px;
    border-top: 1px solid var(--border);
    padding-top: 10px;
  }

  .clt-progress { font-size: 0.68rem; color: var(--text-dim); margin-left: auto; align-self: center; }
</style>

<div class="clt-wrap">
  <p class="clt-subtitle">X&#772;&#8345; = (X&#8321;+&hellip;+X&#8345;)/n &rarr; N(&mu;, &sigma;&sup2;/n)</p>

  <div class="clt-card">
    <div class="clt-dist-row">
      <button class="clt-dist-btn active" data-dist="bernoulli">Bernoulli(p)</button>
      <button class="clt-dist-btn" data-dist="uniform">Uniforme(0, b)</button>
      <button class="clt-dist-btn" data-dist="exponential">Esponenziale(&lambda;)</button>
    </div>
    <div class="clt-controls-grid">
      <div class="clt-ctrl">
        <label id="clt-plabel">Probabilit&agrave; p</label>
        <div class="clt-val-row">
          <input type="range" id="clt-p" min="0.05" max="0.95" step="0.05" value="0.3">
          <span class="clt-val" id="clt-pval">0.30</span>
        </div>
      </div>
      <div class="clt-ctrl">
        <label>Ripetizioni n</label>
        <div class="clt-val-row">
          <input type="range" id="clt-n" min="1" max="50" step="1" value="1">
          <span class="clt-val" id="clt-nval">1</span>
        </div>
      </div>
      <div class="clt-ctrl">
        <label>Campioni M</label>
        <div class="clt-val-row">
          <input type="range" id="clt-m" min="500" max="20000" step="500" value="5000">
          <span class="clt-val" id="clt-mval">5000</span>
        </div>
      </div>
    </div>
    <div class="clt-btn-row">
      <button class="clt-btn primary" id="clt-run">&#9654; Simula</button>
      <button class="clt-btn" id="clt-anim">&#10227; Anima n: 1&rarr;50</button>
      <button class="clt-btn" id="clt-stop" style="display:none">&#9632; Stop</button>
      <span class="clt-progress" id="clt-prog"></span>
    </div>
  </div>

  <div class="clt-card">
    <div class="clt-canvas-wrap">
      <canvas id="clt-cvs" width="820" height="420"></canvas>
    </div>
    <div class="clt-legend">
      <span><span class="clt-ldot" style="background:#e8c547;opacity:0.8"></span>Dist. campionaria di X&#772;&#8345;</span>
      <span><span class="clt-ldot" style="background:#5b9cf6;border-radius:50%"></span>Gaussiana teorica N(&mu;, &sigma;&sup2;/n)</span>
      <span><span class="clt-ldot" style="background:#f06292;border-radius:50%"></span>Gaussiana stimata</span>
    </div>
    <div class="clt-stats-grid">
      <div class="clt-stat"><div class="sl">&mu; teorica</div><div class="sv" id="clt-mu">&mdash;</div></div>
      <div class="clt-stat"><div class="sl">&sigma;/&radic;n (err.std)</div><div class="sv" id="clt-sig2">&mdash;</div></div>
      <div class="clt-stat"><div class="sl">X&#772;&#8345; media camp.</div><div class="sv" id="clt-emp">&mdash;</div></div>
      <div class="clt-stat"><div class="sl">Dev.Std camp.</div><div class="sv" id="clt-empsd">&mdash;</div></div>
    </div>
  </div>

  <div class="clt-card">
    <div class="clt-formula-row">
      <div class="clt-formula" id="clt-f-dist">
        <b>Distribuzione</b><br>
        X ~ Bernoulli(<em>p</em>)<br>
        &mu; = <em>p</em>, &sigma;&sup2; = <em>p(1&minus;p)</em>
      </div>
      <div class="clt-formula">
        <b>Media campionaria</b><br>
        X&#772;&#8345; = (X&#8321;+&hellip;+X&#8345;)/n<br>
        Var(X&#772;&#8345;) = <em>&sigma;&sup2;/n</em>
      </div>
      <div class="clt-formula">
        <b>TLC (n&rarr;&infin;)</b><br>
        (X&#772;&#8345; &minus; <em>&mu;</em>) / (<em>&sigma;</em>/&radic;n)<br>
        <em>&rarr; N(0, 1)</em>
      </div>
      <div class="clt-formula">
        <b>Regola pratica</b><br>
        n &ge; 30 in generale<br>
        n &ge; 50 se <em>molto asimmetrica</em>
      </div>
    </div>
    <div class="clt-note">
      <b style="color:#f06292">Come leggere il grafico:</b>
      Le barre mostrano la distribuzione empirica di X&#772;&#8345; su M campioni (densit&agrave;).
      La curva <span style="color:#5b9cf6">blu</span> &egrave; la Gaussiana teorica N(&mu;, &sigma;&sup2;/n).
      La curva <span style="color:#f06292">rosa</span> &egrave; la Gaussiana stimata sui dati.
      Con n=1 vedi la distribuzione originale; all&rsquo;aumentare di n le barre convergono alla curva &mdash; questo &egrave; il TLC in azione.
      Prova <em>Esponenziale</em> con n=1 per vedere la forte asimmetria, poi anima n&rarr;50.
    </div>
  </div>
</div>

<script>
(function () {
  const cvs = document.getElementById('clt-cvs');
  const ctx = cvs.getContext('2d');
  const W = 820, H = 420;
  cvs.width = W; cvs.height = H;

  let dist = 'bernoulli', param = 0.3, n = 1, M = 5000, animH = null;
  const NBINS = 70;

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

  function sampleOne() {
    if (dist === 'bernoulli') return Math.random() < param ? 1 : 0;
    if (dist === 'uniform') return Math.random() * param;
    return -Math.log(1 - Math.random()) / param;
  }

  function gauss(x, mu, sig) {
    return Math.exp(-0.5 * ((x - mu) / sig) ** 2) / (sig * Math.sqrt(2 * Math.PI));
  }

  function simulate() {
    const mu = getMu(), sigma = getSigma();
    const sigXbar = sigma / Math.sqrt(n);
    const spread = Math.max(4.5 * sigXbar, sigma * 0.28);
    let xMin = mu - spread, xMax = mu + spread;
    if (dist === 'exponential') xMin = Math.max(0, xMin);

    const binW = (xMax - xMin) / NBINS;
    const counts = new Array(NBINS).fill(0);
    let sx = 0, sx2 = 0;

    for (let i = 0; i < M; i++) {
      let sum = 0;
      for (let j = 0; j < n; j++) sum += sampleOne();
      const xbar = sum / n;
      sx += xbar; sx2 += xbar * xbar;
      const bin = Math.floor((xbar - xMin) / binW);
      if (bin >= 0 && bin < NBINS) counts[bin]++;
    }

    const em = sx / M, es = Math.sqrt(Math.max(0, sx2 / M - em * em));
    return { counts, em, es, xMin, xMax, binW };
  }

  function getConvergenceInfo() {
    if (dist === 'bernoulli') {
      const npMin = Math.min(n * param, n * (1 - param));
      if (npMin < 5)  return ['⚠ debole', '#f06292'];
      if (npMin < 10) return ['~ buona',  '#e8c547'];
      return ['✓ ottima', '#5b9cf6'];
    }
    if (dist === 'uniform') {
      if (n < 3)  return ['~ buona',  '#e8c547'];
      return ['✓ ottima', '#5b9cf6'];
    }
    // Exponential: skewness = 2
    if (n < 15) return ['⚠ debole', '#f06292'];
    if (n < 30) return ['~ buona',  '#e8c547'];
    return ['✓ ottima', '#5b9cf6'];
  }

  function updateFormulaCard() {
    const el = document.getElementById('clt-f-dist');
    if (dist === 'bernoulli') {
      el.innerHTML = '<b>Distribuzione</b><br>X ~ Bernoulli(<em>p</em>)<br>&mu; = <em>p</em>, &sigma;&sup2; = <em>p(1&minus;p)</em>';
    } else if (dist === 'uniform') {
      el.innerHTML = '<b>Distribuzione</b><br>X ~ Uniforme(0, <em>b</em>)<br>&mu; = <em>b/2</em>, &sigma;&sup2; = <em>b&sup2;/12</em>';
    } else {
      el.innerHTML = '<b>Distribuzione</b><br>X ~ Esponenziale(<em>&lambda;</em>)<br>&mu; = <em>1/&lambda;</em>, &sigma;&sup2; = <em>1/&lambda;&sup2;</em>';
    }
  }

  function draw() {
    const mu = getMu(), sigma = getSigma();
    const sigXbar = sigma / Math.sqrt(n);
    const { counts, em, es, xMin, xMax, binW } = simulate();

    document.getElementById('clt-mu').textContent   = mu.toFixed(4);
    document.getElementById('clt-sig2').textContent = sigXbar.toFixed(4);
    document.getElementById('clt-emp').textContent  = em.toFixed(4);
    document.getElementById('clt-empsd').textContent = es.toFixed(4);

    ctx.clearRect(0, 0, W, H);
    ctx.fillStyle = '#0d0f14'; ctx.fillRect(0, 0, W, H);

    const PAD = { l: 62, r: 24, t: 30, b: 48 };
    const PW = W - PAD.l - PAD.r, PH = H - PAD.t - PAD.b;

    // Density scale
    const densities = counts.map(c => c / (M * binW));
    const maxDens   = Math.max(...densities, 0.001);
    const gPeak     = gauss(mu, mu, Math.max(sigXbar, 1e-9));
    const yS        = Math.max(maxDens, gPeak) * 1.12;

    const cx = v => PAD.l + ((v - xMin) / (xMax - xMin)) * PW;
    const cy = v => PAD.t + PH - (v / yS) * PH;

    // Grid
    ctx.strokeStyle = '#1e2230'; ctx.lineWidth = 1;
    for (let i = 0; i <= 5; i++) {
      const y = PAD.t + (PH / 5) * i;
      ctx.beginPath(); ctx.moveTo(PAD.l, y); ctx.lineTo(W - PAD.r, y); ctx.stroke();
    }

    // Bars (density)
    for (let k = 0; k < NBINS; k++) {
      const dens = densities[k];
      const bx = cx(xMin + k * binW);
      const bw = Math.max(cx(xMin + (k + 1) * binW) - bx - 1, 1);
      const bh = (dens / yS) * PH;
      const by = PAD.t + PH - bh;
      const g  = ctx.createLinearGradient(0, by, 0, by + bh);
      g.addColorStop(0, 'rgba(232,197,71,0.85)');
      g.addColorStop(1, 'rgba(232,197,71,0.2)');
      ctx.fillStyle = g;
      ctx.fillRect(bx + 0.5, by, bw, bh);
      ctx.strokeStyle = 'rgba(232,197,71,0.35)'; ctx.lineWidth = 0.5;
      ctx.strokeRect(bx + 0.5, by, bw, bh);
    }

    // Theoretical Gaussian
    const steps = 400;
    ctx.beginPath(); ctx.strokeStyle = '#5b9cf6'; ctx.lineWidth = 2.5;
    ctx.shadowColor = '#5b9cf6'; ctx.shadowBlur = 8;
    let first = true;
    for (let i = 0; i <= steps; i++) {
      const xv = xMin + (i / steps) * (xMax - xMin);
      const yv = gauss(xv, mu, Math.max(sigXbar, 1e-9));
      const px = cx(xv), py = cy(yv);
      first ? (ctx.moveTo(px, py), first = false) : ctx.lineTo(px, py);
    }
    ctx.stroke(); ctx.shadowBlur = 0;

    // Estimated Gaussian
    if (es > 1e-9) {
      ctx.beginPath(); ctx.strokeStyle = '#f06292'; ctx.lineWidth = 1.5;
      ctx.setLineDash([5, 4]); ctx.shadowColor = '#f06292'; ctx.shadowBlur = 4;
      let fe = true;
      for (let i = 0; i <= steps; i++) {
        const xv = xMin + (i / steps) * (xMax - xMin);
        const yv = gauss(xv, em, es);
        const px = cx(xv), py = cy(yv);
        fe ? (ctx.moveTo(px, py), fe = false) : ctx.lineTo(px, py);
      }
      ctx.stroke(); ctx.setLineDash([]); ctx.shadowBlur = 0;
    }

    // Axes
    ctx.strokeStyle = '#3a4055'; ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.moveTo(PAD.l, PAD.t); ctx.lineTo(PAD.l, PAD.t + PH); ctx.lineTo(W - PAD.r, PAD.t + PH);
    ctx.stroke();

    // X labels
    ctx.fillStyle = '#5a6275'; ctx.font = '11px JetBrains Mono'; ctx.textAlign = 'center';
    for (let i = 0; i <= 6; i++) {
      const xv = xMin + (i / 6) * (xMax - xMin);
      ctx.fillText(xv.toFixed(2), cx(xv), PAD.t + PH + 18);
    }

    // Y labels
    ctx.textAlign = 'right';
    for (let i = 0; i <= 4; i++) {
      const v = yS * i / 4;
      ctx.fillText(v.toFixed(2), PAD.l - 6, cy(v) + 4);
    }

    // Info text
    const distLabel = dist === 'bernoulli' ? `Bernoulli(${param.toFixed(2)})`
      : dist === 'uniform' ? `Uniforme(0, ${param.toFixed(1)})`
      : `Esponenziale(\u03bb=${param.toFixed(2)})`;
    ctx.fillStyle = '#fff'; ctx.font = '500 12px JetBrains Mono'; ctx.textAlign = 'left';
    ctx.fillText(`${distLabel}   n=${n}   M=${M.toLocaleString()}`, PAD.l + 4, PAD.t + 18);

    const [lbl, col] = getConvergenceInfo();
    ctx.fillStyle = col; ctx.font = '11px JetBrains Mono'; ctx.textAlign = 'right';
    ctx.fillText(`Convergenza: ${lbl}`, W - PAD.r - 4, PAD.t + 18);
  }

  // Distribution buttons
  document.querySelectorAll('.clt-dist-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.clt-dist-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      dist = btn.dataset.dist;
      const label  = document.getElementById('clt-plabel');
      const slider = document.getElementById('clt-p');
      const valEl  = document.getElementById('clt-pval');
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
      updateFormulaCard();
      draw();
    });
  });

  document.getElementById('clt-p').addEventListener('input', e => {
    param = +e.target.value;
    document.getElementById('clt-pval').textContent =
      dist === 'uniform' ? param.toFixed(1) : param.toFixed(2);
    draw();
  });

  document.getElementById('clt-n').addEventListener('input', e => {
    n = +e.target.value;
    document.getElementById('clt-nval').textContent = n;
    draw();
  });

  document.getElementById('clt-m').addEventListener('input', e => {
    M = +e.target.value;
    document.getElementById('clt-mval').textContent = M.toLocaleString();
    draw();
  });

  document.getElementById('clt-run').addEventListener('click', draw);

  document.getElementById('clt-anim').addEventListener('click', () => {
    if (animH) return;
    document.getElementById('clt-anim').style.display = 'none';
    document.getElementById('clt-stop').style.display = '';
    let cur = 1;
    animH = setInterval(() => {
      n = cur;
      document.getElementById('clt-n').value = n;
      document.getElementById('clt-nval').textContent = n;
      document.getElementById('clt-prog').textContent = 'n = ' + n + ' / 50';
      draw(); cur++;
      if (cur > 50) {
        clearInterval(animH); animH = null;
        document.getElementById('clt-anim').style.display = '';
        document.getElementById('clt-stop').style.display = 'none';
        document.getElementById('clt-prog').textContent = '';
      }
    }, 120);
  });

  document.getElementById('clt-stop').addEventListener('click', () => {
    if (animH) { clearInterval(animH); animH = null; }
    document.getElementById('clt-anim').style.display = '';
    document.getElementById('clt-stop').style.display = 'none';
    document.getElementById('clt-prog').textContent = '';
  });

  draw();
})();
</script>
