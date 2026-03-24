<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<title>Serenity Companion Pro</title>

<style>
body {
    font-family: "Helvetica", sans-serif;
    background: #f4f1ea;
    margin: 0;
    padding: 20px;
    color: #2f2f2f;
}

h1 {
    text-align: center;
    letter-spacing: 3px;
}

.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    margin-top: 30px;
}

.card {
    background: #fff;
    border-radius: 18px;
    padding: 20px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.05);
}

.title {
    font-weight: bold;
    margin-bottom: 10px;
}

/* 植物状态 */
.plant {
    font-size: 50px;
    text-align: center;
    transition: 0.3s;
}

/* 按钮 */
button {
    border: none;
    border-radius: 20px;
    padding: 8px 12px;
    margin: 5px;
    cursor: pointer;
    background: #e3eee0;
}
button:hover {
    background: #cfe3cc;
}

/* 呼吸动画 */
.breath-circle {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: #a8c3a6;
    margin: 10px auto;
    animation: none;
}

@keyframes breath {
    0% { transform: scale(1); opacity: 0.7;}
    50% { transform: scale(1.6); opacity: 0.3;}
    100% { transform: scale(1); opacity: 0.7;}
}

/* 进度 */
.progress {
    height: 10px;
    background: #eee;
    border-radius: 10px;
    overflow: hidden;
}
.bar {
    height: 100%;
    background: #7fa87f;
    width: 0%;
}

/* 趋势 */
.trend-bar {
    display: flex;
    align-items: flex-end;
    height: 80px;
}
.trend-bar div {
    width: 10px;
    margin: 2px;
    background: #a8c3a6;
}

textarea {
    width: 100%;
    border-radius: 10px;
    padding: 10px;
}
</style>
</head>

<body>

<h1>SERENITY COMPANION</h1>

<div class="container">

<!-- 植物状态 -->
<div class="card">
    <div class="title">🌿 状态植物</div>
    <div id="plant" class="plant">🌱</div>
    <p id="stateText">等待记录</p>
</div>

<!-- 情绪捕捉 -->
<div class="card">
    <div class="title">情绪捕捉</div>
    <button onclick="setMood(1)">😊</button>
    <button onclick="setMood(0)">😌</button>
    <button onclick="setMood(-1)">😰</button>
    <button onclick="setMood(-2)">😞</button>
</div>

<!-- 树洞 -->
<div class="card">
    <div class="title">匿名树洞</div>
    <textarea id="journal"></textarea>
    <button onclick="saveJournal()">记录</button>
</div>

<!-- 状态诊断 -->
<div class="card">
    <div class="title">状态诊断</div>
    <p id="diagnosis">暂无数据</p>
</div>

<!-- 干预 -->
<div class="card">
    <div class="title">轻干预</div>
    <button onclick="task()">做5分钟简单题</button>
    <button onclick="startBreath()">呼吸</button>
    <div class="breath-circle" id="circle"></div>
    <p id="actionText"></p>
</div>

<!-- 趋势 -->
<div class="card">
    <div class="title">情绪趋势</div>
    <div class="trend-bar" id="trend"></div>
</div>

</div>

<script>
let moods = [];
let progress = 0;

function setMood(val) {
    moods.push(val);
    updatePlant();
    updateDiagnosis();
    updateTrend();
}

function updatePlant() {
    let avg = moods.slice(-3).reduce((a,b)=>a+b,0)/Math.min(3,moods.length);
    let plant = document.getElementById("plant");
    let text = document.getElementById("stateText");

    if(avg > 0.5){
        plant.innerText = "🌳";
        text.innerText = "状态很好，持续成长";
    } else if(avg > -0.5){
        plant.innerText = "🌿";
        text.innerText = "状态平稳";
    } else {
        plant.innerText = "🥀";
        text.innerText = "需要休息一下";
    }
}

function updateDiagnosis(){
    let last3 = moods.slice(-3);
    if(last3.filter(v=>v<0).length>=2){
        document.getElementById("diagnosis").innerText = "连续低状态，建议降低任务难度";
    } else {
        document.getElementById("diagnosis").innerText = "状态正常";
    }
}

function task(){
    document.getElementById("actionText").innerText = "已完成一个小行动 👍";
}

function startBreath(){
    let c = document.getElementById("circle");
    c.style.animation = "breath 3s infinite";
    document.getElementById("actionText").innerText = "跟随呼吸节奏 🌬";
}

function updateTrend(){
    let container = document.getElementById("trend");
    container.innerHTML = "";
    moods.slice(-10).forEach(v=>{
        let bar = document.createElement("div");
        bar.style.height = (v+3)*15 + "px";
        container.appendChild(bar);
    });
}

function saveJournal(){
    alert("已记录");
}
</script>

</body>
</html>
