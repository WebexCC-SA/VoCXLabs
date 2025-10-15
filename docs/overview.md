##ROI Calculator For use for PVT - CX Experience only
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Agent ROI Calculator</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root { color-scheme: only light; }
    body {font-family: Arial, sans-serif; background:#f4f6f9; margin:0; padding:20px;}
    .container {max-width:850px; margin:0 auto; background:#fff; padding:30px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.1); color:#000;}
    h1 {text-align:center; color:#005073; margin-bottom:20px;}
    label {display:block; margin-top:15px; font-weight:bold;}
    small {display:block; color:#555; margin-top:3px; font-size:13px;}
    input[type='number'] {width:100%; padding:10px; border:1px solid #ccc; border-radius:6px; margin-top:5px;}
    button {margin-top:20px; background:#005073; color:#fff; border:none; padding:12px 18px; border-radius:6px; cursor:pointer; font-size:16px;}
    button:hover {background:#0077b6;}
    .preset-buttons {display:flex; flex-wrap:wrap; gap:10px; justify-content:center; margin-top:20px;}
    .preset-buttons button {background:#0077b6; color:white; border:none; padding:10px 14px; border-radius:6px; cursor:pointer;}
    .preset-buttons button:hover {background:#005073;}
    .results {margin-top:30px; background:#e8f4f8; padding:20px; border-radius:8px; color:#000;}
    .results h3 {margin-top:0; color:#005073;}
    canvas {margin-top:20px; width:100%; max-height:300px;}
  </style>
</head>
<body>
  <div class="container">
    <h1>AI Agent ROI Calculator</h1>

    <div class="preset-buttons">
      <button onclick="loadScenario('default')">Default Example</button>
      <button onclick="loadScenario('services200')">Services - 200 Agents</button>
      <button onclick="loadScenario('small50')">Small Contact Center - 50 Agents</button>
      <button onclick="loadScenario('enterprise500')">Enterprise - 500 Agents</button>
    </div>

    <label for="agents">Number of Agents</label>
    <small>Total number of live agents in your contact center. <strong>Default: 100 agents.</strong></small>
    <input type="number" id="agents" placeholder="e.g. 100" />

    <label for="costAgent">Agent Loaded Cost per Year ($)</label>
    <small>Total annual cost per agent including salary, benefits, and tools. <strong>Default: $36,000/year ($3K/month).</strong></small>
    <input type="number" id="costAgent" placeholder="e.g. 36000" />

    <label for="supervisors">Number of Supervisors</label>
    <small>Supervisors typically represent about <strong>10% of total agents</strong>. <strong>Default: 10 supervisors for 100 agents.</strong></small>
    <input type="number" id="supervisors" placeholder="e.g. 10" />

    <label for="costSupervisor">Supervisor Cost per Year ($)</label>
    <small>Supervisors generally cost about <strong>25% more than agents</strong> due to higher pay and responsibility. <strong>Default: $45,000/year ($3.75K/month).</strong></small>
    <input type="number" id="costSupervisor" placeholder="e.g. 45000" />

    <label for="aiLicenses">AI Licenses ($)</label>
    <small>Annual AI software license investment. <strong>Default: $150,000.</strong></small>
    <input type="number" id="aiLicenses" placeholder="e.g. 150000" />

    <label for="aiServices">AI Services ($)</label>
    <small>Annual AI setup, support, or customization cost. <strong>Default: $300,000.</strong></small>
    <input type="number" id="aiServices" placeholder="e.g. 300000" />

    <label for="agentsSaved">Agents Saved (FTE Reduction)</label>
    <small>Estimated full-time equivalents reduced through automation. <strong>Default: 10 agents (10% savings).</strong></small>
    <input type="number" id="agentsSaved" placeholder="e.g. 10" />

    <label for="handleSavings">Handle Time Savings ($)</label>
    <small>Savings from faster handling and reduced call times. <strong>Default: $100,000/year (~5–10% handle-time reduction).</strong></small>
    <input type="number" id="handleSavings" placeholder="e.g. 100000" />

    <label for="hiringSavings">Hiring Cost Savings ($)</label>
    <small>Reduction in recruitment and onboarding expenses. <strong>Default: $50,000/year.</strong></small>
    <input type="number" id="hiringSavings" placeholder="e.g. 50000" />

    <button id="calcBtn">Calculate ROI</button>

    <div class="results" id="results"></div>
    <canvas id="roiGauge"></canvas>
    <canvas id="roiBar"></canvas>
  </div>

  <script>
    let roiGaugeChart, roiBarChart;

    function loadScenario(type) {
      const s = {
        default: {agents:100, costAgent:36000, supervisors:10, costSupervisor:45000, aiLicenses:150000, aiServices:300000, agentsSaved:10, handleSavings:100000, hiringSavings:50000},
        services200: {agents:200, costAgent:36000, supervisors:20, costSupervisor:45000, aiLicenses:300000, aiServices:960000, agentsSaved:57, handleSavings:360000, hiringSavings:244000},
        small50: {agents:50, costAgent:32000, supervisors:5, costSupervisor:40000, aiLicenses:80000, aiServices:200000, agentsSaved:8, handleSavings:60000, hiringSavings:20000},
        enterprise500: {agents:500, costAgent:38000, supervisors:50, costSupervisor:48000, aiLicenses:500000, aiServices:1200000, agentsSaved:120, handleSavings:750000, hiringSavings:350000}
      }[type];

      for (const key in s) {
        document.getElementById(key).value = s[key];
      }
    }

    document.getElementById('calcBtn').addEventListener('click', () => {
      const agents = parseFloat(document.getElementById('agents').value) || 0;
      const costAgent = parseFloat(document.getElementById('costAgent').value) || 0;
      const supervisors = parseFloat(document.getElementById('supervisors').value) || 0;
      const costSupervisor = parseFloat(document.getElementById('costSupervisor').value) || 0;
      const aiLicenses = parseFloat(document.getElementById('aiLicenses').value) || 0;
      const aiServices = parseFloat(document.getElementById('aiServices').value) || 0;
      const agentsSaved = parseFloat(document.getElementById('agentsSaved').value) || 0;
      const handleSavings = parseFloat(document.getElementById('handleSavings').value) || 0;
      const hiringSavings = parseFloat(document.getElementById('hiringSavings').value) || 0;

      const annualLaborCost = (agents * costAgent) + (supervisors * costSupervisor);
      const aiInvestment = aiLicenses + aiServices;
      const aiSavings = agentsSaved * costAgent;
      const totalSavings = aiSavings + handleSavings + hiringSavings;
      const netSavings = totalSavings - aiInvestment;
      const roi = aiInvestment > 0 ? (netSavings / aiInvestment) * 100 : 0;

      document.getElementById('results').innerHTML = `
        <h3>Results</h3>
        <p><strong>Annual Labor Cost:</strong> $${annualLaborCost.toLocaleString()}</p>
        <p><strong>AI Investment:</strong> $${aiInvestment.toLocaleString()}</p>
        <p><strong>Total Annual Savings:</strong> $${totalSavings.toLocaleString()}</p>
        <p><strong>Net Savings:</strong> $${netSavings.toLocaleString(undefined, {maximumFractionDigits:2})}</p>
        <p><strong>ROI:</strong> ${roi.toLocaleString(undefined, {maximumFractionDigits:2})}%</p>
      `;

      updateCharts(roi, totalSavings, aiInvestment);
    });

    function updateCharts(roi, totalSavings, aiInvestment) {
      const ctxGauge = document.getElementById('roiGauge').getContext('2d');
      const ctxBar = document.getElementById('roiBar').getContext('2d');

      if (roiGaugeChart) roiGaugeChart.destroy();
      if (roiBarChart) roiBarChart.destroy();

      let gaugeColor = '#00b300';
      if (roi < 0) gaugeColor = '#d00000';
      else if (roi < 50) gaugeColor = '#ffcc00';

      roiGaugeChart = new Chart(ctxGauge, {
        type: 'doughnut',
        data: {
          labels: ['ROI %', 'Remaining'],
          datasets: [{
            data: [Math.min(Math.max(roi, 0), 100), 100 - Math.min(Math.max(roi, 0), 100)],
            backgroundColor: [gaugeColor, '#d0e7f9'],
            borderWidth: 0
          }]
        },
        options: {
          rotation: -90,
          circumference: 180,
          cutout: '70%',
          plugins: {
            legend: { display: false },
            tooltip: { enabled: false },
            title: {
              display: true,
              text: 'ROI Gauge',
              color: '#005073',
              font: { size: 16, weight: 'bold' }
            }
          }
        },
        plugins: [{
          id: 'centerText',
          afterDraw(chart) {
            const { ctx, chartArea: { width } } = chart;
            ctx.save();
            ctx.font = 'bold 22px Arial';
            ctx.fillStyle = gaugeColor;
            ctx.textAlign = 'center';
            ctx.fillText(`${roi.toFixed(1)}%`, width / 2, chart._metasets[0].data[0].y + 20);
            ctx.restore();
          }
        }]
      });

      roiBarChart = new Chart(ctxBar, {
        type: 'bar',
        data: {
          labels: ['Total Savings', 'AI Investment'],
          datasets: [{
            label: 'USD ($)',
            data: [totalSavings, aiInvestment],
            backgroundColor: ['#00b4d8', '#ffb703']
          }]
        },
        options: {
          scales: {
            y: {beginAtZero: true, ticks: {callback: value => '$' + value.toLocaleString()}}
          },
          plugins: {
            legend: {display: false},
            title: {
              display: true,
              text: 'Total Savings vs Investment',
              color: '#005073',
              font: {size: 16, weight: 'bold'}
            }
          }
        }
      });
    }
  </script>
</body>
</html>
