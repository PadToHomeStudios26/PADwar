<!doctype html>
<html lang="de"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no"><title>PADWAR 3D — Birthday Build</title>
<style>
*{box-sizing:border-box}html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#121a14;color:#edf3e8;font-family:system-ui,Arial;touch-action:none}canvas{display:block;width:100%;height:100%}button,select{font:inherit}button{border:1px solid #657b5d;border-radius:9px;background:#293a2b;color:#fff;padding:10px 13px;font-weight:750}button.primary{background:#cce68c;color:#172016}.screen{position:fixed;inset:0;z-index:10;display:flex;align-items:center;justify-content:center;padding:14px;background:radial-gradient(ellipse at 50% 10%,#476044bb,#08100bf2 75%);overflow:auto}.hidden{display:none!important}.panel{width:min(900px,100%);max-height:96%;overflow:auto;background:#111b15f5;border:1px solid #5c7455;border-radius:17px;padding:clamp(15px,3vw,28px)}.logo{font-size:clamp(40px,8vw,72px);font-weight:1000;letter-spacing:.09em}.logo span{color:#cce68c}.muted,.small{color:#b9c6b4}.small{font-size:12px;line-height:1.5}.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:9px}.tile{padding:12px;border:1px solid #40583f;border-radius:10px;background:#1c2b20}.tile b{display:block;margin-bottom:5px}.row{display:flex;gap:9px;flex-wrap:wrap}.row>*{flex:1}.field{display:flex;flex-direction:column;gap:5px;min-width:130px}label{font-size:12px;color:#b9c6b4}select{padding:10px;background:#101812;color:white;border:1px solid #52694d;border-radius:8px}.title{margin:20px 0 9px;font-size:18px;font-weight:850}#hud{display:none;position:fixed;inset:0;z-index:3;pointer-events:none}.top{position:absolute;top:8px;left:8px;right:8px;display:flex;justify-content:space-between;gap:7px}.box,.hint{background:#101811df;border:1px solid #52664f;border-radius:9px;padding:8px 10px}.lab{font-size:10px;color:#b7c5b1}.val{font-weight:900;font-size:17px}.bar{height:6px;background:#37453a;border-radius:8px;overflow:hidden}.bar i{display:block;height:100%;background:#8ed17c}.bottom{position:absolute;bottom:10px;left:9px;right:9px;display:flex;justify-content:space-between;align-items:end;gap:8px}.hint{font-size:12px}.hudBtns{pointer-events:auto}.touch{display:none;position:fixed;inset:0;z-index:4;pointer-events:none}.touch.active{display:block}.stick{position:absolute;left:18px;bottom:25px;width:130px;height:130px;border:2px solid #b6c8b677;background:#111b1680;border-radius:50%;pointer-events:auto}.knob{position:absolute;left:40px;top:40px;width:46px;height:46px;border-radius:50%;background:#c4d6bd99}.actions{position:absolute;right:10px;bottom:18px;display:grid;grid-template-columns:repeat(2,65px);gap:7px;pointer-events:auto}.actions button{height:50px;padding:3px;background:#152219ed}.actions .fire{grid-column:span 2;background:#702e29}.notice{display:none;position:fixed;z-index:6;top:20%;left:50%;transform:translateX(-50%);padding:10px 15px;border-radius:10px;background:#101811ed;border:1px solid #cce68c;font-weight:800}.pcHint{max-width:60vw}@media(pointer:coarse){.pcHint{display:none}}
</style></head><body><canvas id="view"></canvas>
<div id="hud"><div class="top"><div class="box"><div class="lab">GESUNDHEIT</div><div class="val" id="hp">100 / 100</div><div class="bar"><i id="hpbar"></i></div><div class="lab">PANZERUNG <b id="armor">0</b></div></div><div class="box"><div class="lab">OPERATION</div><div class="val" id="mapname">NIGHTFALL</div><div class="lab" id="objective">Gegner: 24</div></div><div class="box"><div class="lab">MUNITION</div><div class="val" id="ammo">30 / 150</div><div class="lab" id="weapon">STURMGEWEHR</div></div></div><div class="bottom"><div class="hint" id="stats">Kills 0 · Token 0</div><div class="hint pcHint">WASD · Maus · Klick · R laden · E Fahrzeug · V Spezial</div><div class="hudBtns"><button id="pause">Ⅱ</button></div></div></div>
<div class="touch" id="touch"><div class="stick" id="stick"><div class="knob" id="knob"></div></div><div class="actions"><button class="fire" id="fire">FEUER HALTEN</button><button id="reload">LADEN</button><button id="special">SPEZIAL</button><button id="sprint">SPRINT</button><button id="vehicle">FAHRZEUG</button><button id="swap">WAFFE</button></div></div><div class="notice" id="notice"></div>
<div class="screen" id="menu"><div class="panel"><div class="logo">PAD<span>WAR</span></div><b style="letter-spacing:.17em;color:#cce68c">3D BATTLE — BIRTHDAY BUILD</b><p class="muted">Ein echter 3D-Browserkampf mit Ego-Perspektive, Gegnern, Team und steuerbaren Fahrzeugen.</p><div class="grid"><div class="tile"><b>🎯 3D-Kampf</b><span class="small">Ego-Perspektive, Projektilspuren, Treffer, Nachladen.</span></div><div class="tile"><b>🪖 Trupp & KI</b><span class="small">Verbündete folgen dir; Gegner bewegen sich, zielen und schießen.</span></div><div class="tile"><b>🚙 Fahrzeuge</b><span class="small">Jeep, Panzer, Boot und Helikopter als 3D-Modelle zum Steuern.</span></div><div class="tile"><b>🪙 Fortschritt</b><span class="small">Token, Shop, permanente Upgrades und lokaler Spielstand.</span></div></div><div class="title">Operation vorbereiten</div><div class="row"><div class="field"><label>SPIELMODUS</label><select id="mode"><option value="campaign">Gebiet sichern</option><option value="waves">Wellenüberleben</option><option value="capture">Kontrollpunkt</option></select></div><div class="field"><label>KARTE</label><select id="map"><option value="night">Nightfall Base</option><option value="desert">Dust Basin</option><option value="snow">Frostline</option><option value="coast">Coastal Strike</option><option value="jungle">Green Hell</option><option value="city">Iron District</option></select></div><div class="field"><label>GEGNER</label><select id="count"><option>12</option><option selected>24</option><option>36</option><option>50</option></select></div><div class="field"><label>SCHWIERIGKEIT</label><select id="diff"><option value=".8">Rekrut</option><option value="1" selected>Soldat</option><option value="1.3">Veteran</option><option value="1.6">Elite</option></select></div></div><div class="row" style="margin-top:14px"><button class="primary" id="start">▶ 3D-SCHLACHT STARTEN</button><button id="shop">🪙 WAFFENKAMMER</button><button id="help">STEUERUNG</button></div><p class="small">Hinweis: Three.js wird aus einem CDN geladen; zum Starten ist Internet nötig. Einzelspieler, kein Online-Multiplayer. Die 3D-Datei wurde erstellt, aber nicht auf deinem Gerät getestet.</p></div></div>
<div class="screen hidden" id="shopScreen"><div class="panel"><div class="logo" style="font-size:36px">WAFFEN<span>KAMMER</span></div><p class="muted">Token: <b id="tokens">0</b></p><div id="items"></div><div class="row"><button id="shopBack">Zurück</button><button id="reset">Spielstand löschen</button></div></div></div>
<div class="screen hidden" id="helpScreen"><div class="panel"><div class="logo" style="font-size:36px">STEUERUNG</div><div class="grid"><div class="tile"><b>WASD / Pfeile</b><span class="small">Bewegen</span></div><div class="tile"><b>Maus + Klick</b><span class="small">Zielen und schießen</span></div><div class="tile"><b>R · V · Q</b><span class="small">Laden · Spezial · Waffe</span></div><div class="tile"><b>E · Shift · Esc</b><span class="small">Fahrzeug · Sprint · Pause</span></div></div><p>Touch: linker Stick zum Laufen, rechte Bildschirmhälfte zum Umschauen, Aktionen rechts.</p><button class="primary" id="helpBack">Zurück</button></div></div>
<div class="screen hidden" id="pauseScreen"><div class="panel" style="max-width:500px"><div class="logo" style="font-size:44px">PAUSE</div><div class="row"><button class="primary" id="resume">Weiter</button><button id="pauseShop">Waffenkammer</button></div><div class="row"><button id="restart">Neu starten</button><button id="quit">Hauptmenü</button></div></div></div>
<div class="screen hidden" id="resultScreen"><div class="panel" style="max-width:550px;text-align:center"><div id="resultEmoji" style="font-size:48px">🏆</div><div class="logo" style="font-size:36px" id="resultTitle">MISSION ERFÜLLT</div><p id="resultReason" class="muted"></p><div class="grid"><div class="tile"><b>Kills</b><span id="resultKills">0</span></div><div class="tile"><b>Token</b><span id="resultEarned">0</span></div><div class="tile"><b>Zeit</b><span id="resultTime">0:00</span></div></div><div class="row"><button class="primary" id="again">Noch eine Schlacht</button><button id="resultMenu">Menü</button></div></div></div>
<div class="screen hidden" id="fatal"><div class="panel"><div class="logo">PAD<span>WAR</span></div><h2>3D konnte nicht gestartet werden</h2><p id="fatalText"></p><p class="small">Prüfe Internet, Safari/Chrome und WebGL. Die 3D-Engine wird vom CDN geladen.</p><button onclick="location.reload()">Erneut versuchen</button></div></div>
<script type="module">
try{
const THREE=await import('https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js'),$=id=>document.getElementById(id),canvas=$('view');
const renderer=new THREE.WebGLRenderer({canvas,antialias:true,powerPreference:'high-performance'});
renderer.setPixelRatio(Math.min(devicePixelRatio||1,1.5));
renderer.setSize(innerWidth,innerHeight);
renderer.shadowMap.enabled=true;
renderer.outputColorSpace=THREE.SRGBColorSpace;

const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(74,innerWidth/innerHeight,.1,400);
camera.rotation.order='YXZ';

const palettes={
night:['#3a4c3c','#52694b','#586359','#9ead9e'],
desert:['#a68a58','#c0a36a','#7e6c4e','#d1b88c'],
snow:['#aebfc0','#d0d9d5','#64787d','#b9cbd1'],
coast:['#687e67','#829a78','#687165','#a9c1c7'],
jungle:['#2f5639','#47734a','#4b5945','#a2b7a0'],
city:['#59615c','#69756a','#3e4842','#a6b0aa']
};

const weapons=[
{name:'Sturmgewehr',damage:26,rate:.16,mag:30,reload:1.2,speed:95,color:'#ffe2a1',spread:.018},
{name:'Maschinenpistole',damage:15,rate:.085,mag:36,reload:1.1,speed:85,color:'#9de7ff',spread:.05},
{name:'Präzisionsgewehr',damage:68,rate:.52,mag:8,reload:1.55,speed:135,color:'#ffc5a0',spread:.006},
{name:'Leichtes MG',damage:20,rate:.12,mag:60,reload:1.8,speed:100,color:'#ffd17b',spread:.035}
];

const vdefs={
jeep:{name:'Jeep',speed:11,hp:180,damage:24,rate:.24,kind:'land',color:'#a3a56b'},
tank:{name:'Panzer',speed:6.3,hp:480,damage:85,rate:.82,kind:'land',color:'#657f50'},
boat:{name:'Boot',speed:8.5,hp:290,damage:34,rate:.22,kind:'water',color:'#6e9da5'},
heli:{name:'Helikopter',speed:12.5,hp:250,damage:29,rate:.15,kind:'air',color:'#7d9171'}
};

const SAVE='padwar_3d_birthday_save';
let save;
try{
save={
tokens:0,kills:0,wins:0,
upgrades:{damage:0,mag:0,armor:0},
...JSON.parse(localStorage.getItem(SAVE)||'{}')
};
}catch{
save={tokens:0,kills:0,wins:0,upgrades:{damage:0,mag:0,armor:0}};
}
save.upgrades={damage:0,mag:0,armor:0,...save.upgrades};
const persist=()=>{
try{localStorage.setItem(SAVE,JSON.stringify(save))}catch{}
};

let state='menu',g=null,prev=performance.now(),uid=1,
keys={},mouseDown=false,
touch={x:0,z:0,fire:false,sprint:false},
yaw=0,pitch=0,noticeTime=0,aimId=null,aimLast=null;

const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
const rand=(a,b)=>a+Math.random()*(b-a);
const dist=(a,b)=>Math.hypot(a.x-b.x,a.z-b.z);

function material(c){
return new THREE.MeshStandardMaterial({color:c,roughness:.88});
}

function box(parent,pos,size,c){
let m=new THREE.Mesh(new THREE.BoxGeometry(...size),material(c));
m.position.set(...pos);
m.castShadow=true;
m.receiveShadow=true;
parent.add(m);
return m;
}

function notice(s){
$('notice').textContent=s;
$('notice').style.display='block';
noticeTime=1.8;
}

function soldier(team,heavy=false,x=0,z=0){
let m=new THREE.Group();
let color=team==='enemy'?'#c56e5d':'#5da9cc';

let body=new THREE.Mesh(
new THREE.CapsuleGeometry(heavy?.52:.4,heavy?.7:.85,3,7),
material(color)
);
body.position.y=1.05;
body.castShadow=true;
m.add(body);

let head=new THREE.Mesh(
new THREE.SphereGeometry(.28,8,7),
material('#b4b79a')
);
head.position.y=1.9;
m.add(head);

box(m,[.35,1.3,.25],[.15,.15,.9],'#252c25');

let bg=box(m,[0,2.35,0],[1,.07,.07],'#1b241b');
let bar=box(
m,[0,2.4,0],[.9,.055,.055],
team==='enemy'?'#dc7566':'#8bd4ef'
);

m.position.set(x,0,z);
scene.add(m);

return{
id:uid++,team,x,z,hp:heavy?110:70,
maxHp:heavy?110:70,mesh:m,bar,
kind:heavy?'heavy':'rifle',
cool:rand(.2,1),
speed:team==='enemy'?rand(2.8,4):4.1,
skill:rand(.55,.95),alive:true
};
}

function vehicle(type,x,z){
let d=vdefs[type],m=new THREE.Group();

if(type==='boat'){
box(m,[0,.65,0],[2.5,.8,4.4],'#2c4549');
box(m,[0,1.2,-.1],[1.3,.45,1.5],d.color);
}else{
box(
m,[0,.7,0],
type==='tank'?[3.4,.85,4.2]:
type==='heli'?[2.5,.8,3.5]:[2.2,.7,3],
d.color
);

if(type==='tank'){
box(m,[0,1.3,-.2],[2,.5,2],'#536b45');
box(m,[0,1.35,1.3],[.2,.18,2.1],'#30392e');
}

if(type==='jeep'){
box(m,[0,1.25,-.2],[1.5,.65,1.5],'#77865d');

for(let x of [-1.15,1.15])
for(let z of [-1.1,1.1]){
let w=new THREE.Mesh(
new THREE.CylinderGeometry(.35,.35,.22,10),
material('#202520')
);
w.rotation.z=Math.PI/2;
w.position.set(x,.4,z);
m.add(w);
}
}

if(type==='heli'){
box(m,[0,1.1,-1.5],[.22,.22,2.8],'#53664b');
box(m,[0,1.55,0],[4.8,.08,.16],'#202820');
box(m,[0,1,.5],[1.3,.65,1.4],'#76939a');
}
}

m.position.set(x,0,z);
scene.add(m);

return{
id:uid++,type,x,z,hp:d.hp,maxHp:d.hp,
mesh:m,used:false,cool:0
};
}

function buildWorld(){
scene.clear();

let p=palettes[g.mapId];
scene.background=new THREE.Color(p[3]);
scene.fog=new THREE.Fog(p[3],65,220);

scene.add(
new THREE.HemisphereLight(0xe1eadc,0x4b5846,2)
);

let sun=new THREE.DirectionalLight(0xffedcf,2.3);
sun.position.set(-50,80,30);
sun.castShadow=true;
sun.shadow.mapSize.set(1024,1024);
scene.add(sun);

let ground=new THREE.Mesh(
new THREE.PlaneGeometry(260,220),
material(p[0])
);
ground.rotation.x=-Math.PI/2;
ground.receiveShadow=true;
scene.add(ground);

let grid=new THREE.GridHelper(260,65,p[1],p[1]);
grid.position.y=.02;
grid.material.transparent=true;
grid.material.opacity=.18;
scene.add(grid);

for(let x of [-130,130])
box(scene,[x,2,0],[2,4,220],'#26342a');

for(let z of [-110,110])
box(scene,[0,2,z],[260,4,2],'#26342a');

g.obstacles=[];
for(let i=0;i<62;i++){
 let x=rand(-115,115),z=rand(-95,95);
 if(Math.hypot(x,z)<14)continue;
 if(['coast','jungle'].includes(g.mapId)&&x>62&&x<105)continue;
 let rock=Math.random()<.25,
 w=rock?rand(2,5):rand(4,9),
 h=rock?rand(1.5,4):rand(3,8),
 d=rock?rand(2,5):rand(4,9),mesh;

 if(g.mapId==='jungle'&&!rock&&Math.random()<.4){
  mesh=new THREE.Group();
  let tr=new THREE.Mesh(
   new THREE.CylinderGeometry(.3,.5,3,6),
   material('#554d34')
  );
  tr.position.y=1.5;
  mesh.add(tr);
  let crown=new THREE.Mesh(
   new THREE.ConeGeometry(2.4,5,7),
   material('#365b35')
  );
  crown.position.y=4;
  mesh.add(crown);
  mesh.position.set(x,0,z);
  scene.add(mesh);
 }else{
  mesh=box(scene,[x,h/2,z],[w,h,d],rock?p[2]:p[2]);
 }
 g.obstacles.push({x,z,w,d,mesh});
}

if(['coast','jungle'].includes(g.mapId)){
 let water=new THREE.Mesh(
  new THREE.PlaneGeometry(38,220),
  material('#28657d')
 );
 water.rotation.x=-Math.PI/2;
 water.position.set(83,.03,0);
 scene.add(water);

 for(let z=-80;z<90;z+=18){
  box(scene,[58,.15,z],[14,.3,3],'#796c53');
 }
}

for(let i=0;i<3;i++){
 box(scene,[-14+i*14,2,-24],[9,4,8],'#5e6e58');
 box(scene,[-14+i*14,4.2,-24],[3,.5,3],'#313d31');
}

g.vehicles=[];
for(let type of (
 ['coast','jungle'].includes(g.mapId)
 ?['jeep','tank','boat','heli']
 :['jeep','tank','heli']
)){
 g.vehicles.push(
  vehicle(
   type,
   type==='boat'?83:rand(-28,28),
   type==='boat'?rand(-55,55):rand(20,38)
  )
 );
}
}

function start(){
 g={
  mapId:$('map').value,
  mode:$('mode').value,
  diff:+$('diff').value,
  p:{
   x:0,z:0,hp:100,maxHp:100,
   armor:save.upgrades.armor*18,
   weapon:0,
   ammo:weapons[0].mag+save.upgrades.mag*8,
   reserve:180,reload:0,cool:0,special:5,
   vehicle:null,invuln:0
  },
  enemies:[],allies:[],bullets:[],vehicles:[],
  obstacles:[],kills:0,earned:0,elapsed:0,
  start:performance.now(),wave:1,capture:0,ended:false
 };

 buildWorld();

 for(let i=0;i<+$('count').value;i++){
  let x,z;
  do{
   x=rand(-110,110);
   z=rand(-95,95);
  }while(Math.hypot(x,z)<22);

  g.enemies.push(
   soldier('enemy',Math.random()<.18,x,z)
  );
 }

 for(let i=0;i<5;i++){
  g.allies.push(
   soldier('ally',false,rand(-8,8),rand(5,14))
  );
 }

 yaw=0;
 pitch=0;
 camera.position.set(0,1.7,0);
 camera.rotation.set(0,0,0);

 ['menu','shopScreen','helpScreen','pauseScreen','resultScreen']
  .forEach(id=>$(id).classList.add('hidden'));

 $('hud').style.display='block';
 $('touch').classList.add('active');
 state='game';

 if(save.special){
  g.p.special+=save.special;
  delete save.special;
  persist();
 }

 updateHud();
 notice('OPERATION GESTARTET');
}

function move(dx,dz,kind='land'){
 let p=g.p,
 nx=clamp(p.x+dx,-126,126),
 nz=clamp(p.z+dz,-106,106);

 if(kind==='land'&&['coast','jungle'].includes(g.mapId)
    &&nx>62&&nx<105)return;

 for(let o of g.obstacles){
  if(Math.abs(nx-o.x)<o.w/2+.8&&
     Math.abs(nz-o.z)<o.d/2+.8)return;
 }

 p.x=nx;
 p.z=nz;
}

function bullet(pos,dir,owner,damage,speed,color,life=1.1){
 let m=new THREE.Mesh(
  new THREE.SphereGeometry(.055,6,5),
  material(color)
 );
 m.position.copy(pos);
 scene.add(m);

 let line=new THREE.Line(
  new THREE.BufferGeometry().setFromPoints([
   pos.clone(),pos.clone()
  ]),
  new THREE.LineBasicMaterial({
   color,transparent:true,opacity:.8
  })
 );
 scene.add(line);

 g.bullets.push({
  mesh:m,line,dir:dir.clone().normalize(),
  owner,damage,speed,color,life
 });
}

function shoot(special=false){
 if(state!=='game')return;
 let p=g.p;

 if(special){
  if(p.special<=0){
   notice('Keine Spezialmunition');
   return;
  }
 }else{
  if(p.reload>0||p.cool>0)return;
  if(p.ammo<=0){
   reload();
   return;
  }
 }

 let v=p.vehicle?vdefs[p.vehicle.type]:weapons[p.weapon];

 let dir=new THREE.Vector3(
  Math.sin(yaw)*Math.cos(pitch),
  Math.sin(pitch),
  Math.cos(yaw)*Math.cos(pitch)
 ).normalize();

 dir.x+=rand(
  -special?.008:(v.spread||.02),
  special?.008:(v.spread||.02)
 );
 dir.y+=rand(-.01,.01);
 dir.normalize();

 bullet(
  camera.position.clone().add(dir.clone().multiplyScalar(.7)),
  dir,'player',
  special?125:v.damage*(1+save.upgrades.damage*.12),
  special?125:(v.speed||95),
  special?'#81f0ff':v.color||'#ffe2a1',
  1.2
 );

 if(special)p.special--;
 else{
  p.ammo--;
  p.cool=v.rate;
 }
}

function reload(){
 let p=g?.p;
 if(!p||p.reload>0||
    p.ammo>=weapons[p.weapon].mag+save.upgrades.mag*8)return;

 p.reload=weapons[p.weapon].reload;
 notice('NACHLADEN');
}

function enterVehicle(){
 let p=g?.p;
 if(!p)return;

 if(p.vehicle){
  p.vehicle.used=false;
  p.vehicle=null;
  camera.position.set(p.x,1.7,p.z);
  notice('Fahrzeug verlassen');
  return;
 }

 let v=g.vehicles
  .filter(v=>v.hp>0&&!v.used&&Math.hypot(v.x-p.x,v.z-p.z)<7)
  .sort((a,b)=>
   Math.hypot(a.x-p.x,a.z-p.z)-
   Math.hypot(b.x-p.x,b.z-p.z)
  )[0];

 if(!v){
  notice('Kein Fahrzeug in der Nähe');
  return;
 }

 v.used=true;
 p.vehicle=v;
 notice(vdefs[v.type].name+' betreten');
}