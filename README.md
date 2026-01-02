<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Карточка Пропуска</title>
<style>
body{
  margin:0;
  font-family:Arial, sans-serif;
  background:#e5e7eb;
  color:#111;
  display:flex;
  justify-content:center;
  align-items:flex-start;
  min-height:100vh;
}

.container{
  width:360px;
  padding:20px;
  box-sizing:border-box;
  background:#f3f4f6; /* фон контейнера, чтобы видно было шаги */
  border-radius:12px;
  margin-top:20px;
}

input{
  width:100%;
  padding:12px;
  font-size:16px;
  border-radius:10px;
  border:1px solid #ccc;
  margin-bottom:12px;
}

button{
  width:100%;
  padding:14px;
  font-size:16px;
  border:none;
  border-radius:12px;
  background:#2563eb;
  color:white;
  cursor:pointer;
}

.error{
  color:#f87171;
  margin-bottom:10px;
}

.card{
  width:280px;
  height:180px;
  margin:20px auto;
  border-radius:16px;
  padding:20px;
  background:linear-gradient(135deg,#4f46e5,#2563eb);
  color:white;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  box-shadow:0 4px 12px rgba(0,0,0,0.3);
}

.card-title{
  font-size:20px;
  font-weight:bold;
  text-align:center;
}

.lock{
  width:50px;
  height:50px;
  border-radius:50%;
  background:#111;
  color:white;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:24px;
  margin:20px auto 0;
  cursor:pointer;
}

.lock-text{
  text-align:center;
  margin-top:10px;
  display:none;
}

.step{display:none;}
.step.active{display:block;}
</style>
</head>
<body>

<div class="container">

  <!-- Шаг 1: имя -->
  <div class="step" id="step1">
    <h2>Шаг 1: Введите имя</h2>
    <input id="name" placeholder="Введите своё имя">
    <button onclick="step1()">Далее</button>
  </div>

  <!-- Шаг 2: отгадать имя -->
  <div class="step" id="step2">
    <h2>Шаг 2: Отгадайте имя Мисы</h2>
    <div class="error" id="e2"></div>
    <input id="q1" placeholder="Введите имя">
    <button onclick="step2()">Далее</button>
  </div>

  <!-- Шаг 3: отгадать ник Roblox -->
  <div class="step" id="step3">
    <h2>Шаг 3: Угадайте никнейм Мисы в Roblox</h2>
    <div class="error" id="e3"></div>
    <input id="q2" placeholder="Введите никнейм">
    <button onclick="finish()">Готово</button>
  </div>

  <!-- Карточка -->
  <div class="step" id="card">
    <div class="card">
      <div class="card-title" id="cardName"></div>
    </div>
    <div class="lock" onclick="toggleText()">🔐</div>
    <div class="lock-text" id="lockText">Поднесите эту карточку к терминалу.</div>
  </div>

</div>

<script>
// Система сохранения
window.onload = function(){
  const step = localStorage.getItem('step');
  const name = localStorage.getItem('userName');
  const lockVisible = localStorage.getItem('lockVisible') === 'true';

  if(name){
    document.getElementById('cardName').innerText = name;
  }

  if(step){
    show(step);
  }else{
    show('step1');
  }

  if(lockVisible){
    document.getElementById('lockText').style.display = "block";
  }
}

function show(id){
  document.querySelectorAll('.step').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  localStorage.setItem('step', id);
}

function step1(){
  const n=document.getElementById('name').value.trim();
  if(!n)return;
  localStorage.setItem('userName', n);
  show('step2');
}

function step2(){
  if(document.getElementById('q1').value.trim() !== "Карина"){
    document.getElementById('e2').innerText="Ответ неправильный.";
    return;
  }
  show('step3');
}

function finish(){
  if(document.getElementById('q2').value.trim() !== "KROV_Misa"){
    document.getElementById('e3').innerText="Ответ неправильный.";
    return;
  }
  document.getElementById('cardName').innerText = localStorage.getItem('userName');
  show('card');
}

// кнопка 🔐
function toggleText(){
  const txt = document.getElementById('lockText');
  if(txt.style.display === "block"){
    txt.style.display = "none";
    localStorage.setItem('lockVisible', "false");
  }else{
    txt.style.display = "block";
    localStorage.setItem('lockVisible', "true");
  }
}
</script>

</body>
</html>
