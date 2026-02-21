<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ABH_CASINO</title>
<style>
body{margin:0;background:#060612;color:#00ffff;font-family:Arial,sans-serif;text-align:center;}
.neon{text-shadow:0 0 5px #00ffff,0 0 15px #ff0066;}
#topbar{position:absolute;top:15px;right:20px;font-size:18px;}
#logoutBtn{position:fixed;bottom:20px;left:20px;background:#ff0066;color:#fff;padding:8px 16px;border:none;border-radius:6px;cursor:pointer;}
h1{margin-top:70px;}
#balanceBox{font-size:18px;color:#ff0066;margin-top:10px;}
#game{display:grid;grid-template-columns:repeat(5,60px);grid-template-rows:repeat(3,60px);gap:10px;justify-content:center;margin-top:20px;}
.cell{width:60px;height:60px;background:#10102a;border:2px solid #00ffff;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:22px;cursor:pointer;transition:0.2s;}
.safe{background:#001a12;border-color:#00ff99;}
.mine{background:#2b0015;border-color:#ff0066;}
button{padding:8px 16px;margin:5px;border:none;border-radius:6px;background:#111133;color:#00ffff;border:1px solid #00ffff;cursor:pointer;}
input{padding:6px;width:140px;border-radius:6px;border:1px solid #00ffff;background:#10102a;color:#00ffff;margin:5px;text-align:center;}
.hidden{display:none;}
.slot-container{display:flex;justify-content:center;gap:10px;margin:20px;}
.slot{width:60px;height:60px;background:#10102a;border:2px solid #00ffff;display:flex;align-items:center;justify-content:center;font-size:24px;border-radius:6px;}
</style>
</head>
<body>

<!-- ===== Вход ===== -->
<div id="auth">
    <h1 class="neon">Вход</h1>
    <input type="text" id="login" placeholder="Логин"><br>
    <input type="password" id="password" placeholder="Пароль"><br>
    <button onclick="loginUser()">Вход</button>
    <button onclick="showRegister()">Регистрация</button>
</div>

<!-- ===== Регистрация ===== -->
<div id="registerScreen" class="hidden">
    <h1 class="neon">Регистрация</h1>
    <input type="text" id="regLogin" placeholder="Логин"><br>
    <input type="password" id="regPassword" placeholder="Пароль"><br>
    <button onclick="registerUser()">Зарегистрироваться</button>
    <button onclick="backToLogin()">Назад</button>
</div>

<!-- ===== Профиль ===== -->
<div id="profileScreen" class="hidden">
    <h1 class="neon">Профиль</h1>
    <input type="text" id="newLogin" placeholder="Новый логин"><br>
    <input type="password" id="newPassword" placeholder="Новый пароль"><br>
    <button onclick="updateProfile()">Сохранить</button>
    <button onclick="backToApp()">Назад</button>
    <div id="userBalanceProfile"></div>
</div>

<!-- ===== Пополнение ===== -->
<div id="depositScreen" class="hidden">
    <h1 class="neon">Пополнение баланса</h1>
    <p>Переведите от 500 ₽ на номер: <b>2202208452998850</b></p>
    <input type="number" id="depositAmount" placeholder="Сумма перевода от 500"><br>
    <input type="file" id="depositPhoto" accept="image/*"><br>
    <button onclick="sendDepositToBot()">Отправить заявку</button>
    <h3 id="depositStatus"></h3>
    <button onclick="backToApp()">Назад</button>
</div>

<!-- ===== Вывод средств ===== -->
<div id="withdrawScreen" class="hidden">
    <h1 class="neon">Вывод средств</h1>
    <div id="withdrawBalance"></div>
    <input type="number" id="withdrawAmount" placeholder="Сумма от 1000"><br>
    <button onclick="selectWithdraw('карта')">На карту</button>
    <button onclick="selectWithdraw('телефон')">На телефон</button><br>
    <input type="text" id="withdrawInput" class="hidden" placeholder=""><br>
    <button id="confirmWithdrawBtn" class="hidden" onclick="confirmWithdraw()">Вывести</button>
    <h3 id="withdrawStatus"></h3>
    <button onclick="backToApp()">Назад</button>
</div>

<!-- ===== Главный экран ===== -->
<div id="app" class="hidden">
    <button id="logoutBtn" onclick="logout()">Выйти</button>
    <h1 class="neon">💣 ABH_CASINO</h1>
    <div id="balanceBox">Баланс: 🪙 0</div>
    <div id="topbar">
        <button onclick="showProfile()">Профиль</button>
        <button onclick="showDeposit()">Пополнение</button>
        <button onclick="showGameSelection()">Игры</button>
        <button onclick="showWithdraw()">Вывод</button>
    </div>
</div>

<!-- ===== Выбор игры ===== -->
<div id="gameSelectionScreen" class="hidden">
    <h1 class="neon">Выберите игру</h1>
    <button onclick="showMines()">💣 Мины</button>
    <button onclick="showSlots()">🎰 Слоты</button>
    <button onclick="backToApp()">Назад</button>
</div>

<!-- ===== Мины ===== -->
<div id="minesScreen" class="hidden">
    <h1 class="neon">Мины</h1>
    Ставка: <input type="number" id="bet" value="100"><br>
    Мины (2-14): <input type="number" id="mineCount" value="3" min="2" max="14"><br>
    <button onclick="startGame()">Старт</button>
    <h3 id="status"></h3>
    <h3>Текущий выигрыш: 🪙 <span id="currentWin">0</span></h3>
    <button onclick="cashOut()">Забрать выигрыш</button>
    <div id="game"></div>
    <button onclick="showGameSelection()">Назад</button>
</div>

<!-- ===== Слоты ===== -->
<div id="slotsScreen" class="hidden">
    <h1 class="neon">Слоты 🎰</h1>
    Баланс: <span id="slotBalance">0</span><br>
    Ставка: <input type="number" id="slotBet" value="100"><br>
    <button onclick="spinSlots()">Крутить</button>
    <div class="slot-container">
        <div class="slot" id="slot1">🍒</div>
        <div class="slot" id="slot2">🍋</div>
        <div class="slot" id="slot3">🍊</div>
    </div>
    <h3 id="slotResult"></h3>
    <button onclick="showGameSelection()">Назад</button>
</div>

<script>
// ===== Telegram config =====
const TG_BOT_TOKEN = "8272855163:AAEWRU-RTd6Rj8nkxKtbDv5NZQH-RO-bbss";
const TG_CHAT_ID = "8431821142";

// ===== Переменные =====
let currentUser=null;
let balance=0;
let mines=[];
let opened=0;
let bet=0;
let currentWin=0;
let multiplier=1;
let gameActive=false;
let withdrawMethod=null;
const TOTAL_CELLS=15;
const slotSymbols=["🍒","🍋","🍊","🍉","⭐"];

// ===== UI =====
function hideAll(){ document.querySelectorAll("body > div").forEach(d=>d.classList.add("hidden")); }
function showApp(){ hideAll(); document.getElementById("app").classList.remove("hidden"); updateUI(); }
function showDeposit(){ hideAll(); document.getElementById("depositScreen").classList.remove("hidden"); }
function showGameSelection(){ hideAll(); document.getElementById("gameSelectionScreen").classList.remove("hidden"); }
function showMines(){ hideAll(); document.getElementById("minesScreen").classList.remove("hidden"); updateUI(); }
function showSlots(){ hideAll(); document.getElementById("slotsScreen").classList.remove("hidden"); updateUI(); }
function showWithdraw(){ hideAll(); document.getElementById("withdrawScreen").classList.remove("hidden"); updateWithdrawUI(); }
function backToApp(){ showApp(); }
function showRegister(){ hideAll(); document.getElementById("registerScreen").classList.remove("hidden"); }
function backToLogin(){ hideAll(); document.getElementById("auth").classList.remove("hidden"); }

// ===== Профиль =====
function showProfile(){
    hideAll();
    document.getElementById("profileScreen").classList.remove("hidden");
    if(currentUser){
        const data=JSON.parse(localStorage.getItem("user_"+currentUser));
        document.getElementById("newLogin").value=currentUser;
        document.getElementById("newPassword").value=data.password;
        document.getElementById("userBalanceProfile").innerText="Баланс: 🪙 "+data.balance;
    }
}

function updateProfile(){
    let newLogin=document.getElementById("newLogin").value.trim();
    let newPassword=document.getElementById("newPassword").value.trim();
    if(!newLogin||!newPassword){ alert("Введите данные"); return; }
    let data=JSON.parse(localStorage.getItem("user_"+currentUser));
    data.password=newPassword;
    if(newLogin!==currentUser){
        localStorage.removeItem("user_"+currentUser);
        currentUser=newLogin;
    }
    localStorage.setItem("user_"+currentUser,JSON.stringify(data));
    alert("Данные обновлены");
    showProfile();
}

// ===== Auth =====
function registerUser(){
    let login=document.getElementById("regLogin").value.trim();
    let password=document.getElementById("regPassword").value.trim();
    if(!login||!password){ alert("Введите данные"); return; }
    if(localStorage.getItem("user_"+login)){ alert("Пользователь существует"); return; }
    localStorage.setItem("user_"+login,JSON.stringify({password:password,balance:10000}));
    alert("Регистрация успешна");
    backToLogin();
}

function loginUser(){
    let login=document.getElementById("login").value.trim();
    let password=document.getElementById("password").value.trim();
    const data=localStorage.getItem("user_"+login);
    if(!data){ alert("Пользователь не найден"); return; }
    let user=JSON.parse(data);
    if(user.password!==password){ alert("Неверный пароль"); return; }
    currentUser=login;
    localStorage.setItem("activeUser",login);
    loadData();
    showApp();
}

function logout(){ localStorage.removeItem("activeUser"); currentUser=null; backToLogin(); }

// ===== Data =====
function loadData(){ const data=JSON.parse(localStorage.getItem("user_"+currentUser)); balance=data.balance; updateUI(); }
function saveData(){ const data=JSON.parse(localStorage.getItem("user_"+currentUser)); data.balance=balance; localStorage.setItem("user_"+currentUser,JSON.stringify(data)); }

// ===== UI Update =====
function updateUI(){
    document.getElementById("balanceBox").innerText="Баланс: 🪙 "+balance;
    document.getElementById("currentWin").innerText=currentWin;
    const slotEl=document.getElementById("slotBalance"); if(slotEl) slotEl.innerText=balance;
}
function updateWithdrawUI(){ document.getElementById("withdrawBalance").innerText="Баланс: 🪙 "+balance; }

// ===== Мины =====
function startGame(){
    bet=parseInt(document.getElementById("bet").value)||0;
    let mineTotal=parseInt(document.getElementById("mineCount").value)||3;
    if(mineTotal<2||mineTotal>14){ alert("Мины 2-14"); return; }
    if(bet>balance||bet<=0){ alert("Недостаточно средств"); return; }
    balance-=bet; currentWin=bet; multiplier=1; opened=0; gameActive=true; saveData(); updateUI();
    const game=document.getElementById("game"); game.innerHTML=""; mines=[];
    while(mines.length<mineTotal){ let r=Math.floor(Math.random()*TOTAL_CELLS); if(!mines.includes(r)) mines.push(r); }
    for(let i=0;i<TOTAL_CELLS;i++){ const cell=document.createElement("div"); cell.classList.add("cell"); cell.dataset.index=i; cell.onclick=()=>openCell(cell,mineTotal); game.appendChild(cell); }
    document.getElementById("status").innerText="Игра началась";
}
function openCell(cell,mineTotal){
    if(!gameActive) return;
    const index=parseInt(cell.dataset.index);
    if(mines.includes(index)){ cell.classList.add("mine"); cell.innerText="💣"; revealMines(); currentWin=0; gameActive=false; updateUI(); document.getElementById("status").innerText="Вы проиграли"; return; }
    cell.classList.add("safe"); cell.innerText="💎"; opened++; multiplier+=(mineTotal*0.12); currentWin=Math.floor(bet*multiplier); updateUI();
    document.getElementById("status").innerText="Множитель x"+multiplier.toFixed(2);
}
function cashOut(){ balance+=currentWin; currentWin=0; gameActive=false; saveData(); updateUI(); document.getElementById("status").innerText="Вы забрали выигрыш"; }
function revealMines(){ const cells=document.querySelectorAll(".cell"); mines.forEach(i=>{ cells[i].classList.add("mine"); cells[i].innerText="💣"; }); }

// ===== Слоты =====
function spinSlots(){
    const betAmount=parseInt(document.getElementById("slotBet").value)||0;
    if(betAmount>balance||betAmount<=0){ alert("Недостаточно средств"); return; }
    balance-=betAmount; updateUI();
    const s1=slotSymbols[Math.floor(Math.random()*slotSymbols.length)];
    const s2=slotSymbols[Math.floor(Math.random()*slotSymbols.length)];
    const s3=slotSymbols[Math.floor(Math.random()*slotSymbols.length)];
    document.getElementById("slot1").innerText=s1;
    document.getElementById("slot2").innerText=s2;
    document.getElementById("slot3").innerText=s3;
    let win=0;
    if(s1===s2 && s2===s3){ win=betAmount*5; document.getElementById("slotResult").innerText="🎉 Джекпот! Вы выиграли: "+win; }
    else if(s1===s2 || s2===s3 || s1===s3){ win=betAmount*2; document.getElementById("slotResult").innerText="Вы выиграли: "+win; }
    else{ document.getElementById("slotResult").innerText="Попробуйте снова"; }
    balance+=win; saveData(); updateUI();
}

// ===== Пополнение =====
function sendDepositToBot(){
    let amount=parseInt(document.getElementById("depositAmount").value);
    let photoInput=document.getElementById("depositPhoto");
    if(!amount||amount<500){ alert("Введите сумму от 500 ₽"); return; }
    if(photoInput.files.length===0){ alert("Загрузите фото перевода"); return; }
    let file=photoInput.files[0];
    let formData=new FormData();
    formData.append("chat_id",TG_CHAT_ID);
    formData.append("caption",`💰 Заявка на пополнение\nПользователь: ${currentUser}\nСумма: ${amount}`);
    formData.append("photo",file);

    const buttons={
        inline_keyboard:[
            [{text:"✅ Подтвердить",callback_data:`confirm_${currentUser}_${amount}`}],
            [{text:"❌ Отклонить",callback_data:`reject_${currentUser}_${amount}`}]
        ]
    };
    formData.append("reply_markup", JSON.stringify(buttons));

    fetch(`https://api.telegram.org/bot${TG_BOT_TOKEN}/sendPhoto`,{method:"POST",body:formData})
    .then(res=>res.json())
    .then(data=>{
        document.getElementById("depositStatus").innerText="Заявка отправлена, ждите подтверждения!";
    })
    .catch(err=>{
        document.getElementById("depositStatus").innerText="Не удалось отправить заявку!";
    });
}

// ===== Вывод средств =====
function selectWithdraw(method){
    withdrawMethod=method;
    const input=document.getElementById("withdrawInput");
    const btn=document.getElementById("confirmWithdrawBtn");
    input.classList.remove("hidden");
    btn.classList.remove("hidden");
    input.placeholder=method==="карта"?"Номер карты":"Номер телефона";
}

function confirmWithdraw(){
    let amount=parseInt(document.getElementById("withdrawAmount").value);
    let recipient=document.getElementById("withdrawInput").value.trim();
    if(!amount||amount<1000||amount>balance){ alert("Введите корректную сумму"); return; }
    if(!recipient){ alert("Введите данные для вывода"); return; }
    balance-=amount; saveData(); updateWithdrawUI();

    let text=`📤 Заявка на вывод\nПользователь: ${currentUser}\nСумма: ${amount}\nМетод: ${withdrawMethod}\nНомер: ${recipient}`;
    
    fetch(`https://api.telegram.org/bot${TG_BOT_TOKEN}/sendMessage?chat_id=${TG_CHAT_ID}&text=${encodeURIComponent(text)}`);

    document.getElementById("withdrawStatus").innerText="Заявка отправлена, ждите подтверждения!";
    document.getElementById("withdrawInput").value="";
    document.getElementById("withdrawAmount").value="";
    document.getElementById("withdrawInput").classList.add("hidden");
    document.getElementById("confirmWithdrawBtn").classList.add("hidden");
}

// ===== Автологин =====
window.onload = function(){
    let active = localStorage.getItem("activeUser");
    if(active && localStorage.getItem("user_"+active)){
        currentUser = active;
        loadData();
        showApp();
    }
}
</script>
</body>
</html>
