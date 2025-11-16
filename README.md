<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ตารางออมเงิน 30 วัน 🐻</title>
<link rel="stylesheet" href="Untitled-1.css">
<link rel="icon" href="https://i.pinimg.com/736x/72/f3/09/72f3092fba5d73c8c6a10e0bc79c44cb.jpg">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
  body {
    font-family: "Prompt", sans-serif;
    background: linear-gradient(180deg, #fffaf3 0%, #fff5e1 100%);
    text-align: center;
    padding: 40px 10px;
    color: #333;
  }
  h1 { font-size: 32px; margin-bottom: 10px; color: #444; }

  input[type="number"] {
    width: 70px; height: 30px;
    font-size: 16px; text-align: center;
    border: 2px solid #f0c090; border-radius: 8px;
    background-color: #fffaf3; transition: 0.3s;
  }
  input[type="number"]:focus {
    outline: none; border-color: #ffb16f; background-color:#fff5e1;
  }
  .total { font-weight: bold; font-size: 20px; margin: 20px 0; color: #7a4b19; }
  .sum-box { font-size: 26px; color: #d97400; }

  .layout{
    display:flex;
    justify-content:center;
    align-items:flex-start;
    gap:40px;
    margin-top:30px;
  }
  .leftBlock{ flex:0 0 420px; display:flex; flex-direction:column; align-items:center; }
  .rightBlock{ flex:0 0 300px; display:flex; justify-content:center; }

  table {
    width:420px !important;
    margin:0 !important;
    border-collapse: collapse;
    background: #fff;
    border-radius: 15px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }
  th,td { border: 1px solid #f0d8a8; width: calc(100%/7); height:60px; text-align:center; font-size:16px; }
  th { background:#ffe7ba; color:#5a3a00; font-weight:bold;}
  td div { font-weight:bold; color:#aa7a36; }

  .btn{
    background-color:#ffb86f; border:none; color:white; font-size:16px;
    border-radius:10px; padding:8px 20px; margin:10px 5px;
    cursor:pointer; transition:.3s;
  }
  .btn:hover{ background-color:#ffa63d; }

  #chartContainer{
    width:100%; background:#fffef8; padding:20px; border-radius:15px;
    box-shadow:0 3px 10px rgba(0,0,0,0.1); margin:0 !important;
  }
</style>
</head>

<body>
  <h1>ตารางออมเงิน 30 วัน 🐻</h1>
  <div class="total">💰 ยอดรวมทั้งหมด <span id="sum" class="sum-box">0</span> บาท</div>

  <div class="layout">

    <div class="leftBlock">
      <table>
        <thead>
          <tr><th>อา</th><th>จ</th><th>อ</th><th>พ</th><th>พฤ</th><th>ศ</th><th>ส</th></tr>
        </thead>
        <tbody id="savings-table"></tbody>
      </table>
      <button class="btn" onclick="resetAll()">🧼 ล้างใหม่หมด</button>
    </div>

    <div class="rightBlock">
      <div id="chartContainer">
        <canvas id="chart"></canvas>
      </div>
    </div>

  </div>

<script>
  const tableBody=document.getElementById("savings-table");
  const sumDisplay=document.getElementById("sum");
  let day=1;
  for(let r=0;r<5;r++){
    const tr=document.createElement("tr");
    for(let c=0;c<7;c++){
      const td=document.createElement("td");
      if(day<=30){
        td.innerHTML=`<div>${day}</div><input type="number" min="0" class="save-input" id="day-${day}">`;
        day++;
      }
      tr.appendChild(td);
    }
    tableBody.appendChild(tr);
  }

  function loadData(){
    const saved=JSON.parse(localStorage.getItem('savingData30'));
    if(saved){
      Object.keys(saved).forEach((k,i)=>{
        const inp=document.getElementById(`day-${i+1}`);
        if(inp) inp.value=saved[k];
      });
    }
    updateSum(); updateChart();
  }

  function saveData(){
    const data={};
    document.querySelectorAll('.save-input').forEach((i,n)=>data[`day${n+1}`]=i.value);
    localStorage.setItem('savingData30',JSON.stringify(data));
    updateChart();
  }

  function updateSum(){
    let total=0;
    document.querySelectorAll('.save-input').forEach(i=>total+=Number(i.value)||0);
    sumDisplay.textContent=total.toLocaleString();
  }

  function resetAll(){
    if(confirm("ต้องการล้างข้อมูลทั้งหมดใช่ไหม?")){
      document.querySelectorAll('.save-input').forEach(i=>i.value='');
      localStorage.removeItem('savingData30'); updateSum(); updateChart();
    }
  }

  let chart;
  function updateChart(){
    const values=[];
    document.querySelectorAll('.save-input').forEach(i=>values.push(Number(i.value)||0));
    const ctx=document.getElementById('chart').getContext('2d');
    if(chart) chart.destroy();
    chart=new Chart(ctx,{type:'bar',data:{labels:Array.from({length:30},(_,i)=>i+1),
    datasets:[{label:'กราฟต่อวัน (บาท)',data:values,backgroundColor:'#ffb16f',borderRadius:5}]},
    options:{scales:{y:{beginAtZero:true}}}});
  }

  document.addEventListener('input',e=>{
    if(e.target.classList.contains('save-input')){ updateSum(); saveData(); }
  });

  loadData();
</script>
</body>
</html>
