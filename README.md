<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Cut List">
<title>Integrity Cut List</title>
<style>
:root {
  --bg:#f2f2f7; --card:#fff; --border:#e5e5ea; --border-light:#f2f2f7;
  --text:#1c1c1e; --text2:#636366; --text3:#8e8e93;
  --blue:#007aff; --green:#34c759; --orange:#ff9500; --red:#ff3b30;
  --hdr:#1c1c1e;
  --chop:#378ADD; --panel:#1D9E75; --cope:#D85A30; --door:#9E5DB3; --ready:#ff9500;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
html,body{height:100%;}
body{font-family:-apple-system,BlinkMacSystemFont,'SF Pro Text','Segoe UI',sans-serif;background:var(--bg);padding-bottom:90px;}

/* ── TOP HEADER ── */
header{background:var(--hdr);padding:12px 16px 0;position:sticky;top:0;z-index:50;}
.hdr-top{display:flex;align-items:center;justify-content:space-between;gap:10px;padding-bottom:10px;border-bottom:1px solid #3a3a3c;}
header h1{font-size:18px;font-weight:700;color:#fff;letter-spacing:-.3px;flex:1;}
.import-btn{font-size:13px;font-weight:600;padding:8px 14px;border:1px solid #48484a;border-radius:10px;background:#2c2c2e;color:var(--blue);cursor:pointer;touch-action:manipulation;white-space:nowrap;}
.import-btn:active{background:#3a3a3c;}
.sync-row{display:flex;align-items:center;gap:10px;padding:6px 0 0;}
#sync-note{font-size:12px;color:#636366;flex:1;}
.refresh-btn{font-size:13px;color:var(--blue);background:none;border:none;cursor:pointer;font-weight:500;touch-action:manipulation;}

/* ── TAB BAR ── */
.tab-bar{
  display:flex;background:var(--hdr);padding:0 4px 0;
  overflow-x:auto;-webkit-overflow-scrolling:touch;
  scrollbar-width:none;gap:2px;
}
.tab-bar::-webkit-scrollbar{display:none;}
.tab{
  flex:0 0 auto;padding:9px 14px 10px;cursor:pointer;touch-action:manipulation;
  font-size:13px;font-weight:500;color:#8e8e93;white-space:nowrap;
  border-bottom:3px solid transparent;transition:color .15s;
  display:flex;align-items:center;gap:5px;
}
.tab:active{opacity:.7;}
.tab.active{color:#fff;font-weight:700;}
.tab-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
.tab .badge-count{
  font-size:11px;font-weight:700;min-width:18px;height:18px;
  border-radius:9px;background:#ff3b30;color:#fff;
  display:flex;align-items:center;justify-content:center;padding:0 4px;
}

/* ── PAGE CONTENT ── */
.page{display:none;padding:12px;}
.page.active{display:block;}
@media(min-width:680px){.page{padding:14px;}}

/* ── CARD ── */
.card{background:var(--card);border-radius:16px;overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.08);}
.card-header{display:flex;align-items:center;gap:10px;padding:14px 14px 12px;border-bottom:1px solid var(--border-light);}
.dept-bar{width:5px;height:40px;border-radius:3px;flex-shrink:0;}
.dept-info{flex:1;}
.dept-name{font-size:16px;font-weight:700;color:var(--text);}
.dept-stats{font-size:12px;color:var(--text3);margin-top:2px;}
.dept-hint{font-size:11px;padding:5px 14px 4px;font-style:italic;color:var(--text3);border-bottom:1px solid var(--border-light);}

/* ── ADD ROW ── */
.add-row{display:flex;gap:8px;padding:10px 12px;border-bottom:1px solid var(--border-light);background:#fafafa;}
.add-row input{
  flex:1;min-width:0;font-size:16px;padding:10px 13px;
  border:1.5px solid var(--border);border-radius:11px;
  background:var(--card);color:var(--text);outline:none;
  -webkit-appearance:none;appearance:none;
}
.add-row input:focus{border-color:var(--blue);}
.add-row button{
  font-size:16px;font-weight:600;padding:10px 18px;
  border:none;border-radius:11px;background:var(--blue);
  color:#fff;cursor:pointer;white-space:nowrap;touch-action:manipulation;min-width:76px;
}
.add-row button:active{background:#0062cc;transform:scale(.97);}

/* ── ORDER ITEM ── */
.order-list{padding:0;}
.order-item{
  display:flex;align-items:center;gap:10px;
  padding:11px 12px;border-bottom:1px solid var(--border-light);
  min-height:56px;transition:background .1s;
}
.order-item:last-child{border-bottom:none;}
.order-item:active{background:#f5f5f7;}
.order-item.done{background:#fafafa;}

.status-btn{
  width:32px;height:32px;min-width:32px;border-radius:50%;
  border:2px solid #c7c7cc;background:transparent;cursor:pointer;
  display:flex;align-items:center;justify-content:center;
  font-size:14px;touch-action:manipulation;padding:0;flex-shrink:0;
  transition:all .15s;
}
.status-btn:active{transform:scale(.9);}
.status-btn.s-done{background:var(--green);border-color:var(--green);color:#fff;}
/* s-progress removed — no in_progress state */

.order-info{flex:1;min-width:0;}
.order-num{font-size:18px;font-weight:700;color:var(--text);letter-spacing:.3px;line-height:1.2;font-variant-numeric:tabular-nums;}
.order-item.done .order-num{text-decoration:line-through;color:var(--text3);font-weight:400;}
.order-meta{font-size:12px;color:var(--text2);margin-top:2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.order-due{display:inline-block;font-size:11px;font-weight:600;color:var(--orange);margin-top:3px;}

.order-right{display:flex;align-items:center;gap:6px;flex-shrink:0;}
.badge{font-size:11px;font-weight:600;padding:4px 9px;border-radius:20px;white-space:nowrap;}
.badge.pending{background:#f2f2f7;color:#636366;}
/* .badge.in_progress removed */
.badge.done{background:#e8f8ec;color:var(--green);}

.del-btn{
  background:#ff3b3014;border:none;cursor:pointer;color:var(--red);
  font-size:15px;font-weight:700;width:36px;height:36px;min-width:36px;
  border-radius:9px;display:flex;align-items:center;justify-content:center;
  touch-action:manipulation;padding:0;
}
.del-btn:active{background:#ff3b3028;transform:scale(.93);}

/* ── COMPLETED TOGGLE ── */
.completed-toggle{
  display:flex;align-items:center;gap:6px;padding:9px 14px;
  cursor:pointer;border-top:1px solid var(--border-light);
  background:#fafafa;touch-action:manipulation;
}
.completed-toggle:active{background:#f0f0f2;}
.toggle-label{font-size:13px;font-weight:500;color:var(--text3);flex:1;}
.toggle-arrow{font-size:12px;color:var(--text3);transition:transform .2s;}
.toggle-arrow.open{transform:rotate(180deg);}

/* ── READY FOR ASSEMBLY TAB ── */
.ready-intro{
  background:linear-gradient(135deg,#fff6e6,#fff);
  border:1px solid #ffe0a0;border-radius:14px;
  padding:14px 16px;margin-bottom:12px;
}
.ready-intro h2{font-size:15px;font-weight:700;color:#7d5a00;margin-bottom:3px;}
.ready-intro p{font-size:13px;color:#a07830;line-height:1.5;}

.ready-item{
  display:flex;align-items:center;gap:10px;
  padding:12px 14px;border-bottom:1px solid var(--border-light);min-height:60px;
}
.ready-item:last-child{border-bottom:none;}
.ready-num{font-size:20px;font-weight:800;color:var(--text);letter-spacing:.3px;font-variant-numeric:tabular-nums;}
.ready-meta{font-size:12px;color:var(--text2);margin-top:2px;}
.ready-due{font-size:11px;font-weight:600;color:var(--orange);margin-top:3px;}
.ready-badge{
  font-size:12px;font-weight:700;padding:5px 11px;border-radius:20px;
  background:linear-gradient(135deg,#fff0d0,#ffe4a0);color:#7d5a00;white-space:nowrap;
}
.ready-empty{font-size:14px;color:var(--text3);text-align:center;padding:40px 20px;line-height:1.6;}
.ready-empty span{font-size:32px;display:block;margin-bottom:10px;}

.empty{font-size:13px;color:var(--text3);text-align:center;padding:22px 0;}

/* ── SETUP BANNER ── */
.setup-banner{
  background:#fff8e6;border-radius:12px;margin:0 0 12px;
  padding:11px 14px;font-size:13px;color:#7d5a00;line-height:1.5;
  border:1px solid #ffe58f;
}
.setup-banner strong{display:block;margin-bottom:2px;font-size:13px;font-weight:600;}

/* ── IMPORT SHEET ── */
.sheet-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:100;align-items:flex-end;justify-content:center;}
.sheet-bg.open{display:flex;}
@media(min-width:520px){.sheet-bg{align-items:center;padding:24px;}}
.sheet{background:var(--card);border-radius:20px 20px 0 0;padding:20px 18px 44px;width:100%;max-width:500px;max-height:90vh;overflow-y:auto;}
@media(min-width:520px){.sheet{border-radius:18px;padding:22px 20px;}}
.sheet-handle{width:36px;height:4px;background:#e0e0e0;border-radius:2px;margin:0 auto 16px;}
.sheet h3{font-size:18px;font-weight:700;color:var(--text);margin-bottom:5px;}
.sheet-sub{font-size:13px;color:var(--text3);margin-bottom:16px;line-height:1.5;}
.drop-zone{border:2px dashed #c7c7cc;border-radius:14px;padding:30px 16px;text-align:center;cursor:pointer;transition:all .15s;margin-bottom:14px;position:relative;background:#fafafa;}
.drop-zone.over{border-color:var(--blue);background:#e8f4ff;}
.drop-zone input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%;}
.drop-icon{font-size:36px;margin-bottom:8px;}
.drop-label{font-size:15px;font-weight:600;color:var(--text);margin-bottom:4px;}
.drop-sub{font-size:12px;color:var(--text3);}
#preview-area{display:none;margin-bottom:14px;}
#preview-area h4{font-size:13px;font-weight:600;color:var(--text);margin-bottom:7px;}
.preview-list{max-height:200px;overflow-y:auto;border:1px solid var(--border);border-radius:11px;}
.prev-item{display:flex;align-items:center;padding:9px 12px;border-bottom:1px solid var(--border-light);gap:8px;}
.prev-item:last-child{border-bottom:none;}
.prev-item.dup{opacity:.45;}
.prev-main{flex:1;min-width:0;}
.prev-num{font-size:15px;font-weight:700;color:var(--text);}
.prev-meta{font-size:11px;color:var(--text3);margin-top:1px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.prev-tag{font-size:11px;font-weight:600;padding:3px 8px;border-radius:10px;white-space:nowrap;}
.tag-new{background:#e8f4ff;color:var(--blue);}
.tag-dup{background:#f2f2f7;color:var(--text3);}
#preview-count{font-size:12px;color:var(--text3);margin-top:7px;}
.sheet-btns{display:flex;gap:10px;margin-top:4px;}
.sheet-btns button{flex:1;padding:14px;border-radius:13px;border:none;font-size:16px;font-weight:600;cursor:pointer;touch-action:manipulation;}
.btn-cancel{background:#f2f2f7;color:var(--text);}
.btn-cancel:active{background:#e5e5ea;}
.btn-confirm{background:var(--blue);color:#fff;}
.btn-confirm:active{background:#0062cc;}
.btn-confirm:disabled{background:#c7c7cc;}

/* ── CONFIRM SHEET ── */
.confirm-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:200;align-items:center;justify-content:center;padding:20px;}
.confirm-bg.open{display:flex;}
.confirm-box{background:var(--card);border-radius:16px;padding:22px 18px 16px;width:100%;max-width:300px;}
.confirm-box h3{font-size:17px;font-weight:700;color:var(--text);margin-bottom:7px;}
.confirm-box p{font-size:14px;color:var(--text2);margin-bottom:18px;line-height:1.5;}
.confirm-btns{display:flex;gap:10px;}
.confirm-btns button{flex:1;padding:13px;border-radius:11px;border:none;font-size:15px;font-weight:600;cursor:pointer;touch-action:manipulation;}
.btn-del{background:var(--red);color:#fff;}
.btn-del:active{background:#d63429;}
/* ── PIN MODAL ── */
.pin-dot{width:14px;height:14px;border-radius:50%;border:2px solid #c7c7cc;background:transparent;transition:background .15s,border-color .15s;}
.pin-dot.filled{background:#1c1c1e;border-color:#1c1c1e;}
.pin-dot.error{background:#ff3b30;border-color:#ff3b30;}
.pin-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;}
.pin-key{
  padding:16px 8px;border-radius:12px;border:none;
  font-size:20px;font-weight:600;color:var(--text);
  background:#f2f2f7;cursor:pointer;touch-action:manipulation;
  transition:background .1s;
}
.pin-key:active{background:#e0e0e5;transform:scale(.93);}
.pin-cancel-key{font-size:13px;font-weight:600;color:#ff3b30;background:#fff0ef;}
.pin-cancel-key:active{background:#ffe0de;}
.pin-del-key{font-size:18px;background:#f2f2f7;}
</style>
</head>
<body>

<header>
  <div class="hdr-top">
    <h1>🪵 Integrity Cut List</h1>
    <button class="import-btn" id="open-import-btn">⬆ Import</button>
  </div>
  <div class="sync-row">
    <span id="sync-note">Loading…</span>
    <button class="refresh-btn" id="refresh-btn">↻ Refresh</button>
  </div>
  <!-- TAB BAR -->
  <div class="tab-bar" id="tab-bar">
    <div class="tab active" data-tab="chop_saw">
      <div class="tab-dot" style="background:var(--chop)"></div>Chop Saw
    </div>
    <div class="tab" data-tab="panel_saw">
      <div class="tab-dot" style="background:var(--panel)"></div>Panel Saw
    </div>
    <div class="tab" data-tab="cope">
      <div class="tab-dot" style="background:var(--cope)"></div>Cope
    </div>
    <div class="tab" data-tab="door_assy">
      <div class="tab-dot" style="background:var(--door)"></div>Door Assembly
    </div>
    <div class="tab" data-tab="ready">
      <div class="tab-dot" style="background:var(--ready)"></div>Ready for Assembly
    </div>
  </div>
</header>

<!-- DEPT PAGES -->
<div class="page active" id="page-chop_saw"></div>
<div class="page" id="page-panel_saw"></div>
<div class="page" id="page-cope"></div>
<div class="page" id="page-door_assy"></div>

<!-- READY FOR ASSEMBLY PAGE -->
<div class="page" id="page-ready"></div>

<!-- IMPORT SHEET -->
<div class="sheet-bg" id="import-modal">
  <div class="sheet">
    <div class="sheet-handle"></div>
    <h3>Import Orders</h3>
    <p class="sheet-sub">Drop your F43P2 CSV — or any CSV, Excel, or TXT. Order #, species, profile &amp; due date are read automatically. New orders go to all 4 departments.</p>
    <div class="drop-zone" id="drop-zone">
      <input type="file" id="file-input" accept=".csv,.xlsx,.xls,.txt">
      <div class="drop-icon">📂</div>
      <div class="drop-label">Tap to choose a file</div>
      <div class="drop-sub">or drag &amp; drop · CSV · Excel · TXT</div>
    </div>
    <div id="preview-area">
      <h4 id="preview-heading">Preview</h4>
      <div class="preview-list" id="preview-list"></div>
      <div id="preview-count"></div>
    </div>
    <div class="sheet-btns">
      <button class="btn-cancel" id="import-cancel">Cancel</button>
      <button class="btn-confirm" id="import-confirm" disabled>Import</button>
    </div>
  </div>
</div>

<!-- DELETE CONFIRM -->
<div class="confirm-bg" id="confirm-bg">
  <div class="confirm-box">
    <h3 id="confirm-title">Remove order?</h3>
    <p id="confirm-body"></p>
    <div class="confirm-btns">
      <button class="btn-cancel" id="confirm-cancel">Cancel</button>
      <button class="btn-del" id="confirm-ok">Remove</button>
    </div>
  </div>
</div>

<!-- PIN MODAL -->
<div class="confirm-bg" id="pin-bg">
  <div class="confirm-box" style="max-width:320px;">
    <h3 id="pin-title" style="color:#34c759;">✅ Complete All Orders</h3>
    <p id="pin-body" style="margin-bottom:14px;line-height:1.5;">This will mark <strong>all orders as complete</strong>. Enter the PIN to confirm.</p>
    <div id="pin-dots" style="display:flex;justify-content:center;gap:12px;margin-bottom:16px;">
      <div class="pin-dot"></div><div class="pin-dot"></div><div class="pin-dot"></div><div class="pin-dot"></div>
    </div>
    <div id="pin-error" style="font-size:12px;color:#ff3b30;text-align:center;min-height:16px;margin-bottom:10px;"></div>
    <div class="pin-grid">
      <button class="pin-key" data-v="1">1</button>
      <button class="pin-key" data-v="2">2</button>
      <button class="pin-key" data-v="3">3</button>
      <button class="pin-key" data-v="4">4</button>
      <button class="pin-key" data-v="5">5</button>
      <button class="pin-key" data-v="6">6</button>
      <button class="pin-key" data-v="7">7</button>
      <button class="pin-key" data-v="8">8</button>
      <button class="pin-key" data-v="9">9</button>
      <button class="pin-key pin-cancel-key" data-v="cancel">Cancel</button>
      <button class="pin-key" data-v="0">0</button>
      <button class="pin-key pin-del-key" data-v="del">⌫</button>
    </div>
  </div>
</div>

<script>
// ─── CONFIG ────────────────────────────────────────────────────
var FIREBASE_URL = 'https://integrity-cut-list-default-rtdb.firebaseio.com';
// ───────────────────────────────────────────────────────────────

var DEPTS = [
  {id:'chop_saw',  name:'Chop Saw',      color:'#378ADD'},
  {id:'panel_saw', name:'Panel Saw',     color:'#1D9E75'},
  {id:'cope',      name:'Cope',          color:'#D85A30'},
  {id:'door_assy', name:'Door Assembly', color:'#9E5DB3'}
];
var CHOP_ID    = 'chop_saw';
var DOOR_ID    = 'door_assy';
var FEEDER_IDS = ['chop_saw','panel_saw','cope'];
var ALL_IDS    = DEPTS.map(function(d){return d.id;});

var STATUS_CYCLE = {pending:'done',done:'pending'}; // simplified: no in_progress step
var data = {};
var showCompleted = {};
var activeTab = 'chop_saw';

// ─── SORT ──────────────────────────────────────────────────────
function sortOrders(arr) {
  return arr.slice().sort(function(a,b){
    var na=parseFloat(a.num), nb=parseFloat(b.num);
    if(!isNaN(na)&&!isNaN(nb)) return na-nb;
    return a.num.localeCompare(b.num);
  });
}

// ─── STORAGE ───────────────────────────────────────────────────
async function loadDept(id) {
  if (FIREBASE_URL) {
    try {
      var r=await fetch(FIREBASE_URL+'/depts/'+id+'.json');
      var j=await r.json();
      return Array.isArray(j)?j:(j?Object.values(j):[]);
    } catch(e){}
  }
  try{return JSON.parse(localStorage.getItem('icl4_'+id))||[];}catch(e){return [];}
}
async function saveDept(id,orders){
  if(FIREBASE_URL){
    try{
      await fetch(FIREBASE_URL+'/depts/'+id+'.json',{
        method:'PUT',headers:{'Content-Type':'application/json'},body:JSON.stringify(orders)
      });
    }catch(e){}
  }
  try{localStorage.setItem('icl4_'+id,JSON.stringify(orders));}catch(e){}
}
async function saveMany(ids){for(var i=0;i<ids.length;i++) await saveDept(ids[i],data[ids[i]]);}

function timeNow(){return new Date().toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'});}
function note(t){document.getElementById('sync-note').textContent=t;}
function hasOrder(id,num){return (data[id]||[]).some(function(o){return o.num.toLowerCase()===num.toLowerCase();});}

// ─── LOAD ──────────────────────────────────────────────────────
async function loadAll(){
  note('Syncing…');
  for(var i=0;i<DEPTS.length;i++) data[DEPTS[i].id]=await loadDept(DEPTS[i].id);
  renderAll();
  note(FIREBASE_URL?'Live · '+timeNow():'Local · '+timeNow());
}

// ─── TAB SWITCHING ─────────────────────────────────────────────
document.getElementById('tab-bar').addEventListener('click',function(e){
  var tab=e.target.closest('.tab');
  if(!tab) return;
  var id=tab.dataset.tab;
  activeTab=id;
  document.querySelectorAll('.tab').forEach(function(t){t.classList.remove('active');});
  tab.classList.add('active');
  document.querySelectorAll('.page').forEach(function(p){p.classList.remove('active');});
  document.getElementById('page-'+id).classList.add('active');
});

// ─── RENDER ALL ────────────────────────────────────────────────
function renderAll(){
  DEPTS.forEach(function(dept){
    var page=document.getElementById('page-'+dept.id);
    page.innerHTML='';
    if(!FIREBASE_URL){
      var banner=document.createElement('div');
      banner.className='setup-banner';
      banner.innerHTML='<strong>⚙️ Single-device mode</strong>Add Firebase URL to FIREBASE_URL to sync across all phones.';
      page.appendChild(banner);
    }
    page.appendChild(makeDeptCard(dept));
  });
  renderReadyTab();
  updateTabBadges();
}

function updateTabBadges(){
  DEPTS.forEach(function(dept){
    var tab=document.querySelector('.tab[data-tab="'+dept.id+'"]');
    if(!tab) return;
    var active=(data[dept.id]||[]).filter(function(o){return o.status!=='done';}).length;
    // Remove old badge
    var old=tab.querySelector('.badge-count');
    if(old) old.remove();
    if(active>0){
      var b=document.createElement('span');
      b.className='badge-count';
      b.textContent=active;
      tab.appendChild(b);
    }
    // Active tab underline color
    var color=dept.color;
    if(tab.classList.contains('active')) tab.style.borderBottomColor=color;
    else tab.style.borderBottomColor='transparent';
  });
  // Ready tab badge: count ready orders
  var readyTab=document.querySelector('.tab[data-tab="ready"]');
  if(readyTab){
    var readyOrders=getReadyOrders();
    var old2=readyTab.querySelector('.badge-count');
    if(old2) old2.remove();
    if(readyOrders.length>0){
      var b2=document.createElement('span');
      b2.className='badge-count';
      b2.style.background='#ff9500';
      b2.textContent=readyOrders.length;
      readyTab.appendChild(b2);
    }
    if(readyTab.classList.contains('active')) readyTab.style.borderBottomColor='#ff9500';
    else readyTab.style.borderBottomColor='transparent';
  }
}

// ─── READY FOR ASSEMBLY ────────────────────────────────────────
// An order is "ready" when it is marked done in BOTH panel_saw AND cope
function getReadyOrders(){
  var panelDone={};
  (data['panel_saw']||[]).forEach(function(o){
    if(o.status==='done') panelDone[o.num.toLowerCase()]=o;
  });
  var ready=[];
  (data['cope']||[]).forEach(function(o){
    if(o.status==='done' && panelDone[o.num.toLowerCase()]){
      ready.push(o);
    }
  });
  return sortOrders(ready);
}

function clearReadyOrders(){
  // Remove all done orders (that are "ready") from ALL departments
  var readyNums={};
  getReadyOrders().forEach(function(o){ readyNums[o.num.toLowerCase()]=true; });
  var toSave=[];
  ALL_IDS.forEach(function(id){
    var before=(data[id]||[]).length;
    data[id]=(data[id]||[]).filter(function(o){ return !readyNums[o.num.toLowerCase()]; });
    if(data[id].length!==before) toSave.push(id);
  });
  if(toSave.length){
    saveMany(toSave).then(function(){renderAll();note('Cleared · '+timeNow());});
  }
}

// ─── PIN MODAL ─────────────────────────────────────────────────
var PIN_CODE = '1234';
var pinBg = document.getElementById('pin-bg');
var pinEntry = '';
var pinCallback = null;

function showPin(cb, title, body){
  pinEntry=''; pinCallback=cb;
  updatePinDots();
  document.getElementById('pin-error').textContent='';
  if(title) document.getElementById('pin-title').textContent=title;
  if(body)  document.getElementById('pin-body').innerHTML=body;
  pinBg.classList.add('open');
}
function updatePinDots(){
  var dots=document.querySelectorAll('.pin-dot');
  dots.forEach(function(d,i){
    d.classList.toggle('filled', i < pinEntry.length);
    d.classList.remove('error');
  });
}
function pinShake(){
  var dots=document.querySelectorAll('.pin-dot');
  dots.forEach(function(d){ d.classList.add('error'); });
  document.getElementById('pin-error').textContent='Incorrect PIN. Try again.';
  setTimeout(function(){ pinEntry=''; updatePinDots(); },800);
}
document.getElementById('pin-bg').addEventListener('click',function(e){
  // only close if tapping the backdrop itself
  if(e.target===pinBg){ pinEntry=''; pinBg.classList.remove('open'); pinCallback=null; }
});
document.querySelectorAll('.pin-key').forEach(function(key){
  key.addEventListener('click',function(){
    var v=key.dataset.v;
    if(v==='cancel'){ pinEntry=''; pinBg.classList.remove('open'); pinCallback=null; return; }
    if(v==='del'){ pinEntry=pinEntry.slice(0,-1); updatePinDots(); return; }
    if(pinEntry.length>=4) return;
    pinEntry+=v;
    updatePinDots();
    if(pinEntry.length===4){
      if(pinEntry===PIN_CODE){
        pinBg.classList.remove('open');
        var cb=pinCallback; pinCallback=null; pinEntry='';
        setTimeout(cb,120); // slight delay so modal closes first
      } else {
        pinShake();
      }
    }
  });
});

function renderReadyTab(){
  var page=document.getElementById('page-ready');
  page.innerHTML='';

  var readyOrders=getReadyOrders();

  // Intro + clear button row
  var intro=document.createElement('div');
  intro.className='ready-intro';
  var introInner=document.createElement('div');
  introInner.style.cssText='display:flex;align-items:flex-start;justify-content:space-between;gap:10px;';
  var introText=document.createElement('div');
  introText.innerHTML='<h2>🟡 Ready for Door Assembly</h2>'
    +'<p>Orders marked <strong>Complete</strong> in both Panel Saw and Cope.</p>';
  introInner.appendChild(introText);

  if(readyOrders.length>0){
    var clearBtn=document.createElement('button');
    clearBtn.textContent='Clear All';
    clearBtn.style.cssText='flex-shrink:0;font-size:13px;font-weight:600;padding:8px 14px;'
      +'border:none;border-radius:10px;background:#ff3b30;color:#fff;cursor:pointer;touch-action:manipulation;margin-top:2px;';
    clearBtn.addEventListener('click',function(){
      showPin(
        clearReadyOrders,
        '⚠️ Clear All Ready Orders',
        'This will remove <strong>all ready orders from every department</strong>. Enter PIN to confirm.'
      );
    });
    introInner.appendChild(clearBtn);
  }

  intro.appendChild(introInner);
  page.appendChild(intro);

  var card=document.createElement('div');
  card.className='card';

  if(readyOrders.length===0){
    var em=document.createElement('div');
    em.className='ready-empty';
    em.innerHTML='<span>✅</span>No orders are ready yet.<br>Orders appear here when complete in both Panel Saw and Cope.';
    card.appendChild(em);
  } else {
    var list=document.createElement('div');
    list.className='order-list';
    readyOrders.forEach(function(o){
      var item=document.createElement('div');
      item.className='ready-item';

      var info=document.createElement('div');
      info.style.flex='1'; info.style.minWidth='0';

      var numEl=document.createElement('div');
      numEl.className='ready-num'; numEl.textContent=o.num;
      info.appendChild(numEl);

      if(o.species||o.profile){
        var meta=document.createElement('div');
        meta.className='ready-meta';
        var pts=[];
        if(o.species) pts.push(o.species);
        if(o.profile) pts.push(o.profile);
        meta.textContent=pts.join(' · ');
        info.appendChild(meta);
      }
      if(o.due){
        var dueEl=document.createElement('div');
        dueEl.className='ready-due';
        dueEl.textContent='📅 '+o.due;
        info.appendChild(dueEl);
      }

      // Complete + delete buttons
      var rowRight=document.createElement('div');
      rowRight.style.cssText='display:flex;align-items:center;gap:8px;flex-shrink:0;';

      var completeBtn=document.createElement('button');
      completeBtn.type='button';
      completeBtn.textContent='✓ Complete';
      completeBtn.style.cssText='font-size:13px;font-weight:700;padding:8px 14px;'
        +'border:none;border-radius:10px;background:var(--green);color:#fff;'
        +'cursor:pointer;touch-action:manipulation;white-space:nowrap;';
      completeBtn.addEventListener('touchstart',function(){this.style.opacity='0.75';},{passive:true});
      completeBtn.addEventListener('touchend',function(){this.style.opacity='1';},{passive:true});

      var delBtn=document.createElement('button');
      delBtn.type='button'; delBtn.className='del-btn'; delBtn.textContent='✕';
      delBtn.title='Remove this order';

      rowRight.appendChild(completeBtn);
      rowRight.appendChild(delBtn);
      item.appendChild(info);
      item.appendChild(rowRight);

      (function(orderNum){
        // Complete — clears from ALL depts entirely
        completeBtn.addEventListener('click',function(){
          ALL_IDS.forEach(function(id){
            data[id]=(data[id]||[]).filter(function(x){return x.num.toLowerCase()!==orderNum.toLowerCase();});
          });
          saveMany(ALL_IDS).then(function(){renderAll();note('Completed '+orderNum+' · '+timeNow());});
        });
        // Delete — same, remove from all
        delBtn.addEventListener('click',function(){
          showConfirm('Remove '+orderNum+'?','Removes this order from all departments.',function(){
            ALL_IDS.forEach(function(id){
              data[id]=(data[id]||[]).filter(function(x){return x.num.toLowerCase()!==orderNum.toLowerCase();});
            });
            saveMany(ALL_IDS).then(function(){renderAll();});
          });
        });
      })(o.num);

      list.appendChild(item);
    });
    card.appendChild(list);
  }
  page.appendChild(card);
}

// ─── DEPT CARD ─────────────────────────────────────────────────
function makeDeptCard(dept){
  var allOrders=sortOrders(data[dept.id]||[]);
  var active   =allOrders.filter(function(o){return o.status!=='done';});
  var completed=allOrders.filter(function(o){return o.status==='done';});
  var isChop=dept.id===CHOP_ID, isDoor=dept.id===DOOR_ID;

  var card=document.createElement('div');
  card.className='card';

  // Header
  var hd=document.createElement('div');
  hd.className='card-header';
  hd.innerHTML='<div class="dept-bar" style="background:'+dept.color+'"></div>'
    +'<div class="dept-info"><div class="dept-name">'+dept.name+'</div>'
    +'<div class="dept-stats">'+active.length+' active · '+completed.length+' complete</div></div>';

  if(active.length>0){
    var completeAllBtn=document.createElement('button');
    completeAllBtn.type='button';
    completeAllBtn.textContent='✓ Complete All';
    completeAllBtn.style.cssText='font-size:12px;font-weight:600;padding:6px 12px;'
      +'border:none;border-radius:9px;background:#34c75918;color:#34c759;'
      +'cursor:pointer;touch-action:manipulation;white-space:nowrap;flex-shrink:0;';
    (function(deptId,deptName){
      completeAllBtn.addEventListener('click',function(){
        showPin(
          function(){
            (data[deptId]||[]).forEach(function(o){ o.status='done'; });
            saveDept(deptId,data[deptId]).then(function(){
              // If door assembly, also clear feeders for all now-done orders
              if(deptId===DOOR_ID){
                var doneNums={};
                (data[deptId]||[]).forEach(function(o){ doneNums[o.num.toLowerCase()]=true; });
                var toSave=[DOOR_ID];
                FEEDER_IDS.forEach(function(fid){
                  var before=(data[fid]||[]).length;
                  data[fid]=(data[fid]||[]).filter(function(x){return !doneNums[x.num.toLowerCase()];});
                  if(data[fid].length!==before) toSave.push(fid);
                });
                saveMany(toSave).then(function(){renderAll();note('Completed all in '+deptName+' · '+timeNow());});
              } else {
                renderAll();note('Completed all in '+deptName+' · '+timeNow());
              }
            });
          },
          '✅ Complete All — '+deptName,
          'This will mark <strong>all '+active.length+' active order'+(active.length!==1?'s':'')+' as Complete</strong> in '+deptName+'. Enter PIN to confirm.'
        );
      });
    })(dept.id,dept.name);
    hd.appendChild(completeAllBtn);
  }

  card.appendChild(hd);

  // Hint
  var hint=isChop?'Adding here also adds to Panel Saw, Cope & Door Assembly.'
    :isDoor?'Marking an order Complete (✓) removes it from Chop Saw, Panel Saw & Cope.'
    :(dept.id==='panel_saw'||dept.id==='cope')?'Adding here also adds to Door Assembly.':'';
  if(hint){
    var h=document.createElement('div');
    h.className='dept-hint'; h.textContent=hint;
    card.appendChild(h);
  }

  // Add row
  var ar=document.createElement('div');
  ar.className='add-row';
  var inp=document.createElement('input');
  inp.type='text'; inp.placeholder='Order #'; inp.maxLength=40;
  inp.autocomplete='off'; inp.inputMode='decimal';
  var btn=document.createElement('button');
  btn.type='button'; btn.textContent='+ Add';
  ar.appendChild(inp); ar.appendChild(btn);
  card.appendChild(ar);

  function doAdd(){
    var num=inp.value.trim();
    if(!num){inp.focus();return;}
    var targets=isChop?ALL_IDS:isDoor?[DOOR_ID]:[dept.id,DOOR_ID];
    var toUpdate=[];
    targets.forEach(function(id){
      if(!hasOrder(id,num)){
        if(!data[id]) data[id]=[];
        data[id].push({num:num,status:'pending',ts:Date.now()});
        toUpdate.push(id);
      }
    });
    inp.value='';
    if(toUpdate.length) saveMany(toUpdate).then(function(){renderAll();note('Saved '+timeNow());});
  }
  btn.addEventListener('click',doAdd);
  inp.addEventListener('keydown',function(e){if(e.key==='Enter'){e.preventDefault();doAdd();}});

  // Order list
  var list=document.createElement('div');
  list.className='order-list';

  if(allOrders.length===0){
    var em=document.createElement('p');
    em.className='empty'; em.textContent='No orders yet';
    list.appendChild(em);
  } else if(!isDoor){
    // Feeder depts: all orders inline — active first then completed so teams can cross-reference
    active.forEach(function(o){ list.appendChild(makeOrderItem(o,dept)); });
    completed.forEach(function(o){ list.appendChild(makeOrderItem(o,dept)); });
  } else {
    // Door Assembly: active orders shown
    active.forEach(function(o){ list.appendChild(makeOrderItem(o,dept)); });
  }
  card.appendChild(list);

  // Door Assembly only: collapsible completed section
  if(isDoor&&completed.length>0){
    var toggle=document.createElement('div');
    toggle.className='completed-toggle';
    var isOpen=showCompleted[dept.id]||false;
    toggle.innerHTML='<span class="toggle-label">Completed ('+completed.length+')</span>'
      +'<span class="toggle-arrow'+(isOpen?' open':'')+'">▼</span>';
    var cList=document.createElement('div');
    cList.className='order-list';
    cList.style.display=isOpen?'block':'none';
    completed.forEach(function(o){ cList.appendChild(makeOrderItem(o,dept)); });
    toggle.addEventListener('click',function(){
      showCompleted[dept.id]=!showCompleted[dept.id];
      cList.style.display=showCompleted[dept.id]?'block':'none';
      toggle.querySelector('.toggle-arrow').className='toggle-arrow'+(showCompleted[dept.id]?' open':'');
    });
    card.appendChild(toggle);
    card.appendChild(cList);
  }
  return card;
}

// ─── ORDER ITEM ────────────────────────────────────────────────
function makeOrderItem(o,dept){
  var isDoor=dept.id===DOOR_ID;
  var item=document.createElement('div');
  item.className='order-item'+(o.status==='done'?' done':'');

  var sbtn=document.createElement('button');
  sbtn.className='status-btn'+(o.status==='done'?' s-done':'');
  sbtn.type='button';
  sbtn.textContent=o.status==='done'?'✓':'';

  var info=document.createElement('div');
  info.className='order-info';

  var numEl=document.createElement('div');
  numEl.className='order-num';
  numEl.textContent=o.num;
  info.appendChild(numEl);

  if(o.species||o.profile){
    var meta=document.createElement('div');
    meta.className='order-meta';
    var pts=[];
    if(o.species) pts.push(o.species);
    if(o.profile) pts.push(o.profile);
    meta.textContent=pts.join(' · ');
    info.appendChild(meta);
  }
  if(o.due){
    var dueEl=document.createElement('div');
    dueEl.className='order-due';
    dueEl.textContent='📅 '+o.due;
    info.appendChild(dueEl);
  }

  var right=document.createElement('div');
  right.className='order-right';

  var badge=document.createElement('span');
  badge.className='badge '+(o.status||'pending');
  badge.textContent=o.status==='done'?'Complete':'In queue';

  var del=document.createElement('button');
  del.type='button'; del.className='del-btn'; del.textContent='✕';

  right.appendChild(badge);
  right.appendChild(del);
  item.appendChild(sbtn);
  item.appendChild(info);
  item.appendChild(right);

  (function(order,deptId){
    sbtn.addEventListener('click',function(){
      var idx=(data[deptId]||[]).findIndex(function(x){return x.num===order.num;});
      if(idx<0) return;
      var cur=data[deptId][idx].status;
      var ns=cur==='done'?'pending':'done';
      data[deptId][idx].status=ns;
      if(deptId===DOOR_ID&&ns==='done'){
        // Marking done in Door Assembly clears it from all feeder depts
        var toSave=[DOOR_ID];
        FEEDER_IDS.forEach(function(fid){
          var before=(data[fid]||[]).length;
          data[fid]=(data[fid]||[]).filter(function(x){return x.num.toLowerCase()!==order.num.toLowerCase();});
          if(data[fid].length!==before) toSave.push(fid);
        });
        saveMany(toSave).then(function(){renderAll();note('Saved '+timeNow());});
      } else {
        saveDept(deptId,data[deptId]).then(function(){renderAll();note('Saved '+timeNow());});
      }
    });
    del.addEventListener('click',function(e){
      e.stopPropagation();
      showConfirm('Remove '+order.num+'?','Removes from '+dept.name+' only.',function(){
        var idx2=(data[deptId]||[]).findIndex(function(x){return x.num===order.num;});
        if(idx2>=0) data[deptId].splice(idx2,1);
        saveDept(deptId,data[deptId]).then(function(){renderAll();});
      });
    });
  })(o,dept.id);

  return item;
}

// ─── CONFIRM MODAL ─────────────────────────────────────────────
var cBg=document.getElementById('confirm-bg'), pendingCb=null;
function showConfirm(title,body,cb){
  document.getElementById('confirm-title').textContent=title;
  document.getElementById('confirm-body').textContent=body;
  pendingCb=cb; cBg.classList.add('open');
}
document.getElementById('confirm-ok').addEventListener('click',function(){cBg.classList.remove('open');if(pendingCb)pendingCb();pendingCb=null;});
document.getElementById('confirm-cancel').addEventListener('click',function(){cBg.classList.remove('open');pendingCb=null;});
cBg.addEventListener('click',function(e){if(e.target===cBg){cBg.classList.remove('open');pendingCb=null;}});

// ─── IMPORT ────────────────────────────────────────────────────
var importModal=document.getElementById('import-modal');
var dropZone=document.getElementById('drop-zone');
var fileInput=document.getElementById('file-input');
var previewArea=document.getElementById('preview-area');
var previewList=document.getElementById('preview-list');
var previewCount=document.getElementById('preview-count');
var importBtn=document.getElementById('import-confirm');
var previewHead=document.getElementById('preview-heading');
var parsedOrders=[];

document.getElementById('open-import-btn').addEventListener('click',function(){resetImport();importModal.classList.add('open');});
document.getElementById('import-cancel').addEventListener('click',function(){importModal.classList.remove('open');});
importModal.addEventListener('click',function(e){if(e.target===importModal)importModal.classList.remove('open');});

function resetImport(){
  parsedOrders=[];previewArea.style.display='none';
  previewList.innerHTML='';previewCount.textContent='';
  importBtn.disabled=true;fileInput.value='';dropZone.classList.remove('over');
}
dropZone.addEventListener('dragover',function(e){e.preventDefault();dropZone.classList.add('over');});
dropZone.addEventListener('dragleave',function(){dropZone.classList.remove('over');});
dropZone.addEventListener('drop',function(e){e.preventDefault();dropZone.classList.remove('over');if(e.dataTransfer.files[0])handleFile(e.dataTransfer.files[0]);});
fileInput.addEventListener('change',function(){if(fileInput.files[0])handleFile(fileInput.files[0]);});

function handleFile(f){var n=f.name.toLowerCase();if(n.endsWith('.xlsx')||n.endsWith('.xls'))readExcel(f);else readTextFile(f);}

function parseCSVLine(line){
  var result=[],cur='',inQ=false;
  for(var i=0;i<line.length;i++){
    var ch=line[i];
    if(ch==='"'){inQ=!inQ;}
    else if(ch===','&&!inQ){result.push(cur);cur='';}
    else cur+=ch;
  }
  result.push(cur);return result;
}
function parseCSVRows(text){
  var lines=text.split(/\r?\n/);
  var soCol=-1,speciesCol=-1,profileCol=-1,startCol=-1,rows=[];
  for(var i=0;i<lines.length;i++){
    var cols=parseCSVLine(lines[i]);
    var soIdx=cols.findIndex(function(c){return c.replace(/"/g,'').trim()==='S/O #';});
    if(soIdx>=0){
      soCol=soIdx;
      speciesCol=cols.findIndex(function(c){return c.replace(/"/g,'').trim()==='Species';});
      profileCol=cols.findIndex(function(c){return c.replace(/"/g,'').trim()==='Profile';});
      startCol=cols.findIndex(function(c){return c.replace(/"/g,'').trim()==='Start';});
      for(var j=i+1;j<lines.length;j++){
        var row=parseCSVLine(lines[j]);
        if(row.length<soCol+1) continue;
        var num=row[soCol].replace(/"/g,'').trim();
        if(!num) continue;
        rows.push({num:num,species:speciesCol>=0?row[speciesCol].replace(/"/g,'').trim():'',profile:profileCol>=0?row[profileCol].replace(/"/g,'').trim():'',due:startCol>=0?row[startCol].replace(/"/g,'').trim():''});
      }
      break;
    }
  }
  if(soCol<0){lines.forEach(function(l){parseCSVLine(l).forEach(function(c){var v=c.replace(/"/g,'').trim();if(v)rows.push({num:v,species:'',profile:'',due:''}); });});}
  return rows;
}
function readTextFile(file){var r=new FileReader();r.onload=function(e){dedupeAndPreview(parseCSVRows(e.target.result));};r.readAsText(file);}
function readExcel(file){
  function parse(){
    var r=new FileReader();
    r.onload=function(e){
      try{var wb=XLSX.read(new Uint8Array(e.target.result),{type:'array'});dedupeAndPreview(parseCSVRows(XLSX.utils.sheet_to_csv(wb.Sheets[wb.SheetNames[0]])));}
      catch(err){previewHead.textContent='Could not parse. Try CSV.';previewArea.style.display='block';}
    };r.readAsArrayBuffer(file);
  }
  if(window.XLSX){parse();return;}
  var s=document.createElement('script');
  s.src='https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js';
  s.onload=parse;s.onerror=function(){previewHead.textContent='Could not load Excel parser. Try CSV.';previewArea.style.display='block';};
  document.head.appendChild(s);
}
function dedupeAndPreview(rows){
  var seen={},ordered=[];
  rows.forEach(function(r){var k=r.num.toLowerCase();if(!seen[k]){seen[k]=true;ordered.push(r);}});
  ordered.sort(function(a,b){var na=parseFloat(a.num),nb=parseFloat(b.num);if(!isNaN(na)&&!isNaN(nb))return na-nb;return a.num.localeCompare(b.num);});
  parsedOrders=ordered;previewList.innerHTML='';
  var existing={};
  DEPTS.forEach(function(d){(data[d.id]||[]).forEach(function(o){existing[o.num.toLowerCase()]=true;});});
  var newCount=0;
  ordered.forEach(function(r){
    var isDup=existing[r.num.toLowerCase()];if(!isDup)newCount++;
    var row=document.createElement('div');row.className='prev-item'+(isDup?' dup':'');
    var tag=document.createElement('span');tag.className='prev-tag '+(isDup?'tag-dup':'tag-new');tag.textContent=isDup?'exists':'new';
    var main=document.createElement('div');main.className='prev-main';
    var numEl=document.createElement('div');numEl.className='prev-num';numEl.textContent=r.num;main.appendChild(numEl);
    if(r.species||r.profile||r.due){var meta=document.createElement('div');meta.className='prev-meta';var pts=[];if(r.species)pts.push(r.species);if(r.profile)pts.push(r.profile);if(r.due)pts.push('Due: '+r.due);meta.textContent=pts.join(' · ');main.appendChild(meta);}
    row.appendChild(main);row.appendChild(tag);previewList.appendChild(row);
  });
  previewHead.textContent='Preview — '+ordered.length+' order'+(ordered.length!==1?'s':'')+' found';
  previewCount.textContent=newCount+' new · '+(ordered.length-newCount)+' already in list';
  previewArea.style.display='block';
  importBtn.disabled=newCount===0;importBtn.textContent='Import '+newCount+' new';
}
importBtn.addEventListener('click',function(){
  if(!parsedOrders.length) return;
  var toSave={};
  parsedOrders.forEach(function(r){
    ALL_IDS.forEach(function(id){
      if(!hasOrder(id,r.num)){
        if(!data[id]) data[id]=[];
        data[id].push({num:r.num,species:r.species||'',profile:r.profile||'',due:r.due||'',status:'pending',ts:Date.now()});
        toSave[id]=true;
      }
    });
  });
  saveMany(Object.keys(toSave)).then(function(){importModal.classList.remove('open');renderAll();note('Imported · '+timeNow());});
});

// ─── BOOT ──────────────────────────────────────────────────────
document.getElementById('refresh-btn').addEventListener('click',loadAll);
loadAll();
setInterval(loadAll,20000);
</script>
<!--
  README — Integrity Cut List
  SYNC RULES
  • Chop Saw adds    → all 4 depts
  • Panel Saw / Cope → Door Assembly too
  • Door Assembly ▶  → removed from Chop Saw, Panel Saw & Cope
  • ✕ Delete         → that dept only
  • Ready for Assembly tab → shows orders done in BOTH Panel Saw AND Cope

  CROSS-DEVICE SETUP
  1. console.firebase.google.com → new project → Realtime Database → test mode
  2. Copy DB URL → paste into FIREBASE_URL = '...'
  3. Host on app.netlify.com/drop → share URL with all depts
-->
</body>
</html>
