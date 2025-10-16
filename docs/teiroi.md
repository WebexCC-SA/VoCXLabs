## TEI / ROI Calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Webex Contact Center – 3‑Year TEI ROI Calculator (Enhanced)</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    :root{color-scheme: only light}
    body{font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, sans-serif; background:#f6f8fb; margin:0; padding:20px; color:#0a2233}
    .app{max-width:1100px; margin:0 auto; background:#fff; border:1px solid #e6ecf2; border-radius:14px; box-shadow:0 8px 28px rgba(10,25,41,.06)}
    header{padding:22px 24px; border-bottom:1px solid #e6ecf2; display:flex; align-items:center; justify-content:space-between}
    header h1{font-size:20px; margin:0; color:#0a3a57}
    .grid{display:grid; grid-template-columns:repeat(2,1fr); gap:14px}
    .content{padding:22px}
    .card{border:1px solid #e6ecf2; border-radius:12px; padding:16px}
    .card h3{margin:0 0 8px; font-size:15px; color:#0a3a57}
    label{font-size:13px; font-weight:600; display:block}
    .sub{font-size:12px; color:#586776; margin:4px 0 8px}
    input[type=number], select{width:100%; padding:10px; border:1px solid #cad6e0; border-radius:8px; font-size:14px}
    .row{display:grid; grid-template-columns:1fr 140px; gap:10px; align-items:end}
    .btn{display:inline-block; padding:10px 14px; border-radius:10px; border:1px solid #cfe3ff; background:#0b73d1; color:#fff; font-weight:700; cursor:pointer}
    .btn.secondary{background:#eef6ff; color:#0a3a57}
    .btnbar{display:flex; gap:10px; margin-top:8px}
    table{width:100%; border-collapse:collapse}
    th,td{border:1px solid #e6ecf2; padding:10px; text-align:left; font-size:13px}
    th{background:#0a3a57; color:#fff}
    .kpi{display:grid; grid-template-columns:repeat(3,1fr); gap:12px}
    .kpi .tile{background:#f3f9ff; border:1px solid #d9ecff; border-radius:12px; padding:14px}
    .tile h2{margin:0; font-size:22px}
    .tile p{margin:4px 0 0; color:#49616f; font-size:12px}
    .charts{display:grid; grid-template-columns:1fr 1fr; gap:16px}
    @media (max-width:900px){.grid{grid-template-columns:1fr}.kpi{grid-template-columns:1fr}.charts{grid-template-columns:1fr}}
    footer{padding:16px 22px; border-top:1px solid #e6ecf2; color:#60717e; font-size:12px}
    .muted{color:#6b7b88}
  </style>
</head>
<body>
  <div class="app" id="app">
    <header>
      <h1>Webex Contact Center – 3‑Year TEI ROI Calculator (Enhanced)</h1>
      <div class="btnbar">
        <button class="btn secondary" id="loadDefaults">Load Defaults</button>
        <button class="btn" id="calcBtn">Calculate</button>
        <button class="btn" id="exportPDF">Export Report to PDF</button>
      </div>
    </header>

    <div class="content">
      <div class="grid">
        <section class="card">
          <h3>Baseline (Current Environment)</h3>
          <label for="agents">1) Number of contact center agents</label>
          <input type="number" id="agents" value="100" min="0" step="1" />

          <label for="supervisorRatio">Supervisor ratio (% of agents)</label>
          <select id="supervisorRatio">
            <option value="10">10%</option>
            <option value="15" selected>15%</option>
            <option value="20">20%</option>
          </select>

          <label for="itRatio">IT ratio (% of agents)</label>
          <select id="itRatio">
            <option value="5">5%</option>
            <option value="10" selected>10%</option>
            <option value="15">15%</option>
          </select>

          <label for="supervisorsCurr">2) Supervisors (cust. care & QM)</label>
          <input type="number" id="supervisorsCurr" value="15" min="0" step="1" />

          <label for="itCurr">3) IT FTEs managing current CC/UC</label>
          <input type="number" id="itCurr" value="10" min="0" step="0.1" />

          <label for="callsYear">4) Incoming calls per year</label>
          <input type="number" id="callsYear" value="1800000" min="0" step="1" />

          <label for="avgMins">5) Avg. call duration (minutes)</label>
          <input type="number" id="avgMins" value="5" min="0" step="0.1" />

          <label for="deflectCurr">6) % calls deflected today</label>
          <input type="number" id="deflectCurr" value="8" min="0" step="0.1" />

          <label for="licenseCurr">7) Annual license cost (current CC/UC)</label>
          <input type="number" id="licenseCurr" value="1000000" min="0" step="1000" />

          <label for="downHrs">8) Annual downtime (hours)</label>
          <input type="number" id="downHrs" value="15" min="0" step="0.1" />

          <label for="agentChurn">9) Agent churn (annual %)</label>
          <input type="number" id="agentChurn" value="35" min="0" step="0.1" />
        </section>

        <section class="card">
          <h3>Future (With Webex Contact Center)</h3>
          <label for="itFut">10) IT FTEs (future)</label>
          <input type="number" id="itFut" value="2" min="0" step="0.1" />

          <label for="deflectFut">11) % calls deflected (future)</label>
          <input type="number" id="deflectFut" value="20" min="0" step="0.1" />

          <label for="supervisorsFut">12) Supervisors with Webex CC</label>
          <input type="number" id="supervisorsFut" value="65" min="0" step="1" />

          <label>13) Reduction of legacy license spend</label>
          <input type="number" id="licRed1" value="95" min="0" max="100" step="0.1" />
          <input type="number" id="licRed2" value="100" min="0" max="100" step="0.1" />

          <label>14) Downtime reduction with Webex CC</label>
          <input type="number" id="downRed1" value="45" min="0" max="100" step="0.1" />
          <input type="number" id="downRed2" value="50" min="0" max="100" step="0.1" />

          <label for="subAnnual">15) Subscription (annual $)</label>
          <input type="number" id="subAnnual" value="140000" min="0" step="1000" />

          <label for="proServ">16) Professional services (one-time $)</label>
          <input type="number" id="proServ" value="58000" min="0" step="1000" />

          <label for="implTeam">17) Implementation team size (people)</label>
          <input type="number" id="implTeam" value="6" min="0" step="1" />

          <label for="implMonths">18) Implementation duration (months)</label>
          <input type="number" id="implMonths" value="5" min="0" step="0.1" />

          <label for="downCostHour">19) Cost of downtime ($/hr)</label>
          <input type="number" id="downCostHour" value="225000" min="0" step="1000" />

          <label for="trainingCosts">24) Annual training costs ($)</label>
          <input type="number" id="trainingCosts" value="50000" min="0" step="1000" />

          <h3 style="margin-top:16px">Salary Assumptions</h3>
          <label for="salIT">20) IT salary ($/yr)</label>
          <input type="number" id="salIT" value="85000" min="0" step="1000" />

          <label for="salSup">21) Supervisor salary ($/yr)</label>
          <input type="number" id="salSup" value="40000" min="0" step="1000" />

          <label for="salAgent">22) Agent salary ($/yr)</label>
          <input type="number" id="salAgent" value="30000" min="0" step="1000" />

          <label for="salImpl">23) Implementation team salary ($/yr)</label>
          <input type="number" id="salImpl" value="120000" min="0" step="1000" />
        </section>
      </div>

      <div class="btnbar" style="margin-top:14px">
        <button class="btn secondary" id="syncFromAgents">Sync calc. fields from agents</button>
        <button class="btn" id="calcBtn2">Recalculate</button>
      </div>

      <section class="card" style="margin-top:16px">
        <h3>Three‑Year Financial Summary (risk‑adjusted)</h3>
        <div class="kpi">
          <div class="tile"><h2 id="kpiROI">–</h2><p>ROI (Present Value)</p></div>
          <div class="tile"><h2 id="kpiBenefits">–</h2><p>Benefits – 3‑yr PV</p></div>
          <div class="tile"><h2 id="kpiPayback">–</h2><p>Payback Period</p></div>
        </div>

        <div class="charts" style="margin-top:12px">
          <canvas id="gauge"></canvas>
          <canvas id="bar"></canvas>
        </div>

        <div style="margin-top:16px; overflow:auto">
          <table id="tblBenefits">
            <thead><tr><th>Benefit Category</th><th>Y1</th><th>Y2</th><th>Y3</th><th>3‑yr Total</th><th>PV</th></tr></thead>
            <tbody></tbody>
          </table>
        </div>

        <div style="margin-top:16px; overflow:auto">
          <table id="tblCosts">
            <thead><tr><th>Cost Category</th><th>Y1</th><th>Y2</th><th>Y3</th><th>3‑yr Total</th><th>PV</th></tr></thead>
            <tbody></tbody>
          </table>
        </div>
      </section>
    </div>

    <footer>
      Enhanced version with training costs, editable ratios, and PDF export.
    </footer>
  </div>

<script>
(function(){
  const $ = id => document.getElementById(id);
  const nf = (v, p=0, fmt='en-US') => (isFinite(v) ? Number(v).toLocaleString(fmt, {maximumFractionDigits:p}) : '–');
  const discRate = 0.10;

  function syncFromAgents(){
    const agents = +$('agents').value || 0;
    const supRatio = +$('supervisorRatio').value / 100;
    const itRatio = +$('itRatio').value / 100;
    $('supervisorsCurr').value = Math.round(agents * supRatio);
    $('itCurr').value = (agents * itRatio).toFixed(1);
  }

  function pv(y, val){ return val / Math.pow(1+discRate, y); }

  function calc(){
    const agents = +$('agents').value||0;
    const supervisorsCurr = +$('supervisorsCurr').value||0;
    const itCurr = +$('itCurr').value||0;
    const callsYear = +$('callsYear').value||0;
    const avgMins = +$('avgMins').value||0;
    const deflectCurr = (+$('deflectCurr').value||0)/100;
    const licenseCurr = +$('licenseCurr').value||0;
    const downHrs = +$('downHrs').value||0;

    const itFut = +$('itFut').value||0;
    const deflectFut = (+$('deflectFut').value||0)/100;
    const supervisorsFut = +$('supervisorsFut').value||0;

    const licRed1 = (+$('licRed1').value||0)/100;
    const licRed2 = (+$('licRed2').value||0)/100;

    const downRed1 = (+$('downRed1').value||0)/100;
    const downRed2 = (+$('downRed2').value||0)/100;

    const subAnnual = +$('subAnnual').value||0;
    const proServ = +$('proServ').value||0;
    const implTeam = +$('implTeam').value||0;
    const implMonths = +$('implMonths').value||0;

    const downCostHour = +$('downCostHour').value||0;
    const salIT = +$('salIT').value||0;
    const salSup = +$('salSup').value||0;
    const salAgent = +$('salAgent').value||0;
    const salImpl = +$('salImpl').value||0;
    const trainingCosts = +$('trainingCosts').value||0;

    const itSavingsYr = (itCurr - itFut) * salIT;
    const itSavings = [itSavingsYr, itSavingsYr, itSavingsYr];

    const callsHandledCurr = callsYear * (1 - deflectCurr);
    const callsHandledFut = callsYear * (1 - deflectFut);
    const minutesSaved = Math.max(0, (callsHandledCurr - callsHandledFut)) * avgMins;
    const agentHourly = salAgent / 2080;
    const laborSavingsYr = (minutesSaved/60) * agentHourly;
    const laborSavings = [laborSavingsYr, laborSavingsYr, laborSavingsYr];

    const licSave = [licenseCurr * licRed1, licenseCurr * licRed2, licenseCurr * licRed2];

    const supDelta = supervisorsCurr - supervisorsFut;
    const supSavingsYr = supDelta * salSup;
    const supSavings = [supSavingsYr, supSavingsYr, supSavingsYr];

    const downSave = [downHrs * downCostHour * downRed1, downHrs * downCostHour * downRed2, downHrs * downCostHour * downRed2];

    const benCats = [
      {name:'Reduced IT Support Costs', y:itSavings},
      {name:'Labor Savings From Deflected Calls', y:laborSavings},
      {name:'Legacy License Savings', y:licSave},
      {name:'Streamlined Supervisor Labor', y:supSavings},
      {name:'Avoided Downtime Costs', y:downSave}
    ];

    const internalImpl = implTeam * (salImpl * (implMonths/12));
    const implCosts = [proServ + internalImpl, 0, 0];
    const subsCosts = [subAnnual, subAnnual
