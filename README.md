from pathlib import Path

html = r'''<!doctype html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>PADWAR — Battle Edition</title>
<style>
:root{--lime:#b7d58b;--panel:#111811e8;--line:#718461}
*{box-sizing:border-box}html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#182019;color:#f2f4ec;font-family:system-ui,Arial,sans-serif;touch-action:none}
canvas{display:block;width:100%;height:100%;image-rendering:auto}
#hud{position:fixed;inset:0;pointer-events:none}
.panel{background:var(--panel);border:1px solid #71846188;border-radius:7px;padding:8px 10px;text-shadow:1px 2px #000}
#logo{position:absolute;top:10px;left:10px;font-weight:900;letter-spacing:2px;color:var(--lime);font-size:15px}
#objective{position:absolute;top:10px;left:50%;transform:translateX(-50%);text-align:center;font-size:12px;min-width:180px}
#topright{position:absolute;right:10px;top:10px;display:flex;gap:6px;pointer-events:auto}
#bottomleft{position:absolute;bottom:12px;left:12px;width:175px;font-size:12px}
.bar{height:8px;background:#3b4437;border-radius:6px;overflow:hidden;margin:5px 0}.bar>i{display:block;height:100%;background:#9ec86f;width:100%}
#bottomright{position:absolute;bottom:12px;right:12px;text-align:right;font-weight:800;font-size:20px}
#bottomright small{display:block;font-size:11px;color:#c3cbb8;font-weight:500}
#cross{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:26px;color:white;text-shadow:0 0 4px black}
#toast{position:absolute;top:23%;left:50%;transform:translateX(-50%);font-weight:900;text-align:center;text-shadow:2px 2px 4px black}
button,select{background:#293729;color:#f6f7f0;border:1px solid #82966d;border-radius:5px;padding:9px 11px;font:inherit}
button:active{background:#5e7548}button{cursor:pointer}
#menu,#shop,#end{position:fixed;inset:0;z-index:5;background:#090e0deF;display:flex;align-items:center;justify-content:center;padding:16px}
.card{width:min(560px,100%);max-height:94%;overflow:auto;border:1px solid var(--line);border-radius:12px;background:linear-gradient(145deg,#202b20,#0e140f);padding:22px;box-shadow:0 15px 55px #0009}
h1{font-size:clamp(27px,7vw,42px);line-height:1;margin:0;color:var(--lime);letter-spacing:2px}h2{margin:14px 0 8px;font-size:18px}
.sub{color:#b9c8ad;letter-spacing:3px;font-size:11px;margin:8px 0 17px}
.muted{color:#b6c0af;font-size:13px;line-height:1.5}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin:10px 0}.grid button{text-align:left;min-height:60px}.wide{width:100%;margin:5px 0}.primary{background:#526d3e;font-weight:800}.statrow{display:flex;justify-content:space-between;gap:10px;margin:6px 0;font-size:13px}
#mobile{display:none;position:fixed;inset:0;z-index:3;pointer-events:none}
#joy{position:absolute;left:18px;bottom:22px;width:138px;height:138px;border:2px solid #bec5b18c;background:#343a32aa;border-radius:50%;pointer-events:auto;touch-action:none}
#knob{position:absolute;left:38px;top:38px;width:58px;height:58px;border:2px solid #d5ddce;background:#899184b5;border-radius:50%}
.mbtn{position:absolute;pointer-events:auto;border-radius:50%;width:65px;height:65px;padding:4px;background:#202b20df;border:2px solid #c0c9b2a8;font-size:11px;font-weight:900;touch-action:none}
#fire{right:18px;bottom:52px;width:82px;height:82px;background:#733b31e8}#reload{right:25px;bottom:143px}#special{right:96px;bottom:22px}#sprint{right:105px;bottom:135px}#interact{right:25px;bottom:231px}
#shop{display:none;z-index:7}#shopItems{display:grid;grid-template-columns:1fr 1fr;gap:8px}.item{border:1px solid #53654a;border-radius:7px;padding:10px;background:#182219}.item b{display:block;color:var(--lime)}.item p{font-size:12px;color:#c2cbb9;min-height:34px}.item button{width:100%}
#end{display:none;z-index:8}
@media(pointer:coarse){#mobile{display:block}#objective{top:76px}#topright{top:10px}#bottomleft{bottom:8px}#bottomright{bottom:8px}}
</style>
</head>
<body>
<canvas id="game"></canvas>
<div id="hud">
 <div id="logo" class="panel">PADWAR <span style="color:#f4f5ee">/ BATTLE EDITION</span></div>
 <div id="objective" class="panel">OPERATION: <b id="mapname">NIGHTFALL BASE</b><br><span id="missiontext">Eliminiere die Gegner</span></div>
 <div id="topright"><button id="shopOpen">🪙 <span id="tokens">0</span> TOKEN · SHOP</button><button id="pause">Ⅱ</button></div>
 <div id="cross">+</div><div id="toast"></div>
 <div id="bottomleft" class="panel">GESUNDHEIT <b id="hpText">100</b><div class="bar"><i id="hpBar"></i></div>TEAM <b id="teamText">BLAU</b><br>KILLS <b id="kills">0</b> · SIEGE <b id="wins">0</b></div>
 <div id="bottomright" class="panel"><span id="weapon">M4 CARBINE</span><br><span id="ammo">30 / 120</span><small id="ammoKind">STANDARD</small></div>
</div>
<div id="mobile"><div id="joy"><div id="knob"></div></div><button id="fire" class="mbtn">FEUER</button><button id="reload" class="mbtn">NACHLADEN</button><button id="special" class="mbtn">VOID<br>AMMO</button><button id="sprint" class="mbtn">SPRINT</button><button id="interact" class="mbtn">FAHRZEUG</button></div>
<div id="menu"><div class="card">
 <h1>PADWAR</h1><div class="sub">BATTLE EDITION · FIELD TEST</div>
 <p class="muted">Ein eigenständiger, lokal spielbarer Schlachtfeld-Prototyp. Wähle eine Karte und starte ein Gefecht. Verdiene Token durch Kills und Siege und kaufe Skins oder Upgrades.</p>
 <h2>Karte auswählen</h2><div class="grid" id="mapChoices">
 <button data-map="0">🌲 NIGHTFALL BASE<br><small>Militärbasis & Wald</small></button><button data-map="1">🏜️ DUST BASIN<br><small>Wüste & Außenposten</small></button>
 <button data-map="2">❄️ FROSTLINE<br><small>Schneefeld & Bunker</small></button><button data-map="3">🌊 COASTAL STRIKE<br><small>Küste & Hafen</small></button></div>
 <div class="grid"><button id="modeStory">🎖️ STORY TEST</button><button id="modeFree">⚔️ FREIES GEFECHT</button></div>
 <div class="statrow"><label for="count">Gegneranzahl</label><select id="count"><option value="8">8 · klein</option><option value="14" selected>14 · mittel</option><option value="22">22 · groß</option><option value="32">32 · massiv</option></select></div>
 <button id="start" class="wide primary">GEFECHT STARTEN</button>
 <p class="muted">PC: WASD bewegen · Maus zielen · Linksklick schießen · R nachladen · V Spezialmunition · Shift sprinten · E Fahrzeug. Mobil: linker Stick, rechte Bildschirmseite zum Zielen, Buttons rechts.</p>
 <p class="muted">Hinweis: Dies ist ein 2D-Top-down-Prototyp mit simulierten Projektilen und Fahrzeugen, kein vollständiges 3D-Spiel. Alles in dieser Datei; kein CDN nötig.</p>
</div></div>
<div id="shop"><div class="card"><h1>PADWAR SHOP</h1><div class="sub">DEINE TOKEN: <span id="shopTokens">0</span></div><p class="muted">10 Token pro Kill · 50 Token pro Sieg. Käufe werden auf diesem Gerät gespeichert.</p><div id="shopItems"></div><button id="shopClose" class="wide primary">ZURÜCK INS GEFECHT</button></div></div>
<div id="end"><div class="card"><h1 id="endTitle">GEFECHT ENDE</h1><p id="endText" class="muted"></p><button id="again" class="wide primary">NOCHMAL SPIELEN</button><button id="menuBack" class="wide">HAUPTMENÜ</button></div></div>
<script>
(()=>{'use strict';
const $=id=>document.getElementById(id), canvas=$('game'),ctx=canvas.getContext('2d');
let W=innerWidth,H=innerHeight,dpr=Math.min(devicePixelRatio||1,1.5);
function resize(){W=innerWidth;H=innerHeight;canvas.width=W*dpr;canvas.height=H*dpr;canvas.style.width=W+'px';canvas.style.height=H+'px';ctx.setTransform(dpr,0,0,dpr,0,0)}addEventListener('resize',resize);resize();
const maps=[
{name:'NIGHTFALL BASE',ground:'#596b48',accent:'#8a8e73',water:false},
{name:'DUST BASIN',ground:'#aa8b5b',accent:'#c6ad7b',water:false},
{name:'FROSTLINE',ground:'#a6b5b5',accent:'#d9e1df',water:false},
{name:'COASTAL STRIKE',ground:'#647b68',accent:'#a7b7a0',water:true}
];
const items=[
{id:'woodland',name:'Waldläufer-Skin',desc:'Dunkle Tarnung für deinen Soldaten.',price:1500,type:'skin'},
{id:'arctic',name:'Arktis-Skin',desc:'Helles Schnee-Camouflage.',price:2200,type:'skin'},
{id:'red',name:'Crimson-Waffenlackierung',desc:'Rote Optik für dein Sturmgewehr.',price:1800,type:'skin'},
{id:'mag',name:'Großes Magazin',desc:'+15 Patronen pro Magazin.',price:2000,type:'upgrade'},
{id:'optic',name:'Präzisionsvisier',desc:'Engeres Zielbild beim Zielen.',price:2500,type:'upgrade'},
{id:'damage',name:'Verstärkte Munition',desc:'Etwas mehr Schaden pro Treffer.',price:3000,type:'upgrade'},
{id:'vehicle',name:'Fahrzeugpanzerung',desc:'Mehr Haltbarkeit für dein Fahrzeug.',price:3600,type:'upgrade'}
];
let saved={tokens:0,owned:[],wins:0,upgrades:{}};try{const s=JSON.parse(localStorage.getItem('padwar_save_v1')||'{}');saved={...saved,...s,upgrades:s.upgrades||{},owned:s.owned||[]}}catch{}
function save(){try{localStorage.setItem('padwar_save_v1',JSON.stringify(saved))}catch{}$('tokens').textContent=saved.tokens;$('wins').textContent=saved.wins;$('shopTokens').textContent=saved.tokens}
save();
let selectedMap=0,mode='free',active=false,paused=false,ending=false,player,enemies=[],allies=[],obstacles=[],vehicles=[],bullets=[],particles=[],pickups=[];
let kills=0,round=0,baseDifficulty=1,skill=0,fireHeld=false,reloadTime=0,specialAmmo=5,special=false,lastShot=0,toastTime=0,toastMsg='';
let keys={},joy={x:0,y:0},mouse={x:W/2,y:H/2,down:false},touchLook=null,aiming=false,driving=null,botSpawnTimer=0;
const rand=(a,b)=>a+Math.random()*(b-a),clamp=(v,a,b)=>Math.max(a,Math.min(b,v)),dist=(a,b)=>Math.hypot(a.x-b.x,a.y-b.y);
function say(s){toastMsg=s;toastTime=2}
function newPlayer(){return {x:0,y:0,a:-Math.PI/2,hp:100,maxhp:100,ammo:saved.upgrades.mag?45:30,reserve:150,r:13,speed:180,team:0,skin:saved.owned.includes('arctic')?'#d6ddd4':saved.owned.includes('woodland')?'#283e2c':'#4c6d49',vehicle:null}}
function generateWorld(){
 obstacles=[];vehicles=[];bullets=[];particles=[];pickups=[];enemies=[];allies=[];
 // Water strip on coast map
 if(selectedMap===3)obstacles.push({x:0,y:240,w:1200,h:220,type:'water'});
 for(let i=0;i<48;i++){
  let x=rand(-850,850),y=rand(-750,750);
  if(Math.hypot(x,y)<140){i--;continue}
  let type=Math.random()<.45?'tree':Math.random()<.7?'crate':'building';
  obstacles.push({x,y,w:type==='building'?rand(75,135):type==='tree'?26:rand(35,55),h:type==='building'?rand(65,115):type==='tree'?26:rand(30,50),type,hp:type==='crate'?50:999});
 }
 // POIs / structures
 for(let i=0;i<10;i++){let x=rand(-650,650),y=rand(-600,600);obstacles.push({x,y,w:rand(90,145),h:rand(70,120),type:'building',hp:999})}
 player=newPlayer();
 const count=+$('count').value;
 for(let i=0;i<count;i++)spawnBot('enemy',true);
 for(let i=0;i<Math.ceil(count/3);i++)spawnBot('ally',true);
 vehicles.push({x:-100,y:70,a:0,type:'jeep',hp:130,maxhp:130,r:25,color:'#6b7650',name:'JEEP'});
 vehicles.push({x:130,y:-110,a:Math.PI/2,type:'tank',hp:280,maxhp:280,r:34,color:'#424d3b',name:'TANK'});
 vehicles.push({x:220,y:180,a:0,type:'boat',hp:180,maxhp:180,r:30,color:'#526d78',name:'PATROL BOAT'});
 vehicles.push({x:-240,y:-180,a:0,type:'heli',hp:200,maxhp:200,r:31,color:'#586b50',name:'HELICOPTER'});
 round++;kills=0;skill=0;specialAmmo=5;special=false;reloadTime=0;driving=null;active=true;paused=false;ending=false;
 $('mapname').textContent=maps[selectedMap].name;$('missiontext').textContent=mode==='story'?'MISSION: SICHERE DEN SEKTOR':'FREIES GEFECHT · RUNDE '+round;
 updateHUD();say('GEFECHT BEGINNT — VIEL ERFOLG!');
}
function spawnBot(team,far=false){
 let a=rand(0,Math.PI*2),r=far?rand(300,700):rand(180,420),x=player.x+Math.cos(a)*r,y=player.y+Math.sin(a)*r;
 if(maps[selectedMap].water&&y>130&&Math.abs(x)<600)y-=400;
 const difficulty=1+skill*.055;
 const b={x,y,a:0,hp:team==='enemy'?100:100,maxhp:100,r:12,team,spd:(team==='enemy'?rand(42,66):rand(45,65))*Math.min(1.7,difficulty),cool:rand(.4,2),state:'advance',strafe:rand(-1,1),skill:clamp(.12+skill*.025,.12,.8),dead:false,skin:team==='enemy'?'#8a4938':'#477ba0',specialist:Math.random()<.15};
 // Avoid spawning in water / structure
 if(maps[selectedMap].water&&y>140)y=-rand(200,600);
 enemiesOrAllies(team).push(b);
}
function enemiesOrAllies(t){return t==='enemy'?enemies:allies}
function updateHUD(){
 $('hpText').textContent=Math.max(0,Math.ceil(player?.hp||0));$('hpBar').style.width=clamp(player?.hp||0,0,100)+'%';
 $('kills').textContent=kills;$('ammo').textContent=(player?.ammo||0)+' / '+(player?.reserve||0);
 $('ammoKind').textContent=special?'VOID AMMO · '+specialAmmo:'STANDARD'+(saved.upgrades.damage?' · POWER':'');
 $('weapon').textContent='M4 CARBINE'+(saved.upgrades.mag?' EXT':'')+(saved.upgrades.optic?' + OPTIC':'');
 save();
}
function worldToScreen(x,y){return {x:W/2+(x-player.x),y:H/2+(y-player.y)}}
function circle(x,y,r,color){ctx.fillStyle=color;ctx.beginPath();ctx.arc(x,y,r,0,Math.PI*2);ctx.fill()}
function drawWorld(){
 ctx.fillStyle=maps[selectedMap].ground;ctx.fillRect(0,0,W,H);
 // subtle grid
 ctx.strokeStyle='#ffffff0b';ctx.lineWidth=1;
 const spacing=80,ox=((W/2-player.x)%spacing+spacing)%spacing,oy=((H/2-player.y)%spacing+spacing)%spacing;
 for(let x=ox;x<W;x+=spacing){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke()}
 for(let y=oy;y<H;y+=spacing){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke()}
 if(selectedMap===3){const p=worldToScreen(0,240);ctx.fillStyle='#416f82';ctx.fillRect(p.x-600,p.y-110,1200,220);ctx.strokeStyle='#c5d6d8';ctx.lineWidth=6;ctx.strokeRect(p.x-600,p.y-110,1200,220)}
 // road
 const road=worldToScreen(0,0);ctx.fillStyle=maps[selectedMap].accent;ctx.fillRect(road.x-34,0,68,H);
 // draw obstacles
 for(const o of obstacles){if(o.type==='water')continue;const p=worldToScreen(o.x,o.y);if(p.x<-150||p.x>W+150||p.y<-150||p.y>H+150)continue;
 if(o.type==='tree'){circle(p.x+3,p.y+5,Math.max(o.w,o.h)*.56,'#0003');ctx.fillStyle='#574b35';ctx.fillRect(p.x-3,p.y,6,13);circle(p.x,p.y-4,o.w*.58,selectedMap===2?'#526d65':'#2e4a30');circle(p.x-5,p.y-8,o.w*.35,selectedMap===2?'#6f8880':'#3b5a37')}
 else {ctx.fillStyle='#0003';ctx.fillRect(p.x-o.w/2+5,p.y-o.h/2+7,o.w,o.h);ctx.fillStyle=o.type==='building'?'#777d70':'#6b6048';ctx.fillRect(p.x-o.w/2,p.y-o.h/2,o.w,o.h);ctx.fillStyle='#9ca18e';ctx.fillRect(p.x-o.w/2,p.y-o.h/2,o.w,7);ctx.fillStyle='#4c554a';ctx.fillRect(p.x-8,p.y-5,16,18)}
 }
 for(const v of vehicles)drawVehicle(v);
 for(const p of pickups){const s=worldToScreen(p.x,p.y);circle(s.x,s.y,8,'#e8d778');ctx.fillStyle='#283020';ctx.fillText('+',s.x-3,s.y+4)}
 for(const b of allies)if(!b.dead)drawBot(b);
 for(const b of enemies)if(!b.dead)drawBot(b);
 for(const b of bullets){const s=worldToScreen(b.x,b.y);const e=worldToScreen(b.x-b.vx*.035,b.y-b.vy*.035);ctx.strokeStyle=b.special?'#ba72ff':b.team===0?'#f5e6a4':'#ff9c74';ctx.lineWidth=b.special?4:2;ctx.beginPath();ctx.moveTo(s.x,s.y);ctx.lineTo(e.x,e.y);ctx.stroke()}
 for(const p of particles){const s=worldToScreen(p.x,p.y);ctx.globalAlpha=clamp(p.life,0,1);circle(s.x,s.y,p.r,p.color);ctx.globalAlpha=1}
 drawPlayer();
}
function drawVehicle(v){
 const s=worldToScreen(v.x,v.y);ctx.save();ctx.translate(s.x,s.y);ctx.rotate(v.a);
 ctx.fillStyle='#0004';ctx.fillRect(-v.r+4,-v.r*.55+5,v.r*2,v.r*1.1);
 ctx.fillStyle=v.color;
 if(v.type==='boat'){ctx.beginPath();ctx.moveTo(-v.r,-v.r*.65);ctx.lineTo(v.r,-v.r*.65);ctx.lineTo(v.r*.72,v.r*.6);ctx.lineTo(-v.r*.7,v.r*.6);ctx.closePath();ctx.fill()}
 else {ctx.fillRect(-v.r,-v.r*.62,v.r*2,v.r*1.24);if(v.type==='tank'){ctx.fillStyle='#30392b';ctx.fillRect(-v.r*.45,-v.r*.42,v.r*.9,v.r*.84);ctx.strokeStyle='#242b21';ctx.lineWidth=6;ctx.beginPath();ctx.moveTo(0,0);ctx.lineTo(v.r*1.1,0);ctx.stroke()}else if(v.type==='heli'){ctx.strokeStyle='#222a21';ctx.lineWidth=3;ctx.beginPath();ctx.moveTo(-v.r*1.4,0);ctx.lineTo(v.r*1.4,0);ctx.moveTo(0,-v.r*1.2);ctx.lineTo(0,v.r*1.2);ctx.stroke()}else{ctx.fillStyle='#333a2e';ctx.fillRect(-v.r*.45,-v.r*.4,v.r*.9,v.r*.8)}}
 ctx.restore();ctx.fillStyle='#0008';ctx.fillRect(s.x-v.r,s.y-v.r-9,v.r*2,4);ctx.fillStyle='#b9d581';ctx.fillRect(s.x-v.r,s.y-v.r-9,v.r*2*clamp(v.hp/v.maxhp,0,1),4);
}
function drawBot(b){
 const s=worldToScreen(b.x,b.y);ctx.save();ctx.translate(s.x,s.y);ctx.rotate(b.a);
 ctx.fillStyle='#0004';ctx.beginPath();ctx.ellipse(3,5,12,8,0,0,7);ctx.fill();
 ctx.fillStyle=b.skin;ctx.fillRect(-8,-9,16,19);circle(0,-11,6,b.team==='enemy'?'#caa78c':'#c4c9b7');
 ctx.strokeStyle='#222720';ctx.lineWidth=4;ctx.beginPath();ctx.moveTo(2,-1);ctx.lineTo(19,0);ctx.stroke();ctx.restore();
 if(b.team==='enemy'){ctx.fillStyle='#0008';ctx.fillRect(s.x-12,s.y-23,24,3);ctx.fillStyle='#d77965';ctx.fillRect(s.x-12,s.y-23,24*clamp(b.hp/b.maxhp,0,1),3)}
}
function drawPlayer(){
 if(driving){const v=driving,s=worldToScreen(v.x,v.y);ctx.save();ctx.translate(s.x,s.y);ctx.rotate(v.a);ctx.fillStyle='#d3e1a0';ctx.fillRect(-8,-7,16,14);ctx.restore();return}
 const s=worldToScreen(player.x,player.y);ctx.save();ctx.translate(s.x,s.y);ctx.rotate(player.a);
 ctx.fillStyle='#1118';ctx.beginPath();ctx.ellipse(2,4,14,10,0,0,7);ctx.fill();
 ctx.fillStyle=player.skin;ctx.fillRect(-9,-10,18,21);circle(0,-12,6,'#d0b397');
 ctx.strokeStyle='#222a20';ctx.lineWidth=5;ctx.beginPath();ctx.moveTo(2,-1);ctx.lineTo(22,0);ctx.stroke();
 // muzzle position and small barrel
 ctx.fillStyle='#151b14';ctx.fillRect(17,-2,10,4);ctx.restore();
}
function lineBlocked(x1,y1,x2,y2){
 for(const o of obstacles){if(o.type==='water')continue;const minX=o.x-o.w/2,maxX=o.x+o.w/2,minY=o.y-o.h/2,maxY=o.y+o.h/2;
 // segment sample
 for(let t=.1;t<1;t+=.12){const x=x1+(x2-x1)*t,y=y1+(y2-y1)*t;if(x>minX&&x<maxX&&y>minY&&y<maxY)return true}}
 return false;
}
function spawnBullet(x,y,a,team,specialShot=false,damage=25,speed=650){
 bullets.push({x,y,vx:Math.cos(a)*speed,vy:Math.sin(a)*speed,team,special:specialShot,damage,life:1.4,r:3});
}
function shoot(){
 if(!active||paused||ending||reloadTime>0)return;
 const now=performance.now();if(now-lastShot<145)return;lastShot=now;
 if(player.ammo<=0){say('MAGAZIN LEER — R ZUM NACHLADEN');return}
 player.ammo--;const muzzleDist=28, mx=player.x+Math.cos(player.a)*muzzleDist,my=player.y+Math.sin(player.a)*muzzleDist;
 // muzzle flash
 for(let i=0;i<5;i++)particles.push({x:mx,y:my,vx:rand(-35,35)+Math.cos(player.a)*rand(30,110),vy:rand(-35,35)+Math.sin(player.a)*rand(30,110),life:rand(.08,.2),r:rand(2,5),color:'#ffd77c'});
 const damage=saved.upgrades.damage?38:28;
 spawnBullet(mx,my,player.a,0,special,special?125:damage,special?560:680);
 if(special){specialAmmo--;if(specialAmmo<=0)special=false}
 // recoil
 player.a+=rand(-.025,.025);updateHUD();
}
function reload(){if(!active||reloadTime>0||player.ammo>=(saved.upgrades.mag?45:30)||player.reserve<=0)return;reloadTime=1.25;say('NACHLADEN …')}
function toggleSpecial(){if(special){special=false;say('STANDARDMUNITION')}else if(specialAmmo>0){special=true;say('VOID-MUNITION AKTIV')}else say('KEINE VOID-MUNITION MEHR');updateHUD()}
function addTokens(n,why){saved.tokens+=n;save();say('+'+n+' TOKEN · '+why)}
function damagePlayer(n){if(!active||ending)return;player.hp-=n;updateHUD();if(player.hp<=0)endGame(false)}
function endGame(win){if(ending)return;ending=true;active=false;fireHeld=false;if(win){saved.wins++;addTokens(50,'SIEG')}save();$('endTitle').textContent=win?'SEKTOR GESICHERT':'GEFECHT VERLOREN';$('endText').textContent=`Kills: ${kills}. ${win?'Siegbonus: 50 Token erhalten.':'Du wurdest ausgeschaltet. Versuche eine andere Taktik.'}`;$('end').style.display='flex'}
function distPointRect(x,y,o,r=0){return x>o.x-o.w/2-r&&x<o.x+o.w/2+r&&y>o.y-o.h/2-r&&y<o.y+o.h/2+r}
function canMove(x,y,r=12){if(Math.abs(x)>1000||Math.abs(y)>1000)return false;for(const o of obstacles)if(o.type!=='water'&&distPointRect(x,y,o,r))return false;if(maps[selectedMap].water&&y>130&&Math.abs(x)<600&&!driving)return false;return true}
function update(dt){
 if(!active||paused||ending)return;
 skill=Math.max(skill,kills/4+round*.2);baseDifficulty=1+skill*.05;
 if(reloadTime>0){reloadTime-=dt;if(reloadTime<=0){let cap=saved.upgrades.mag?45:30,n=Math.min(cap-player.ammo,player.reserve);player.ammo+=n;player.reserve-=n;updateHUD();say('NACHLADEN FERTIG')}}
 const f=(keys.KeyW||keys.ArrowUp?1:0)-(keys.KeyS||keys.ArrowDown?1:0)-joy.y;
 const side=(keys.KeyD||keys.ArrowRight?1:0)-(keys.KeyA||keys.ArrowLeft?1:0)+joy.x;
 const sprint=keys.ShiftLeft||keys.ShiftRight;
 if(driving){
  const v=driving;v.a=player.a;
  const spd=(v.type==='tank'?130:v.type==='boat'?150:v.type==='heli'?175:210)*(sprint?1.3:1);
  const nx=v.x+Math.cos(v.a)*f*spd*dt,ny=v.y+Math.sin(v.a)*f*spd*dt;
  if(canMove(nx,ny,v.r*.55)||v.type==='boat'){v.x=nx;v.y=ny}
  if(side){v.a+=side*dt*(v.type==='tank'?1.2:1.8);player.a=v.a}
  player.x=v.x;player.y=v.y;
 }else{
  const speed=player.speed*(sprint?1.55:1),nx=player.x+(Math.cos(player.a)*f-Math.sin(player.a)*side)*speed*dt,ny=player.y+(Math.sin(player.a)*f+Math.cos(player.a)*side)*speed*dt;
  if(canMove(nx,player.y,player.r))player.x=nx;if(canMove(player.x,ny,player.r))player.y=ny;
 }
 // AI bots: seek cover-ish, flank and fire with accuracy scaling as player kills rise
 for(const b of enemies.concat(allies)){
  if(b.dead)continue;
  const target=b.team===1?enemies.filter(e=>!e.dead).sort((a,c)=>dist(a,b)-dist(c,b))[0]:player;
  if(!target)continue;
  const d=dist(b,target),ang=Math.atan2(target.y-b.y,target.x-b.x);b.a=ang;
  b.cool-=dt;
  const approach=d>(b.team===1?130:210),strafe=Math.sin(performance.now()/700+b.x)*.35;
  if(approach||d<65){const dir=approach?1:-.4;const nx=b.x+Math.cos(ang)*b.spd*dir*dt-Math.sin(ang)*b.spd*strafe*dt,ny=b.y+Math.sin(ang)*b.spd*dir*dt+Math.cos(ang)*b.spd*strafe*dt;if(canMove(nx,b.y,10))b.x=nx;if(canMove(b.x,ny,10))b.y=ny}
  if(d<490&&b.cool<=0&&!lineBlocked(b.x,b.y,target.x,target.y)){
   b.cool=rand(.8,1.65)/Math.min(1.8,1+skill*.06);
   if(Math.random()<clamp(b.skill+skill*.018,.12,.78)){
    if(b.team===0){target.hp-=saved.upgrades.damage?18:13;if(target.hp<=0){target.dead=true;makeParticles(target.x,target.y,'#a5c1a0',12)}}
    else {const spread=rand(-.28,.28)*(1-clamp(skill*.025,0,.45));spawnBullet(b.x+Math.cos(ang)*18,b.y+Math.sin(ang)*18,ang+spread,1,false,rand(5,9)*(1+skill*.03),450)}
   }
  }
 }
 // bullets physically travel from weapon muzzle; obstacles stop them
 for(let i=bullets.length-1;i>=0;i--){
  const b=bullets[i],ox=b.x,oy=b.y;b.x+=b.vx*dt;b.y+=b.vy*dt;b.life-=dt;
  let hit=false;
  for(const o of obstacles){if(o.type!=='water'&&distPointRect(b.x,b.y,o,2)){if(o.type==='crate'){o.hp-=b.damage;if(o.hp<=0){makeParticles(o.x,o.y,'#9a8b68',14);obstacles.splice(obstacles.indexOf(o),1)}}else makeParticles(b.x,b.y,'#d6bd86',2);hit=true;break}}
  if(!hit&&b.team===0){
   for(const e of enemies){if(!e.dead&&Math.hypot(e.x-b.x,e.y-b.y)<e.r+4){e.hp-=b.damage;makeParticles(b.x,b.y,b.special?'#b678ff':'#e4c58c',4);hit=true;if(e.hp<=0){e.dead=true;kills++;addTokens(10,'KILL');if(b.special)makeParticles(e.x,e.y,'#ad7aff',28);else makeParticles(e.x,e.y,'#a75b46',12);if(Math.random()<.18)pickups.push({x:e.x,y:e.y});updateHUD()}break}}
  }else if(!hit&&b.team===1&&!driving&&Math.hypot(player.x-b.x,player.y-b.y)<player.r+3){damagePlayer(b.damage);hit=true}
  if(hit||b.life<=0||Math.abs(b.x)>1200||Math.abs(b.y)>1200)bullets.splice(i,1);
 }
 // enemy difficulty: reinforcements in free battle
 if(mode==='free'&&enemies.filter(e=>!e.dead).length<4&&botSpawnTimer<=0){for(let i=0;i<3;i++)spawnBot('enemy',true);botSpawnTimer=10+rand(0,5);say('FEINDLICHE VERSTÄRKUNG!')}botSpawnTimer-=dt;
 for(let i=particles.length-1;i>=0;i--){const p=particles[i];p.x+=p.vx*dt;p.y+=p.vy*dt;p.life-=dt;p.vx*=.97;p.vy*=.97;if(p.life<=0)particles.splice(i,1)}
 for(let i=pickups.length-1;i>=0;i--){if(dist(pickups[i],player)<24){player.hp=Math.min(100,player.hp+20);pickups.splice(i,1);say('+20 GESUNDHEIT');updateHUD()}}
 // win when story enemies eliminated, free battle stays alive until death
 if(mode==='story'&&enemies.every(e=>e.dead))endGame(true);
 if(toastTime>0){toastTime-=dt;$('toast').textContent=toastMsg;if(toastTime<=0)$('toast').textContent=''}
 updateHUD();
}
function makeParticles(x,y,color,n){for(let i=0;i<n;i++)particles.push({x,y,vx:rand(-120,120),vy:rand(-120,120),life:rand(.25,.8),r:rand(2,5),color})}
function drawMinimap(){
 const size=104,x=W-size-12,y=H-size-105;ctx.fillStyle='#0b100dcc';ctx.fillRect(x,y,size,size);ctx.strokeStyle='#9aa78c';ctx.strokeRect(x,y,size,size);
 const sx=size/500;
 for(const b of enemies)if(!b.dead){ctx.fillStyle='#e17a65';ctx.fillRect(x+size/2+(b.x-player.x)*sx-2,y+size/2+(b.y-player.y)*sx-2,4,4)}
 for(const b of allies)if(!b.dead){ctx.fillStyle='#79bde1';ctx.fillRect(x+size/2+(b.x-player.x)*sx-2,y+size/2+(b.y-player.y)*sx-2,4,4)}
 ctx.fillStyle='#d9ef9a';ctx.beginPath();ctx.arc(x+size/2,y+size/2,4,0,7);ctx.fill()
}
function frame(){requestAnimationFrame(frame);const now=performance.now();let dt=Math.min((now-(frame.last||now))/1000,.04);frame.last=now;if(active&&!paused&&!ending){if(fireHeld)shoot();update(dt)}drawWorld();drawMinimap();if(!active&&!ending&&$('menu').style.display==='none'){}}
requestAnimationFrame(frame);
// Aim with mouse; pointer coordinates converted to world direction
canvas.addEventListener('mousemove',e=>{mouse.x=e.clientX;mouse.y=e.clientY;if(active&&!paused&&!matchMedia('(pointer:coarse)').matches){const wx=player.x+(e.clientX-W/2),wy=player.y+(e.clientY-H/2);player.a=Math.atan2(wy-player.y,wx-player.x)}});
canvas.addEventListener('mousedown',e=>{if(e.button===0&&active&&!paused){fireHeld=true;if(!matchMedia('(pointer:coarse)').matches){const wx=player.x+(e.clientX-W/2),wy=player.y+(e.clientY-H/2);player.a=Math.atan2(wy-player.y,wx-player.x)}}});
addEventListener('mouseup',e=>{if(e.button===0)fireHeld=false});
canvas.addEventListener('touchstart',e=>{if(!active)return;for(const t of e.changedTouches)if(t.clientX>W*.4){touchLook={id:t.identifier,x:t.clientX,y:t.clientY}}},{passive:true});
canvas.addEventListener('touchmove',e=>{if(!touchLook)return;for(const t of e.changedTouches)if(t.identifier===touchLook.id){const dx=t.clientX-touchLook.x,dy=t.clientY-touchLook.y;if(Math.hypot(dx,dy)>1){player.a=Math.atan2(player.y+dy-player.y,player.x+dx-player.x); // overwritten below with stable incremental aim
 player.a+=dx*.006;touchLook.x=t.clientX;touchLook.y=t.clientY;}}},{passive:true});
canvas.addEventListener('touchend',e=>{for(const t of e.changedTouches)if(t.identifier===touchLook?.id)touchLook=null},{passive:true});
addEventListener('keydown',e=>{keys[e.code]=true;if(e.code==='KeyR')reload();if(e.code==='KeyV')toggleSpecial();if(e.code==='KeyE')interact();if(e.code==='Escape'&&active){paused=!paused;say(paused?'PAUSE':'WEITER')}});addEventListener('keyup',e=>keys[e.code]=false);
const joyEl=$('joy'),knob=$('knob');let joyId=null;
function joyMove(e){const r=joyEl.getBoundingClientRect(),cx=r.left+r.width/2,cy=r.top+r.height/2;let dx=e.clientX-cx,dy=e.clientY-cy,l=Math.hypot(dx,dy);if(l>42){dx=dx/l*42;dy=dy/l*42}knob.style.left=(40+dx)+'px';knob.style.top=(40+dy)+'px';joy.x=dx/42;joy.y=dy/42}
joyEl.addEventListener('pointerdown',e=>{joyId=e.pointerId;joyEl.setPointerCapture(joyId);joyMove(e)});joyEl.addEventListener('pointermove',e=>{if(e.pointerId===joyId)joyMove(e)});function joyReset(){joyId=null;joy.x=joy.y=0;knob.style.left='38px';knob.style.top='38px'}joyEl.addEventListener('pointerup',joyReset);joyEl.addEventListener('pointercancel',joyReset);
function hold(id,on,off){$(id).addEventListener('pointerdown',e=>{e.preventDefault();on()});$(id).addEventListener('pointerup',off);$(id).addEventListener('pointercancel',off);$(id).addEventListener('pointerleave',off)}
hold('fire',()=>fireHeld=true,()=>fireHeld=false);$('reload').addEventListener('pointerdown',e=>{e.preventDefault();reload()});$('special').addEventListener('pointerdown',e=>{e.preventDefault();toggleSpecial()});hold('sprint',()=>keys.ShiftLeft=true,()=>keys.ShiftLeft=false);$('interact').addEventListener('pointerdown',e=>{e.preventDefault();interact()});
function interact(){if(!active)return;if(driving){player.x=driving.x+45;player.y=driving.y+45;driving=null;say('FAHRZEUG VERLASSEN');return}let v=vehicles.filter(v=>dist(v,player)<70&&v.hp>0).sort((a,b)=>dist(a,player)-dist(b,player))[0];if(v){driving=v;player.x=v.x;player.y=v.y;player.a=v.a;say('EINGESTIEGEN: '+v.name)}else say('KEIN FAHRZEUG IN DER NÄHE')}
$('start').onclick=()=>{$('menu').style.display='none';$('end').style.display='none';generateWorld()};
document.querySelectorAll('[data-map]').forEach(b=>b.onclick=()=>{selectedMap=+b.dataset.map;document.querySelectorAll('[data-map]').forEach(x=>x.style.outline='');b.style.outline='2px solid #b7d58b';$('mapname').textContent=maps[selectedMap].name});
$('modeStory').onclick=()=>{mode='story';$('modeStory').style.outline='2px solid #b7d58b';$('modeFree').style.outline='';say('STORY TEST AUSGEWÄHLT')};
$('modeFree').onclick=()=>{mode='free';$('modeFree').style.outline='2px solid #b7d58b';$('modeStory').style.outline='';say('FREIES GEFECHT AUSGEWÄHLT')};
$('pause').onclick=()=>{if(active){paused=!paused;say(paused?'PAUSE':'WEITER')}else{$('menu').style.display='flex'}};
$('again').onclick=()=>{ $('end').style.display='none';generateWorld()};$('menuBack').onclick=()=>{$('end').style.display='none';$('menu').style.display='flex';active=false};
function renderShop(){
 $('shopTokens').textContent=saved.tokens;$('shopItems').innerHTML='';
 items.forEach(it=>{const owned=saved.owned.includes(it.id)||(it.type==='upgrade'&&saved.upgrades[it.id]);const div=document.createElement('div');div.className='item';div.innerHTML=`<b>${it.name}</b><p>${it.desc}</p><div class="statrow"><span>${owned?'BESITZT':'Preis'}</span><strong>${owned?'✓':it.price.toLocaleString('de-DE')+' T'}</strong></div><button ${owned||saved.tokens<it.price?'disabled':''}>${owned?'AUSGERÜSTET':saved.tokens<it.price?'ZU TEUER':'KAUFEN'}</button>`;div.querySelector('button').onclick=()=>{if(owned||saved.tokens<it.price)return;saved.tokens-=it.price;if(it.type==='skin')saved.owned.push(it.id);else saved.upgrades[it.id]=true;save();renderShop();say('GEKAUFT: '+it.name);if(it.id==='mag'&&player){player.ammo=45}updateHUD()};$('shopItems').appendChild(div)})
}
$('shopOpen').onclick=()=>{renderShop();$('shop').style.display='flex';paused=true};$('shopClose').onclick=()=>{$('shop').style.display='none';if(active)paused=false};
})();
</script>
</body></html>'''
path = Path("/mnt/data/PADWAR_BATTLE_EDITION.html")
path.write_text(html, encoding="utf-8")
print(f"Erstellt: {path} ({path.stat().st_size:,} Bytes)")
display(HTML(f'<div style="padding:12px;border:1px solid #718461;border-radius:10px;font-family:Arial"><b>PADWAR — Battle Edition</b><p>HTML-Datei erstellt · {path.stat().st_size:,} Bytes</p><a href="sandbox:/mnt/data/PADWAR_BATTLE_EDITION.html" download>PADWAR_BATTLE_EDITION.html herunterladen</a></div>'))
