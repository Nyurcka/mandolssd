
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>LSCSO — Sheriff Command Dashboard</title>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" defer></script>

<style>
/* ============================================================
   RESET & VARIABLES
============================================================ */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root{
  --bg-base:#080c09;
  --bg-panel:#0c1009;
  --bg-card:#101510;
  --bg-row:#0d110d;
  --bg-row-hover:#141e14;
  --bg-inp:#0a0e0a;

  --gp:#4a7c59;  /* green primary */
  --gb:#5db86e;  /* green bright  */
  --gg:#3a6147;  /* green glow    */
  --ga:#2ecc71;  /* green accent  */
  --gd:#253d2e;  /* green dim     */
  --gbr:#1d3325; /* green border  */

  --tp:#cce5d2;  /* text primary   */
  --ts:#6e9479;  /* text secondary */
  --tm:#3a5442;  /* text muted     */
  --th:#9dcfa8;  /* text header    */

  --db:#0f2e1c;--df:#4ddb8a;--dbd:#2a6642; /* disponible  */
  --nb:#2e0f0f;--nf:#e87070;--nbd:#663333; /* nodisponible*/
  --tb:#0f1a2e;--tf:#70aaeb;--tbd:#2a4066; /* TAC         */
  --sb:#2b2410;--sf:#e8c96a;--sbd:#665a28; /* ST          */
  --mb:#2b1b10;--mf:#e89a4e;--mbd:#664030; /* MT          */
  --gtb:#1e1030;--gtf:#ab72e8;--gtbd:#4a2866;/* GT        */

  --r:5px;--rl:10px;
  --font:'Segoe UI',Arial,sans-serif;
  --mono:'Courier New',Consolas,monospace;
}

html,body{width:100%;min-height:100vh;background:var(--bg-base);color:var(--tp);font-family:var(--font);font-size:14px}

body{
  background-image:
    repeating-linear-gradient(0deg,transparent,transparent 48px,rgba(74,124,89,.025) 48px,rgba(74,124,89,.025) 49px),
    repeating-linear-gradient(90deg,transparent,transparent 48px,rgba(74,124,89,.025) 48px,rgba(74,124,89,.025) 49px);
}

::-webkit-scrollbar{width:6px;height:6px}
::-webkit-scrollbar-track{background:var(--bg-base)}
::-webkit-scrollbar-thumb{background:var(--gd);border-radius:3px}
::-webkit-scrollbar-thumb:hover{background:var(--gp)}

/* ============================================================
   HEADER
============================================================ */
#header{
  position:sticky;top:0;z-index:200;
  background:linear-gradient(180deg,#090d09,var(--bg-panel));
  border-bottom:2px solid var(--gbr);
  box-shadow:0 4px 32px rgba(0,0,0,.7);
}
.hdr-inner{
  display:flex;align-items:center;justify-content:space-between;
  padding:12px 28px;gap:16px;flex-wrap:wrap;
}

/* Shield */
.hdr-brand{display:flex;align-items:center;gap:14px}
.shield{
  width:50px;height:50px;flex-shrink:0;
  background:linear-gradient(135deg,var(--gp),var(--gd));
  border-radius:50%;border:2px solid var(--gb);
  display:flex;align-items:center;justify-content:center;
  animation:pulseShield 3.5s ease-in-out infinite;
}
.shield svg{width:26px;height:26px;fill:#cce5d2}
@keyframes pulseShield{
  0%,100%{box-shadow:0 0 14px rgba(74,124,89,.35)}
  50%{box-shadow:0 0 28px rgba(93,184,110,.65)}
}

.brand-title{
  font-size:24px;font-weight:800;letter-spacing:4px;
  color:var(--gb);text-transform:uppercase;line-height:1;
  text-shadow:0 0 18px rgba(93,184,110,.35);
}
.brand-sub{
  font-size:9px;letter-spacing:3px;color:var(--ts);
  text-transform:uppercase;margin-top:3px;font-family:var(--mono);
}

/* Stat chips */
.hdr-stats{display:flex;gap:10px;flex-wrap:wrap}
.chip{
  background:var(--bg-card);border:1px solid var(--gbr);border-radius:var(--r);
  padding:7px 16px;display:flex;flex-direction:column;align-items:center;
  min-width:78px;transition:border-color .3s;
}
.chip:hover{border-color:var(--gp)}
.chip .cv{font-size:22px;font-weight:800;color:var(--gb);line-height:1;font-family:var(--mono)}
.chip .cl{font-size:8px;letter-spacing:1.5px;color:var(--tm);text-transform:uppercase;margin-top:2px}

/* Right */
.hdr-right{display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.sys-badge{
  display:flex;align-items:center;gap:7px;
  background:var(--bg-card);border:1px solid var(--gbr);
  border-radius:var(--r);padding:6px 13px;
}
.sys-dot{width:7px;height:7px;background:var(--ga);border-radius:50%;animation:blink 2s ease-in-out infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.2}}
.sys-lbl{font-size:10px;letter-spacing:2px;color:var(--ts);text-transform:uppercase;font-family:var(--mono)}

#clock{font-family:var(--mono);font-size:20px;font-weight:700;letter-spacing:2px;color:var(--gb);text-shadow:0 0 12px rgba(93,184,110,.4)}

/* ============================================================
   TOOLBAR
============================================================ */
#toolbar{
  background:var(--bg-panel);border-bottom:1px solid var(--gbr);
  padding:9px 28px;display:flex;align-items:center;gap:9px;flex-wrap:wrap;
}
.tb-lbl{font-size:9px;letter-spacing:2px;color:var(--tm);text-transform:uppercase;font-family:var(--mono);margin-right:3px}
.tb-sep{width:1px;height:26px;background:var(--gbr)}
#tb-save{margin-left:auto;font-size:9px;letter-spacing:1px;color:var(--tm);font-family:var(--mono)}

/* Buttons */
.btn{
  display:inline-flex;align-items:center;gap:7px;
  padding:7px 17px;border-radius:var(--r);
  font-size:12px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;
  cursor:pointer;border:1px solid transparent;transition:all .2s;font-family:var(--font);
}
.btn-add{
  background:linear-gradient(135deg,var(--gp),var(--gg));color:#fff;border-color:var(--gb);
  box-shadow:0 2px 10px rgba(74,124,89,.3);
}
.btn-add:hover{background:linear-gradient(135deg,var(--gb),var(--gp));box-shadow:0 4px 18px rgba(74,124,89,.55);transform:translateY(-1px)}
.btn-out{background:transparent;color:var(--ts);border-color:var(--gbr)}
.btn-out:hover{background:var(--bg-card);color:var(--tp);border-color:var(--gp)}
.btn-del-g{background:rgba(80,20,20,.55);color:#e87070;border-color:#4a2020}
.btn-del-g:hover{background:rgba(120,30,30,.75);border-color:#e87070;transform:translateY(-1px);box-shadow:0 4px 12px rgba(200,60,60,.3)}

/* ============================================================
   MAIN / TABLE
============================================================ */
#main{padding:18px 28px}
.tbl-wrap{
  background:var(--bg-panel);border:1px solid var(--gbr);border-radius:var(--rl);
  overflow:hidden;box-shadow:0 4px 28px rgba(0,0,0,.55),0 0 20px rgba(74,124,89,.08);
}
.tbl-bar{
  background:linear-gradient(90deg,var(--bg-card),#0d130d);
  border-bottom:1px solid var(--gbr);padding:9px 18px;
  display:flex;align-items:center;gap:10px;
}
.tbl-bar-icon{font-size:16px}
.tbl-bar-ttl{font-size:11px;font-weight:700;letter-spacing:3px;text-transform:uppercase;color:var(--th)}
#last-save{margin-left:auto;font-size:9px;letter-spacing:1.5px;color:var(--tm);text-transform:uppercase;font-family:var(--mono)}

.tbl-scroll{overflow-x:auto}
table{width:100%;border-collapse:collapse}

thead th{
  background:var(--bg-card);border-bottom:2px solid var(--gd);
  padding:10px 11px;font-size:10px;font-weight:700;letter-spacing:2.5px;
  text-transform:uppercase;color:var(--ts);white-space:nowrap;user-select:none;
}
thead th .ico{margin-right:5px}

tbody tr{
  background:var(--bg-row);border-bottom:1px solid var(--gbr);
  transition:background .2s,box-shadow .2s;
}
tbody tr:hover{background:var(--bg-row-hover);box-shadow:inset 3px 0 0 var(--gp)}
tbody tr:last-child{border-bottom:none}
td{padding:8px 10px;vertical-align:middle}

#empty-row td{text-align:center;padding:52px 20px;color:var(--tm)}
.empty-icon{font-size:36px;display:block;margin-bottom:10px;opacity:.35}
.empty-msg{font-size:11px;letter-spacing:2px;text-transform:uppercase;opacity:.45;font-family:var(--mono)}

@keyframes rowIn{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
.row-new{animation:rowIn .3s ease forwards}

/* ============================================================
   FORM CONTROLS
============================================================ */
.fc{
  background:var(--bg-inp);color:var(--tp);border:1px solid var(--gbr);
  border-radius:var(--r);padding:5px 9px;font-size:13px;width:100%;
  transition:border-color .2s,box-shadow .2s;outline:none;font-family:var(--font);
  appearance:auto;
}
.fc:focus{border-color:var(--gp);box-shadow:0 0 0 2px rgba(74,124,89,.2)}
.fc::placeholder{color:var(--tm)}
select.fc option{background:#0e140e;color:var(--tp)}
textarea.fc{resize:vertical;min-height:54px;max-height:110px;line-height:1.45}

/* ============================================================
   ESTADO BADGE
============================================================ */
.estado-wrap{cursor:pointer}
.estado-badge{
  display:inline-flex;align-items:center;gap:6px;
  font-size:12px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;
  padding:4px 11px;border-radius:4px;border:1px solid;white-space:nowrap;user-select:none;
}
.edo-dot{width:6px;height:6px;border-radius:50%;background:currentColor;flex-shrink:0}
select.estado-sel{display:none;min-width:140px}

.eb-none{background:transparent;color:var(--tm);border-color:var(--gbr)}
.eb-disp{background:var(--db);color:var(--df);border-color:var(--dbd)}
.eb-nodis{background:var(--nb);color:var(--nf);border-color:var(--nbd)}
.eb-tac{background:var(--tb);color:var(--tf);border-color:var(--tbd)}
.eb-st{background:var(--sb);color:var(--sf);border-color:var(--sbd)}
.eb-mt{background:var(--mb);color:var(--mf);border-color:var(--mbd)}
.eb-gt{background:var(--gtb);color:var(--gtf);border-color:var(--gtbd)}

/* ============================================================
   AVISOS
============================================================ */
.avisos-wrap{position:relative}
.avisos-ta{padding-right:54px!important}
.av-cnt{
  position:absolute;top:5px;right:5px;
  background:var(--gd);color:var(--ts);border:1px solid var(--gp);border-radius:12px;
  padding:1px 7px;font-size:10px;font-family:var(--mono);
  pointer-events:none;transition:all .2s;
}
.av-cnt.on{background:rgba(46,204,113,.12);color:var(--ga);border-color:var(--ga)}

/* ============================================================
   TIMESTAMP
============================================================ */
.ts-cell{font-family:var(--mono);font-size:14px;color:var(--ts);white-space:nowrap}
.ts-age{display:block;font-size:15px;color:var(--tm);margin-top:2px}

/* Row delete button */
.btn-row-del{
  background:transparent;border:1px solid #2e1010;color:#5a2828;
  border-radius:var(--r);padding:5px 10px;cursor:pointer;
  transition:all .2s;font-size:14px;
  display:flex;align-items:center;justify-content:center;
}
.btn-row-del:hover{background:rgba(200,60,60,.15);border-color:#e87070;color:#e87070;transform:scale(1.1)}

/* ============================================================
   FOOTER
============================================================ */
#footer{
  border-top:1px solid var(--gbr);padding:9px 28px;
  display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;
  background:var(--bg-panel);
}
.foot-txt{font-size:9px;letter-spacing:1.5px;color:var(--tm);text-transform:uppercase;font-family:var(--mono)}
.foot-txt span{color:var(--gp)}

/* ============================================================
   MODAL
============================================================ */
.modal-backdrop{background:rgba(0,0,0,.75)!important}
.modal-content{
  background:var(--bg-card)!important;border:1px solid var(--gbr)!important;
  color:var(--tp)!important;
  box-shadow:0 20px 60px rgba(0,0,0,.75)!important;
}
.modal-header{border-bottom:1px solid var(--gbr)!important;padding:16px 22px!important}
.modal-title{font-size:16px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:var(--th)!important}
.modal-body{padding:22px!important;color:var(--ts)!important;font-size:14px;line-height:1.6}
.modal-body strong{color:#e87070}
.modal-footer{border-top:1px solid var(--gbr)!important;padding:12px 22px!important;display:flex;gap:8px;justify-content:flex-end}
.btn-close{filter:invert(1) opacity(.35)!important}

/* ============================================================
   TOAST
============================================================ */
#toast-area{position:fixed;bottom:22px;right:22px;z-index:9999;display:flex;flex-direction:column;gap:7px}
.toast-item{
  background:var(--bg-card);border:1px solid var(--gp);border-left:4px solid var(--gb);
  color:var(--tp);padding:10px 16px;border-radius:var(--r);
  font-size:12px;font-family:var(--mono);letter-spacing:.5px;
  box-shadow:0 4px 20px rgba(0,0,0,.5);display:flex;align-items:center;gap:9px;
  min-width:220px;max-width:300px;
  animation:tIn .3s ease,tOut .35s ease 2.65s forwards;
}
.toast-item.err{border-color:#e87070;border-left-color:#e87070}
.t-ico{font-size:16px;flex-shrink:0}
@keyframes tIn{from{transform:translateX(50px);opacity:0}to{transform:translateX(0);opacity:1}}
@keyframes tOut{from{opacity:1;transform:translateX(0)}to{opacity:0;transform:translateX(16px)}}

/* ============================================================
   RESPONSIVE
============================================================ */
@media(max-width:768px){
  .hdr-inner{padding:10px 14px}
  #toolbar{padding:9px 14px}
  #main{padding:14px}
  .hdr-stats{display:none}
  .brand-title{font-size:19px}
  #clock{font-size:16px}
}
</style>
</head>
<body>

<!-- HEADER -->
<header id="header">
  <div class="hdr-inner">

    <div class="hdr-brand">
      <div class="shield">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 1L3 5v6c0 5.25 3.75 10.15 9 11.35C17.25 21.15 21 16.25 21 11V5l-9-4zm-1 14l-3-3 1.41-1.41L11 12.17l4.59-4.58L17 9l-6 6z"/>
        </svg>
      </div>
      <div>
        <div class="brand-title">LSCSO</div>
        <div class="brand-sub">Los Santos County Sheriff's Office &bull; Command Dashboard</div>
      </div>
    </div>

    <div class="hdr-stats">
      <div class="chip"><span class="cv" id="stat-u">0</span><span class="cl">Unidades</span></div>
      <div class="chip"><span class="cv" id="stat-d">0</span><span class="cl">Disponibles</span></div>
      <div class="chip"><span class="cv" id="stat-a">0</span><span class="cl">Avisos</span></div>
    </div>

    <div class="hdr-right">
      <div class="sys-badge">
        <div class="sys-dot"></div>
        <span class="sys-lbl">Sistema Activo</span>
      </div>
      <div id="clock">00:00:00</div>
    </div>

  </div>
</header>

<!-- TOOLBAR -->
<div id="toolbar">
  <span class="tb-lbl">Gestion</span>
  <button class="btn btn-add" onclick="addRow()">+ Anadir Unidad</button>
  <div class="tb-sep"></div>
  <button class="btn btn-out" onclick="saveNow()">&#128190; Guardar</button>
  <button class="btn btn-del-g" onclick="confirmClear()">&#128465; Limpiar Todo</button>
  <span id="tb-save"></span>
</div>

<!-- MAIN -->
<div id="main">
  <div class="tbl-wrap">
    <div class="tbl-bar">
      <span class="tbl-bar-icon">&#9783;</span>
      <span class="tbl-bar-ttl">Registro de Unidades en Servicio</span>
      <span id="last-save">SIN GUARDAR</span>
    </div>
    <div class="tbl-scroll">
      <table>
        <thead>
          <tr>
            <th><span class="ico">&#128225;</span>Malla</th>
            <th><span class="ico">&#128100;</span>Unidad</th>
            <th><span class="ico">&#9679;</span>Estado</th>
            <th><span class="ico">&#128680;</span>Avisos Acudidos</th>
            <th><span class="ico">&#128203;</span>Notas</th>
            <th><span class="ico">&#128336;</span>Ult. Actualizacion</th>
            <th></th>
          </tr>
        </thead>
        <tbody id="tbody">
          <tr id="empty-row">
            <td colspan="7">
              <span class="empty-icon">&#128737;</span>
              <span class="empty-msg">Sin unidades en servicio &mdash; Aniade una unidad para comenzar</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>

<!-- FOOTER -->
<footer id="footer">
  <span class="foot-txt">LSCSO &bull; <span>Command Dashboard</span> &bull; v2.1</span>
  <span class="foot-txt">Datos en <span>localStorage</span> &bull; Recarga segura</span>
</footer>

<!-- MODAL -->
<div class="modal fade" id="modalClear" tabindex="-1" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">&#9888; Confirmar Borrado</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">
        Seguro que deseas <strong>eliminar todas las unidades</strong>?<br>
        Esta accion borrara todos los datos y no se puede deshacer.
      </div>
      <div class="modal-footer">
        <button class="btn btn-out" data-bs-dismiss="modal">Cancelar</button>
        <button class="btn btn-del-g" onclick="clearAll()">&#128465; Si, limpiar todo</button>
      </div>
    </div>
  </div>
</div>

<div id="toast-area"></div>

<script>
/* ===== CONFIG ===== */
const MALLAS=[
  '',
  ...[10,20,30,40,50,60,70,80,90,100].map(n=>`Alpha ${n}`),
  ...[10,20,30,40,50,60,70,80,90,100].map(n=>`Bravo ${n}`),
  ...[10,20,30].map(n=>`Charlie ${n}`),
  'Ocelot 10','Ocelot 20','Bike 10','Bike 20','Yankee','Roger',
  'Foxtrot 10','Foxtrot 20','Mike 10','Mike 20',
  'Dogma 10','Dogma 20','Victor 10','Victor 20',
  'Tiger 10','Tiger 20','Ranger','Shell','PIA'
];

const ESTADOS={
  '':          {lbl:'-- Estado --', cls:'eb-none'},
  'disponible':{lbl:'Disponible',   cls:'eb-disp'},
  'nodis':     {lbl:'No Disponible',cls:'eb-nodis'},
  'tac':       {lbl:'TAC',          cls:'eb-tac'},
  'st':        {lbl:'ST',           cls:'eb-st'},
  'mt':        {lbl:'MT',           cls:'eb-mt'},
  'gt':        {lbl:'GT',           cls:'eb-gt'},
};

/* ===== STATE ===== */
let rows=[], uid=0;
const SK='lscso_v3';

/* ===== CLOCK ===== */
function tick(){
  const n=new Date(),p=v=>String(v).padStart(2,'0');
  document.getElementById('clock').textContent=`${p(n.getHours())}:${p(n.getMinutes())}:${p(n.getSeconds())}`;
}
setInterval(tick,1000);tick();

/* ===== TIME HELPERS ===== */
function fmtTime(ts){
  if(!ts)return'--:--';
  const d=new Date(ts);
  return String(d.getHours()).padStart(2,'0')+':'+String(d.getMinutes()).padStart(2,'0');
}
function ago(ts){
  if(!ts)return'';
  const s=Math.floor((Date.now()-ts)/1000);
  if(s<60)return`hace ${s}s`;
  const m=Math.floor(s/60);
  if(m<60)return`hace ${m}m`;
  const h=Math.floor(m/60),rm=m%60;
  return`hace ${h}h ${rm}m`;
}

/* ===== AVISO COUNT ===== */
function cntAv(t){if(!t||!t.trim())return 0;return t.split(/[\n,]+/).map(s=>s.trim()).filter(Boolean).length}

/* ===== STATS ===== */
function stats(){
  document.getElementById('stat-u').textContent=rows.length;
  document.getElementById('stat-d').textContent=rows.filter(r=>r.estado==='disponible').length;
  document.getElementById('stat-a').textContent=rows.reduce((a,r)=>a+cntAv(r.avisos),0);
}

/* ===== EMPTY ROW ===== */
function refreshEmpty(){
  const e=document.getElementById('empty-row');
  if(e)e.style.display=rows.length===0?'':'none';
}

/* ===== HTML BUILDERS ===== */
function mallaOpts(sel){
  return MALLAS.map(o=>`<option value="${o}"${o===sel?' selected':''}>${o||'-- Malla --'}</option>`).join('');
}

function estadoBadge(e){
  const cfg=ESTADOS[e]||ESTADOS[''];
  const dot=e?`<span class="edo-dot"></span>`:'';
  return `<span class="estado-badge ${cfg.cls}">${dot}${cfg.lbl}</span>`;
}

function estadoOpts(sel){
  return Object.entries(ESTADOS).map(([k,v])=>`<option value="${k}"${k===sel?' selected':''}>${v.lbl}</option>`).join('');
}

/* ===== ESCAPE ===== */
function esc(s){return(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;')}

/* ===== ADD ROW ===== */
function addRow(data){
  const id=++uid;
  const r=data?{...data,id}:{id,malla:'',unidad:'',estado:'',avisos:'',notas:'',updatedAt:null};
  rows.push(r);

  const ac=cntAv(r.avisos);
  const tr=document.createElement('tr');
  tr.id=`row-${id}`;
  tr.className='row-new';
  tr.innerHTML=`
    <td style="min-width:138px">
      <select class="fc" onchange="onMalla(this)">${mallaOpts(r.malla)}</select>
    </td>
    <td style="min-width:120px">
      <input class="fc" type="text" placeholder="Agente..." value="${esc(r.unidad)}" oninput="onUnidad(this)"/>
    </td>
    <td style="min-width:155px">
      <div class="estado-wrap" onclick="toggleEstado(this)">
        ${estadoBadge(r.estado)}
        <select class="fc estado-sel" onchange="onEstado(this)">${estadoOpts(r.estado)}</select>
      </div>
    </td>
    <td style="min-width:175px">
      <div class="avisos-wrap">
        <textarea class="fc avisos-ta" rows="2" placeholder="Joyeria, AMMU, mina..." oninput="onAvisos(this)">${esc(r.avisos)}</textarea>
        <span class="av-cnt${ac>0?' on':''}">${ac>0?ac+' av.':'0'}</span>
      </div>
    </td>
    <td style="min-width:145px">
      <textarea class="fc" rows="2" placeholder="Notas..." oninput="onNotas(this)">${esc(r.notas)}</textarea>
    </td>
    <td class="ts-cell" style="min-width:130px">
      <span class="ts-time">${fmtTime(r.updatedAt)}</span>
      <span class="ts-age">${ago(r.updatedAt)}</span>
    </td>
    <td style="width:46px">
      <button class="btn-row-del" title="Eliminar fila" onclick="deleteRow(this)">&#10005;</button>
    </td>
  `;
  document.getElementById('tbody').appendChild(tr);
  refreshEmpty();stats();
  if(!data){saveData();toast('Unidad anadida','&#10010;');}
}

/* ===== GET ROW DATA ===== */
function getRow(el){
  const id=parseInt(el.closest('tr').id.replace('row-',''));
  return rows.find(r=>r.id===id);
}

/* ===== HANDLERS ===== */
function onMalla(s){const r=getRow(s);if(r){r.malla=s.value;saveData();stats();}}
function onUnidad(i){const r=getRow(i);if(r){r.unidad=i.value;saveData();}}

function toggleEstado(wrap){
  const badge=wrap.querySelector('.estado-badge');
  const sel=wrap.querySelector('.estado-sel');
  if(!sel)return;
  badge.style.display='none';
  sel.style.display='block';
  sel.focus();
  sel.onblur=()=>{sel.style.display='none';badge.style.display='';};
}

function onEstado(sel){
  const r=getRow(sel);if(!r)return;
  r.estado=sel.value;r.updatedAt=Date.now();

  const wrap=sel.closest('.estado-wrap');
  const badge=wrap.querySelector('.estado-badge');
  badge.outerHTML=estadoBadge(r.estado);
  wrap.onclick=()=>toggleEstado(wrap);

  const tr=sel.closest('tr');
  const tst=tr.querySelector('.ts-time');
  const tsa=tr.querySelector('.ts-age');
  if(tst)tst.textContent=fmtTime(r.updatedAt);
  if(tsa)tsa.textContent=ago(r.updatedAt);

  saveData();stats();
  toast(`Estado: ${ESTADOS[r.estado]?.lbl||'--'}`,'&#9679;');
}

function onAvisos(ta){
  const r=getRow(ta);if(!r)return;
  r.avisos=ta.value;
  const cnt=ta.closest('.avisos-wrap').querySelector('.av-cnt');
  const n=cntAv(ta.value);
  cnt.textContent=n>0?n+' av.':'0';
  cnt.className='av-cnt'+(n>0?' on':'');
  saveData();stats();
}

function onNotas(ta){const r=getRow(ta);if(r){r.notas=ta.value;saveData();}}

/* ===== DELETE ROW ===== */
function deleteRow(btn){
  const tr=btn.closest('tr');
  const id=parseInt(tr.id.replace('row-',''));
  rows=rows.filter(r=>r.id!==id);
  tr.style.cssText='transition:opacity .25s,transform .25s;opacity:0;transform:translateX(18px)';
  setTimeout(()=>{tr.remove();refreshEmpty();stats();saveData();},260);
  toast('Unidad eliminada','&#10006;',true);
}

/* ===== CLEAR ALL ===== */
function confirmClear(){new bootstrap.Modal(document.getElementById('modalClear')).show();}
function clearAll(){
  bootstrap.Modal.getInstance(document.getElementById('modalClear')).hide();
  rows=[];
  document.getElementById('tbody').innerHTML=`
    <tr id="empty-row">
      <td colspan="7">
        <span class="empty-icon">&#128737;</span>
        <span class="empty-msg">Sin unidades en servicio &mdash; Aniade una unidad para comenzar</span>
      </td>
    </tr>`;
  saveData();stats();
  toast('Dashboard limpiado','&#128465;',true);
}

/* ===== PERSISTENCE ===== */
function syncDOM(){
  rows.forEach(r=>{
    const tr=document.getElementById(`row-${r.id}`);if(!tr)return;
    const inp=tr.querySelector('input.fc');
    const tas=tr.querySelectorAll('textarea.fc');
    const msl=tr.querySelectorAll('select.fc')[0];
    const esl=tr.querySelector('select.estado-sel');
    if(inp)r.unidad=inp.value;
    if(tas[0])r.avisos=tas[0].value;
    if(tas[1])r.notas=tas[1].value;
    if(msl)r.malla=msl.value;
    if(esl)r.estado=esl.value;
  });
}

function saveData(){
  syncDOM();
  localStorage.setItem(SK,JSON.stringify({rows,uid}));
  const n=new Date(),p=v=>String(v).padStart(2,'0');
  const ts=`${p(n.getHours())}:${p(n.getMinutes())}:${p(n.getSeconds())}`;
  const lbl=`Guardado ${ts}`;
  document.getElementById('last-save').textContent=lbl;
  document.getElementById('tb-save').textContent=lbl;
}

function saveNow(){saveData();toast('Datos guardados','&#128190;');}

function loadData(){
  try{
    const raw=localStorage.getItem(SK);if(!raw)return;
    const d=JSON.parse(raw);
    if(d&&Array.isArray(d.rows)){
      uid=d.uid||0;
      d.rows.forEach(r=>addRow(r));
      document.getElementById('last-save').textContent='Datos restaurados';
    }
  }catch(e){console.warn('LSCSO load err',e);}
}

/* ===== PERIODIC ===== */
setInterval(saveData,30000);
setInterval(()=>{
  rows.forEach(r=>{
    const tr=document.getElementById(`row-${r.id}`);if(!tr||!r.updatedAt)return;
    const a=tr.querySelector('.ts-age');if(a)a.textContent=ago(r.updatedAt);
  });
},12000);

/* ===== TOAST ===== */
function toast(msg,ico='&#9432;',err=false){
  const area=document.getElementById('toast-area');
  const d=document.createElement('div');
  d.className='toast-item'+(err?' err':'');
  d.innerHTML=`<span class="t-ico" style="color:${err?'#e87070':'var(--gb)'}">${ico}</span>${msg}`;
  area.appendChild(d);
  setTimeout(()=>d.remove(),3200);
}

/* ===== INIT ===== */
document.addEventListener('DOMContentLoaded',()=>{loadData();stats();refreshEmpty();});
</script>
</body>
</html>
