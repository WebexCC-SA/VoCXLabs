STLDP
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>WFOx — Mission Control for Hybrid Workforces (v3)</title>

<style>
/* ---------- THEME TOKENS ---------- */
:root{
  --bg:#ffffff; --surface:#f7f9fc; --panel:#ffffff; --panel-2:#f2f5fb;
  --text:#0b1633; --sub:#506099; --line:#e3e8f2;
  --shadow:0 10px 28px rgba(16,24,40,.08),0 2px 4px rgba(16,24,40,.06);
  --accent:#2563eb; --accent-2:#7c3aed;
  --ai:#2563eb; --human:#16a34a; --ok:#1a9e6f; --warn:#f59e0b; --bad:#ef4444;
  --chip:#eef2ff; --chip-b:#c7d2fe; --muted:#5b6b99;
}
[data-theme="dark"]{
  --bg:#0f1221; --surface:#10142b; --panel:#14183a; --panel-2:#121635;
  --text:#e7ebff; --sub:#8ea0d0; --line:#22295a;
  --shadow:0 12px 32px rgba(0,0,0,.35), inset 0 1px 0 rgba(255,255,255,.04);
  --accent:#5b8cff; --accent-2:#9b6bff;
  --ai:#5b8cff; --human:#3ad29f; --ok:#42d392; --warn:#ffd166; --bad:#ff6b6b;
  --chip:#1b2147; --chip-b:#2a3060; --muted:#9fb0ee;
}

/* ---------- BASE ---------- */
*{box-sizing:border-box;margin:0;padding:0}
body{background:var(--bg);color:var(--text);font:14px/1.45 Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial;}
.wrap{padding:20px;max-width:1400px;margin:0 auto;}
header{display:grid;grid-template-columns:1fr auto auto;gap:16px;align-items:center;margin-bottom:16px;}
.title{font-size:20px;font-weight:800;display:flex;gap:10px;align-items:center}
.badge{font-size:12px;padding:4px 8px;border-radius:999px;background:var(--surface);color:var(--sub);border:1px solid var(--line)}
.controls{display:flex;gap:10px;flex-wrap:wrap;align-items:center}
select,button,.pill{background:var(--panel);color:var(--text);border:1px solid var(--line);
  padding:10px 12px;border-radius:10px;font:inherit;cursor:pointer;box-shadow:var(--shadow);}
.toggle{display:inline-flex;align-items:center;gap:6px;padding:10px 12px;border-radius:10px;
  border:1px solid var(--line);background:var(--panel);box-shadow:var(--shadow);cursor:pointer;}
.kpi-bar{display:flex;gap:14px;flex-wrap:wrap;justify-content:flex-end}
.kpi{background:var(--panel);padding:10px 12px;border:1px solid var(--line);border-radius:12px;min-width:140px;box-shadow:var(--shadow)}
.kpi .lbl{color:var(--sub);font-size:12px}
.kpi .val{font-weight:800;font-size:18px}

/* ---------- GRID ---------- */
.grid{display:grid;gap:16px;grid-template-columns:1.25fr 1fr;grid-auto-rows:minmax(240px,auto);}
.panel{background:var(--panel);border:1px solid var(--line);border-radius:16px;padding:16px;
  box-shadow:var(--shadow);overflow:hidden;}
.panel h3{margin-bottom:10px;font-size:15px;}
.muted{color:var(--muted);font-size:12px}
.row{display:flex;gap:16px;flex-wrap:wrap;align-items:flex-start}
.col{flex:1 1 260px}

/* ---------- BUTTONS ---------- */
.tile-btn{display:inline-flex;align-items:center;gap:8px;padding:10px 12px;border-radius:10px;
  border:1px solid var(--line);background:linear-gradient(180deg,var(--panel),var(--panel-2));
  box-shadow:var(--shadow);position:relative;transition:transform .08s ease,box-shadow .2s ease;}
.tile-btn:hover{transform:translateY(-1px);box-shadow:0 16px 30px rgba(0,0,0,.12)}
.tile-btn[data-tip]:hover::after{content:attr(data-tip);position:absolute;top:-34px;left:0;white-space:nowrap;
  background:#111827;color:#fff;padding:6px 8px;border-radius:8px;font-size:12px;box-shadow:0 8px 20px rgba(0,0,0,.25);}

/* ---------- DONUT ---------- */
.donut{--p:60;width:170px;height:170px;border-radius:50%;
  background:conic-gradient(var(--ai) calc(var(--p)*1%), var(--human) 0);
  position:relative;display:grid;place-items:center;}
.donut::after{content:"";width:118px;height:118px;background:var(--panel);
  border-radius:50%;border:1px solid var(--line);}
.donut-label{position:absolute;text-align:center;font-weight:800;z-index:1;line-height:1.1}

/* ---------- GAUGE ---------- */
.gauge{--val:82;width:220px;height:110px;border-radius:220px 220px 0 0/220px 220px 0 0;
  background:conic-gradient(from 180deg,var(--ok) calc(var(--val)*1%),#e3e8f2 0) top/100% 200% no-repeat;
  position:relative;overflow:hidden;border:1px solid var(--line);}
.gauge::after{content:"";position:absolute;inset:12px 12px auto 12px;height:calc(100% - 12px);
  background:var(--panel);border-radius:220px 220px 0 0;border:1px solid var(--line);}
.gauge-read{position:absolute;inset:auto 0 -8px 0;text-align:center;font-weight:800}

/* ---------- TABLES & CARDS ---------- */
table{width:100%;border-collapse:collapse}
th,td{padding:10px 8px;border-bottom:1px solid var(--line);text-align:left;font-size:13px}
th{color:var(--sub);font-weight:700}
.right{text-align:right}
.ok{color:var(--ok)}.warn{color:var(--warn)}.bad{color:var(--bad)}
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:12px;}
.card{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:12px;box-shadow:var(--shadow)}
.card .row{display:flex;flex-wrap:wrap;gap:10px;justify-content:flex-start;}
.card .chip{flex:1 1 180px;white-space:nowrap;}

/* ---------- SIDEBAR ---------- */
.sidebar{position:sticky;top:12px;align-self:start;background:var(--panel);border:1px solid var(--line);
  border-radius:16px;padding:16px;box-shadow:var(--shadow);}

/* ---------- FOOTER ---------- */
footer{margin:18px 0 40px;display:flex;gap:12px;align-items:center;justify-content:space-between;flex-wrap:wrap}
.status{display:flex;gap:10px;align-items:center}
.bullet{width:10px;height:10px;border-radius:50%}
.green{background:var(--ok)}
@media (max-width:1100px){.grid{grid-template-columns:1fr}}
</style>
</head>

<body>
<div class="wrap">
<header>
  <div class="title">
    <span style="display:inline-block;width:10px;height:10px;border-radius:3px;background:linear-gradient(180deg,var(--ai),var(--accent-2));"></span>
    WFOx — Mission Control for Hybrid Workforces
    <span class="badge">Hybrid (AI + Human)</span>
  </div>
  <div class="controls">
    <select><option>Manager</option><option>CX Director</option><option>AI Ops</option></select>
    <label class="toggle"><input type="checkbox" id="themeChk"> Dark Mode</label>
  </div>
  <div class="kpi-bar">
    <div class="kpi"><div class="lbl">Projected Savings</div><div class="val">$1.80M</div></div>
    <div class="kpi"><div class="lbl">CSAT Impact</div><div class="val">-0.2</div></div>
    <div class="kpi"><div class="lbl">CX Health</div><div class="val">82</div></div>
  </div>
</header>

<div class="grid">

<!-- AI vs Human -->
<section class="panel">
  <div class="row" style="justify-content:space-between;align-items:center;">
    <h3>AI vs Human Mix & Performance</h3>
    <button class="tile-btn" data-tip="Coming soon">View in Webex Contact Center</button>
  </div>
  <div class="row">
    <div class="col" style="display:flex;gap:16px;align-items:center;">
      <div class="donut" style="--p:60">
        <div class="donut-label"><div style="font-size:26px">60%</div><div class="muted">AI Coverage</div></div>
      </div>
      <div>
        <div class="chip">AI Efficiency: <b>92%</b></div>
        <div class="chip">AHT — AI: <b>1.8m</b> | Human: <b>4.2m</b></div>
        <div class="chip">Deflection: <b>34%</b></div>
        <div class="chip">AI Coverage <input type="range" min="0" max="100" value="60"></div>
        <p class="muted">Adjust AI coverage to see cost & CSAT change.</p>
      </div>
    </div>
    <div class="col">
      <div class="gauge" style="--val:82"></div>
      <div class="gauge-read">82/100 CX Health</div>
    </div>
  </div>
</section>

<!-- CX Health -->
<section class="panel">
  <div class="row" style="justify-content:space-between;align-items:center;">
    <h3>CX Health & Sentiment Trends</h3>
    <button class="tile-btn" data-tip="Coming soon">View in Webex Engage</button>
  </div>
  <div class="cards">
    <div class="card">
      <h4>Top Topics (Last 30d)</h4>
      <div class="muted">refunds, shipping delays, login, promo code, warranty</div>
    </div>
    <div class="card">
      <h4>Demographic CSAT</h4>
      <table>
        <tr><th>Segment</th><th class="right">AI Pref</th><th class="right">ΔCSAT</th></tr>
        <tr><td>Gen Z</td><td class="right ok">+AI</td><td class="right ok">+0.6</td></tr>
        <tr><td>Millennials</td><td class="right ok">+AI</td><td class="right ok">+0.3</td></tr>
        <tr><td>Gen X</td><td class="right">Neutral</td><td class="right">0.0</td></tr>
        <tr><td><b>Boomers</b></td><td class="right bad">+Human</td><td class="right bad">-0.7</td></tr>
      </table>
    </div>
  </div>
</section>

<!-- ROI -->
<section class="panel">
  <div class="row" style="justify-content:space-between;align-items:center;">
    <h3>Operational Efficiency & ROI Simulator</h3>
    <button class="tile-btn" data-tip="Coming soon">View in Webex Connect</button>
  </div>
  <div class="cards">
    <div class="card">
      <table>
        <tr><th>Metric</th><th class="right">Value</th></tr>
        <tr><td>Cost per Contact — AI</td><td class="right">$0.42</td></tr>
        <tr><td>Cost per Contact — Human</td><td class="right">$3.10</td></tr>
        <tr><td>Current Savings (YTD)</td><td class="right">$54K</td></tr>
        <tr><td>Potential Uplift (↑ AI +10%)</td><td class="right">$27K</td></tr>
        <tr><td>Cost / Contact (Weighted)</td><td class="right">$1.49</td></tr>
        <tr><td>Annualized Impact</td><td class="right">$59K</td></tr>
      </table>
    </div>
    <div class="card">
      <h4>Scenario Controls</h4>
      <div class="row">
        <div class="chip">AI Coverage: <b>60%</b></div>
        <div class="chip">Min CSAT Threshold <input type="range" min="70" max="95" value="85"></div>
        <div class="chip">Projected ROI: <b>+11%</b></div>
        <div class="chip">CX Risk: <b class="warn">Medium</b></div>
      </div>
    </div>
  </div>
</section>

<!-- Gamification -->
<section class="panel">
  <div class="row" style="justify-content:space-between;align-items:center;">
    <h3>Gamification & Leaderboards</h3>
    <button class="tile-btn" data-tip="Coming soon">View in Webex AI Agent</button>
  </div>
  <div class="cards">
    <div class="card">
      <table>
        <tr><th>Team</th><th class="right">Savings</th><th class="right">ΔCSAT</th><th class="right">Opt. Score</th><th>Badges</th></tr>
        <tr><td>North AM</td><td class="right">$420K</td><td class="right ok">+0.6</td><td class="right">892</td><td>🏁 ⚙️ 💬</td></tr>
        <tr><td>EMEA</td><td class="right">$380K</td><td class="right">+0.1</td><td class="right">861</td><td>⚙️ 💬</td></tr>
        <tr><td>APJC</td><td class="right">$315K</td><td class="right bad">-0.3</td><td class="right">803</td><td>🏁</td></tr>
      </table>
    </div>
    <div class="card">
      <h4>This Week’s Challenge</h4>
      <p class="muted">Reduce AHT by 5% using AI Assist on Tier-1 “order status” intents.</p>
      <div class="row">
        <div class="chip">North AM — 46%</div>
        <div class="chip">EMEA — 72%</div>
        <div class="chip">APJC — 28%</div>
      </div>
    </div>
  </div>
</section>

<!-- Sidebar -->
<aside class="sidebar">
  <h3>Smart Recommendations</h3>
  <div class="rec">Shift +12% of “order status” to AI after 8 pm; expected savings $68K/qtr; ΔCSAT –0.1.</div>
  <div class="rec">Retrain refund intent model; underperforms by 0.6% vs target.</div>
  <div class="rec">Offer micro-breaks for Team APJC; burnout risk trending ↑.</div>
  <button class="tile-btn" data-tip="Coming soon-ish">Simulate Impact</button>
</aside>
</div>

<footer>
  <div class="status"><div class="bullet green"></div>
    <div class="muted">Compliance: SOC2 & ISO 27001 • AI Fairness Score: 93</div>
  </div>
  <div class="muted">Mockup only — simulated for demo purposes.</div>
</footer>
</div>

<script>
document.getElementById("themeChk").addEventListener("change", e=>{
  document.documentElement.setAttribute("data-theme", e.target.checked?"dark":"light");
});
</script>
</body>
</html>


