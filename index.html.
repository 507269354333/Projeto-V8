<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#101114">
<title>Projeto V8 — Performance & Evolução</title>
<style>
:root{--bg:#0b0c0f;--panel:#17191f;--panel2:#20232b;--line:#383c47;--txt:#f4f4f5;--muted:#a4a7b0;--green:#ff3b30;--green2:#b51f1a;--red:#ff7169;--yellow:#f2bd55}
*{box-sizing:border-box}body{margin:0;background:radial-gradient(ellipse at 50% -15%,#3b1516 0%,#151116 38%,#0b0c0f 76%);color:var(--txt);font:15px/1.45 system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif}
.wrap{max-width:1050px;margin:auto;padding:20px 16px 48px}.top{display:flex;align-items:center;justify-content:space-between;gap:14px;margin-bottom:18px}.brand{font-weight:900;letter-spacing:.12em;font-size:13px;color:#ff5a50;text-transform:uppercase}h1{font-size:clamp(27px,5vw,42px);line-height:1.05;margin:7px 0}.sub{color:var(--muted);font-size:13px}.tag{border:1px solid #8f302d;color:#ff6b62;padding:6px 10px;border-radius:99px;font-size:12px;white-space:nowrap}
nav{display:flex;gap:8px;overflow:auto;padding:5px 0 14px}nav button{white-space:nowrap;border:1px solid var(--line);background:var(--panel);color:var(--muted);padding:10px 14px;border-radius:12px;cursor:pointer}nav button.active{background:var(--green);color:#fff;border-color:var(--green);font-weight:800}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}.card{background:linear-gradient(160deg,var(--panel2),var(--panel));border:1px solid var(--line);border-radius:17px;padding:16px;min-width:0}.metric small{display:block;color:var(--muted);font-size:12px}.metric strong{font-size:clamp(23px,4vw,32px);display:block;margin:4px 0}.metric em{font-style:normal;color:var(--green);font-size:12px}.section{display:none}.section.active{display:block}.section-title{font-size:20px;margin:18px 0 12px}.progress{height:9px;background:#29372f;border-radius:99px;overflow:hidden;margin:10px 0}.progress span{display:block;height:100%;background:linear-gradient(90deg,#a51d1a,#ff5148);width:0}
.row{display:flex;gap:10px;flex-wrap:wrap}.row>*{flex:1;min-width:150px}.field{display:flex;flex-direction:column;gap:5px;margin:9px 0}.field label{font-size:12px;color:var(--muted)}input,select,textarea{width:100%;background:#0e1712;border:1px solid var(--line);border-radius:10px;padding:11px;color:var(--txt);font:inherit}input:focus,select:focus,textarea:focus{outline:2px solid #477b58;border-color:var(--green)}
button.primary{background:linear-gradient(135deg,#ff5148,#bd211d);color:#fff;border:0;font-weight:800;padding:12px 16px;border-radius:11px;cursor:pointer}button.secondary,.btn{background:#2b2024;border:1px solid #693238;color:var(--txt);padding:10px 13px;border-radius:10px;cursor:pointer}button.danger{background:#3a211e;border:1px solid #70423b;color:#ffd0c9;padding:8px 10px;border-radius:9px;cursor:pointer}
.actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}.muted{color:var(--muted)}.small{font-size:12px}.notice{background:#241b20;border-left:3px solid var(--green);padding:11px 13px;border-radius:8px;color:#c9d8ce;font-size:13px;margin:12px 0}
table{width:100%;border-collapse:collapse;font-size:13px}th,td{text-align:left;padding:10px 8px;border-bottom:1px solid var(--line)}th{color:var(--muted);font-weight:600}.tablewrap{overflow:auto}
.day{padding:12px;border:1px solid var(--line);border-radius:12px;margin:8px 0;background:#17191f}.day h3{margin:0 0 8px;font-size:15px}.exercise{display:grid;grid-template-columns:1fr auto;gap:8px;padding:7px 0;border-top:1px solid #27362e}.exercise span:last-child{color:var(--green);white-space:nowrap}
.empty{padding:18px;text-align:center;color:var(--muted);border:1px dashed var(--line);border-radius:12px}
.barlist{display:grid;gap:8px}.barrow{display:grid;grid-template-columns:82px 1fr 50px;gap:8px;align-items:center;font-size:12px}.bar{height:9px;background:#29372f;border-radius:99px;overflow:hidden}.bar i{display:block;height:100%;background:var(--green)}
footer{color:var(--muted);font-size:11px;margin-top:22px}
@media(max-width:700px){.grid{grid-template-columns:repeat(2,1fr)}.top{align-items:flex-start}.tag{font-size:10px}.wrap{padding:16px 12px 36px}.card{padding:13px}th,td{padding:8px 6px}}
.top{position:relative;padding:18px 18px 20px;border:1px solid #39313a;border-radius:20px;background:linear-gradient(115deg,rgba(33,25,31,.96),rgba(18,20,26,.94));box-shadow:0 16px 40px #0008;overflow:hidden}.top:after{content:"V8";position:absolute;right:18px;bottom:-34px;font-size:clamp(100px,19vw,180px);font-weight:1000;letter-spacing:-.1em;color:#ffffff08;font-style:italic;pointer-events:none}.brand:before{content:"◢ ";color:#ff3b30}.tag{background:#23191e;letter-spacing:.08em;font-weight:800}.metric strong{font-variant-numeric:tabular-nums}.progress{background:#343039}.progress span,.bar i{background:linear-gradient(90deg,#b31e1a,#ff5148)}button.primary{box-shadow:0 5px 18px #ff3b3030}nav button.active{box-shadow:0 5px 18px #ff3b3030}@media(max-width:600px){.top{padding:16px}.top:after{right:10px;bottom:-20px;font-size:100px}}
</style>
</head>
<body>
<div class="wrap">
<header class="top"><div><div class="brand">PROJETO V8 / PERFORMANCE CLUB</div><h1>Alta performance. <span style="color:var(--green)">Sua evolução.</span></h1><div class="sub">Primeira etapa: 105 kg → 95 kg até 20/12/2026 · Meta final: 80 kg</div></div><div class="tag">DRIVER PERFORMANCE</div></header>
<nav>
<button class="active" data-tab="painel">Visão geral</button><button data-tab="registrar">Registrar treino</button><button data-tab="peso">Peso & medidas</button><button data-tab="nutricao">Nutrição</button><button data-tab="historico">Histórico</button><button data-tab="plano">Plano semanal</button>
</nav>
<section id="painel" class="section active">
<div class="grid">
<div class="card metric"><small>Peso mais recente</small><strong id="dashWeight">105,0 kg</strong><em id="weightDelta">Ponto de partida</em></div>
<div class="card metric"><small>Meta de dezembro</small><strong>95 kg</strong><em id="to95">Faltam 10 kg</em></div>
<div class="card metric"><small>Meta final</small><strong>80 kg</strong><em id="to80">Faltam 25 kg</em></div>
<div class="card metric"><small>Treinos registrados</small><strong id="dashWorkouts">0</strong><em>desde o primeiro registro</em></div>
</div>
<div class="card" style="margin-top:12px"><div class="row" style="align-items:center"><div><b>Progresso até 95 kg</b><div class="sub">A primeira etapa do projeto</div></div><strong id="progressText" style="color:var(--green)">0%</strong></div><div class="progress"><span id="progressBar"></span></div><div class="row"><span class="muted small">105 kg</span><span class="muted small" style="text-align:right">95 kg</span></div></div>
<div class="grid" style="margin-top:12px">
<div class="card metric"><small>Calorias de exercício (estim.)</small><strong id="dashKcal">0</strong><em>kcal registradas</em></div>
<div class="card metric"><small>Minutos de cardio</small><strong id="dashCardio">0</strong><em>minutos registrados</em></div>
<div class="card metric"><small>Calorias ingeridas hoje</small><strong id="dashFood">—</strong><em>meta: 2.350 kcal</em></div>
<div class="card metric"><small>Proteína hoje</small><strong id="dashProtein">—</strong><em>referência: 180 g</em></div>
</div>
<h2 class="section-title">Últimos 7 dias</h2><div class="card"><div id="weeklyBars" class="barlist"><div class="empty">Registre seu primeiro treino para visualizar a atividade.</div></div></div>
<div class="notice">Calorias de exercício são estimativas, não uma medida exata do gasto. Não “coma de volta” automaticamente as calorias indicadas pela bicicleta ou pelo aplicativo.</div>
</section>

<section id="registrar" class="section">
<h2 class="section-title">Registrar atividade</h2>
<div class="card">
<div class="row">
<div class="field"><label>Data</label><input id="wDate" type="date"></div>
<div class="field"><label>Tipo de atividade</label><select id="wType"><option>Musculação</option><option>Bicicleta</option><option>Caminhada</option><option>Elíptico</option><option>Outro cardio</option></select></div>
</div>
<div class="row">
<div class="field"><label>Nome / foco do treino</label><input id="wName" placeholder="Ex.: Treino A, bicicleta contínua"></div>
<div class="field"><label>Duração total (minutos)</label><input id="wDuration" type="number" min="0" placeholder="Ex.: 60"></div>
</div>
<div id="strengthFields">
<div class="row">
<div class="field"><label>Exercício</label><input id="wExercise" placeholder="Ex.: Leg press"></div>
<div class="field"><label>Séries</label><input id="wSets" type="number" min="0" placeholder="3"></div>
<div class="field"><label>Repetições por série</label><input id="wReps" type="number" min="0" placeholder="12"></div>
<div class="field"><label>Carga (kg)</label><input id="wLoad" type="number" min="0" step="0.5" placeholder="Ex.: 60"></div>
</div>
</div>
<div id="cardioFields" style="display:none">
<div class="row">
<div class="field"><label>Distância (km), se disponível</label><input id="wDistance" type="number" min="0" step="0.1" placeholder="Ex.: 12.5"></div>
<div class="field"><label>Intensidade percebida (0–10)</label><input id="wRpe" type="number" min="0" max="10" placeholder="Ex.: 5"></div>
</div>
</div>
<div class="row">
<div class="field"><label>Calorias exibidas no aparelho (opcional)</label><input id="wMachineKcal" type="number" min="0" placeholder="Deixe em branco se não souber"></div>
<div class="field"><label>Observações</label><input id="wNotes" placeholder="Ex.: carga confortável, sem dor"></div>
</div>
<div class="notice">Se não informar calorias, a plataforma estima o gasto por duração, peso corporal e MET aproximado. É uma estimativa com margem de erro; o valor do aparelho também pode variar.</div>
<button class="primary" id="saveWorkout">Salvar atividade</button>
</div>
<h2 class="section-title">Registrar alimentação de hoje</h2>
<div class="card">
<div class="row">
<div class="field"><label>Calorias consumidas (kcal)</label><input id="foodKcal" type="number" min="0" placeholder="Ex.: 750"></div>
<div class="field"><label>Proteína consumida (g)</label><input id="foodProtein" type="number" min="0" placeholder="Ex.: 55"></div>
</div>
<div class="field"><label>Observação / refeição</label><input id="foodNote" placeholder="Ex.: almoço"></div>
<button class="primary" id="saveFood">Adicionar alimentação</button>
</div>
</section>

<section id="peso" class="section">
<h2 class="section-title">Peso e medidas corporais</h2>
<div class="card">
<div class="row">
<div class="field"><label>Data</label><input id="bDate" type="date"></div>
<div class="field"><label>Peso (kg)</label><input id="bWeight" type="number" step="0.1" min="30" placeholder="105"></div>
<div class="field"><label>Cintura (cm) — opcional</label><input id="bWaist" type="number" step="0.1" min="0" placeholder="Ex.: 108"></div>
</div>
<div class="field"><label>Observações</label><input id="bNotes" placeholder="Ex.: medida ao acordar"></div>
<button class="primary" id="saveBody">Salvar medição</button>
</div>
<h2 class="section-title">Evolução do peso</h2><div class="card"><div id="weightChart" class="empty">Adicione medições para construir seu histórico.</div></div>
<h2 class="section-title">Registros</h2><div class="card tablewrap"><table><thead><tr><th>Data</th><th>Peso</th><th>Cintura</th><th></th></tr></thead><tbody id="bodyRows"></tbody></table></div>
</section>

<section id="nutricao" class="section">
<h2 class="section-title">Metas nutricionais de referência</h2>
<div class="grid">
<div class="card metric"><small>Meta calórica</small><strong>2.350</strong><em>kcal/dia (inicial)</em></div>
<div class="card metric"><small>Proteína</small><strong>180 g</strong><em>por dia</em></div>
<div class="card metric"><small>Carboidratos</small><strong>238 g</strong><em>referência</em></div>
<div class="card metric"><small>Gorduras</small><strong>75 g</strong><em>referência</em></div>
</div>
<h2 class="section-title">Consumo de hoje</h2><div class="card">
<div class="row"><div><small class="muted">Calorias registradas</small><h2 id="foodTodayKcal">0 kcal</h2></div><div><small class="muted">Proteína registrada</small><h2 id="foodTodayProtein">0 g</h2></div></div>
<div class="progress"><span id="foodProgress"></span></div><div class="sub" id="foodRemaining">2.350 kcal de referência</div>
</div>
<h2 class="section-title">Lançamentos alimentares</h2><div class="card tablewrap"><table><thead><tr><th>Hora/data</th><th>Descrição</th><th>Kcal</th><th>Proteína</th><th></th></tr></thead><tbody id="foodRows"></tbody></table></div>
<div class="notice">As metas são referências iniciais calculadas a partir dos dados informados. Ajuste com nutricionista, especialmente se houver condições clínicas ou restrições alimentares.</div>
</section>

<section id="historico" class="section">
<h2 class="section-title">Histórico de atividades</h2><div class="card tablewrap"><table><thead><tr><th>Data</th><th>Atividade</th><th>Duração</th><th>Kcal estimadas</th><th>Detalhes</th><th></th></tr></thead><tbody id="workoutRows"></tbody></table></div>
<div class="actions"><button class="secondary" id="exportData">Exportar meus dados (JSON)</button><label class="btn" style="display:inline-flex;align-items:center;gap:6px">Importar backup<input id="importData" type="file" accept="application/json" style="display:none"></label><button class="danger" id="clearData">Apagar todos os dados</button></div>
</section>

<section id="plano" class="section">
<h2 class="section-title">Plano semanal sugerido</h2>
<div class="notice">Use como referência e respeite a liberação médica específica para musculação, bicicleta e esforço abdominal. Interrompa se houver dor na região operada, sangramento ou abertura da ferida.</div>
<div id="planDays"></div>
</section>
<footer>Projeto V8 · Dados salvos localmente neste navegador/dispositivo. Faça backups em “Histórico” regularmente. A plataforma não substitui avaliação médica, nutricional ou monitor cardíaco.</footer>
</div>
<script>
const KEY='projeto80_v1';
const initial={profile:{startWeight:105,goalStage:95,goalFinal:80,kcal:2350,protein:180},workouts:[],body:[{date:'2026-11-04',weight:105,waist:'',notes:'Peso inicial informado'}],food:[]};
let db=load();
function load(){try{return JSON.parse(localStorage.getItem(KEY))||structuredClone(initial)}catch(e){return structuredClone(initial)}}
function save(){localStorage.setItem(KEY,JSON.stringify(db));render()}
function today(){return new Date().toISOString().slice(0,10)}
function fmt(n,d=0){return Number(n||0).toLocaleString('pt-BR',{minimumFractionDigits:d,maximumFractionDigits:d})}
function esc(s){return String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
document.querySelectorAll('nav button').forEach(b=>b.addEventListener('click',()=>{document.querySelectorAll('nav button').forEach(x=>x.classList.toggle('active',x===b));document.querySelectorAll('.section').forEach(x=>x.classList.toggle('active',x.id===b.dataset.tab));render()}));
document.getElementById('wDate').value=today();document.getElementById('bDate').value=today();
document.getElementById('wType').addEventListener('change',e=>{let cardio=e.target.value!=='Musculação';document.getElementById('strengthFields').style.display=cardio?'none':'block';document.getElementById('cardioFields').style.display=cardio?'block':'none'});
function estimateKcal(type,duration,weight,rpe){
 const met=type==='Bicicleta'? (rpe>=7?7.5:rpe>=5?6:4.5):type==='Caminhada'? (rpe>=6?4.5:3.5):type==='Elíptico'? (rpe>=7?7:5):type==='Musculação'?3.5:4;
 return Math.round(met*3.5*weight/200*duration);
}
document.getElementById('saveWorkout').onclick=()=>{
 const type=document.getElementById('wType').value,dur=Number(document.getElementById('wDuration').value);
 if(!dur||dur<=0){alert('Informe a duração da atividade.');return}
 const weight=db.body.length?Number([...db.body].sort((a,b)=>b.date.localeCompare(a.date))[0].weight):105;
 const machine=document.getElementById('wMachineKcal').value;
 const item={id:Date.now(),date:document.getElementById('wDate').value||today(),type,name:document.getElementById('wName').value||type,duration:dur,exercise:document.getElementById('wExercise').value,sets:document.getElementById('wSets').value,reps:document.getElementById('wReps').value,load:document.getElementById('wLoad').value,distance:document.getElementById('wDistance').value,rpe:Number(document.getElementById('wRpe').value||0),kcal:machine?Number(machine):estimateKcal(type,dur,weight,Number(document.getElementById('wRpe').value||0)),kcalSource:machine?'Aparelho':'Estimativa MET',notes:document.getElementById('wNotes').value};
 db.workouts.push(item);save();['wName','wDuration','wExercise','wSets','wReps','wLoad','wDistance','wRpe','wMachineKcal','wNotes'].forEach(id=>document.getElementById(id).value='');alert('Atividade salva!');
};
document.getElementById('saveBody').onclick=()=>{
 const weight=Number(document.getElementById('bWeight').value);if(!weight||weight<30){alert('Informe um peso válido.');return}
 db.body.push({id:Date.now(),date:document.getElementById('bDate').value||today(),weight,waist:document.getElementById('bWaist').value,notes:document.getElementById('bNotes').value});save();document.getElementById('bWeight').value='';document.getElementById('bWaist').value='';document.getElementById('bNotes').value='';alert('Medição salva!');
};
document.getElementById('saveFood').onclick=()=>{
 const kcal=Number(document.getElementById('foodKcal').value||0),protein=Number(document.getElementById('foodProtein').value||0);
 if(!kcal&&!protein){alert('Informe calorias ou proteína.');return}
 db.food.push({id:Date.now(),date:today(),note:document.getElementById('foodNote').value||'Registro alimentar',kcal,protein});save();['foodKcal','foodProtein','foodNote'].forEach(id=>document.getElementById(id).value='');
};
function del(arr,id){if(confirm('Excluir este registro?')){db[arr]=db[arr].filter(x=>x.id!==id);save()}}
document.getElementById('exportData').onclick=()=>{let blob=new Blob([JSON.stringify(db,null,2)],{type:'application/json'}),a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download='projeto-v8-backup-'+today()+'.json';a.click();URL.revokeObjectURL(a.href)};
document.getElementById('importData').onchange=async e=>{try{let data=JSON.parse(await e.target.files[0].text());if(!data.workouts||!data.body||!data.food)throw Error();if(confirm('Importar backup e substituir os dados atuais?')){db=data;save();alert('Backup importado.')}}catch(err){alert('Arquivo de backup inválido.')}e.target.value=''};
document.getElementById('clearData').onclick=()=>{if(confirm('Tem certeza? Todos os registros locais serão apagados. Exporte um backup antes.')){db=structuredClone(initial);save()}};
const plan=[
['SEGUNDA · MUSCULAÇÃO A',[['Leg press 45°','3 × 10–12'],['Chest press','3 × 10–12'],['Puxada frontal','3 × 10–12'],['Cadeira extensora','2 × 12–15'],['Remada baixa','2 × 12'],['Elevação lateral','2 × 12–15'],['Tríceps na polia','2 × 12']],'Bicicleta leve/moderada · 20 min'],
['TERÇA · CARDIO',['Bicicleta ergométrica · 45 min','Ritmo moderado, conversa possível'],'Aquecimento e desaceleração incluídos · 5 min cada'],
['QUARTA · MUSCULAÇÃO B',[['Mesa flexora','3 × 12'],['Supino inclinado máquina','3 × 10–12'],['Remada sentada','3 × 10–12'],['Cadeira abdutora','2 × 15'],['Desenvolvimento máquina','2 × 12'],['Rosca bíceps','2 × 12'],['Panturrilha sentada','2 × 15']],'Bicicleta leve/moderada · 20 min'],
['QUINTA · CARDIO',['Bicicleta ergométrica · 45 min','Ritmo moderado, constante'],'Aquecimento e volta à calma · 5 min cada'],
['SEXTA · MUSCULAÇÃO C',[['Leg press','3 × 12'],['Puxada frontal','3 × 12'],['Chest press','3 × 12'],['Mesa flexora','3 × 12'],['Elevação lateral','2 × 15'],['Rosca bíceps','2 × 12'],['Tríceps na polia','2 × 12']],'Bicicleta leve/moderada · 20 min'],
['SÁBADO · CARDIO PROLONGADO',['Bicicleta ergométrica · 50–60 min','Intensidade moderada'],'Aquecimento e desaceleração · 5 min cada'],
['DOMINGO · RECUPERAÇÃO',['Descanso','Mobilidade leve opcional'],'Sem cardio obrigatório']
];
document.getElementById('planDays').innerHTML=plan.map(d=>`<div class="day"><h3>${d[0]}</h3>${d[1].map(x=>Array.isArray(x)?`<div class="exercise"><span>${x[0]}</span><span>${x[1]}</span></div>`:`<div class="exercise"><span>${x}</span><span></span></div>`).join('')}<div class="sub" style="margin-top:8px">${d[2]}</div></div>`).join('');
function render(){
 const latest=[...db.body].sort((a,b)=>b.date.localeCompare(a.date))[0]||{weight:105},w=Number(latest.weight),loss=105-w,progress=Math.max(0,Math.min(100,loss/10*100));
 document.getElementById('dashWeight').textContent=fmt(w,1)+' kg';document.getElementById('weightDelta').textContent=loss>0?`−${fmt(loss,1)} kg desde o início`:loss<0?`+${fmt(-loss,1)} kg desde o início`:'Ponto de partida';
 document.getElementById('to95').textContent=w>95?`Faltam ${fmt(w-95,1)} kg`: 'Meta de dezembro alcançada';document.getElementById('to80').textContent=w>80?`Faltam ${fmt(w-80,1)} kg`:'Meta final alcançada';
 document.getElementById('dashWorkouts').textContent=db.workouts.length;document.getElementById('progressText').textContent=fmt(progress)+'%';document.getElementById('progressBar').style.width=progress+'%';
 const kcal=db.workouts.reduce((s,x)=>s+Number(x.kcal||0),0),cardio=db.workouts.filter(x=>x.type!=='Musculação').reduce((s,x)=>s+Number(x.duration||0),0);
 document.getElementById('dashKcal').textContent=fmt(kcal);document.getElementById('dashCardio').textContent=fmt(cardio);
 const foods=db.food.filter(x=>x.date===today()),fk=foods.reduce((s,x)=>s+Number(x.kcal||0),0),fp=foods.reduce((s,x)=>s+Number(x.protein||0),0);
 document.getElementById('dashFood').textContent=foods.length?fmt(fk):'—';document.getElementById('dashProtein').textContent=foods.length?fmt(fp)+' g':'—';
 document.getElementById('foodTodayKcal').textContent=fmt(fk)+' kcal';document.getElementById('foodTodayProtein').textContent=fmt(fp)+' g';document.getElementById('foodProgress').style.width=Math.min(100,fk/2350*100)+'%';document.getElementById('foodRemaining').textContent=`${fmt(Math.max(0,2350-fk))} kcal restantes da referência diária`;
 const last7=Array.from({length:7},(_,i)=>{let d=new Date();d.setDate(d.getDate()-(6-i));let key=d.toISOString().slice(0,10);return {key,label:d.toLocaleDateString('pt-BR',{weekday:'short'}),mins:db.workouts.filter(x=>x.date===key).reduce((s,x)=>s+Number(x.duration||0),0)}}),max=Math.max(30,...last7.map(x=>x.mins));
 document.getElementById('weeklyBars').innerHTML=last7.map(x=>`<div class="barrow"><span>${x.label}</span><div class="bar"><i style="width:${x.mins/max*100}%"></i></div><b>${x.mins} min</b></div>`).join('');
 document.getElementById('workoutRows').innerHTML=[...db.workouts].sort((a,b)=>b.date.localeCompare(a.date)).map(x=>`<tr><td>${esc(x.date)}</td><td>${esc(x.name)}<br><span class="muted">${esc(x.type)}</span></td><td>${x.duration} min</td><td>${fmt(x.kcal)}<br><span class="muted">${esc(x.kcalSource)}</span></td><td>${esc(x.exercise||'')}${x.load?' · '+esc(x.load)+' kg':''}${x.distance?' · '+esc(x.distance)+' km':''}<br>${esc(x.notes||'')}</td><td><button class="danger" onclick="del('workouts',${x.id})">Excluir</button></td></tr>`).join('')||'<tr><td colspan="6" class="muted">Nenhuma atividade registrada.</td></tr>';
 document.getElementById('bodyRows').innerHTML=[...db.body].sort((a,b)=>b.date.localeCompare(a.date)).map(x=>`<tr><td>${esc(x.date)}</td><td>${fmt(x.weight,1)} kg</td><td>${x.waist?fmt(x.waist,1)+' cm':'—'}</td><td><button class="danger" onclick="del('body',${x.id})">Excluir</button></td></tr>`).join('');
 document.getElementById('foodRows').innerHTML=[...db.food].sort((a,b)=>b.id-a.id).map(x=>`<tr><td>${esc(x.date)}</td><td>${esc(x.note)}</td><td>${fmt(x.kcal)}</td><td>${fmt(x.protein)} g</td><td><button class="danger" onclick="del('food',${x.id})">Excluir</button></td></tr>`).join('')||'<tr><td colspan="5" class="muted">Nenhum alimento registrado.</td></tr>';
 let sorted=[...db.body].sort((a,b)=>a.date.localeCompare(b.date));
 if(sorted.length>1){let min=Math.min(...sorted.map(x=>x.weight))-2,maxw=Math.max(...sorted.map(x=>x.weight))+2;document.getElementById('weightChart').innerHTML=`<div class="barlist">${sorted.map(x=>`<div class="barrow"><span>${esc(x.date.slice(5))}</span><div class="bar"><i style="width:${Math.max(5,(x.weight-min)/(maxw-min)*100)}%"></i></div><b>${fmt(x.weight,1)}</b></div>`).join('')}</div>`}
 else document.getElementById('weightChart').innerHTML='<div class="empty">Adicione mais medições para visualizar a evolução.</div>';
}
render();
</script>
</body></html>
