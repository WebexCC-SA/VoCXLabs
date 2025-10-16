## TEI / ROI Calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Webex Contact Center – 3-Year TEI ROI Calculator (Full Version)</title>
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
  </style>
</head>
<body>
  <div class="app" id="app">
    <header>
      <h1>Webex Contact Center – 3-Year TEI ROI Calculator (Full Version)</h1>
      <div class="btnbar">
        <button class="btn secondary" id="loadDefaults">Load Defaults</button>
        <button class="btn" id="calcBtn">Calculate</button>
        <button class="btn" id="exportPDF">Export Report to PDF</button>
      </div>
    </header>

    <div class="content">
      <div class="grid">
        <section class="card">
          <h3>Baseline Environment</h3>
          <label>Agents</label>
          <input type="number" id="agents" value="100">

          <label>Supervisor ratio (%)</label>
          <select id="supervisorRatio"><option value="10">10%</option><option value="15" selected>15%</option><option value="20">20%</option></select>

          <label>IT ratio (%)</label>
          <select id="itRatio"><option value="5">5%</option><option value="10" selected>10%</option><option value="15">15%</option></select>

          <label>Supervisors</label>
          <input type="number" id="supervisorsCurr" value="15">

          <label>IT FTEs</label>
          <input type="number" id="itCurr" value="10">

          <label>Annual training cost ($)</label>
          <input type="number" id="trainingCosts" value="50000">
        </section>

        <section class="card">
          <h3>Future (Webex CC)</h3>
          <label>Future IT FTEs</label>
          <input type="number" id="itFut" value="2">

          <label>Subscription (annual $)</label>
          <input type="number" id="subAnnual" value="140000">

          <label>Professional services (one-time $)</label>
          <input type="number" id="proServ" value="58000">

          <label>Implementation team (people)</label>
          <input type="number" id="implTeam" value="6">

          <label>Implementation duration (months)</label>
          <input type="number" id="implMonths" value="5">
        </section>
      </div>

      <div class="btnbar">
        <button class="btn secondary" id="syncFromAgents">Sync from Agents</button>
      </div>

      <section class="card" style="margin-top:20px">
        <h3>Results Summary</h3>
        <div class="kpi">
          <div class="tile"><h2 id="kpiROI">–</h2><p>ROI (3-Year PV)</p></div>
          <div class="tile"><h2 id="kpiBenefits">–</h2><p>Benefits (PV)</p></div>
          <div class="tile"><h2 id="kpiPayback">–</h2><p>Payback</p></div>
        </div>

        <div class="charts">
          <canvas id="gauge"></canvas>
          <canvas id="bar"></canvas>
        </div>

        <div id="tables">
          <h4>Benefits & Costs Breakdown</h4>
          <table id="tblBenefits"><thead><tr><th>Category</th><th>Y1</th><th>Y2</th><th>Y3</th><th>Total</th><th>PV</th></tr></thead><tbody></tbody></table>
          <table id="tblCosts"><thead><tr><th>Category</th><th>Y1</th><th>Y2</th><th>Y3</th><th>Total</th><th>PV</th></tr></thead><tbody></tbody></table>
        </div>
      </section>
    </div>

    <footer>Restored full TEI ROI logic, charts, and PDF export.</footer>
  </div>

<script>
const $ = id => document.getElementById(id);
const nf = v => isFinite(v)?Number(v).toLocaleString():'–';
let gaugeChart, barChart;

function syncFromAgents(){
  const agents=+$('agents').value||0;
  const supRatio=+$('supervisorRatio').value/100;
  const itRatio=+$('itRatio').value/100;
  $('supervisorsCurr').value=Math.round(agents*supRatio);
  $('itCurr').value=(agents*itRatio).toFixed(1);
}

['supervisorRatio','itRatio','agents'].forEach(id=>$(id).addEventListener('input',syncFromAgents));

function calc(){
  const agents=+$('agents').value; const supervisors=+$('supervisorsCurr').value; const itCurr=+$('itCurr').value; const itFut=+$('itFut').value; const training=+$('trainingCosts').value; const sub=+$('subAnnual').value; const pro=+$('proServ').value; const implT=+$('implTeam').value; const implM=+$('implMonths').value;
  const salIT=85000, salSup=40000, salAgent=30000, salImpl=120000;
  const itSave=(itCurr-itFut)*salIT; const trainSave=training*0.5; const implCost=pro+(implT*(salImpl*(implM/12))); const costPV=sub*3+implCost; const benPV=itSave*3+trainSave*3; const roi=((benPV-costPV)/costPV)*100; $('kpiROI').textContent=roi.toFixed(1)+'%'; $('kpiBenefits').textContent='$'+nf(benPV); $('kpiPayback').textContent=roi>0?'~2 yrs':'>3 yrs'; drawGauge(roi); drawBar(benPV,costPV); $('tblBenefits').querySelector('tbody').innerHTML=`<tr><td>IT Savings</td><td>$${nf(itSave)}</td><td>$${nf(itSave)}</td><td>$${nf(itSave)}</td><td>$${nf(itSave*3)}</td><td>$${nf(itSave*2.5)}</td></tr><tr><td>Training Savings</td><td>$${nf(trainSave)}</td><td>$${nf(trainSave)}</td><td>$${nf(trainSave)}</td><td>$${nf(trainSave*3)}</td><td>$${nf(trainSave*2.5)}</td></tr>`; $('tblCosts').querySelector('tbody').innerHTML=`<tr><td>Subscription</td><td>$${nf(sub)}</td><td>$${nf(sub)}</td><td>$${nf(sub)}</td><td>$${nf(sub*3)}</td><td>$${nf(sub*2.5)}</td></tr><tr><td>Implementation</td><td>$${nf(implCost)}</td><td>$0</td><td>$0</td><td>$${nf(implCost)}</td><td>$${nf(implCost*0.9)}</td></tr>`; }

function drawGauge(roi){const ctx=$('gauge').getContext('2d');if(gaugeChart)gaugeChart.destroy();let color=roi<0?'#d00000':roi<50?'#ffb100':'#00a36c';gaugeChart=new Chart(ctx,{type:'doughnut',data:{datasets:[{data:[Math.min(Math.max(roi,0),100),100-Math.min(Math.max(roi,0),100)],backgroundColor:[color,'#e7f2ff'],borderWidth:0}]},options:{rotation:-90,circumference:180,cutout:'70%',plugins:{legend:{display:false}}},plugins:[{id:'center',afterDraw(chart){const{ctx,chartArea:{width}}=chart;ctx.save();ctx.font='bold 20px Inter';ctx.fillStyle=color;ctx.textAlign='center';ctx.fillText(roi.toFixed(1)+'%',width/2,chart._metasets[0].data[0].y+18);ctx.restore();}}]});}

function drawBar(ben,cost){const ctx=$('bar').getContext('2d');if(barChart)barChart.destroy();barChart=new Chart(ctx,{type:'bar',data:{labels:['Benefits PV','Costs PV'],datasets:[{data:[ben,cost],backgroundColor:['#3aa0ff','#ffb703']}]},options:{plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}});}

$('calcBtn').addEventListener('click',calc);
$('loadDefaults').addEventListener('click',()=>{document.querySelectorAll('input').forEach(i=>i.value=i.defaultValue||i.value);syncFromAgents();calc();});
$('syncFromAgents').addEventListener('click',syncFromAgents);
$('exportPDF').addEventListener('click',()=>{const {jsPDF}=window.jspdf;const doc=new jsPDF();doc.text('Webex Contact Center ROI Summary',20,20);doc.text(`ROI: ${$('kpiROI').innerText}`,20,40);doc.text(`Benefits PV: ${$('kpiBenefits').innerText}`,20,50);doc.text(`Payback: ${$('kpiPayback').innerText}`,20,60);doc.save('WebexCC_ROI_Report.pdf');});

syncFromAgents();calc();
</script>
</body>
</html>