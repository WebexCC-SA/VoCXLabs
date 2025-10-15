STLDP
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>WFOx — Mission Control for Hybrid Workforces (Mockup)</title>
<style>
  :root{
    --bg:#0f1221; --panel:#151935; --panel-2:#10132a; --muted:#8ea0d0;
    --accent:#5b8cff; --accent-2:#9b6bff; --success:#42d392; --warn:#ffd166;
    --danger:#ff6b6b; --human:#3ad29f; --ai:#5b8cff; --grid-gap:16px;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; background:linear-gradient(180deg,#0a0d1a,#0d1130);
    color:#e7ebff; font:14px/1.45 Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial;
  }
  .wrap{
    padding:20px; max-width:1400px; margin:0 auto;
  }
  header{
    display:grid; grid-template-columns: 1fr auto auto auto; gap:var(--grid-gap);
    align-items:center; margin-bottom:var(--grid-gap);
  }
  .title{
    font-size:20px; font-weight:700; letter-spacing:.3px;
    display:flex; gap:10px; align-items:center;
  }
  .badge{font-size:12px; padding:4px 8px; border-radius:999px; background:#1b2147; color:#cfe0ff}
  .controls{display:flex; gap:10px; flex-wrap:wrap}
  select, button, .chip{
    background:#1b2147; color:#e7ebff; border:1px solid #2a3060;
    padding:10px 12px; border-radius:10px; font:inherit; cursor:pointer;
  }
  select{appearance:none}
  button.primary{background:linear-gradient(180deg,var(--accent),#4a76ff); border:none; font-weight:700}
  .kpi-bar{display:flex; gap:14px; flex-wrap:wrap; justify-content:flex-end}
  .kpi{background:#111532; padding:10px 12px; border:1px solid #1f2450; border-radius:12px; min-width:120px}
  .kpi .lbl{color:#a9b6e5; font-size:12px}
  .kpi .val{font-weight:800; font-size:18px}

  /* Layout grid */
  .grid{
    display:grid; gap:var(--grid-gap);
    grid-template-columns: 1.2fr 1fr; grid-auto-rows: minmax(220px, auto);
  }
  .panel{
    background:linear-gradient(180deg,#111532,#0f1431);
    border:1px solid #22295a; border-radius:16px; padding:16px;
    box-shadow: 0 10px 30px rgba(0,0,0,.25), inset 0 1px 0 rgba(255,255,255,.04);
  }
  .panel h3{margin:0 0 10px; font-size:15px; letter-spacing:.2px}
  .muted{color:var(--muted); font-size:12px}
  .row{display:flex; gap:16px; flex-wrap:wrap}
  .col{flex:1 1 220px}
  .chip{font-size:12px; border-radius:999px}

  /* Donut & gauges */
  .donut{
    --p:50; /* percent AI */
    width:160px; height:160px; border-radius:50%;
    background:
      conic-gradient(var(--ai) calc(var(--p)*1%), var(--human) 0);
    position:relative; display:grid; place-items:center;
    box-shadow: inset 0 0 40px rgba(0,0,0,.35);
  }
  .donut::after{
    content:""; width:110px; height:110px; background:#0e1330; border-radius:50%;
    box-shadow: inset 0 1px 0 rgba(255,255,255,.06);
  }
  .donut-label{
    position:absolute; text-align:center; font-weight:800; z-index:1; line-height:1.1;
  }

  .gauge{
    --val:70; /* 0-100 */
    width:200px; height:100px; border-radius:200px 200px 0 0 / 200px 200px 0 0;
    background: conic-gradient(from 180deg, var(--success) calc(var(--val)*1%), #253060 0) top / 100% 200% no-repeat;
    position:relative; overflow:hidden; border:1px solid #263066;
  }
  .gauge::after{
    content:""; position:absolute; inset:12px 12px auto 12px; height:calc(100% - 12px);
    background:linear-gradient(180deg,#0f1431,#0b1030); border-radius:200px 200px 0 0;
  }
  .gauge-read{
    position:absolute; inset:auto 0 -8px 0; text-align:center; font-weight:800;
    filter:drop-shadow(0 2px 8px rgba(0,0,0,.5));
  }

  /* Sliders */
  .form{
    display:grid; gap:12px; grid-template-columns: 1fr auto; align-items:center;
  }
  input[type="range"]{width:100%}
  .pill{
    background:#1b2147; padding:10px 12px; border-radius:12px; border:1px solid #2a3060;
  }

  /* Tables */
  table{width:100%; border-collapse:collapse}
  th, td{padding:10px 8px; border-bottom:1px solid #22295a; text-align:left; font-size:13px}
  th{color:#9fb0ee; font-weight:700}
  .right{text-align:right}
  .ok{color:var(--success)} .warn{color:var(--warn)} .bad{color:var(--danger)}

  /* Cards */
  .cards{display:grid; grid-template-columns: repeat(3, minmax(200px,1fr)); gap:12px}
  .card{background:#101431; border:1px solid #20275a; border-radius:12px; padding:12px}
  .card h4{margin:0 0 6px; font-size:13px}
  .progress{
    --pct:60;
    width:72px; height:72px; border-radius:50%;
    background: conic-gradient(var(--accent) calc(var(--pct)*1%), #263066 0);
    position:relative; display:inline-grid; place-items:center; margin-right:8px;
  }
  .progress::after{content:""; width:52px; height:52px; background:#0f1431; border-radius:50%}
  .progress span{position:absolute; font-weight:800; font-size:12px}

  /* Sidebar */
  .sidebar{
    position:sticky; top:12px; align-self:start;
    background:linear-gradient(180deg,#111532,#0f1431); border:1px solid #22295a; border-radius:16px; padding:16px;
  }
  .rec{border-left:3px solid var(--accent); padding-left:10px; margin:12px 0; background:#0f1434}
  .small{font-size:12px}

  /* Footer */
  footer{
    margin:18px 0 40px; display:flex; gap:12px; align-items:center; justify-content:space-between; flex-wrap:wrap
  }
  .status{display:flex; gap:10px; align-items:center}
  .dot{width:10px; height:10px; border-radius:50%}
  .dot.green{background:var(--success)} .dot.yellow{background:var(--warn)} .dot.red{background:var(--danger)}

  /* Responsive */
  @media (max-width: 1100px){
    .grid{grid-template-columns: 1fr}
  }
</style>
</head>
<body>
  <div class="wrap">
    <!-- HEADER -->
    <header aria-label="Global Controls">
      <div class="title">
        <span style="display:inline-block;width:10px;height:10px;border-radius:3px;background:linear-gradient(180deg,var(--ai),var(--accent-2)); box-shadow:0 0 20px #5b8cff;"></span>
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
          <option>Last 7 days</option>
          <option selected>Last 30 days</option>
          <option>Quarter to date</option>
          <option>Custom…</option>
        </select>
        <select id="region">
          <option>Global</option><option>AMER</option><option>EMEA</option><option>APJC</option>
        </select>
      </div>
      <div class="controls">
        <div class="pill" style="display:flex;gap:10px;align-items:center;">
          <strong>AI Coverage</strong>
          <input id="aiSlider" type="range" min="0" max="100" value="60" />
          <span id="aiPct" aria-live="polite">60%</span>
        </div>
        <button id="simulate" class="primary">Simulate Scenario</button>
      </div>
      <div class="kpi-bar">
        <div class="kpi"><div class="lbl">Projected Savings</div><div class="val" id="kpiSavings">$1.80M</div></div>
        <div class="kpi"><div class="lbl">CSAT Impact</div><div class="val" id="kpiCsat">-0.2</div></div>
        <div class="kpi"><div class="lbl">CX Health</div><div class="val" id="kpiCxHealth">82</div></div>
      </div>
    </header>

    <div class="grid">
      <!-- TOP LEFT: AI vs HUMAN MIX -->
      <section class="panel" aria-labelledby="mixTitle">
        <h3 id="mixTitle">AI vs Human Mix & Performance</h3>
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
              <p class="muted small">Drag the AI Coverage slider above to see cost & CSAT change in real time.</p>
            </div>
          </div>
          <div class="col">
            <div style="display:flex; gap:16px; align-items:flex-end;">
              <div>
                <div class="gauge" id="cxGauge" style="--val:82"></div>
                <div class="gauge-read"><span id="cxGaugeVal">82</span>/100 CX Health</div>
              </div>
              <div>
                <svg id="stackedArea" width="320" height="120" role="img" aria-label="AI vs Human mix over time">
                  <!-- background -->
                  <rect x="0" y="0" width="320" height="120" fill="#0e1330" stroke="#22295a"/>
                  <!-- filled areas computed in JS -->
                </svg>
                <div class="muted small">24h mix trend (simulated)</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- TOP RIGHT: CX HEALTH & SENTIMENT -->
      <section class="panel" aria-labelledby="cxTitle">
        <h3 id="cxTitle">CX Health & Sentiment Trends</h3>
        <div class="row">
          <div class="col">
            <svg id="sentimentChart" width="100%" height="160" viewBox="0 0 360 160" role="img" aria-label="Sentiment over time">
              <rect x="0" y="0" width="360" height="160" fill="#0e1330" stroke="#22295a"/>
              <!-- lines in JS -->
            </svg>
            <div class="muted small">Positive / Neutral / Negative sentiment (simulated)</div>
          </div>
          <div class="col">
            <div class="cards" style="grid-template-columns: repeat(2, 1fr);">
              <div class="card">
                <h4>Top Topics (Word Cloud-ish)</h4>
                <div class="muted small">refunds, shipping delays, login, promo code, warranty, address change</div>
              </div>
              <div class="card">
                <h4>Demographic CSAT</h4>
                <table>
                  <tr><th>Segment</th><th class="right">AI Pref</th><th class="right">ΔCSAT</th></tr>
                  <tr><td>Gen Z</td><td class="right ok">+AI</td><td class="right ok">+0.6</td></tr>
                  <tr><td>Millennials</td><td class="right ok">+AI</td><td class="right ok">+0.3</td></tr>
                  <tr><td>Gen X</td><td class="right">Neutral</td><td class="right">+0.0</td></tr>
                  <tr><td>Seniors</td><td class="right bad">+Human</td><td class="right bad">-0.7</td></tr>
                </table>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- BOTTOM LEFT: ROI & SAVINGS -->
      <section class="panel" aria-labelledby="roiTitle">
        <h3 id="roiTitle">Operational Efficiency & ROI Simulator</h3>
        <div class="row">
          <div class="col">
            <table>
              <tr><th>Metric</th><th class="right">Value</th></tr>
