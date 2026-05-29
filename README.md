<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>EcoFishing 🎣</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;user-select:none;-webkit-tap-highlight-color:transparent;}
html,body{
  background:#0a1a0f;
  display:flex;
  align-items:flex-start;
  justify-content:center;
  min-height:100vh;
  min-height:100dvh;
  font-family:system-ui,sans-serif;
  overflow-x:hidden;
}
#game{
  width:100%;
  max-width:680px;
  position:relative;
  background:#1a4a2e;
  display:flex;
  flex-direction:column;
  min-height:100vh;
  min-height:100dvh;
}
@media(min-width:700px){
  #game{
    border-radius:16px;
    margin:20px auto;
    min-height:0;
    box-shadow:0 8px 40px rgba(0,0,0,0.6);
  }
  html,body{align-items:center;}
}

/* HUD */
#hud{
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:8px 10px;
  background:rgba(0,0,0,0.5);
  color:#fff;
  font-weight:600;
  gap:6px;
  flex-shrink:0;
  min-height:48px;
}
#hud .score{font-size:clamp(15px,4vw,20px);font-weight:800;color:#ffe066;}
#hud .timer{font-size:clamp(14px,3.5vw,18px);font-weight:800;color:#ff6b6b;}
#hud .hud-label{font-size:clamp(8px,2vw,10px);opacity:0.7;}
#clean-bar-wrap{flex:1;background:rgba(255,255,255,0.18);border-radius:8px;height:11px;overflow:hidden;min-width:0;}
#clean-bar{height:100%;background:#4caf50;border-radius:8px;transition:width 0.5s;}

/* RIVER */
#river{
  position:relative;
  width:100%;
  flex:1 1 auto;
  overflow:hidden;
  cursor:crosshair;
  touch-action:none;
  min-height:120px;
}
canvas#gc{
  position:absolute;
  inset:0;
  width:100%;
  height:100%;
  z-index:2;
  display:block;
}
#fisher-wrap{
  position:absolute;
  bottom:0;
  left:50%;
  transform:translateX(-50%);
  z-index:10;
  pointer-events:none;
}
#fisher-wrap svg{
  width:clamp(50px,10vw,70px);
  height:auto;
}
#line-svg{
  position:absolute;
  inset:0;
  width:100%;
  height:100%;
  z-index:9;
  pointer-events:none;
}

/* DISCARD PANEL */
#discard-panel{
  display:none;
  position:absolute;
  inset:0;
  background:rgba(0,0,0,0.88);
  z-index:50;
  align-items:center;
  justify-content:center;
  flex-direction:column;
  gap:8px;
  padding:12px;
}
#discard-panel.show{display:flex;}
#discard-panel .dp-icon{font-size:clamp(36px,10vw,52px);}
#discard-panel .dp-name{color:#fff;font-size:clamp(12px,3.5vw,15px);font-weight:700;text-align:center;}
#discard-panel .dp-q{color:#ffe066;font-size:clamp(11px,3vw,13px);}
#discard-bins{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;width:100%;max-width:380px;}
.dbb{
  padding:7px 8px;
  border-radius:10px;
  border:none;
  font-size:clamp(16px,4.5vw,20px);
  cursor:pointer;
  transition:transform 0.15s;
  display:flex;
  flex-direction:column;
  align-items:center;
  gap:2px;
  flex:1 1 calc(33% - 6px);
  min-width:60px;
  max-width:90px;
}
.dbb span{font-size:clamp(7px,1.8vw,9px);font-weight:800;color:#fff;letter-spacing:0.3px;text-align:center;}
.dbb:hover,.dbb:active{transform:scale(1.1);}

/* MSG */
#msg{
  position:absolute;
  top:40%;
  left:50%;
  transform:translate(-50%,-50%);
  font-size:clamp(14px,4vw,20px);
  font-weight:800;
  color:#fff;
  text-shadow:0 2px 8px rgba(0,0,0,0.9);
  z-index:60;
  pointer-events:none;
  opacity:0;
  transition:opacity 0.25s;
  white-space:nowrap;
}
#msg.show{opacity:1;}

/* OVERLAY */
#overlay{
  display:none;
  position:absolute;
  inset:0;
  background:rgba(0,0,0,0.92);
  z-index:80;
  align-items:center;
  justify-content:center;
  flex-direction:column;
  gap:10px;
  text-align:center;
  padding:16px 20px;
  overflow-y:auto;
}
#overlay.show{display:flex;}
#overlay h2{color:#ffe066;font-size:clamp(18px,5vw,22px);font-weight:800;}
#overlay p{color:#fff;font-size:clamp(11px,3vw,14px);line-height:1.6;max-width:300px;}
.big-pct{font-size:clamp(48px,15vw,64px);font-weight:900;line-height:1;}
#overlay button{
  padding:11px 28px;
  border-radius:12px;
  background:#4caf50;
  border:none;
  color:#fff;
  font-size:clamp(14px,4vw,17px);
  font-weight:800;
  cursor:pointer;
  margin-top:4px;
  -webkit-tap-highlight-color:transparent;
}
#overlay button:active{transform:scale(0.97);}

/* BIN AREA */
#bin-area{
  display:flex;
  justify-content:space-around;
  padding:5px 3px;
  background:rgba(0,0,0,0.55);
  gap:2px;
  flex-shrink:0;
}
.bin{
  flex:1;
  min-width:0;
  border-radius:8px;
  padding:5px 1px;
  display:flex;
  flex-direction:column;
  align-items:center;
  gap:2px;
  cursor:pointer;
  transition:transform 0.15s;
  border:2px solid transparent;
}
.bin:hover,.bin:active{transform:translateY(-3px);}
.bin.correct{animation:cflash 0.4s;}
.bin.wrong{animation:wshake 0.4s;}
@keyframes cflash{0%,100%{opacity:1}50%{opacity:0.2}}
@keyframes wshake{0%,100%{transform:translateX(0)}25%{transform:translateX(-5px)}75%{transform:translateX(5px)}}
.bin-icon{font-size:clamp(14px,4.5vw,20px);}
.bin-label{font-size:clamp(6px,1.8vw,8px);font-weight:800;color:#fff;text-align:center;text-transform:uppercase;letter-spacing:0.3px;line-height:1.2;}

/* BONUS ANIM */
.bonus-anim{
  position:absolute;
  font-size:clamp(13px,3.5vw,17px);
  font-weight:800;
  text-shadow:0 1px 4px rgba(0,0,0,0.7);
  z-index:70;
  pointer-events:none;
  animation:riseup 1.1s forwards;
}
@keyframes riseup{0%{opacity:1;transform:translateY(0)}100%{opacity:0;transform:translateY(-55px)}}

/* HINT */
#hint{
  position:absolute;
  bottom:6px;
  left:50%;
  transform:translateX(-50%);
  color:rgba(255,255,255,0.85);
  font-size:clamp(9px,2.5vw,11px);
  font-weight:600;
  z-index:5;
  pointer-events:none;
  background:rgba(0,0,0,0.45);
  padding:3px 10px;
  border-radius:20px;
  white-space:nowrap;
}
</style>
</head>
<body>
<div id="game">
  <div id="hud">
    <div>
      <div class="hud-label">PONTOS</div>
      <div class="score" id="score-display">0</div>
    </div>
    <div style="flex:1;padding:0 6px;">
      <div class="hud-label" style="margin-bottom:3px;">🌿 LIMPEZA DO RIO</div>
      <div id="clean-bar-wrap"><div id="clean-bar" style="width:0%"></div></div>
    </div>
    <div style="text-align:right">
      <div class="hud-label">TEMPO</div>
      <div class="timer" id="timer-display">3:00</div>
    </div>
  </div>

  <div id="river">
    <canvas id="gc"></canvas>
    <div id="fisher-wrap">
      <svg viewBox="0 0 70 90">
        <ellipse cx="35" cy="78" rx="22" ry="7" fill="#0d3a50" opacity="0.4"/>
        <rect x="20" y="55" width="30" height="22" rx="6" fill="#c8a46e"/>
        <circle cx="35" cy="48" r="12" fill="#f5c18a"/>
        <ellipse cx="35" cy="44" rx="14" ry="7" fill="#7a4e2d"/>
        <rect x="30" y="48" width="10" height="3" rx="2" fill="#e8a06a"/>
        <rect x="47" y="51" width="20" height="3" rx="2" fill="#7a4e2d" transform="rotate(-15 47 51)"/>
        <circle cx="22" cy="55" r="5" fill="#c8a46e"/>
        <circle cx="48" cy="55" r="5" fill="#c8a46e"/>
      </svg>
    </div>
    <svg id="line-svg" style="position:absolute;inset:0;z-index:9;pointer-events:none;"></svg>
    <div id="msg"></div>
    <div id="discard-panel">
      <div class="dp-icon" id="dp-icon"></div>
      <div class="dp-name" id="dp-name"></div>
      <div class="dp-q">Onde descartar?</div>
      <div id="discard-bins"></div>
    </div>
    <div id="overlay" class="show">
      <h2>🎣 EcoFishing</h2>
      <p style="font-size:clamp(10px,2.8vw,12px);opacity:0.75;">
        Dê <b>duplo toque</b> nas manchas escuras na água para pescar!<br>
        Segure o animal para libertá-lo. Recicle o lixo corretamente.
      </p>
      <div style="font-size:clamp(10px,2.8vw,12px);color:#adf;line-height:2;">
        ♻️ Reciclar certo: <b>+16pts</b> &nbsp;|&nbsp; ❌ Errado: <b>-12pts</b><br>
        🐢 Salvar animal: <b>+40pts</b> &nbsp;|&nbsp; 🎣 Pescar: <b>+8pts</b>
      </div>
      <button id="start-btn">🎮 Começar!</button>
    </div>
    <div id="hint">Duplo toque na mancha escura para pescar!</div>
  </div>

  <div id="bin-area">
    <div class="bin" style="background:#1565c0" data-type="papel" onclick="tryDiscard('papel')">
      <div class="bin-icon">📄</div><div class="bin-label">Azul<br>Papel</div>
    </div>
    <div class="bin" style="background:#c62828" data-type="plastico" onclick="tryDiscard('plastico')">
      <div class="bin-icon">🧴</div><div class="bin-label">Verm.<br>Plástico</div>
    </div>
    <div class="bin" style="background:#2e7d32" data-type="vidro" onclick="tryDiscard('vidro')">
      <div class="bin-icon">🍶</div><div class="bin-label">Verde<br>Vidro</div>
    </div>
    <div class="bin" style="background:#f9a825" data-type="metal" onclick="tryDiscard('metal')">
      <div class="bin-icon">🥫</div><div class="bin-label">Amar.<br>Metal</div>
    </div>
    <div class="bin" style="background:#4e342e" data-type="organico" onclick="tryDiscard('organico')">
      <div class="bin-icon">🌿</div><div class="bin-label">Marr.<br>Orgân.</div>
    </div>
    <div class="bin" style="background:#424242" data-type="rejeito" onclick="tryDiscard('rejeito')">
      <div class="bin-icon">🚫</div><div class="bin-label">Cinza<br>Rejeito</div>
    </div>
  </div>
</div>

<script>
const TRASH=[
  {icon:'🥤',name:'Canudo',type:'plastico'},{icon:'🛍️',name:'Sacola plástica',type:'plastico'},
  {icon:'🧴',name:'Embalagem',type:'plastico'},{icon:'🥤',name:'Copo descartável',type:'plastico'},
  {icon:'🪣',name:'Balde quebrado',type:'plastico'},{icon:'🧸',name:'Brinquedo plástico',type:'plastico'},
  {icon:'🥫',name:'Lata de refrigerante',type:'metal'},{icon:'⛓️',name:'Corrente enferrujada',type:'metal'},
  {icon:'🪝',name:'Anzol velho',type:'metal'},{icon:'🍳',name:'Panela velha',type:'metal'},
  {icon:'🫙',name:'Pote de conserva',type:'vidro'},{icon:'🍶',name:'Garrafa quebrada',type:'vidro'},
  {icon:'📰',name:'Jornal molhado',type:'papel'},{icon:'📦',name:'Caixa de papelão',type:'papel'},
  {icon:'🧻',name:'Embalagem de papel',type:'papel'},{icon:'👶',name:'Fralda',type:'rejeito'},
  {icon:'😷',name:'Máscara descartável',type:'rejeito'},{icon:'🦺',name:'Pano rasgado',type:'rejeito'},
  {icon:'🔋',name:'Pilha enferrujada',type:'rejeito'}
];
const ANIMALS=[
  {icon:'🐍',name:'Cobra',trash:{icon:'🛍️',name:'Rede plástica',type:'plastico'}},
  {icon:'🐢',name:'Tartaruga',trash:{icon:'🥤',name:'Canudo',type:'plastico'}},
  {icon:'🐸',name:'Sapo',trash:{icon:'🍶',name:'Garrafa',type:'vidro'}},
  {icon:'🐟',name:'Peixe',trash:{icon:'🥫',name:'Lacre de lata',type:'metal'}},
  {icon:'🦈',name:'Piranha',trash:{icon:'🥾',name:'Bota velha',type:'rejeito'}},
  {icon:'🦆',name:'Pato',trash:{icon:'🛢️',name:'Óleo',type:'rejeito'}},
  {icon:'🦦',name:'Lontra',trash:{icon:'🧶',name:'Anel plástico',type:'plastico'}}
];
const BINS_CONFIG=[
  {type:'papel',color:'#1565c0',label:'Azul – Papel',e:'📄'},
  {type:'plastico',color:'#c62828',label:'Vermelho – Plástico',e:'♻️'},
  {type:'vidro',color:'#2e7d32',label:'Verde – Vidro',e:'🍶'},
  {type:'metal',color:'#f9a825',label:'Amarelo – Metal',e:'🥫'},
  {type:'organico',color:'#4e342e',label:'Marrom – Orgânico',e:'🌿'},
  {type:'rejeito',color:'#424242',label:'Cinza – Rejeito',e:'🚫'}
];
const LEVELS=[
  {pct:0, water:'#2d5a1b',trashDensity:6,fish:false},
  {pct:25,water:'#1a5c6e',trashDensity:3,fish:false},
  {pct:50,water:'#1565a0',trashDensity:1,fish:false},
  {pct:75,water:'#0d9ed4',trashDensity:0,fish:true}
];

const canvas=document.getElementById('gc');
const ctx=canvas.getContext('2d');
const lineSvg=document.getElementById('line-svg');
const river=document.getElementById('river');

// Canvas dimensions — always match the actual rendered size
let RW=0,RH=0;

function resizeCanvas(){
  RW=river.clientWidth;
  RH=river.clientHeight;
  canvas.width=RW;
  canvas.height=RH;
  lineSvg.setAttribute('viewBox',`0 0 ${RW} ${RH}`);
}

let score=0,timeLeft=180,gameRunning=false,casting=false,pendingTrash=null,cleanPct=0;
let msgTimeout=null,spots=[],animals=[],floatingFish=[],floatingTrash=[];
let lastTouchTime=0,lastTouchSpot=null,waveT=0;

function rand(a,b){return a+Math.random()*(b-a);}
function randInt(a,b){return Math.floor(rand(a,b+0.999));}
function el(id){return document.getElementById(id);}
function getLevelIdx(p){let i=0;for(let j=0;j<LEVELS.length;j++)if(p>=LEVELS[j].pct)i=j;return i;}

el('start-btn').onclick=startGame;

// Fisher position (relative — bottom-center)
function fisherPos(){return{x:RW/2,y:RH-30};}

function startGame(){
  resizeCanvas();
  el('overlay').classList.remove('show');
  gameRunning=true;score=0;timeLeft=180;cleanPct=0;
  spots=[];animals=[];floatingFish=[];floatingTrash=[];
  spawnSpots();spawnAnimals();updateHUD();
  startTimer();requestAnimationFrame(loop);
}

function startTimer(){
  const iv=setInterval(()=>{
    if(!gameRunning){clearInterval(iv);return;}
    timeLeft--;updateHUD();
    if(timeLeft<=0){clearInterval(iv);endGame();}
  },1000);
}

function updateHUD(){
  const m=Math.floor(timeLeft/60),s=timeLeft%60;
  el('timer-display').textContent=`${m}:${s.toString().padStart(2,'0')}`;
  el('score-display').textContent=Math.max(0,score);
  el('clean-bar').style.width=Math.min(100,cleanPct)+'%';
}

function spawnSpots(){
  const count=3+randInt(0,2);
  for(let i=0;i<count;i++)spots.push(newSpot());
}

function newSpot(){
  return{x:rand(60,RW-60),y:rand(45,RH-70),r:rand(22,32),pulse:Math.random()*Math.PI*2,active:true};
}

function spawnAnimals(){
  animals=[];
  const count=getLevelIdx(cleanPct)<2?2:1;
  for(let i=0;i<count;i++)addAnimal();
}

function addAnimal(){
  const a=ANIMALS[randInt(0,ANIMALS.length-1)];
  animals.push({...a,x:rand(50,RW-50),y:rand(40,RH-90),holdProgress:0,holding:false,holdInterval:null,freed:false});
}

function spawnFloatingTrash(){
  floatingTrash=[];
  const lvl=LEVELS[getLevelIdx(cleanPct)];
  const ems=['🛍️','🥤','🍶','🧴','📦','🥫'];
  for(let i=0;i<lvl.trashDensity;i++){
    floatingTrash.push({x:rand(20,RW-20),y:rand(20,RH-20),emoji:ems[randInt(0,5)],ox:rand(-15,15),oy:rand(-15,15),t:Math.random()*Math.PI*2});
  }
}

function spawnFloatingFish(){
  floatingFish=[];
  for(let i=0;i<5;i++)
    floatingFish.push({x:rand(30,RW-30),y:rand(30,RH-60),dir:Math.random()<0.5?1:-1,speed:rand(0.3,0.7),t:Math.random()*Math.PI*2});
}

function loop(){
  if(!gameRunning)return;
  waveT+=0.018;
  const li=getLevelIdx(cleanPct);
  drawRiver(li);drawSpots();drawFloatingTrash(li);drawAnimals();drawFloatingFish(li);
  requestAnimationFrame(loop);
}

function drawRiver(li){
  ctx.clearRect(0,0,RW,RH);
  ctx.fillStyle=LEVELS[li].water;
  ctx.fillRect(0,0,RW,RH);
  for(let i=0;i<4;i++){
    ctx.beginPath();ctx.strokeStyle=`rgba(255,255,255,${0.05-i*0.01})`;ctx.lineWidth=1.2;
    for(let x=0;x<=RW;x+=5){
      const y=60+i*40+Math.sin(x*0.012+waveT+i)*10+Math.sin(x*0.025+waveT*1.4+i)*5;
      x===0?ctx.moveTo(x,y):ctx.lineTo(x,y);
    }
    ctx.stroke();
  }
  if(li===0){
    ctx.fillStyle='rgba(50,80,10,0.18)';
    for(let i=0;i<8;i++){
      const bx=(i*97+waveT*8)%RW;
      ctx.beginPath();ctx.ellipse(bx,30+i*28,18+i%3*6,6,0,0,Math.PI*2);ctx.fill();
    }
  }
  if(li<=1){
    ctx.fillStyle=`rgba(80,60,20,${li===0?0.22:0.1})`;
    for(let i=0;i<5;i++){
      const sx=(i*137+waveT*12)%RW;
      ctx.beginPath();ctx.ellipse(sx,50+i*40,12+i%2*8,4,0,0,Math.PI*2);ctx.fill();
    }
  }
}

function drawSpots(){
  spots.forEach(s=>{
    if(!s.active)return;
    s.pulse+=0.04;
    const p=0.5+0.5*Math.sin(s.pulse);
    ctx.save();
    ctx.globalAlpha=0.55+p*0.2;ctx.fillStyle='rgba(0,0,0,0.6)';
    ctx.beginPath();ctx.ellipse(s.x,s.y,s.r+p*4,s.r*0.55+p*2,0,0,Math.PI*2);ctx.fill();
    ctx.globalAlpha=0.3+p*0.2;ctx.fillStyle='rgba(0,0,0,0.7)';
    ctx.beginPath();ctx.ellipse(s.x,s.y,s.r*0.6,s.r*0.3,0,0,Math.PI*2);ctx.fill();
    ctx.restore();
  });
}

function drawFloatingTrash(li){
  if(li>=3){floatingTrash=[];return;}
  if(floatingTrash.length===0)spawnFloatingTrash();
  floatingTrash.forEach(ft=>{
    ft.t+=0.02;ctx.save();ctx.font='18px serif';ctx.globalAlpha=0.55;
    ctx.fillText(ft.emoji,ft.x+Math.sin(ft.t)*ft.ox,ft.y+Math.cos(ft.t*0.7)*ft.oy);ctx.restore();
  });
}

function drawAnimals(){
  animals.forEach(a=>{
    if(a.freed)return;
    ctx.save();ctx.font='28px serif';ctx.fillText(a.icon,a.x-14,a.y+10);
    if(a.holding&&a.holdProgress>0){
      ctx.strokeStyle='rgba(76,175,80,0.9)';ctx.lineWidth=3;
      ctx.beginPath();ctx.arc(a.x,a.y,20,-Math.PI/2,-Math.PI/2+Math.PI*2*a.holdProgress);ctx.stroke();
    } else {
      ctx.strokeStyle='rgba(255,220,50,0.7)';ctx.lineWidth=1.5;ctx.setLineDash([3,3]);
      ctx.beginPath();ctx.arc(a.x,a.y,20,0,Math.PI*2);ctx.stroke();
    }
    ctx.restore();
  });
}

function drawFloatingFish(li){
  if(li<3){floatingFish=[];return;}
  if(floatingFish.length===0)spawnFloatingFish();
  floatingFish.forEach(ff=>{
    ff.t+=0.015;ff.x+=ff.speed*ff.dir;
    if(ff.x<-30)ff.x=RW+20;if(ff.x>RW+30)ff.x=-20;
    ctx.save();ctx.font='20px serif';ctx.globalAlpha=0.75;
    if(ff.dir<0){ctx.scale(-1,1);ctx.fillText('🐠',-ff.x-10,ff.y+Math.sin(ff.t)*8);}
    else ctx.fillText('🐠',ff.x-10,ff.y+Math.sin(ff.t)*8);
    ctx.restore();
  });
}

function getCanvasPos(e){
  const rect=canvas.getBoundingClientRect();
  let cx,cy;
  if(e.touches){cx=e.touches[0].clientX-rect.left;cy=e.touches[0].clientY-rect.top;}
  else{cx=e.clientX-rect.left;cy=e.clientY-rect.top;}
  return{cx,cy};
}

function findSpot(cx,cy){
  for(const s of spots){if(!s.active)continue;const dx=cx-s.x,dy=cy-s.y;if(Math.sqrt(dx*dx+dy*dy)<s.r+12)return s;}
  return null;
}

canvas.addEventListener('dblclick',e=>{
  if(!gameRunning||casting||pendingTrash)return;
  const{cx,cy}=getCanvasPos(e);
  const hit=findSpot(cx,cy);
  if(!hit)return;
  animateCast(hit.x,hit.y,()=>fishFromSpot(hit));
});

canvas.addEventListener('mousedown',e=>{
  if(!gameRunning||casting||pendingTrash)return;
  const{cx,cy}=getCanvasPos(e);
  startHold(cx,cy);
});
canvas.addEventListener('mouseup',releaseHold);
canvas.addEventListener('mouseleave',releaseHold);

canvas.addEventListener('touchstart',e=>{
  e.preventDefault();
  if(!gameRunning||pendingTrash)return;
  const{cx,cy}=getCanvasPos(e);
  const now=Date.now();
  if(now-lastTouchTime<380&&lastTouchSpot&&!casting){
    const s=lastTouchSpot;
    const dx=cx-s.x,dy=cy-s.y;
    if(Math.sqrt(dx*dx+dy*dy)<s.r+14){animateCast(s.x,s.y,()=>fishFromSpot(s));}
  } else {
    lastTouchSpot=findSpot(cx,cy);
    lastTouchTime=now;
    startHold(cx,cy);
  }
},{passive:false});
canvas.addEventListener('touchend',e=>{e.preventDefault();releaseHold();},{passive:false});

function startHold(cx,cy){
  for(const a of animals){
    if(a.freed)continue;
    const dx=cx-a.x,dy=cy-a.y;
    if(Math.sqrt(dx*dx+dy*dy)<28){
      a.holding=true;a.holdProgress=0;
      a.holdInterval=setInterval(()=>{
        a.holdProgress+=0.08;
        if(a.holdProgress>=1){clearInterval(a.holdInterval);freeAnimal(a);}
      },80);
      break;
    }
  }
}

function releaseHold(){
  animals.forEach(a=>{
    if(a.holding&&!a.freed){a.holding=false;a.holdProgress=0;clearInterval(a.holdInterval);}
  });
}

function freeAnimal(a){
  a.freed=true;a.holding=false;
  addScore(40);cleanPct=Math.min(100,cleanPct+4);
  showMsg(`+40 ❤️ ${a.name} salvo!`,'#81c784');
  spawnBonus('+40 ❤️','#81c784',a.x,a.y);
  pendingTrash=a.trash;
  setTimeout(()=>openDiscardPanel(a.trash),400);
  setTimeout(()=>{
    animals=animals.filter(an=>!an.freed);
    if(animals.length<2&&cleanPct<75)addAnimal();
  },700);
}

function animateCast(tx,ty,cb){
  casting=true;
  const fp=fisherPos();
  const fx=fp.x,fy=fp.y;
  let prog=0;
  lineSvg.innerHTML='';
  const path=document.createElementNS('http://www.w3.org/2000/svg','path');
  path.setAttribute('stroke','rgba(180,130,60,0.85)');
  path.setAttribute('stroke-width','1.5');
  path.setAttribute('fill','none');
  lineSvg.appendChild(path);
  const hook=document.createElementNS('http://www.w3.org/2000/svg','circle');
  hook.setAttribute('r','4');hook.setAttribute('fill','#c0c0c0');
  lineSvg.appendChild(hook);
  function step(){
    prog+=0.07;if(prog>1)prog=1;
    const cx=fx+(tx-fx)*prog,cy=fy+(ty-fy)*prog-60*Math.sin(prog*Math.PI);
    path.setAttribute('d',`M${fx},${fy} Q${(fx+tx)/2},${Math.min(fy,ty)-50} ${cx},${cy}`);
    hook.setAttribute('cx',cx);hook.setAttribute('cy',cy);
    if(prog<1)requestAnimationFrame(step);
    else setTimeout(()=>{lineSvg.innerHTML='';casting=false;cb();},350);
  }
  requestAnimationFrame(step);
}

function fishFromSpot(spot){
  spot.active=false;spots=spots.filter(s=>s.active);
  addScore(8);cleanPct=Math.min(100,cleanPct+1.5);
  const t=TRASH[randInt(0,TRASH.length-1)];
  pendingTrash=t;
  showMsg('+8 🎣 Pescou!','#80deea');
  spawnBonus('+8','#b2ebf2',spot.x,spot.y);
  openDiscardPanel(t);
  setTimeout(()=>{if(spots.length<2){spots.push(newSpot(),newSpot());}},1500);
}

function openDiscardPanel(t){
  el('dp-icon').textContent=t.icon;el('dp-name').textContent=t.name;
  const dbins=el('discard-bins');dbins.innerHTML='';
  BINS_CONFIG.forEach(b=>{
    const btn=document.createElement('button');
    btn.className='dbb';btn.style.background=b.color;
    btn.innerHTML=`${b.e}<span>${b.label}</span>`;
    btn.onclick=()=>discardItem(b.type);
    dbins.appendChild(btn);
  });
  el('discard-panel').classList.add('show');
}

function discardItem(chosen){
  if(!pendingTrash)return;
  const correct=pendingTrash.type===chosen;
  el('discard-panel').classList.remove('show');
  const binEl=document.querySelector(`.bin[data-type="${chosen}"]`);
  if(binEl){binEl.classList.add(correct?'correct':'wrong');setTimeout(()=>binEl.classList.remove('correct','wrong'),500);}
  if(correct){addScore(16);cleanPct=Math.min(100,cleanPct+2);showMsg('+16 ♻️ Correto!','#a5d6a7');spawnBonus('+16 ♻️','#a5d6a7',RW/2,RH/2);}
  else{addScore(-12);showMsg('-12 ❌ Errado!','#ef9a9a');spawnBonus('-12 ❌','#ef9a9a',RW/2,RH/2);}
  pendingTrash=null;updateHUD();
  const li=getLevelIdx(cleanPct);
  if(li===3&&floatingFish.length===0)spawnFloatingFish();
  if(li<3)floatingFish=[];
}

function tryDiscard(type){if(pendingTrash)discardItem(type);}

function addScore(pts){score+=pts;if(score<0)score=0;updateHUD();}

function showMsg(text,color){
  const m=el('msg');m.textContent=text;m.style.color=color;
  m.classList.add('show');clearTimeout(msgTimeout);
  msgTimeout=setTimeout(()=>m.classList.remove('show'),850);
}

function spawnBonus(text,color,x,y){
  const d=document.createElement('div');
  d.className='bonus-anim';d.textContent=text;d.style.color=color;
  d.style.left=Math.max(5,Math.min(85,x/RW*100))+'%';
  d.style.top=Math.max(10,Math.min(70,y/RH*100))+'%';
  river.appendChild(d);setTimeout(()=>d.remove(),1200);
}

function endGame(){
  gameRunning=false;
  const pct=Math.round(Math.min(100,cleanPct));
  let emoji,msg,color;
  if(pct>=75){emoji='🌊';msg='Rio cristalino! Você é um guardião das águas!';color='#4fc3f7';}
  else if(pct>=50){emoji='🌿';msg='Rio limpo! Grande trabalho!';color='#81c784';}
  else if(pct>=25){emoji='💧';msg='Boa tentativa! O rio agradece!';color='#fff176';}
  else{emoji='😢';msg='O rio ainda precisa de ajuda...';color='#ef9a9a';}
  el('overlay').innerHTML=`
    <h2>⏱️ Tempo esgotado!</h2>
    <div class="big-pct" style="color:${color}">${pct}%</div>
    <div style="font-size:clamp(10px,2.5vw,13px);color:rgba(255,255,255,0.6);margin-top:-4px">do rio limpo</div>
    <div style="font-size:clamp(24px,8vw,32px);margin:4px 0">${emoji}</div>
    <p style="max-width:280px">${msg}</p>
    <p style="color:#ffe066;font-size:clamp(15px,4.5vw,19px);font-weight:800;margin-top:4px">${score} pontos</p>
    <button onclick="location.reload()">🔄 Jogar de novo</button>
  `;
  el('overlay').classList.add('show');
}

// Handle resize — recompute canvas size and clamp game entities
window.addEventListener('resize',()=>{
  if(!gameRunning)return;
  resizeCanvas();
  // Clamp spots and animals to new bounds
  spots.forEach(s=>{s.x=Math.min(s.x,RW-40);s.y=Math.min(s.y,RH-60);});
  animals.forEach(a=>{a.x=Math.min(a.x,RW-40);a.y=Math.min(a.y,RH-80);});
  floatingTrash.forEach(ft=>{ft.x=Math.min(ft.x,RW-20);ft.y=Math.min(ft.y,RH-20);});
});

// Initial canvas size setup
resizeCanvas();
spawnSpots();
</script>
</body>
</html>
spawnSpots();
</script>
</body>
</html>
