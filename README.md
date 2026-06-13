[SKALA_reactor_terminal.html](https://github.com/user-attachments/files/28909511/SKALA_reactor_terminal.html)
# ComputianGoverment.github.io
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');
  *{box-sizing:border-box;margin:0;padding:0}
  .skala{background:#050f05;color:#33ff33;font-family:'Share Tech Mono',monospace;font-size:13px;min-height:600px;padding:12px;line-height:1.5}
  .crt{position:relative}
  .crt::before{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,0.08) 2px,rgba(0,0,0,0.08) 4px);pointer-events:none;z-index:10}
  .header{border:1px solid #33ff33;padding:6px 10px;margin-bottom:8px;text-align:center;letter-spacing:2px;font-size:14px;color:#88ffaa}
  .subheader{font-size:11px;color:#55aa55;text-align:center;margin-bottom:10px;letter-spacing:1px}
  .panels{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:8px}
  .panel{border:1px solid #1a661a;padding:6px 8px}
  .panel-title{color:#88ffaa;font-size:11px;letter-spacing:1px;border-bottom:1px solid #1a661a;padding-bottom:3px;margin-bottom:6px}
  .readout{display:flex;justify-content:space-between;margin:2px 0}
  .readout-label{color:#55aa55;font-size:12px}
  .readout-val{color:#33ff33;font-size:12px}
  .readout-val.warn{color:#ffcc00}
  .readout-val.crit{color:#ff3333;animation:blink 0.7s step-end infinite}
  .bar-row{margin:3px 0}
  .bar-label{color:#55aa55;font-size:11px;margin-bottom:1px}
  .bar-track{background:#0a280a;border:1px solid #1a661a;height:10px;position:relative}
  .bar-fill{height:100%;background:#33ff33;transition:width 1.2s ease}
  .bar-fill.warn{background:#ffcc00}
  .bar-fill.crit{background:#ff3333}
  .terminal{border:1px solid #1a661a;padding:6px 8px;margin-bottom:8px}
  .term-out{height:130px;overflow-y:auto;margin-bottom:6px;font-size:12px;color:#33ff33}
  .term-out .line{margin:1px 0}
  .term-out .sys{color:#55aa55}
  .term-out .err{color:#ff3333}
  .term-out .ok{color:#88ffaa}
  .term-row{display:flex;align-items:center;gap:6px}
  .term-row span{color:#88ffaa;font-size:12px;white-space:nowrap}
  .term-row input{background:transparent;border:none;border-bottom:1px solid #33ff33;color:#33ff33;font-family:'Share Tech Mono',monospace;font-size:12px;flex:1;outline:none;padding:2px 4px}
  .btn-row{display:grid;grid-template-columns:repeat(4,1fr);gap:4px;margin-bottom:8px}
  .btn{background:transparent;border:1px solid #33ff33;color:#33ff33;font-family:'Share Tech Mono',monospace;font-size:11px;padding:5px 4px;cursor:pointer;letter-spacing:0.5px;text-align:center;transition:background 0.15s}
  .btn:hover{background:#0a280a;color:#88ffaa}
  .btn:active{background:#1a661a}
  .btn.active{background:#1a661a;color:#88ffaa}
  .status-row{display:flex;justify-content:space-between;border-top:1px solid #1a661a;padding-top:6px;font-size:11px}
  .status-item{color:#55aa55}
  .status-item span{color:#33ff33}
  @keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
  .alarm{color:#ff3333;animation:blink 0.7s step-end infinite;font-weight:bold}
  .cursor{animation:blink 0.8s step-end infinite}
  ::-webkit-scrollbar{width:4px}
  ::-webkit-scrollbar-track{background:#050f05}
  ::-webkit-scrollbar-thumb{background:#1a661a}
</style>

<div class="skala crt">
<h2 class="sr-only" style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0,0,0,0)">SKALA reactor control terminal simulation — Chernobyl RBMK nuclear power plant computer interface</h2>

<div class="header">СКАЛА — СИСТЕМА КОНТРОЛЯ АППАРАТА ЛЕНИНГРАДСКОЙ АТОМНОЙ</div>
<div class="subheader">ЧЕРНОБЫЛЬСКАЯ АЭС · ЭНЕРГОБЛОК 4 · V-3M PROCESSOR FRAME 2 · PROGRAM: KRV/DREG/PRIZMA</div>

<div class="panels">
  <div class="panel">
    <div class="panel-title">РЕАКТОР / REACTOR CORE STATUS</div>
    <div class="readout"><span class="readout-label">THERMAL POWER (MW):</span><span class="readout-val" id="pwr">3200</span></div>
    <div class="readout"><span class="readout-label">NEUTRON FLUX (n/cm²s):</span><span class="readout-val" id="nflux">3.14E+13</span></div>
    <div class="readout"><span class="readout-label">REACTIVITY (βeff):</span><span class="readout-val" id="react">0.0047</span></div>
    <div class="readout"><span class="readout-label">CONTROL RODS IN CORE:</span><span class="readout-val" id="rods">207</span></div>
    <div class="readout"><span class="readout-label">VOID COEFF (Δk/k):</span><span class="readout-val warn" id="void">+0.0012</span></div>
    <div class="readout"><span class="readout-label">ORM (OPER.REACTIV.MARGIN):</span><span class="readout-val warn" id="orm">15 rods</span></div>
    <div style="margin-top:6px">
      <div class="bar-row"><div class="bar-label">POWER LEVEL:</div><div class="bar-track"><div class="bar-fill" id="pbar" style="width:80%"></div></div></div>
      <div class="bar-row"><div class="bar-label">ROD INSERTION:</div><div class="bar-track"><div class="bar-fill warn" id="rbar" style="width:55%"></div></div></div>
    </div>
  </div>
  <div class="panel">
    <div class="panel-title">ТЕПЛОНОСИТЕЛЬ / COOLANT SYSTEMS (DREG)</div>
    <div class="readout"><span class="readout-label">COOLANT FLOW (t/h):</span><span class="readout-val" id="flow">48000</span></div>
    <div class="readout"><span class="readout-label">FEED TEMP (°C):</span><span class="readout-val" id="tin">270</span></div>
    <div class="readout"><span class="readout-label">OUTLET TEMP (°C):</span><span class="readout-val" id="tout">284</span></div>
    <div class="readout"><span class="readout-label">STEAM PRESSURE (MPa):</span><span class="readout-val" id="spress">6.28</span></div>
    <div class="readout"><span class="readout-label">DRUM LEVEL (mm):</span><span class="readout-val" id="drum">+15</span></div>
    <div class="readout"><span class="readout-label">FEEDWATER PUMPS:</span><span class="readout-val ok" id="pumps">8/8 OK</span></div>
    <div style="margin-top:6px">
      <div class="bar-row"><div class="bar-label">FLOW RATE:</div><div class="bar-track"><div class="bar-fill" id="fbar" style="width:88%"></div></div></div>
      <div class="bar-row"><div class="bar-label">STEAM PRESSURE:</div><div class="bar-track"><div class="bar-fill" id="spbar" style="width:72%"></div></div></div>
    </div>
  </div>
</div>

<div class="btn-row">
  <button class="btn active" id="b-krv" onclick="switchProg('KRV')">KRV</button>
  <button class="btn" id="b-dreg" onclick="switchProg('DREG')">DREG</button>
  <button class="btn" id="b-prizma" onclick="switchProg('PRIZMA')">PRIZMA</button>
  <button class="btn" id="b-az5" onclick="pressAZ5()">⚠ AZ-5</button>
</div>

<div class="terminal">
  <div class="term-out" id="tout-div"></div>
  <div class="term-row">
    <span>SKALA&gt;</span>
    <input type="text" id="cmdinput" placeholder="enter command code (type HELP)" autocomplete="off" onkeydown="handleCmd(event)"/>
  </div>
</div>

<div class="status-row">
  <div class="status-item">PROC: <span id="prog-ind">KRV</span></div>
  <div class="status-item">TIME: <span id="clock">01:23:42</span></div>
  <div class="status-item">TAPE: <span id="tape-st">PRIZMA-RDY</span></div>
  <div class="status-item">ALARMS: <span id="alarm-ct">0</span></div>
  <div class="status-item">FRAME: <span style="color:#33ff33">V-3M/2 OK</span></div>
</div>
</div>

<script>
const tout = document.getElementById('tout-div');
let alarmCount = 0;
let az5pressed = false;
let currentProg = 'KRV';
let tickInterval;

function log(msg, cls=''){
  const d = document.createElement('div');
  d.className = 'line' + (cls?' '+cls:'');
  d.textContent = msg;
  tout.appendChild(d);
  tout.scrollTop = tout.scrollHeight;
}

const helpText = [
  ['HELP','Show this command list'],
  ['STATUS','Full system status report'],
  ['RODS [N]','Set control rod insertion (0-211)'],
  ['POWER [%]','Set target reactor power level'],
  ['FLOW [N]','Adjust coolant flow (t/h)'],
  ['KRV','Load KRV (core map) program'],
  ['DREG','Load DREG (coolant) program'],
  ['PRIZMA','Load PRIZMA (parameter) program'],
  ['AZ-5','Emergency reactor shutdown'],
  ['TAPE','Read PRIZMA magnetic tape data'],
  ['RESET','Clear alarm state'],
  ['CLEAR','Clear terminal output'],
];

const commands = {
  HELP(){ log('--- SKALA COMMAND REFERENCE ---','sys'); helpText.forEach(([c,d])=>log(`  ${c.padEnd(14)} ${d}`,'sys')); },
  STATUS(){
    log('--- SYSTEM STATUS ---','ok');
    log(`  PROGRAM:     ${currentProg}`);
    log(`  POWER:       ${document.getElementById('pwr').textContent} MW`);
    log(`  NEUTRON:     ${document.getElementById('nflux').textContent}`);
    log(`  ORM:         ${document.getElementById('orm').textContent}`);
    log(`  COOLANT:     ${document.getElementById('flow').textContent} t/h`);
    log(`  STEAM PRESS: ${document.getElementById('spress').textContent} MPa`);
    log(`  ALARMS:      ${alarmCount}`);
  },
  RODS(args){
    const n = parseInt(args[0]);
    if(isNaN(n)||n<0||n>211){log('ERROR: VALUE MUST BE 0-211','err');return;}
    document.getElementById('rods').textContent = n;
    document.getElementById('rbar').style.width = Math.round(n/211*100)+'%';
    document.getElementById('rbar').className = 'bar-fill'+(n<15?' crit':n<30?' warn':'');
    const ormEl = document.getElementById('orm');
    ormEl.textContent = n+' rods';
    ormEl.className = 'readout-val'+(n<15?' crit':n<30?' warn':'');
    log(`RODS SET: ${n} — ORM ${n<15?'CRITICAL!':n<30?'WARNING':' NORMAL'}`,(n<15?'err':n<30?'':'ok'));
    if(n<15){triggerAlarm('ORM CRITICALLY LOW — DANGER');}
  },
  POWER(args){
    const p = parseInt(args[0]);
    if(isNaN(p)||p<0||p>100){log('ERROR: VALUE 0-100','err');return;}
    const mw = Math.round(3200*p/100);
    document.getElementById('pwr').textContent = mw;
    document.getElementById('pbar').style.width = p+'%';
    document.getElementById('pbar').className = 'bar-fill'+(p>90?' warn':'');
    log(`POWER TARGET: ${p}% (${mw} MW)`,p>90?'err':'ok');
    if(p>90) triggerAlarm('HIGH POWER LEVEL');
  },
  FLOW(args){
    const n = parseInt(args[0]);
    if(isNaN(n)||n<30000||n>60000){log('ERROR: FLOW 30000-60000 t/h','err');return;}
    document.getElementById('flow').textContent = n;
    document.getElementById('fbar').style.width = Math.round((n-30000)/30000*100)+'%';
    log(`COOLANT FLOW: ${n} t/h`,'ok');
  },
  KRV(){switchProg('KRV');log('KRV PROGRAM LOADED — CORE POWER MAP ACTIVE','ok');},
  DREG(){switchProg('DREG');log('DREG PROGRAM LOADED — COOLANT MONITORING ACTIVE','ok');},
  PRIZMA(){switchProg('PRIZMA');log('PRIZMA PROGRAM LOADED — PARAMETER LOGGING ACTIVE','ok');},
  'AZ-5'(){pressAZ5();},
  TAPE(){
    log('-- READING PRIZMA MAGNETIC TAPE --','sys');
    setTimeout(()=>log('  RECORD 0482: 01:23:40  PWR=3200MW  ORM=15  FL=48000','sys'),200);
    setTimeout(()=>log('  RECORD 0483: 01:23:41  PWR=3200MW  ORM=14  FL=47900','sys'),600);
    setTimeout(()=>log('  RECORD 0484: 01:23:42  PWR=3200MW  ORM=13  FL=47700','sys'),1000);
    setTimeout(()=>log('  TAPE READ COMPLETE — 3 RECORDS','ok'),1400);
  },
  RESET(){
    alarmCount = 0;
    document.getElementById('alarm-ct').textContent = '0';
    document.getElementById('alarm-ct').className = '';
    log('ALARM STATE CLEARED','ok');
  },
  CLEAR(){tout.innerHTML = '';},
};

function handleCmd(e){
  if(e.key!=='Enter') return;
  const inp = document.getElementById('cmdinput');
  const raw = inp.value.trim().toUpperCase();
  inp.value = '';
  if(!raw) return;
  log(`SKALA> ${raw}`);
  const parts = raw.split(/\s+/);
  const cmd = parts[0];
  const args = parts.slice(1);
  if(commands[cmd]) commands[cmd](args);
  else log(`UNKNOWN COMMAND: ${cmd} — TYPE HELP`,'err');
}

function switchProg(p){
  currentProg = p;
  document.getElementById('prog-ind').textContent = p;
  ['KRV','DREG','PRIZMA'].forEach(x=>{
    document.getElementById('b-'+x.toLowerCase()).className = 'btn'+(x===p?' active':'');
  });
}

function pressAZ5(){
  if(az5pressed) return;
  az5pressed = true;
  log('*** AZ-5 EMERGENCY SHUTDOWN INITIATED ***','err');
  log('INSERTING ALL CONTROL RODS...','err');
  let r = parseInt(document.getElementById('rods').textContent)||207;
  const target = 211;
  const iv = setInterval(()=>{
    r = Math.min(r+4, target);
    document.getElementById('rods').textContent = r;
    document.getElementById('rbar').style.width = Math.round(r/211*100)+'%';
    if(r>=target){
      clearInterval(iv);
      document.getElementById('pwr').textContent = '0';
      document.getElementById('pbar').style.width = '0%';
      document.getElementById('nflux').textContent = '0.00E+00';
      log('REACTOR SHUTDOWN COMPLETE','ok');
      log('POWER: 0 MW — ALL SYSTEMS STANDBY','sys');
      az5pressed = false;
    }
  },120);
}

function triggerAlarm(msg){
  alarmCount++;
  const el = document.getElementById('alarm-ct');
  el.textContent = alarmCount;
  el.className = 'alarm';
  log(`⚠ ALARM: ${msg}`,'err');
}

function updateClock(){
  const now = new Date();
  document.getElementById('clock').textContent =
    String(now.getHours()).padStart(2,'0')+':'+
    String(now.getMinutes()).padStart(2,'0')+':'+
    String(now.getSeconds()).padStart(2,'0');
}

function flicker(id,val,delta,min,max,dec=0){
  const el = document.getElementById(id);
  let v = parseFloat(val);
  v += (Math.random()-0.5)*delta;
  v = Math.max(min, Math.min(max, v));
  if(id==='nflux') el.textContent = v.toExponential(2).toUpperCase();
  else el.textContent = dec>0 ? v.toFixed(dec) : Math.round(v);
  return v;
}

let _pwr=3200,_flow=48000,_tin=270,_tout2=284,_spress=6.28,_drum=15,_react=0.0047,_void=0.0012;

function tick(){
  updateClock();
  _pwr = flicker('pwr',_pwr,20,2800,3400);
  _flow = flicker('flow',_flow,200,44000,52000);
  _tin = flicker('tin',_tin,0.5,268,272,1);
  _tout2 = flicker('tout',_tout2,0.5,282,286,1);
  _spress = flicker('spress',_spress,0.03,6.1,6.5,2);
  _drum = flicker('drum',_drum,2,-30,40);
  const drumEl = document.getElementById('drum');
  drumEl.textContent = (_drum>=0?'+':'')+Math.round(_drum);
  _react = flicker('react',_react,0.0002,0.003,0.007,4);
  _void = flicker('void',_void,0.0001,0.0005,0.002,4);
  const voidEl = document.getElementById('void');
  voidEl.textContent = '+'+_void.toFixed(4);
}

setInterval(tick, 1800);
updateClock();
setInterval(updateClock, 1000);

log('SKALA SYSTEM INITIALIZED — V-3M FRAME 2','ok');
log('PROGRAM KRV LOADED FROM MAGNETIC TAPE','sys');
log('26 APRIL 1986  01:23:42  UNIT 4 OPERATIONAL','sys');
log('TYPE HELP FOR COMMAND LIST','sys');
</script>
