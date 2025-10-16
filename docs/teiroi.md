## TEI / ROI Calculator
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>WFOx — Mission Control for Hybrid Workforces (Mockup v2)</title>
<style>
  /* ---------- THEME TOKENS ---------- */
  :root{
    /* Light (corporate white) */
    --bg:#ffffff; --surface:#f7f9fc; --panel:#ffffff; --panel-2:#f2f5fb; --text:#0b1633; --sub:#506099;
    --line:#e3e8f2; --shadow:0 10px 28px rgba(16,24,40,.08), 0 2px 4px rgba(16,24,40,.06);
    --accent:#2563eb; --accent-2:#7c3aed;
    --ai:#2563eb; --human:#16a34a; --ok:#1a9e6f; --warn:#f59e0b; --bad:#ef4444;
    --chip:#eef2ff; --chip-b:#c7d2fe; --muted:#5b6b99;
  }
  [data-theme="dark"]{
    --bg:#0f1221; --surface:#10142b; --panel:#14183a; --panel-2:#121635; --text:#e7ebff; --sub:#8ea0d0;
    --line:#22295a; --shadow:0 12px 32px rgba(0,0,0,.35), inset 0 1px 0 rgba(255,255,255,.04);
    --accent:#5b8cff; --accent-2:#9b6bff;
    --ai:#5b8cff; --human:#3ad29f; --ok:#42d392; --warn:#ffd166; --bad:#ff6b6b;
    --chip:#1b2147; --chip-b:#2a3060; --muted:#9fb0ee;
  }

  /* ---------- BASE ---------- */
  *{box-sizing:border-box}
  body{margin:0; background:var(--bg); color:var(--text);
       font:14px/1.45 Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial;}
  .wrap{padding:20px; max-width:1400px; margin:0 auto;}
  header{
    display:grid; grid-template-columns:1fr auto auto; gap:16px; align-items:center; margin-bottom:16px;
  }
  .title{font-size:20px; font-weight:800; letter-spacing:.2px; display:flex; gap:10px; align-items:center}
  .badge{font-size:12px; padding:4px 8px; border-radius:999px; background:var(--surface); color:var(--sub); border:1px solid var(--line)}
  .controls{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
  select, button, .pill{
    background:var(--panel); color:var(--text); border:1px solid var(--line);
    padding:10px 12px; border-radius:10px; font:inherit; cursor:pointer;
    box-shadow:var(--shadow);
  }
  select{appearance:none}
  .toggle{
    display:inline-flex; align-items:center; gap:6px; padding:10px 12px; border-radius:10px; border:1px solid var(--line);
    background:var(--panel); box-shadow:var(--shadow); cursor:pointer; user-select:none;
  }
  .kpi-bar{display:flex; gap:14px; flex-wrap:wrap; justify-content:flex-end}
  .kpi{background:var(--panel); padding:10px 12px; border:1px solid var(--line); border-radius:12px; min-width:140px; box-shadow:var(--shadow)}
  .kpi .lbl{color:var(--sub); font-size:12px}
  .kpi .val{font-weight:800; font-size:18px}

  /* ---------- GRID LAYOUT ---------- */
  .grid{display:grid; gap:16px; grid-template-columns:1.25fr 1fr; grid-auto-rows:minmax(240px, auto);}
  .panel{
    background:var(--panel); border:1px solid var(--line); border-radius:16px; padding:16px; box-shadow:var(--shadow);
  }
  .panel h3{margin:0 0 10px; font-size:15px; letter-spacing:.2px}
  .muted{color:var(--muted); font-size:12px}
  .row{display:flex; gap:16px; flex-wrap:wrap}
  .col{flex:1 1 260px}

  /* ---------- CHIP, BUTTONS, TILES ---------- */
  .chip{font-size:12px; border-radius:999px; padding:8px 10px; background:var(--chip); border:1px solid var(--chip-b); color:var(--text)}
  .tile-btn{
    display:inline-flex; align-items:center; gap:8px; padding:10px 12px; border-radius:10px; border:1px solid var(--line);
    background:linear-gradient(180deg, var(--panel), var(--panel-2));
    box-shadow:var(--shadow); position:relative; transition:transform .08s ease, box-shadow .2s ease;
  }
  .tile-btn:hover{transform:translateY(-1px); box-shadow:0 16px 30px rgba(0,0,0,.12)}
  .tile-btn[data-tip]:hover::after{
    content:attr(data-tip); position:absolute; top:-34px; left:0; white-space:nowrap;
    background:#111827; color:#fff; padding:6px 8px; border-radius:8px; font-size:12px;
    box-shadow:0 8px 20px rgba(0,0,0,.25);
  }

  /* ---------- DONUT ---------- */
  .donut{--p:60; width:170px; height:170px; border-radius:50%;
    background:conic-gradient(var(--ai) calc(var(--p)*1%), var(--human) 0);
    position:relative; display:grid; place-items:center;}
  .donut::after{content:""; width:118px; height:118px; background:var(--panel); border-radius:50%;
    border:1px solid var(--line);}
  .donut-label{position:absolute; text-align:center; font-weight:800; z-index:1; line-height:1.1}

  /* ---------- GAUGE ---------- */
  .gauge{--val:82; width:220px; height:110px; border-radius:220px 220px 0 0 / 220px 220px 0 0;
    background:conic-gradient(from 180deg, var(--ok) calc(var(--val)*1%), #e3e8f2 0) top/100% 200% no-repeat;
    position:relative; overflow:hidden; border:1px solid var(--line);}
  .gauge::after{content:""; position:absolute; inset:12px 12px auto 12px; height:calc(100% - 12px);
    background:var(--panel); border-radius:220px 220px 0 0; border:1px solid var(--line);}
  .gauge-read{position:absolute; inset:auto 0 -8px 0; text-align:center; font-weight:800}

  /* ---------- SVG CHARTS ---------- */
  svg{max-width:100%}
  .legend{display:flex; gap:12px; align-items:center; margin-top:6px}
  .dot{width:10px; height:10px; border-radius:50%}
  .aiC{background:var(--ai)} .humanC{background:var(--human)}
  .posC{background:#10b981} .neuC{background:#a6b1e8} .negC{background:#ef4444}

  /* ---------- TABLES / CARDS ---------- */
  table{width:100%; border-collapse:collapse}
  th,td{padding:10px 8px; border-bottom:1px solid var(--line); text-align:left; font-size:13px}
  th{color:var(--sub); font-weight:700}
  .right{text-align:right}
  .ok{color:var(--ok)} .warn{color:var(--warn)} .bad{color:var(--bad)}
  .cards{display:grid; grid-template-columns: repeat(2, minmax(200px,1fr)); gap:12px}
  .card{background:var(--surface); border:1px solid var(--line); border-radius:12px; padding:12px; box-shadow:var(--shadow)}
  .card h4{margin:0 0 6px; font-size:13px}

  /* ---------- SIDEBAR ---------- */
  .sidebar{position:sticky; top:12px; align-self:start; background:var(--panel); border:1px solid var(--line);
    border-radius:16px; padding:16px; box-shadow:var(--shadow)}
  .rec{border-left:3px solid var(--accent); padding-left:10px; margin:12px 0; background:var(--panel-2)}

  /* ---------- FOOTER ---------- */
  footer{margin:18px 0 40px; display:flex; gap:12px; align-items:center; justify-content:space-between; flex-wrap:wrap}
  .status{display:flex; gap:10px; align-items:center}
  .bullet{width:10px; height:10px; border-radius:50%}
  .green{background:var(--ok)} .yellow{background:var(--warn)} .red{background:var(--bad)}

  @media (max-width:1100px){.grid{grid-template-columns:1fr}}
</style>
</head>
<body>
<div class="wrap">
  <!-- HEADER -->
  <header aria-label="Global Controls">
    <div class="title">
      <span style="display:inline-block;width:10px;height:10px;border-radius:3px;background:linear-gradient(180deg,var(--ai),var(--accent-2));"></span>
      WFOx — Mission Control for Hybrid Workforces
      <span class="badge">Hybrid (AI + Human)</span>
    </div>
    <div class="controls">
      <select id="persona">
        <option value="manager">Persona: Contact Center Manager</option>
        <option value="cxd">Persona: CX Director</option>
        <option value="aiops">Persona: AI Ops Strategist</option>
        <option value="fin">Persona: Finance/Exec</option>
      </select>
      <select id="dateRange">
        <option>Last 7 days</option><option selected>Last 30 days</option><option>Quarter to date</option><option>Custom…</option>
      </select>
      <select id="region">
        <option>Global</option><option>AMER</option><option>EMEA</option><option>APJC</option>
      </select>
      <label class="toggle" id="themeToggle">
        <input type="checkbox" id="themeChk" style="accent-color:var(--accent)"/>
        <span>Dark Mode</span>
      </label>
    </div>
    <div class="kpi-bar">
      <div class="kpi"><div class="lbl">Projected Savings</div><div class="val" id="kpiSavings">$1.80M</div></div>
      <div class="kpi"><div class="lbl">CSAT Impact</div><div class="val" id="kpiCsat">-0.2</div></div>
      <div class="kpi"><div class="lbl">CX Health</div><div class="val" id="kpiCxHealth">82</div></div>
    </div>
  </header>

  <div class="grid">
    <!-- AI vs HUMAN MIX -->
    <section class="panel" aria-labelledby="mixTitle">
      <div class="row" style="justify-content:space-between; align-items:center;">
        <h3 id="mixTitle">AI vs Human Mix & Performance</h3>
        <button class="tile-btn" data-tip="Coming soon">View in Webex Contact Center</button>
      </div>
      <div class="row">
        <div class="col" style="display:flex; gap:16px; align-items:center;">
          <div class="donut" id="mixDonut" style="--p:60">
            <div class="donut-label">
              <div style="font-size:26px" id="mixDonutAI">60%</div>
              <div class="muted" style="font-size:11px">AI Coverage</div>
            </div>
          </div>
          <div>
            <div class="chip">AI Efficiency: <b id="aiEff">92%</b></div>
            <div class="chip">AHT — AI: <b id="ahtAI">1.8m</b> | Human: <b id="ahtHuman">4.2m</b></div>
            <div class="chip">Deflection: <b id="deflect">34%</b></div>
            <div class="chip">AI Coverage <input id="aiSlider" type="range" min="0" max="100" value="60" style="vertical-align:middle; margin-left:6px"/> <b id="aiPct">60%</b></div>
            <p class="muted">Adjust AI coverage to see cost & CSAT change in real time.</p>
          </div>
        </div>
        <div class="col">
          <div style="display:flex; gap:16px; align-items:flex-end; flex-wrap:wrap">
            <div>
              <div class="gauge" id="cxGauge" style="--val:82"></div>
              <div class="gauge-read"><span id="cxGaugeVal">82</span>/100 CX Health</div>
            </div>
            <!-- Enhanced 24h trend -->
            <svg id="mixTrend" width="380" height="140" role="img" aria-label="24h AI vs Human mix">
              <defs>
                <linearGradient id="gradAI" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="var(--ai)" stop-opacity="0.55"/>
                  <stop offset="100%" stop-color="var(--ai)" stop-opacity="0.05"/>
                </linearGradient>
                <linearGradient id="gradHuman" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="var(--human)" stop-opacity="0.45"/>
                  <stop offset="100%" stop-color="var(--human)" stop-opacity="0.05"/>
                </linearGradient>
                <filter id="glow">
                  <feGaussianBlur stdDeviation="3" result="coloredBlur"/>
                  <feMerge>
                    <feMergeNode in="coloredBlur"/><feMergeNode in="SourceGraphic"/>
                  </feMerge>
                </filter>
              </defs>
              <rect x="0" y="0" width="380" height="140" fill="var(--panel)" stroke="var(--line)"/>
              <!-- paths added by JS -->
            </svg>
            <div class="legend">
              <span class="dot aiC"></span><span class="muted">AI</span>
              <span class="dot humanC"></span><span class="muted">Human</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CX HEALTH & SENTIMENT -->
    <section class="panel" aria-labelledby="cxTitle">
      <div class="row" style="justify-content:space-between; align-items:center;">
        <h3 id="cxTitle">CX Health & Sentiment Trends</h3>
        <button class="tile-btn" data-tip="Coming soon">View in Webex Engage</button>
      </div>
      <div class="row">
        <div class="col">
          <svg id="sentimentChart" width="100%" height="180" viewBox="0 0 380 180" role="img" aria-label="30-day sentiment">
            <rect x="0" y="0" width="380" height="180" fill="var(--panel)" stroke="var(--line)"/>
            <!-- axes & lines via JS -->
          </svg>
          <div class="legend">
            <span class="dot posC"></span><span class="muted">Positive</span>
            <span class="dot" style="background:#a6b1e8"></span><span class="muted">Neutral</span>
            <span class="dot" style="background:#ef4444"></span><span class="muted">Negative</span>
          </div>
          <p class="muted" id="sentimentStats" style="margin:.5rem 0 0">—</p>
        </div>
        <div class="col">
          <div class="cards">
            <div class="card">
              <h4>Top Topics (Last 30d)</h4>
              <div class="muted">refunds, shipping delays, login, promo code, warranty, address change</div>
            </div>
            <div class="card">
              <h4>Demographic CSAT</h4>
              <table>
                <tr><th>Segment</th><th class="right">AI Pref</th><th class="right">ΔCSAT</th></tr>
                <tr><td>Gen Z</td><td class="right ok">+AI</td><td class="right ok">+0.6</td></tr>
                <tr><td>Millennials</td><td class="right ok">+AI</td><td class="right ok">+0.3</td></tr>
                <tr><td>Gen X</td><td class="right">Neutral</td><td class="right">+0.0</td></tr>
                <tr><td><b>Boomers</b></td><td class="right bad">+Human</td><td class="right bad">-0.7</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ROI & SAVINGS -->
    <section class="panel" aria-labelledby="roiTitle">
      <div class="row" style="justify-content:space-between; align-items:center;">
        <h3 id="roiTitle">Operational Efficiency & ROI Simulator</h3>
        <button class="tile-btn" data-tip="Coming soon">View in Webex Connect</button>
      </div>
      <div class="row">
        <div class="col">
          <table>
            <tr><th>Metric</th><th class="right">Value</th></tr>
            <tr><td>Cost per Contact — AI</td><td class="right" id="cpcAI">$0.42</td></tr>
            <tr><td>Cost per Contact — Human</td><td class="right" id="cpcHuman">$3.10</td></tr>
            <tr><td>Current Savings (YTD)</td><td class="right" id="curSavings">$1.80M</td></tr>
            <tr><td>Potential Uplift (↑ AI +10%)</td><td class="right" id="uplift">$240K</td></tr>
            <tr><td>Cost / Contact (Weighted)</td><td class="right" id="weightedCpc">$1.87</td></tr>
            <tr><td>Annualized Impact</td><td class="right" id="annualImpact">$2.04M</td></tr>
          </table>
        </div>
        <div class="col">
          <div class="card">
            <h4>Scenario Controls</h4>
            <div class="row" style="align-items:center; gap:12px; flex-wrap:wrap">
              <div class="chip">AI Coverage: <b id="aiPctChip">60%</b></div>
              <div class="chip">Min CSAT Threshold <input id="csatThreshold" type="range" min="70" max="95" value="85" style="vertical-align:middle; margin-left:6px"/></div>
              <div class="chip">Projected ROI: <b id="projRoi">+128%</b></div>
              <div class="chip">CX Risk: <b id="cxRisk" class="ok">Low</b></div>
            </div>
          </div>
          <div class="cards" style="margin-top:10px">
            <div class="card">
              <h4>Current State</h4>
              <div class="muted small">AI 60% • CX Health 82 • Cost/Contact $1.87</div>
            </div>
            <div class="card">
              <h4>Predicted</h4>
              <div class="muted small" id="predictedCard">AI 60% • CX Health 82 • Cost/Contact $1.87</div>
            </div>
            <div class="card">
              <h4>Ideal (Model)</h4>
              <div class="muted small">AI 72–78% • CX ≥ 80 • Cost/Contact ≤ $1.60</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- GAMIFICATION -->
    <section class="panel" aria-labelledby="gameTitle">
      <div class="row" style="justify-content:space-between; align-items:center;">
        <h3 id="gameTitle">Gamification & Leaderboards</h3>
        <button class="tile-btn" data-tip="Coming soon">View in Webex AI Agent</button>
      </div>
      <div class="row">
        <div class="col">
          <table>
            <tr><th>Team</th><th class="right">Savings</th><th class="right">ΔCSAT</th><th class="right">Opt. Score</th><th>Badges</th></tr>
            <tr><td>North AM</td><td class="right">$420K</td><td class="right ok">+0.6</td><td class="right">892</td><td>🏁 ⚙️ 💬</td></tr>
            <tr><td>EMEA</td><td class="right">$380K</td><td class="right">+0.1</td><td class="right">861</td><td>⚙️ 💬</td></tr>
            <tr><td>APJC</td><td class="right">$315K</td><td class="right bad">-0.3</td><td class="right">803</td><td>🏁</td></tr>
          </table>
          <div class="muted small">Badges: 🏁 Efficiency • ⚙️ Tuning • 💬 CX</div>
        </div>
        <div class="col">
          <div class="card">
            <h4>This Week’s Challenge</h4>
            <p class="muted small">Reduce AHT by 5% using AI Assist on Tier-1 “order status” intents.</p>
            <div class="row">
              <div class="chip">North AM — 46%</div>
              <div class="chip">EMEA — 72%</div>
              <div class="chip">APJC — 28%</div>
            </div>
          </div>
          <div class="card">
            <h4>Achievements</h4>
            <div class="row">
              <div class="chip">Deflection > 30%</div>
              <div class="chip">AHT ↓ 10%</div>
              <div class="chip">CSAT ≥ 90% (30 days)</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SIDEBAR -->
    <aside class="sidebar" aria-label="Smart Recommendations">
      <h3>Smart Recommendations</h3>
      <div class="rec" id="rec1">Shift +12% of “order status” to AI after 8pm local; expected savings $68K/qtr; ΔCSAT -0.1.</div>
      <div class="rec" id="rec2">Retrain refund intent model; underperforms by 0.6% vs target.</div>
      <div class="rec" id="rec3">Offer microbreaks for Team APJC; burnout risk trending ↑.</div>
      <button id="applyRec" class="tile-btn" data-tip="Coming soon-ish">Simulate Impact</button>
      <p class="muted small">Recommendations adapt when AI Coverage or CSAT threshold changes.</p>
    </aside>
  </div>

  <!-- FOOTER -->
  <footer>
    <div class="status">
      <div class="bullet green" title="Compliant"></div>
      <div class="muted">Compliance: SOC2 & ISO 27001 • AI Fairness Score: <b id="fairScore">93</b></div>
    </div>
    <div class="muted small">Mockup only — figures simulated for demo purposes.</div>
  </footer>
</div>

<script>
/* ----------- Theme toggle ----------- */
const themeChk = document.getElementById('themeChk');
themeChk.addEventListener('change', ()=>{
  document.documentElement.setAttribute('data-theme', themeChk.checked? 'dark' : 'light');
});

/* ----------- State ----------- */
const S = {
  aiPct: 60,
  interactions: 100000, // per period (simulated)
  cpcAI: 0.42,
  cpcHuman: 3.10,
  baseCsat: 85,
  csatPenaltyPer10PctAI: 0.25,
  aiEff: 0.92,
  ahtAI: 1.8, ahtHuman: 4.2,
  deflect: 0.34
};
const $ = (id)=>document.getElementById(id);
const fmtMoney=(n)=> (n<1000? "$"+n.toFixed(2) : "$"+(n/1000).toFixed(0)+"K");
const fmtBig=(n)=> n>=1e6 ? "$"+(n/1e6).toFixed(2)+"M" : n>=1e3? "$"+(n/1e3).toFixed(0)+"K" : "$"+n.toFixed(0);

/* ----------- Core calculators ----------- */
function projectedCosts(aiPct){
  const aiCount = S.interactions * (aiPct/100);
  const humanCount = S.interactions - aiCount;
  return aiCount*S.cpcAI + humanCount*S.cpcHuman;
}
function csatFromAI(aiPct){
  const penalty = (aiPct-50)/10 * S.csatPenaltyPer10PctAI;
  let cs = S.baseCsat - penalty;
  if (aiPct>=55 && aiPct<=78) cs += 0.6; // sweet spot
  return Math.max(70, Math.min(95, +cs.toFixed(1)));
}
function cxHealth(aiPct){
  const cs = csatFromAI(aiPct);
  const effBoost = (aiPct/100)*10;
  return Math.round(Math.max(60, Math.min(100, cs*0.8 + effBoost)));
}
function savingsVsBaseline(aiPct){
  const base = projectedCosts(40);
  const cur  = projectedCosts(aiPct);
  return base - cur;
}
function roiPercent(aiPct){
  const save = savingsVsBaseline(aiPct);
  const invest = 500000;
  return Math.round((save / invest) * 100);
}

/* ----------- Enhanced Mix Trend (smooth gradient areas) ----------- */
function drawMixTrend(aiPct){
  const svg = $('mixTrend');
  while (svg.lastChild && svg.childNodes.length>1) svg.removeChild(svg.lastChild);
  const w=380,h=140,pad=16;
  const steps=32;
  const xStep=(w - pad*2)/(steps-1);

  // Generate a smooth AI percentage across 24h-ish with small oscillations around current aiPct
  const pts=[];
  for(let i=0;i<steps;i++){
    const t=i/(steps-1);
    const osc = Math.sin(t*Math.PI*2)*6 + Math.cos(t*Math.PI*1.5)*3;
    const val = Math.max(8, Math.min(95, aiPct + osc));
    const x = pad + i*xStep;
    const yAI = pad + (100-val)/100 * (h - pad*2);
    pts.push([x, yAI, val]);
  }

  // Helpers to make a smooth path
  const pathFrom = (arr, baselineY)=>{
    const d = [];
    d.push(`M ${arr[0][0]},${arr[0][1]}`);
    for(let i=1;i<arr.length;i++){
      const [x,y] = arr[i];
      const [px,py] = arr[i-1];
      const cx = (px + x)/2;
      d.push(`C ${cx},${py} ${cx},${y} ${x},${y}`);
    }
    // close to baseline
    d.push(`L ${arr[arr.length-1][0]},${baselineY}`);
    d.push(`L ${arr[0][0]},${baselineY} Z`);
    return d.join(' ');
  };

  const areaAI = document.createElementNS('http://www.w3.org/2000/svg','path');
  areaAI.setAttribute('d', pathFrom(pts, h-pad));
  areaAI.setAttribute('fill', 'url(#gradAI)');
  areaAI.setAttribute('filter','url(#glow)');
  svg.appendChild(areaAI);

  // Human area is the inverse
  const ptsHuman = pts.map(([x,y,val])=>{
    const yH = pad + (val/100) * (h - pad*2);
    return [x, yH];
  });
  const areaH = document.createElementNS('http://www.w3.org/2000/svg','path');
  areaH.setAttribute('d', pathFrom(ptsHuman, h-pad));
  areaH.setAttribute('fill', 'url(#gradHuman)');
  areaH.setAttribute('filter','url(#glow)');
  svg.appendChild(areaH);

  // Outline line for AI for crispness
  const lineAI = document.createElementNS('http://www.w3.org/2000/svg','path');
  lineAI.setAttribute('d', (()=> {
    const d = [`M ${pts[0][0]},${pts[0][1]}`];
    for(let i=1;i<pts.length;i++){
      const [x,y] = pts[i];
      const [px,py] = pts[i-1];
      const cx=(px+x)/2;
      d.push(`C ${cx},${py} ${cx},${y} ${x},${y}`);
    }
    return d.join(' ');
  })());
  lineAI.setAttribute('fill','none');
  lineAI.setAttribute('stroke','var(--ai)');
  lineAI.setAttribute('stroke-width','2');
  svg.appendChild(lineAI);
}

/* ----------- Sentiment (fake 30-day) ----------- */
const sentimentData = (()=> {
  // create 30 points trending slightly positive
  const days=30;
  const pos=[], neu=[], neg=[];
  let p=62, n=26, g=12;
  for(let i=0;i<days;i++){
    p = Math.max(48, Math.min(78, p + (Math.random()*4-2) + (i%7===0?2:0)));
    n = Math.max(15, Math.min(35, n + (Math.random()*2-1)));
    g = Math.max(5 , Math.min(22, 100 - p - n + (Math.random()*2-1)));
    pos.push(p); neu.push(n); neg.push(g);
  }
  return {pos, neu, neg};
})();

function drawSentiment(){
  const svg = $('sentimentChart');
  while (svg.lastChild && svg.childNodes.length>1) svg.removeChild(svg.lastChild);
  const w=380,h=180,pad=24;
  // grid
  const grid=document.createElementNS('http://www.w3.org/2000/svg','g');
  for(let i=1;i<4;i++){
    const y=pad + i*((h-pad*2)/4);
    const line=document.createElementNS('http://www.w3.org/2000/svg','line');
    line.setAttribute('x1', pad); line.setAttribute('x2', w-pad);
    line.setAttribute('y1', y); line.setAttribute('y2', y);
    line.setAttribute('stroke', getComputedStyle(document.documentElement).getPropertyValue('--line'));
    svg.appendChild(line);
  }
  svg.appendChild(grid);

  const xStep=(w - pad*2)/(sentimentData.pos.length-1);
  function pathFor(arr, color){
    const pts = arr.map((v,i)=>[pad + i*xStep, pad + (100-v)/100*(h - pad*2)]);
    const d=[`M ${pts[0][0]},${pts[0][1]}`];
    for(let i=1;i<pts.length;i++){
      const [x,y]=pts[i], [px,py]=pts[i-1]; const cx=(px+x)/2;
      d.push(`C ${cx},${py} ${cx},${y} ${x},${y}`);
    }
    const p=document.createElementNS('http://www.w3.org/2000/svg','path');
    p.setAttribute('d', d.join(' '));
    p.setAttribute('fill','none');
    p.setAttribute('stroke', color);
    p.setAttribute('stroke-width','2');
    svg.appendChild(p);
  }
  pathFor(sentimentData.pos, '#10b981'); // positive
  pathFor(sentimentData.neu, '#a6b1e8'); // neutral
  pathFor(sentimentData.neg, '#ef4444'); // negative

  // Stats
  const avg = a => (a.reduce((s,v)=>s+v,0)/a.length);
  const pAvg=avg(sentimentData.pos), nAvg=avg(sentimentData.neu), gAvg=avg(sentimentData.neg);
  $('sentimentStats').textContent = `Avg Positive ${pAvg.toFixed(1)}% • Neutral ${nAvg.toFixed(1)}% • Negative ${gAvg.toFixed(1)}% (30d)`;
}

/* ----------- UI updates ----------- */
function updateAll(aiPct=S.aiPct){
  S.aiPct = aiPct = Math.round(aiPct);
  $('aiPct').textContent = aiPct + '%';
  $('aiPctChip').textContent = aiPct + '%';
  $('mixDonut').style.setProperty('--p', aiPct);
  $('mixDonutAI').textContent = aiPct + '%';
  $('aiEff').textContent = Math.round(S.aiEff*100) + '%';
  $('ahtAI').textContent = S.ahtAI.toFixed(1)+'m';
  $('ahtHuman').textContent = S.ahtHuman.toFixed(1)+'m';
  $('deflect').textContent = Math.round(S.deflect*100)+'%';

  // costs & savings
  const save = savingsVsBaseline(aiPct);
  $('kpiSavings').textContent = fmtBig(save);
  $('curSavings').textContent = fmtBig(save);
  const upl = savingsVsBaseline(Math.min(100, aiPct+10)) - save;
  $('uplift').textContent = fmtBig(upl);

  // weighted CPC & annual impact
  const totalCost = projectedCosts(aiPct);
  const cpc = totalCost / S.interactions;
  $('weightedCpc').textContent = '$'+cpc.toFixed(2);
  $('annualImpact').textContent = fmtBig(save * 1.1); // fudge factor for annualization demo

  // csat / CX health
  const cs = csatFromAI(aiPct);
  $('kpiCsat').textContent = ((cs - S.baseCsat)>=0? '+' : '') + (cs - S.baseCsat).toFixed(1);
  const health = cxHealth(aiPct);
  $('kpiCxHealth').textContent = health;
  $('cxGauge').style.setProperty('--val', health);
  $('cxGaugeVal').textContent = health;

  // Predicted card
  $('predictedCard').textContent = `AI ${aiPct}% • CX Health ${health} • Cost/Contact $${cpc.toFixed(2)}`;

  // ROI + risk
  const roi = roiPercent(aiPct);
  $('projRoi').textContent = (roi>=0? '+' : '') + roi + '%';
  const thr = +$('csatThreshold').value;
  const riskEl = $('cxRisk');
  if (cs < thr) { riskEl.textContent='High'; riskEl.className='bad'; }
  else if (cs < thr+1) { riskEl.textContent='Medium'; riskEl.className='warn'; }
  else { riskEl.textContent='Low'; riskEl.className='ok'; }

  // charts
  drawMixTrend(aiPct);
  drawSentiment();
}

/* ----------- Events ----------- */
$('aiSlider').addEventListener('input', e => updateAll(+e.target.value));
$('csatThreshold').addEventListener('input', ()=> updateAll(S.aiPct));
$('applyRec').addEventListener('click', ()=>{
  const next = Math.min(100, S.aiPct + 8);
  $('aiSlider').value = next; updateAll(next);
});

/* init */
updateAll(S.aiPct);
</script>
</body>
</html>
