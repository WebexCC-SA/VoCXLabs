# AI Agent Voice Calculator - for PVT CX Experience Training ONLY
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Voice AI Unit Calculator</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    :root{color-scheme: only light}
    body{font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, sans-serif; background:#f6f8fb; margin:0; padding:20px; color:#0a2233}
    .app{max-width:1100px; margin:0 auto; background:#fff; border:1px solid #e6ecf2; border-radius:14px; box-shadow:0 8px 28px rgba(10,25,41,.06)}
    header{padding:22px 24px; border-bottom:1px solid #e6ecf2; display:flex; align-items:center; justify-content:space-between}
    header h1{font-size:20px; margin:0; color:#0a3a57}
    .content{padding:22px}
    .grid{display:grid; grid-template-columns:repeat(2,1fr); gap:16px}
    .card{border:1px solid #e6ecf2; border-radius:12px; padding:16px}
    .card h3{margin:0 0 8px; font-size:15px; color:#0a3a57}
    label{font-size:13px; font-weight:600; display:block}
    .sub{font-size:12px; color:#586776; margin:4px 0 8px}
    input[type=number], select{width:100%; padding:10px; border:1px solid #cad6e0; border-radius:8px; font-size:14px}
    input[readonly]{background-color:#f0f2f5; color:#666; cursor:not-allowed;}
    .row{display:grid; grid-template-columns:1fr 1fr; gap:10px}
    .btn{display:inline-block; padding:10px 14px; border-radius:10px; border:1px solid #cfe3ff; background:#0b73d1; color:#fff; font-weight:700; cursor:pointer}
    .btn.secondary{background:#eef6ff; color:#0a3a57}
    .btnbar{display:flex; gap:10px; margin-top:8px; flex-wrap:wrap}
    .kpi{display:grid; grid-template-columns:repeat(3,1fr); gap:12px}
    .kpi .tile{background:#f3f9ff; border:1px solid #d9ecff; border-radius:12px; padding:14px}
    .tile h2{margin:0; font-size:22px}
    .tile p{margin:4px 0 0; color:#49616f; font-size:12px}
    .results{display:grid; grid-template-columns:1fr 1fr; gap:16px}
    .mono{font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; font-size:12px}
    table{width:100%; border-collapse:collapse}
    th,td{border:1px solid #e6ecf2; padding:10px; text-align:left; font-size:13px}
    th{background:#0a3a57; color:#fff}
    @media (max-width:900px){.grid{grid-template-columns:1fr}.results{grid-template-columns:1fr}.kpi{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <div class="app" id="app">
    <header>
      <h1>Voice AI Unit Calculator</h1>
      <div class="btnbar">
        <button class="btn secondary" id="loadDefaults">Load Defaults</button>
        <button class="btn" id="calcBtn">Calculate</button>
        <button class="btn" id="exportPDF">Export PDF</button>
      </div>
    </header>

    <div class="content">
      <div class="grid">
        <section class="card">
          <h3>Inputs</h3>
          <label for="callsYear">Calls per Year</label>
          <div class="sub">Total inbound calls in the last 12 months. <strong>Default: 1,000,000</strong></div>
          <input type="number" id="callsYear" value="1000000" min="0" step="1" />

          <label for="acl">Average Call Length (seconds)</label>
          <div class="sub">Total interaction time including IVR and wrap-up. <strong>Default: 180</strong></div>
          <input type="number" id="acl" value="180" min="0" step="1" />

          <label for="aht">Average Handle Time (seconds)</label>
          <div class="sub">Portion handled by live agents (AHT). <strong>Default: 60</strong></div>
          <input type="number" id="aht" value="60" min="0" step="1" />

          <label for="sessionSeconds">Session Duration (seconds)</label>
          <div class="sub">Billing session length for AI voice. <strong>Default: 120</strong></div>
          <input type="number" id="sessionSeconds" value="120" min="1" step="1" />

          <label for="overagePct">Overage Buffer (%)</label>
          <div class="sub">Capacity buffer to ensure coverage. <strong>Default: 5%</strong></div>
          <input type="number" id="overagePct" value="5" min="0" step="0.1" />
        </section>

        <section class="card">
          <h3>AI Unit Catalog</h3>
          <label for="aiType">AI Agent Type</label>
          <select id="aiType">
            <option value="scripted" selected>Scripted</option>
            <option value="autonomous">Autonomous</option>
          </select>

          <div class="row">
            <div>
              <label for="sessionsPerUnitScript">Sessions per Unit – Scripted</label>
              <div class="sub">Static value: 1,200 sessions / unit</div>
              <input type="number" id="sessionsPerUnitScript" value="1200" readonly />
            </div>
            <div>
              <label for="sessionsPerUnitAuto">Sessions per Unit – Autonomous</label>
              <div class="sub">Static value: 200 sessions / unit</div>
              <input type="number" id="sessionsPerUnitAuto" value="200" readonly />
            </div>
          </div>

          <div class="row">
            <div>
              <label for="showSteps">Show Step-by-step Math</label>
              <select id="showSteps">
                <option value="yes" selected>Yes</option>
                <option value="no">No</option>
              </select>
            </div>
            <div>
              <label for="chartMode">Chart Mode</label>
              <select id="chartMode">
                <option value="compare" selected>Compare Scripted vs Autonomous</option>
                <option value="single">Show Selected Type Only</option>
              </select>
            </div>
          </div>
        </section>
      </div>

      <section class="card" style="margin-top:16px">
        <h3>Results</h3>
        <div class="kpi">
          <div class="tile"><h2 id="kpiSessions">–</h2><p>Total Sessions / Month</p></div>
          <div class="tile"><h2 id="kpiUnitsScript">–</h2><p>Units – Scripted (with buffer)</p></div>
          <div class="tile"><h2 id="kpiUnitsAuto">–</h2><p>Units – Autonomous (with buffer)</p></div>
        </div>

        <div class="results" style="margin-top:12px">
          <div>
            <canvas id="barChart"></canvas>
          </div>
          <div>
            <table>
              <thead><tr><th>Step</th><th>Formula</th><th>Value</th></tr></thead>
              <tbody id="stepsBody" class="mono"></tbody>
            </table>
          </div>
        </div>
      </section>
    </div>
  </div>

<script>
(function(){
  const $ = id => document.getElementById(id);
  const nf = (v, p=0) => isFinite(v) ? Number(v).toLocaleString('en-US', {maximumFractionDigits:p}) : '–';

  let barChart;

  function compute() {
    const callsYear = +$('callsYear').value || 0;
    const acl = +$('acl').value || 0; // seconds
    const aht = +$('aht').value || 0; // seconds
    const sessionSeconds = +$('sessionSeconds').value || 120;
    const overagePct = (+$('overagePct').value || 0) / 100;
    const sps = 1200; // static
    const spa = 200;  // static

    const callsMonth = callsYear / 12;
    const aiTimePerCall = Math.max(0, acl - aht); // seconds
    const totalAISeconds = callsMonth * aiTimePerCall; // seconds per month
    const sessions = totalAISeconds / sessionSeconds; // sessions per month

    const scriptedUnitsBase = sessions / sps;
    const autoUnitsBase = sessions / spa;

    const scriptedUnitsBuffered = scriptedUnitsBase * (1 + overagePct);
    const autoUnitsBuffered = autoUnitsBase * (1 + overagePct);

    // KPIs
    $('kpiSessions').textContent = nf(sessions, 0);
    $('kpiUnitsScript').textContent = nf(Math.ceil(scriptedUnitsBuffered), 0);
    $('kpiUnitsAuto').textContent = nf(Math.ceil(autoUnitsBuffered), 0);

    // Steps table
    const showSteps = $('showSteps').value === 'yes';
    const steps = [
      ['1) Calls per Month', 'Calls/Year ÷ 12', nf(callsMonth,0)],
      ['2) AI Time per Call (sec)', 'ACL − AHT', nf(aiTimePerCall,0)],
      ['3) Total AI Seconds / Month', 'Calls/Month × AI Time/Call', nf(totalAISeconds,0)],
      ['4) Sessions / Month', 'Total AI Seconds ÷ Session Duration', nf(sessions,0)],
      ['5) Scripted Units (base)', 'Sessions ÷ 1,200', nf(scriptedUnitsBase,2)],
      ['6) Autonomous Units (base)', 'Sessions ÷ 200', nf(autoUnitsBase,2)],
      ['7) Buffer Applied', 'Units × (1 + buffer%)', `${nf(overagePct*100,1)}%`],
      ['8) Scripted Units (rounded w/ buffer)', 'ceil(5 × (1+buf))', nf(Math.ceil(scriptedUnitsBuffered),0)],
      ['9) Autonomous Units (rounded w/ buffer)', 'ceil(6 × (1+buf))', nf(Math.ceil(autoUnitsBuffered),0)]
    ];

    const tbody = $('stepsBody');
    tbody.innerHTML = '';
    if (showSteps) {
      steps.forEach(([s,f,v]) => {
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${s}</td><td>${f}</td><td>${v}</td>`;
        tbody.appendChild(tr);
      });
    }

    // Chart
    const mode = $('chartMode').value;
    const labels = mode === 'compare' ? ['Scripted', 'Autonomous'] : [$('aiType').value === 'autonomous' ? 'Autonomous' : 'Scripted'];
    const dataVals = mode === 'compare' ? [Math.ceil(scriptedUnitsBuffered), Math.ceil(autoUnitsBuffered)] : [ $('aiType').value === 'autonomous' ? Math.ceil(autoUnitsBuffered) : Math.ceil(scriptedUnitsBuffered) ];

    const ctx = $('barChart').getContext('2d');
    if (barChart) barChart.destroy();
    barChart = new Chart(ctx, {
      type: 'bar',
      data: { labels, datasets: [{ data: dataVals, backgroundColor: ['#3aa0ff','#00a36c'] }] },
      options: {
        plugins: { legend: { display:false }, title: { display:true, text:'Units Required (with buffer)'} },
        scales: { y: { beginAtZero:true, ticks: { callback: v => v.toLocaleString() } } }
      }
    });
  }

  function loadDefaults(){
    $('callsYear').value = 1000000;
    $('acl').value = 180;
    $('aht').value = 60;
    $('sessionSeconds').value = 120;
    $('overagePct').value = 5;
    $('aiType').value = 'scripted';
  }

  function exportPDF(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.text('Voice AI Unit Calculator – Summary', 14, 16);
    doc.text(`Calls/Year: ${$('callsYear').value}`, 14, 28);
    doc.text(`ACL (s): ${$('acl').value} | AHT (s): ${$('aht').value}`, 14, 36);
    doc.text(`Sessions/Month: ${$('kpiSessions').textContent}`, 14, 44);
    doc.text(`Scripted Units (buffered): ${$('kpiUnitsScript').textContent}`, 14, 52);
    doc.text(`Autonomous Units (buffered): ${$('kpiUnitsAuto').textContent}`, 14, 60);
    doc.save('Voice_AI_Unit_Summary.pdf');
  }

  ['callsYear','acl','aht','sessionSeconds','overagePct','aiType','showSteps','chartMode'].forEach(id=>{
    $(id).addEventListener('input', compute);
    $(id).addEventListener('change', compute);
  });
  $('calcBtn').addEventListener('click', compute);
  $('calcBtn2').addEventListener('click', compute);
  $('loadDefaults').addEventListener('click', ()=>{ loadDefaults(); compute(); });
  $('presetExample').addEventListener('click', ()=>{ loadDefaults(); compute(); });
  $('exportPDF').addEventListener('click', exportPDF);

  loadDefaults();
  compute();
})();
</script>
</body>
</html>
