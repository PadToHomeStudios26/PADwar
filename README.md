
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<title>BLACKLIST: OPERATIVE V1</title>
<style>
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#101510;color:white;font-family:Arial,sans-serif;touch-action:none}
canvas{display:block}
#hud{position:fixed;inset:0;pointer-events:none;z-index:2}
.panel{background:#101610c9;border:1px solid #91a17b70;padding:9px 12px;border-radius:5px;text-shadow:1px 1px #000}
#brand{position:absolute;top:12px;left:12px;font-weight:900;letter-spacing:2px;font-size:16px}
#mission{position:absolute;top:56px;left:12px;font-size:12px}
#stats{position:absolute;bottom:14px;left:14px;min-width:165px}
#healthbar{height:8px;background:#392c28;margin-top:6px;border-radius:5px;overflow:hidden}
#healthfill{height:100%;width:100%;background:#8cb75b}
#ammo{position:absolute;bottom:17px;right:15px;text-align:right;font-size:20px;font-weight:bold}
#ammo small{font-size:11px;color:#c5d0bd}
#objective{position:absolute;top:14px;left:50%;transform:translateX(-50%);text-align:center;font-size:12px;min-width:190px}
#objective b{color:#f0d58a}
#crosshair{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:25px;font-weight:300;color:#fff}
#hitmark{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:36px;color:#f2d98b;opacity:0}
#toast{position:absolute;top:25%;left:50%;transform:translateX(-50%);font-weight:bold;text-align:center;text-shadow:2px 2px 4px #000}
#menu{position:fixed;inset:0;z-index:10;display:flex;align-items:center;justify-content:center;background:linear-gradient(#090e0dcc,#090e0df2);padding:18px}
#menuCard{width:min(440px,100%);max-height:92%;overflow:auto;background:#151c16;border:1px solid #7d9467;padding:22px;border-radius:8px;text-align:center}
h1{margin:0;color:#d6e2c6;font-size:27px;letter-spacing:2px}
.sub{color:#9ba991;font-size:12px;letter-spacing:2px;margin:8px 0 20px}
button,select{font:inherit;color:white;background:#2b3829;border:1px solid #82936e;border-radius:4px;padding:11px;cursor:pointer}
button:active{background:#60754b}
.mainbtn{width:100%;font-weight:bold;background:#607747;margin:6px 0}
.menurow{display:flex;gap:8px;margin:8px 0}
.menurow>*{flex:1;min-width:0}
#controls{font-size:12px;line-height:1.65;color:#b6c1af;text-align:left;margin-top:14px}
#mobile{display:none;position:fixed;inset:0;z-index:4;pointer-events:none}
#stickBase{position:absolute;bottom:30px;left:22px;width:140px;height:140px;border-radius:50%;background:#292e2dcc;border:2px solid #9299918c;pointer-events:auto;touch-action:none}
#stick{position:absolute;width:60px;height:60px;border-radius:50%;left:38px;top:38px;background:#777e77b8;border:2px solid #b4bbb1}
.mbtn{position:absolute;pointer-events:auto;border-radius:50%;width:66px;height:66px;padding:4px;background:#252d26d9;border:2px solid #a0ad94b0;font-size:12px;font-weight:bold;touch-action:none}
#fire{right:22px;bottom:65px;width:82px;height:82px;background:#743a32dd}
#reload{right:28px;bottom:160px}
#special{right:100px;bottom:25px}
#jump{right:110px;bottom:145px}
#aim{right:28px;bottom:245px}
#attach{position:fixed;z-index:5;right:10px;top:100px;pointer-events:auto;display:none;font-size:11px}
#attach button{display:block;margin:5px 0;padding:8px;background:#182118e8}
#result{display:none;position:fixed;inset:0;z-index:9;background:#080b09e8;align-items:center;justify-content:center;text-align:center;padding:25px}
@media(pointer:coarse){#mobile{display:block}#attach{display:block}#mission{top:64px}#brand{font-size:14px}#objective{top:12px}}
</style>
</head>
<body>
<div id="hud">
  <div id="brand" class="panel">BLACKLIST <span style="color:#9bb77d">/ OPERATIVE</span></div>
  <div id="mission" class="panel">OPERATION NIGHTFALL<br><span style="color:#bdc9b4">Sektor 01 · Militärzone</span></div>
  <div id="objective" class="panel">AUFTRAG: <b>ELIMINIERE DIE FEINDE</b><br><span id="enemyCount">Gegner: 12</span></div>
  <div id="crosshair">+</div><div id="hitmark">×</div>
  <div id="toast"></div>
  <div id="stats" class="panel">GESUNDHEIT <b id="healthText">100</b><div id="healthbar"><div id="healthfill"></div></div><div style="margin-top:7px;font-size:11px;color:#b9c6ae">KILLS: <b id="kills">0</b> · <span id="modeLabel">STORY TEST</span></div></div>
  <div id="ammo" class="panel"><span id="weaponName">M4</span><br><span id="ammoText">30 / 120</span><br><small id="ammoType">STANDARDMUNITION</small></div>
</div>

<div id="mobile">
  <div id="stickBase"><div id="stick"></div></div>
  <button id="fire" class="mbtn">FEUER</button>
  <button id="reload" class="mbtn">NACHLADEN</button>
  <button id="special" class="mbtn">VOID<br>AMMO</button>
  <button id="jump" class="mbtn">SPRINT</button>
  <button id="aim" class="mbtn">VISIER</button>
</div>

<div id="attach">
  <button id="suppress">SCHALLDÄMPFER: AUS</button>
  <button id="mag">MAGAZIN: 30</button>
  <button id="sight">VISIER: AUS</button>
</div>

<div id="menu">
 <div id="menuCard">
  <h1>BLACKLIST</h1>
  <div class="sub">OPERATIVE · FIELD PROTOTYPE</div>
  <p style="color:#c9d2c0">OPERATION NIGHTFALL</p>
  <p style="font-size:13px;color:#aab6a2">Kämpfe auf einem offenen Militärgelände gegen KI-Gegner. Rüste deine Waffe aus und teste Spezialmunition.</p>
  <div class="menurow">
    <select id="difficulty" aria-label="Schwierigkeit">
      <option value="normal">Schwierigkeit: Normal</option>
      <option value="easy">Schwierigkeit: Einfach</option>
      <option value="hard">Schwierigkeit: Schwer</option>
    </select>
  </div>
  <div class="menurow">
    <select id="battleMode" aria-label="Spielmodus">
      <option value="story">Story-Testmission</option>
      <option value="free">Freies Gefecht</option>
    </select>
  </div>
  <button class="mainbtn" id="start">MISSION STARTEN</button>
  <div id="controls">
   <b>PC:</b> WASD bewegen · Maus zielen · Linksklick schießen · R nachladen · Shift sprinten · V Spezialmunition · Esc Maus freigeben.<br>
   <b>Handy/iPad:</b> Linker Stick bewegen · rechte Bildschirmseite ziehen zum Zielen · FEUER, NACHLADEN und VOID AMMO benutzen.
  </div>
 </div>
</div>

<div id="result"><div><h1 id="resultTitle">MISSION</h1><p id="resultText"></p><button class="mainbtn" id="again">NOCHMAL SPIELEN</button><button class="mainbtn" id="back">ZUM MENÜ</button></div></div>

<script type="module">
import * as THREE from 'https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js';

const $=id=>document.getElementById(id);
const scene=new THREE.Scene();
scene.background=new THREE.Color(0x91a69a);
scene.fog=new THREE.Fog(0x91a69a,65,230);

const camera=new THREE.PerspectiveCamera(76,innerWidth/innerHeight,.1,450);
camera.rotation.order='YXZ';
const renderer=new THREE.WebGLRenderer({antialias:false,powerPreference:'high-performance'});
renderer.setSize(innerWidth,innerHeight);
renderer.setPixelRatio(Math.min(devicePixelRatio,1.5));
renderer.shadowMap.enabled=false;
renderer.outputColorSpace=THREE.SRGBColorSpace;
document.body.appendChild(renderer.domElement);

scene.add(new THREE.HemisphereLight(0xdce7dc,0x343c2b,2.0));
const sun=new THREE.DirectionalLight(0xffe5c3,2.2);
sun.position.set(-35,65,30);scene.add(sun);

const mat=(color,roughness=1)=>new THREE.MeshStandardMaterial({color,roughness});
const grass=mat(0x667653), dirt=mat(0x625c46), concrete=mat(0x777c75), dark=mat(0x333a35);
const wood=mat(0x6b573b), metal=mat(0x46534b), leaf=mat(0x354c32), sandbag=mat(0x95896a);
const ground=new THREE.Mesh(new THREE.PlaneGeometry(420,420),grass);
ground.rotation.x=-Math.PI/2;ground.position.y=-.12;scene.add(ground);

const solids=[];
function box(x,y,z,w,h,d,material,solid=true){
 const mesh=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),material);
 mesh.position.set(x,y,z);scene.add(mesh);
 if(solid)solids.push({x,z,w,d,top:y+h/2,mesh});
 return mesh;
}
function tree(x,z,s=1){
 const trunk=new THREE.Mesh(new THREE.CylinderGeometry(.22*s,.34*s,2*s,5),wood);
 trunk.position.set(x,s,z);scene.add(trunk);
 const crown=new THREE.Mesh(new THREE.ConeGeometry(1.45*s,3.3*s,6),leaf);
 crown.position.set(x,3*s,z);scene.add(crown);
}
function building(x,z,w,d,h){
 box(x,h/2,z,w,h,d,concrete);
 box(x,h+.12,z,w+.5,.25,d+.5,dark,false);
 for(let i=-1;i<=1;i++){
  box(x+i*w*.24,h*.62,z-d/2-.03,.8,.9,.08,dark,false);
 }
 // Türöffnung als dunkle Türfläche
 box(x,h*.28,z+d/2+.045,1.5,h*.55,.09,dark,false);
}
function road(x,z,w,d){
 const m=new THREE.Mesh(new THREE.PlaneGeometry(w,d),dirt);
 m.rotation.x=-Math.PI/2;m.position.set(x,-.095,z);scene.add(m);
}
road(0,0,14,250);road(0,-40,180,9);road(0,42,160,8);
for(let i=0;i<20;i++){
 const x=(Math.random()-.5)*160,z=(Math.random()-.5)*180;
 if(Math.abs(x)<13)continue;
 if(Math.random()<.55)tree(x,z,.75+Math.random()*.65);
 else box(x,.7,z,2+Math.random()*3,1.4,2+Math.random()*3,wood);
}
building(-22,-22,18,15,8);
building(24,-47,18,14,7);
building(-42,34,15,12,6);
building(42,30,16,13,8);
building(0,-85,26,18,10);
for(let i=0;i<8;i++){
 box(-8+i*2,0.65,-12,1.6,1.3,1.2,sandbag);
 box(17+i*2,0.65,20,1.6,1.3,1.2,sandbag);
}
for(let i=0;i<5;i++)box(-10+i*5,1,-65,2,2,2,metal);
for(let i=0;i<7;i++)box(-65+i*22,.25,-100,7,.5,1.2,dark);

// Stylisierte Deko-Landezone
const helipad=new THREE.Mesh(new THREE.CircleGeometry(8,32),mat(0x4d574e));
helipad.rotation.x=-Math.PI/2;helipad.position.set(48,-.07,-75);scene.add(helipad);
const hMark=new THREE.Mesh(new THREE.PlaneGeometry(4,4),mat(0xe0dfc6));
hMark.rotation.x=-Math.PI/2;hMark.position.set(48,-.05,-75);scene.add(hMark);

// Spielzustand
const player={x:0,y:1.7,z:15,yaw:0,pitch:0,health:100,ammo:30,reserve:120,kills:0,sprinting:false};
let running=false,ended=false,mouseLocked=false,fireHeld=false,reloading=false;
let special=false,suppressor=false,extended=false,optic=false,aiming=false;
let lastShot=0,lastEnemyShot=0,reloadTimer=0,toastTimer=0;
let enemies=[],effects=[],keys={};
let moveX=0,moveY=0,lookPointer=null,lastTouchX=0,lastTouchY=0;
let targetCount=12;

const playerGroup=new THREE.Group();
const weapon=new THREE.Group();
const gunMat=mat(0x303832),gunDark=mat(0x161b18),gunMetal=mat(0x6b756c);
function gunBox(x,y,z,w,h,d,m){
 const o=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),m);o.position.set(x,y,z);weapon.add(o);return o;
}
gunBox(0,0,0,.16,.18,.58,gunMat);
gunBox(0,-.11,.02,.12,.15,.26,gunDark);
gunBox(0,.015,-.38,.075,.075,.72,gunMetal);
gunBox(0,-.07,-.2,.10,.09,.28,gunDark);
gunBox(0,.13,-.02,.13,.055,.22,gunMat);
const sightMesh=gunBox(0,.16,-.02,.08,.06,.1,gunMetal);
const silencer=gunBox(0,.015,-.84,.10,.10,.32,gunDark);
weapon.position.set(.34,-.29,-.68);camera.add(weapon);scene.add(camera);
scene.add(playerGroup);

function say(s){
 $('toast').textContent=s;toastTimer=1.8;
}
function makeEnemy(x,z){
 const g=new THREE.Group();
 const uniform=mat(0x4b5546),skin=mat(0xb49a7c),vest=mat(0x30372e);
 const body=new THREE.Mesh(new THREE.BoxGeometry(.7,1,.38),uniform);body.position.y=1.05;g.add(body);
 const armor=new THREE.Mesh(new THREE.BoxGeometry(.74,.48,.42),vest);armor.position.y=1.2;g.add(armor);
 const head=new THREE.Mesh(new THREE.BoxGeometry(.35,.35,.35),skin);head.position.y=1.75;g.add(head);
 const helmet=new THREE.Mesh(new THREE.BoxGeometry(.42,.16,.42),dark);helmet.position.y=1.94;g.add(helmet);
 for(const side of [-1,1]){
  const leg=new THREE.Mesh(new THREE.BoxGeometry(.23,.7,.25),uniform);leg.position.set(side*.19,.35,0);g.add(leg);
  const arm=new THREE.Mesh(new THREE.BoxGeometry(.2,.72,.22),uniform);arm.position.set(side*.43,1.12,-.02);g.add(arm);
 }
 const rifle=new THREE.Mesh(new THREE.BoxGeometry(.12,.12,.75),gunDark);rifle.position.set(.12,1.15,-.42);g.add(rifle);
 g.position.set(x,0,z);scene.add(g);
 return {g,hp:100,x,z,shot:Math.random()*2,alive:true,walk:Math.random()*6};
}
function spawnEnemies(){
 enemies.forEach(e=>scene.remove(e.g));enemies=[];
 const count=$('battleMode').value==='free'?20:12;
 targetCount=count;
 for(let i=0;i<count;i++){
  let x,z;
  do{x=(Math.random()-.5)*110;z=-20-Math.random()*100;}while(Math.abs(x)<8&&z> -60);
  enemies.push(makeEnemy(x,z));
 }
 updateHud();
}
function updateHud(){
 $('healthText').textContent=Math.max(0,Math.round(player.health));
 $('healthfill').style.width=Math.max(0,player.health)+'%';
 $('ammoText').textContent=player.ammo+' / '+player.reserve;
 $('ammoType').textContent=special?'VOID-MUNITION · '+(player.specialAmmo||5):'STANDARDMUNITION';
 $('enemyCount').textContent='Gegner: '+enemies.filter(e=>e.alive).length;
 $('kills').textContent=player.kills;
 $('weaponName').textContent='M4'+(suppressor?' SUP':'')+(optic?' OPT':'');
}
function startGame(){
 $('menu').style.display='none';$('result').style.display='none';
 player.x=0;player.y=1.7;player.z=15;player.yaw=0;player.pitch=0;
 player.health=100;player.ammo=extended?45:30;player.reserve=150;player.kills=0;player.specialAmmo=5;
 special=false;reloading=false;ended=false;running=true;fireHeld=false;
 camera.position.set(player.x,player.y,player.z);
 spawnEnemies();updateHud();
 if(matchMedia('(pointer:fine)').matches)renderer.domElement.requestPointerLock?.();
 say('MISSION START: FEINDE ELIMINIEREN');
}
function finish(win){
 if(ended)return;ended=true;running=false;fireHeld=false;
 $('result').style.display='flex';
 $('resultTitle').textContent=win?'MISSION ABGESCHLOSSEN':'MISSION FEHLGESCHLAGEN';
 $('resultText').textContent=win?'Sektor gesichert. Eliminierungen: '+player.kills:'Du wurdest ausgeschaltet. Eliminierungen: '+player.kills;
 document.exitPointerLock?.();
}
function reload(){
 if(!running||reloading||player.ammo>=(extended?45:30)||player.reserve<=0)return;
 reloading=true;reloadTimer=1.25;say('NACHLADEN …');
}
function toggleSpecial(){
 if(!running)return;
 special=!special;
 if(special&&player.specialAmmo<=0){special=false;say('VOID-MUNITION AUFGEBRAUCHT');}
 else say(special?'VOID-MUNITION AKTIV':'STANDARDMUNITION');
 updateHud();
}
function makeEffect(pos,color,size=.4,count=8){
 for(let i=0;i<count;i++){
  const m=new THREE.Mesh(new THREE.BoxGeometry(size,size,size),mat(color));
  m.position.copy(pos);
  const vel=new THREE.Vector3((Math.random()-.5)*5,Math.random()*5,(Math.random()-.5)*5);
  scene.add(m);effects.push({m,vel,t:.5+Math.random()*.8});
 }
}
function shoot(){
 if(!running||reloading)return;
 const now=performance.now();
 if(now-lastShot<105)return;
 lastShot=now;
 if(player.ammo<=0){say('MAGAZIN LEER · R ZUM NACHLADEN');return;}
 player.ammo--;
 weapon.position.z=-.62;
 setTimeout(()=>{weapon.position.z=-.68},55);
 const origin=camera.position.clone();
 const dir=new THREE.Vector3(0,0,-1).applyQuaternion(camera.quaternion).normalize();
 const ray=new THREE.Raycaster(origin,dir,0,120);
 const candidates=enemies.filter(e=>e.alive).map(e=>e.g);
 const hits=ray.intersectObjects(candidates,true);
 if(hits.length){
  let obj=hits[0].object;
  let enemy=enemies.find(e=>e.alive&&e.g.getObjectById(obj.id));
  if(!enemy){
   for(let o=obj;o;o=o.parent){enemy=enemies.find(e=>e.alive&&e.g===o);if(enemy)break;}
  }
  if(enemy){
   enemy.hp-=special?200:34;
   $('hitmark').style.opacity=1;setTimeout(()=>$('hitmark').style.opacity=0,120);
   if(enemy.hp<=0){
    enemy.alive=false;player.kills++;
    if(special){
     makeEffect(enemy.g.position.clone().add(new THREE.Vector3(0,1,0)),0x7d62ff,.18,24);
     scene.remove(enemy.g);
    }else{
     enemy.g.rotation.x=Math.PI/2;enemy.g.position.y=-.1;
     makeEffect(enemy.g.position.clone().add(new THREE.Vector3(0,1,0)),0x4b3429,.08,5);
     // Körper bleibt als sichtbarer Gefallener liegen
    }
    say(special?'VOID-ELIMINIERUNG':'FEIND ELIMINIERT');
   }
  }
 }
 if(special){player.specialAmmo--;if(player.specialAmmo<=0)special=false;}
 updateHud();
}
function hitPlayer(dmg){
 if(!running)return;
 player.health-=dmg;updateHud();
 if(player.health<=0)finish(false);
}
function blocked(x,z){
 // Begrenzung der Karte
 if(Math.abs(x)>195||Math.abs(z)>195)return true;
 for(const s of solids){
  if(Math.abs(x-s.x)<s.w/2+.45&&Math.abs(z-s.z)<s.d/2+.45)return true;
 }
 return false;
}
function movePlayer(dt){
 let forward=0,side=0;
 if(keys.KeyW||keys.ArrowUp)forward++;
 if(keys.KeyS||keys.ArrowDown)forward--;
 if(keys.KeyD||keys.ArrowRight)side++;
 if(keys.KeyA||keys.ArrowLeft)side--;
 forward+=-moveY;side+=moveX;
 const length=Math.hypot(forward,side);
 if(length>1){forward/=length;side/=length;}
 const sprint=keys.ShiftLeft||keys.ShiftRight;
 const speed=sprint?12:7;
 const dx=(-Math.sin(player.yaw)*forward+Math.cos(player.yaw)*side)*speed*dt;
 const dz=(-Math.cos(player.yaw)*forward-Math.sin(player.yaw)*side)*speed*dt;
 if(!blocked(player.x+dx,player.z))player.x+=dx;
 if(!blocked(player.x,player.z+dz))player.z+=dz;
 camera.position.set(player.x,player.y,player.z);
 camera.rotation.set(player.pitch,player.yaw,0,'YXZ');
 weapon.position.x=aiming?.08:.34;
 weapon.position.y=aiming?-.23:-.29;
 camera.fov=aiming?48:76;camera.updateProjectionMatrix();
}
function updateEnemies(dt){
 const now=performance.now()/1000;
 for(const e of enemies){
  if(!e.alive)continue;
  const dx=player.x-e.x,dz=player.z-e.z,dist=Math.hypot(dx,dz);
  if(dist>4){
   const speed=dist>35?1.9:1.15;
   const nx=e.x+dx/dist*speed*dt,nz=e.z+dz/dist*speed*dt;
   let blockedEnemy=false;
   for(const s of solids)if(Math.abs(nx-s.x)<s.w/2+.5&&Math.abs(nz-s.z)<s.d/2+.5){blockedEnemy=true;break;}
   if(!blockedEnemy){e.x=nx;e.z=nz;e.g.position.x=nx;e.g.position.z=nz;}
  }
  e.g.rotation.y=Math.atan2(-dx,-dz);
  e.walk+=dt;
  if(dist<36&&now-e.shot>( $('difficulty').value==='hard'?.8:$('difficulty').value==='easy'?2.2:1.4)){
   e.shot=now;
   if(Math.random()<.65){
    const accuracy=dist<12?.32:.12;
    if(Math.random()<accuracy)hitPlayer($('difficulty').value==='hard'?13:8);
   }
  }
 }
 if(enemies.every(e=>!e.alive))finish(true);
}
function update(dt){
 if(!running)return;
 if(reloading){
  reloadTimer-=dt;
  if(reloadTimer<=0){
   const cap=extended?45:30,needed=cap-player.ammo,take=Math.min(needed,player.reserve);
   player.ammo+=take;player.reserve-=take;reloading=false;updateHud();say('NACHLADEN ABGESCHLOSSEN');
  }
 }
 if(fireHeld)shoot();
 movePlayer(dt);updateEnemies(dt);
 for(let i=effects.length-1;i>=0;i--){
  const e=effects[i];e.t-=dt;
  e.m.position.addScaledVector(e.vel,dt);e.vel.y-=8*dt;
  e.m.scale.multiplyScalar(.985);
  if(e.t<=0){scene.remove(e.m);e.m.geometry.dispose();effects.splice(i,1);}
 }
 if(toastTimer>0){toastTimer-=dt;if(toastTimer<=0)$('toast').textContent='';}
}
let last=performance.now();
function loop(now){
 requestAnimationFrame(loop);
 const dt=Math.min((now-last)/1000,.04);last=now;
 update(dt);renderer.render(scene,camera);
}
requestAnimationFrame(loop);

// PC-Steuerung
addEventListener('keydown',e=>{
 keys[e.code]=true;
 if(e.code==='KeyR')reload();
 if(e.code==='KeyV')toggleSpecial();
 if(e.code==='KeyF'&&ended)startGame();
});
addEventListener('keyup',e=>keys[e.code]=false);
renderer.domElement.addEventListener('click',()=>{
 if(running&&matchMedia('(pointer:fine)').matches&&!mouseLocked)renderer.domElement.requestPointerLock?.();
});
document.addEventListener('pointerlockchange',()=>{mouseLocked=document.pointerLockElement===renderer.domElement;});
document.addEventListener('mousemove',e=>{
 if(!running||!mouseLocked)return;
 player.yaw-=e.movementX*.0022;
 player.pitch-=e.movementY*.0022;
 player.pitch=Math.max(-1.35,Math.min(1.35,player.pitch));
});
renderer.domElement.addEventListener('mousedown',e=>{if(e.button===0&&running&&mouseLocked)fireHeld=true;});
addEventListener('mouseup',e=>{if(e.button===0)fireHeld=false;});

// Mobile-look: ziehe auf der rechten Bildschirmseite
renderer.domElement.addEventListener('touchstart',e=>{
 if(!running)return;
 for(const t of e.changedTouches){
  if(t.clientX>innerWidth*.37&&!e.target.closest?.('button')){
   lookPointer=t.identifier;lastTouchX=t.clientX;lastTouchY=t.clientY;
  }
 }
},{passive:true});
renderer.domElement.addEventListener('touchmove',e=>{
 if(lookPointer===null||!running)return;
 for(const t of e.changedTouches)if(t.identifier===lookPointer){
  const dx=t.clientX-lastTouchX,dy=t.clientY-lastTouchY;
  player.yaw-=dx*.006;player.pitch-=dy*.006;
  player.pitch=Math.max(-1.35,Math.min(1.35,player.pitch));
  lastTouchX=t.clientX;lastTouchY=t.clientY;
 }
},{passive:true});
renderer.domElement.addEventListener('touchend',e=>{
 for(const t of e.changedTouches)if(t.identifier===lookPointer)lookPointer=null;
},{passive:true});

// Virtueller Joystick
const base=$('stickBase'),stick=$('stick');
let joyId=null;
function joyUpdate(e){
 const r=base.getBoundingClientRect(),cx=r.left+r.width/2,cy=r.top+r.height/2;
 let dx=e.clientX-cx,dy=e.clientY-cy;
 const max=40,len=Math.hypot(dx,dy);
 if(len>max){dx=dx/len*max;dy=dy/len*max;}
 stick.style.left=(40+dx)+'px';stick.style.top=(40+dy)+'px';
 moveX=dx/max;moveY=dy/max;
}
base.addEventListener('pointerdown',e=>{joyId=e.pointerId;base.setPointerCapture(joyId);joyUpdate(e);});
base.addEventListener('pointermove',e=>{if(e.pointerId===joyId)joyUpdate(e);});
function resetJoy(){joyId=null;moveX=moveY=0;stick.style.left='38px';stick.style.top='38px';}
base.addEventListener('pointerup',resetJoy);base.addEventListener('pointercancel',resetJoy);

// Buttons
function holdButton(id,down,up=()=>{}){
 const el=$(id);
 el.addEventListener('pointerdown',e=>{e.preventDefault();down();});
 el.addEventListener('pointerup',e=>{e.preventDefault();up();});
 el.addEventListener('pointercancel',up);
 el.addEventListener('pointerleave',up);
}
holdButton('fire',()=>fireHeld=true,()=>fireHeld=false);
$('reload').addEventListener('pointerdown',e=>{e.preventDefault();reload();});
$('special').addEventListener('pointerdown',e=>{e.preventDefault();toggleSpecial();});
$('jump').addEventListener('pointerdown',e=>{e.preventDefault();keys.ShiftLeft=true;});
$('jump').addEventListener('pointerup',()=>keys.ShiftLeft=false);
$('jump').addEventListener('pointercancel',()=>keys.ShiftLeft=false);
$('aim').addEventListener('pointerdown',e=>{e.preventDefault();aiming=!aiming;$('aim').textContent=aiming?'VISIER AN':'VISIER';});
$('start').addEventListener('click',startGame);
$('again').addEventListener('click',startGame);
$('back').addEventListener('click',()=>{
 $('result').style.display='none';$('menu').style.display='flex';running=false;
});
$('suppress').addEventListener('click',()=>{
 suppressor=!suppressor;$('suppress').textContent='SCHALLDÄMPFER: '+(suppressor?'AN':'AUS');updateHud();
});
$('mag').addEventListener('click',()=>{
 extended=!extended;$('mag').textContent='MAGAZIN: '+(extended?'45':'30');
 player.ammo=extended?45:30;updateHud();
});
$('sight').addEventListener('click',()=>{
 optic=!optic;sightMesh.visible=optic;
 $('sight').textContent='VISIER: '+(optic?'AN':'AUS');updateHud();
});
addEventListener('resize',()=>{
 camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();
 renderer.setSize(innerWidth,innerHeight);
});
</script>
</body>
</html>
