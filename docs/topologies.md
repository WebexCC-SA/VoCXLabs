# AI Agent Voice Calculator
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>AI Agent Voice Unit Calculator</title>
<style>
body {
font-family: Arial, sans-serif;
background-color: #f4f6f9;
margin: 0;
padding: 20px;
}
.container {
max-width: 700px;
margin: 0 auto;
background: white;
border-radius: 12px;
box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
padding: 30px;
}
h1 {
color: #005073;
text-align: center;
}
label {
font-weight: bold;
display: block;
margin-top: 15px;
}
input[type="number"] {
width: 100%;
padding: 8px;
border-radius: 6px;
border: 1px solid #ccc;
margin-top: 5px;
}
button {
background-color: #005073;
color: white;
border: none;
padding: 10px 20px;
border-radius: 6px;
cursor: pointer;
margin-top: 20px;
}
button:hover {
background-color: #0077b6;
}
.result {
margin-top: 20px;
padding: 15px;
background-color: #e8f4f8;
border-radius: 8px;
}
</style>
</head>
<body>
<div class="container">
<h1>AI Agent Voice Unit Calculator</h1>
<label>Number of Calls per Year</label>
<input type="number" id="callsYear" placeholder="e.g. 1000000" />


<label>Average Interaction Time (AIT - includes AHT + Wrap-up) (seconds)</label>
<input type="number" id="ait" placeholder="e.g. 120" />


<label>Session Duration (seconds)</label>
<input type="number" id="sessionDuration" placeholder="e.g. 120" />


<label>Sessions per Unit - Script Only</label>
<input type="number" id="scriptSessions" placeholder="e.g. 1200" />


<label>Sessions per Unit - Autonomous</label>
<input type="number" id="autoSessions" placeholder="e.g. 200" />


<button id="calcBtn">Calculate</button>


<div class="result" id="resultArea"></div>
</div>


<script>
document.getElementById('calcBtn').addEventListener('click', function() {
const callsYear = parseFloat(document.getElementById('callsYear').value);
const ait = parseFloat(document.getElementById('ait').value);
const sessionDuration = parseFloat(document.getElementById('sessionDuration').value);
const scriptSessions = parseFloat(document.getElementById('scriptSessions').value);
const autoSessions = parseFloat(document.getElementById('autoSessions').value);


if (isNaN(callsYear) || isNaN(ait) || isNaN(sessionDuration) || isNaN(scriptSessions) || isNaN(autoSessions)) {
document.getElementById('resultArea').innerHTML = '<p style="color:red">Please fill in all fields correctly.</p>';
return;
}


// Calculate Monthly Calls
const callsMonth = callsYear / 12;


// Number of minutes required (based on provided formula)
const totalMinutes = callsMonth * (ait);


// Number of sessions required
const totalSessions = totalMinutes / sessionDuration;


// Sessions needed for Script and Autonomous
const scriptUnits = totalSessions / scriptSessions;
const autoUnits = totalSessions / autoSessions;


document.getElementById('resultArea').innerHTML = `
<h3>Results:</h3>
<p><strong>Calls per Month:</strong> ${callsMonth.toLocaleString()}</p>
<p><strong>Total Minutes Required:</strong> ${totalMinutes.toLocaleString(undefined, {maximumFractionDigits: 2})}</p>
<p><strong>Total Sessions Required:</strong> ${totalSessions.toLocaleString(undefined, {maximumFractionDigits: 2})}</p>
<p><strong>Script Units Needed:</strong> ${scriptUnits.toLocaleString(undefined, {maximumFractionDigits: 2})}</p>
<p><strong>Autonomous Units Needed:</strong> ${autoUnits.toLocaleString(undefined, {maximumFractionDigits: 2})}</p>
`;
});
</script>
</body>
</html>