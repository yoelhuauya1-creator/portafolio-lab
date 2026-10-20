<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PortfolioLab — Construye tu portafolio de inversiones</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
  :root{
    --bg:#0b1220; --bg2:#0e1730; --card:#131c2e; --card2:#18233a;
    --border:#22314f; --text:#e6edf7; --muted:#8fa3c0;
    --accent:#22d3ee; --green:#34d399; --red:#f87171; --yellow:#fbbf24;
  }
  *{margin:0;padding:0;box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{font-family:'Segoe UI',system-ui,-apple-system,sans-serif;background:var(--bg);color:var(--text);line-height:1.6}
  a{color:var(--accent);text-decoration:none}
  .container{max-width:1100px;margin:0 auto;padding:0 20px}

  /* NAV */
  nav{position:sticky;top:0;z-index:100;background:rgba(11,18,32,.92);backdrop-filter:blur(8px);border-bottom:1px solid var(--border)}
  nav .container{display:flex;align-items:center;justify-content:space-between;height:60px}
  .logo{font-weight:800;font-size:1.25rem;color:var(--text)}
  .logo span{color:var(--accent)}
  nav ul{display:flex;gap:22px;list-style:none;flex-wrap:wrap}
  nav ul a{color:var(--muted);font-size:.92rem}
  nav ul a:hover{color:var(--accent)}

  /* HERO */
  .hero{padding:90px 0 60px;text-align:center;background:radial-gradient(ellipse at top,#12203f 0%,var(--bg) 65%)}
  .hero h1{font-size:2.6rem;line-height:1.15;margin-bottom:16px}
  .hero h1 span{background:linear-gradient(90deg,var(--accent),var(--green));-webkit-background-clip:text;background-clip:text;color:transparent}
  .hero p{color:var(--muted);max-width:640px;margin:0 auto 28px;font-size:1.1rem}
  .btn{display:inline-block;padding:12px 26px;border-radius:10px;font-weight:700;cursor:pointer;border:none;font-size:.95rem;transition:.2s}
  .btn-primary{background:linear-gradient(90deg,#0891b2,#059669);color:#fff}
  .btn-primary:hover{transform:translateY(-2px);box-shadow:0 8px 24px rgba(34,211,238,.25)}
  .btn-ghost{background:transparent;color:var(--accent);border:1px solid var(--border);margin-left:10px}
  .btn-ghost:hover{border-color:var(--accent)}
  .btn-sm{padding:7px 14px;font-size:.82rem}
  .btn-danger{background:transparent;color:var(--red);border:1px solid #4a2530}
  .btn-danger:hover{border-color:var(--red)}

  section{padding:64px 0}
  .section-tag{color:var(--accent);text-transform:uppercase;letter-spacing:.12em;font-size:.78rem;font-weight:700}
  h2{font-size:1.9rem;margin:6px 0 10px}
  .section-desc{color:var(--muted);max-width:680px;margin-bottom:32px}

  /* CARDS */
  .grid{display:grid;gap:18px}
  .grid-3{grid-template-columns:repeat(auto-fit,minmax(280px,1fr))}
  .grid-2{grid-template-columns:repeat(auto-fit,minmax(340px,1fr))}
  .card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:22px}
  .card h3{margin-bottom:8px;font-size:1.08rem}
  .card p{color:var(--muted);font-size:.92rem}
  .card .icon{font-size:1.6rem;margin-bottom:10px}

  /* TABLES */
  table{width:100%;border-collapse:collapse;font-size:.9rem}
  th,td{padding:10px 12px;text-align:left;border-bottom:1px solid var(--border)}
  th{color:var(--muted);font-size:.78rem;text-transform:uppercase;letter-spacing:.05em}
  tr:hover td{background:var(--card2)}

  /* FORMS */
  label{display:block;font-size:.82rem;color:var(--muted);margin:12px 0 4px}
  input,select,textarea{width:100%;padding:10px 12px;border-radius:9px;border:1px solid var(--border);background:var(--bg2);color:var(--text);font-size:.92rem}
  input:focus,select:focus{outline:none;border-color:var(--accent)}
  .form-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:14px;align-items:end}

  /* BADGES & ALERTS */
  .badge{display:inline-block;padding:3px 10px;border-radius:20px;font-size:.75rem;font-weight:700}
  .b-green{background:rgba(52,211,153,.14);color:var(--green)}
  .b-red{background:rgba(248,113,113,.14);color:var(--red)}
  .b-blue{background:rgba(34,211,238,.14);color:var(--accent)}
  .b-yellow{background:rgba(251,191,36,.14);color:var(--yellow)}
  .alert{padding:13px 16px;border-radius:10px;font-size:.88rem;margin:14px 0;border:1px solid}
  .a-info{background:rgba(34,211,238,.08);border-color:rgba(34,211,238,.3);color:#a5f3fc}
  .a-warn{background:rgba(251,191,36,.08);border-color:rgba(251,191,36,.3);color:#fde68a}
  .a-ok{background:rgba(52,211,153,.08);border-color:rgba(52,211,153,.3);color:#a7f3d0}

  /* QUIZ */
  .question{margin-bottom:20px}
  .question h4{font-size:.98rem;margin-bottom:8px}
  .option{display:block;background:var(--bg2);border:1px solid var(--border);border-radius:9px;padding:10px 14px;margin-bottom:7px;cursor:pointer;font-size:.9rem}
  .option:hover{border-color:var(--accent)}
  .option input{margin-right:8px;width:auto}

  .bar-row{display:flex;align-items:center;gap:10px;margin:8px 0;font-size:.88rem}
  .bar-row .lbl{width:150px;color:var(--muted)}
  .bar-track{flex:1;background:var(--bg2);border-radius:6px;height:22px;overflow:hidden}
  .bar-fill{height:100%;border-radius:6px;background:linear-gradient(90deg,var(--accent),var(--green));transition:width .6s}
  .bar-val{width:52px;text-align:right;font-weight:700}

  footer{border-top:1px solid var(--border);padding:34px 0;color:var(--muted);font-size:.85rem;text-align:center}
  .chart-box{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:20px;max-width:480px;margin:0 auto}
  #apiStatus{font-size:.82rem;margin-top:6px;color:var(--muted)}
  @media(max-width:640px){.hero h1{font-size:1.9rem}nav ul{gap:12px}}
</style>
<base target="_blank">
</head>
<body>

<nav>
  <div class="container">
    <div class="logo">Portfolio<span>Lab</span></div>
    <ul>
      <li><a href="#aprender">Aprender</a></li>
      <li><a href="#indices">Índices</a></li>
      <li><a href="#test">Test de perfil</a></li>
      <li><a href="#constructor">Constructor</a></li>
      <li><a href="#mercado">Mercado</a></li>
    </ul>
  </div>
</nav>

<header class="hero">
  <div class="container">
    <h1>Construye tu propio <span>portafolio de inversiones</span></h1>
    <p>Aprende los conceptos clave, descubre tu perfil de riesgo, arma tu portafolio con instrumentos de las principales bolsas del mundo y consulta precios de mercado. Todo en una sola página.</p>
    <a href="#test" class="btn btn-primary">Descubre tu perfil de riesgo</a>
    <a href="#constructor" class="btn btn-ghost">Ir al constructor →</a>
  </div>
</header>

<!-- ================= APRENDER ================= -->
<section id="aprender">
  <div class="container">
    <span class="section-tag">Nivel principiante</span>
    <h2>Conceptos esenciales</h2>
    <p class="section-desc">Antes de invertir, domina estas ideas básicas. Si ya tienes experiencia, puedes saltar directo al constructor.</p>
    <div class="grid grid-3">
      <div class="card"><div class="icon">📈</div><h3>Acción</h3><p>Una parte de la propiedad de una empresa. Si la empresa crece, tu acción suele subir de valor y puede pagar <strong>dividendos</strong>.</p></div>
      <div class="card"><div class="icon">🧺</div><h3>ETF</h3><p>Fondo que replica un índice o sector y se compra como una acción. Es la forma más fácil de diversificar: con un solo ETF posees cientos de empresas.</p></div>
      <div class="card"><div class="icon">📜</div><h3>Bono</h3><p>Un préstamo que le haces a un gobierno o empresa a cambio de intereses. Es de <strong>renta fija</strong>: menor riesgo y menor rendimiento esperado.</p></div>
      <div class="card"><div class="icon">🌍</div><h3>Diversificación</h3><p>No poner todos los huevos en una canasta. Combinar acciones, bonos, sectores y países reduce el riesgo sin sacrificar tanto rendimiento.</p></div>
      <div class="card"><div class="icon">🎢</div><h3>Perfil de riesgo</h3><p>Tu capacidad y disposición a soportar caídas del mercado. Define la mezcla entre renta variable (acciones) y renta fija (bonos).</p></div>
      <div class="card"><div class="icon">⏳</div><h3>Horizonte de inversión</h3><p>Tiempo que mantendrás el dinero invertido. A mayor horizonte (10+ años), más sentido tiene asumir riesgo en acciones.</p></div>
    </div>

    <h2 style="margin-top:48px">Glosario rápido</h2>
    <div class="grid grid-2">
      <div class="card">
        <table>
          <tr><td><strong>Índice bursátil</strong></td><td>Medidor de un mercado (ej. S&P 500 = 500 grandes empresas de EE.UU.).</td></tr>
          <tr><td><strong>Volatilidad</strong></td><td>Intensidad con que sube y baja un precio. Alta volatilidad = mayor riesgo y mayor potencial.</td></tr>
          <tr><td><strong>Dividendo</strong></td><td>Parte de las utilidades que una empresa reparte a sus accionistas.</td></tr>
          <tr><td><strong>Broker</strong></td><td>Plataforma autorizada para comprar y vender instrumentos en la bolsa.</td></tr>
        </table>
      </div>
      <div class="card">
        <table>
          <tr><td><strong>Renta fija</strong></td><td>Instrumentos con pagos predecibles: bonos, letras del tesoro.</td></tr>
          <tr><td><strong>Renta variable</strong></td><td>Instrumentos cuyo precio fluctúa: acciones, ETFs, criptomonedas.</td></tr>
          <tr><td><strong>PER (P/E)</strong></td><td>Precio / utilidad. Ayuda a evaluar si una acción está cara o barata frente a sus ganancias.</td></tr>
          <tr><td><strong>Composición (%) </strong></td><td>Qué porcentaje de tu dinero va a cada activo del portafolio.</td></tr>
        </table>
      </div>
    </div>
  </div>
</section>

<!-- ================= ÍNDICES ================= -->
<section id="indices" style="background:var(--bg2)">
  <div class="container">
    <span class="section-tag">Nivel intermedio</span>
    <h2>Los grandes índices del mundo y cómo invertir en ellos</h2>
    <p class="section-desc">No necesitas analizar empresa por empresa: los ETFs de índice te permiten invertir en mercados enteros de un solo golpe.</p>
    <div class="card">
      <table>
        <thead><tr><th>Índice</th><th>Mercado</th><th>Qué mide</th><th>ETF de referencia</th></tr></thead>
        <tbody>
          <tr><td><strong>S&P 500</strong></td><td>EE.UU. (NYSE/NASDAQ)</td><td>Las 500 mayores empresas estadounidenses</td><td>VOO / SPY</td></tr>
          <tr><td><strong>Dow Jones</strong></td><td>EE.UU. (NYSE)</td><td>30 empresas industriales emblemáticas</td><td>DIA</td></tr>
          <tr><td><strong>NASDAQ 100</strong></td><td>EE.UU. (NASDAQ)</td><td>100 grandes empresas tecnológicas y de crecimiento</td><td>QQQ</td></tr>
          <tr><td><strong>Euro Stoxx 50</strong></td><td>Zona Euro (Euronext)</td><td>50 líderes de la eurozona</td><td>FEZ</td></tr>
          <tr><td><strong>IBEX 35</strong></td><td>España (BME)</td><td>35 principales empresas españolas</td><td>EWP (MSCI España)</td></tr>
          <tr><td><strong>Nikkei 225</strong></td><td>Japón (TSE)</td><td>225 principales empresas japonesas</td><td>EWJ (MSCI Japón)</td></tr>
        </tbody>
      </table>
      <div class="alert a-info" style="margin-top:16px">💡 <strong>Idea de portafolio global:</strong> combinar un ETF del S&P 500 (núcleo), un ETF tecnológico (QQQ) como satélite, y un porcentaje en bonos para reducir riesgo, según tu perfil.</div>
    </div>
  </div>
</section>

<!-- ================= TEST DE PERFIL ================= -->
<section id="test">
  <div class="container">
    <span class="section-tag">Paso 1</span>
    <h2>Test: ¿cuál es tu perfil de inversionista?</h2>
    <p class="section-desc">Responde con honestidad. Al final obtendrás una sugerencia de asignación que podrás aplicar con un clic al constructor.</p>

    <div class="grid grid-2">
      <div class="card" id="quizCard"></div>
      <div class="card">
        <h3>Tu resultado</h3>
        <div id="quizResult"><p style="color:var(--muted)">Responde las preguntas y pulsa <strong>Calcular mi perfil</strong>.</p></div>
        <div id="quizAlloc"></div>
        <button class="btn btn-primary btn-sm" id="applyProfile" style="display:none;margin-top:14px">Aplicar al constructor ➜</button>
      </div>
    </div>
  </div>
</section>

<!-- ================= CONSTRUCTOR ================= -->
<section id="constructor" style="background:var(--bg2)">
  <div class="container">
    <span class="section-tag">Paso 2</span>
    <h2>Constructor de portafolio</h2>
    <p class="section-desc">Agrega los instrumentos que quieras con su porcentaje de asignación. El total debe sumar 100%. Se guarda automáticamente en tu navegador.</p>

    <div class="card" style="margin-bottom:22px">
      <div class="form-row">
        <div><label>Ticker / símbolo</label><input id="fTicker" placeholder="ej. AAPL, VOO, QQQ"></div>
        <div><label>Nombre</label><input id="fName" placeholder="ej. Apple Inc."></div>
        <div><label>Tipo</label>
          <select id="fType"><option>Acción</option><option>ETF</option><option>Bono</option><option>Fondo</option><option>Otro</option></select>
        </div>
        <div><label>Bolsa</label>
          <select id="fExch"><option>NASDAQ</option><option>NYSE</option><option>Euronext</option><option>BME (España)</option><option>TSE (Japón)</option><option>Otra</option></select>
        </div>
        <div><label>Peso (%)</label><input id="fWeight" type="number" min="0" max="100" placeholder="ej. 25"></div>
        <div><button class="btn btn-primary" id="addBtn">+ Agregar</button></div>
      </div>
      <div id="formMsg"></div>
    </div>

    <div class="grid grid-2" style="align-items:start">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
          <h3 style="margin:0">Mis activos</h3>
          <span id="sumBadge" class="badge b-red">Suma: 0%</span>
        </div>
        <table>
          <thead><tr><th>Ticker</th><th>Nombre</th><th>Tipo</th><th>Peso</th><th></th></tr></thead>
          <tbody id="portfolioBody"><tr><td colspan="5" style="color:var(--muted)">Portafolio vacío. Agrega tu primer activo ↑</td></tr></tbody>
        </table>
        <div style="margin-top:14px;display:flex;gap:10px">
          <button class="btn btn-ghost btn-sm" id="clearBtn">🗑 Vaciar</button>
          <button class="btn btn-ghost btn-sm" id="demoBtn">✨ Cargar ejemplo</button>
        </div>
      </div>
      <div class="chart-box">
        <h3 style="text-align:center;margin-bottom:14px">Composición del portafolio</h3>
        <canvas id="allocChart"></canvas>
      </div>
    </div>
  </div>
</section>

<!-- ================= MERCADO ================= -->
<section id="mercado">
  <div class="container">
    <span class="section-tag">Paso 3</span>
    <h2>Cotizaciones de mercado</h2>
    <p class="section-desc">Consulta precios reales con una API key gratuita de <a href="https://twelvedata.com/" target="_blank">Twelve Data</a> (plan free: 8 consultas/min), o usa el modo demo con datos simulados para practicar sin límites.</p>

    <div class="card" style="margin-bottom:22px">
      <div class="form-row">
        <div style="grid-column:span 2"><label>Tu API key de Twelve Data (se guarda solo en tu navegador)</label><input id="apiKey" type="password" placeholder="Pega aquí tu key gratuita"></div>
        <div><label>&nbsp;</label><button class="btn btn-ghost" id="saveKeyBtn">Guardar key</button></div>
        <div><label>&nbsp;</label><button class="btn btn-primary" id="quoteBtn">🔍 Consultar precios reales</button></div>
        <div><label>&nbsp;</label><button class="btn btn-ghost" id="demoQuoteBtn">🎲 Modo demo</button></div>
      </div>
      <div id="apiStatus"></div>
    </div>

    <div class="card">
      <table>
        <thead><tr><th>Ticker</th><th>Nombre</th><th>Precio</th><th>Cambio %</th><th>Fuente</th></tr></thead>
        <tbody id="quotesBody"><tr><td colspan="5" style="color:var(--muted)">Aún no hay cotizaciones. Usa los botones de arriba.</td></tr></tbody>
      </table>
    </div>
  </div>
</section>

<footer>
  <div class="container">
    <p><strong>⚠️ Aviso importante:</strong> PortfolioLab es un proyecto con fines exclusivamente educativos. Nada de lo aquí mostrado constituye asesoría financiera ni recomendación de inversión. Invertir en la bolsa implica riesgo, incluida la posible pérdida del capital.</p>
    <p style="margin-top:10px">Hecho con fines educativos · Datos de mercado: Twelve Data / modo simulado</p>
  </div>
</footer>

<script>
/* ============ ESTADO ============ */
const LS_PORT='pl_portfolio', LS_KEY='pl_apikey';
let portfolio = JSON.parse(localStorage.getItem(LS_PORT)||'[]');
let chart = null;

/* ============ TEST DE PERFIL ============ */
const QUESTIONS=[
 {q:"¿Para qué horizonte de tiempo piensas invertir?",o:["Menos de 2 años","Entre 2 y 7 años","Más de 7 años"]},
 {q:"Si tu portafolio cae 20% en un mes, tú…",o:["Vendo todo para no perder más","No hago nada y espero","Aprovecho para comprar más barato"]},
 {q:"¿Qué importancia tiene para ti no perder dinero?",o:["Máxima: no tolero pérdidas","Media: acepto caídas temporales","Baja: busco el mayor crecimiento"]},
 {q:"¿Cómo describirías tu conocimiento de finanzas?",o:["Principiante","Intermedio","Avanzado"]},
 {q:"¿Cuánta parte de tus ahorros destinarías a invertir?",o:["Menos del 20%","Entre 20% y 50%","Más del 50%"]},
 {q:"¿Prefieres rendimientos estables o potenciales altos con riesgo?",o:["Estables y seguros","Un equilibrio entre ambos","Potenciales altos aunque arriesgue"]}
];
const PROFILES={
 conservador:{label:"Conservador",desc:"Priorizas la seguridad. Tu portafolio sugerido se inclina hacia renta fija con algo de renta variable para no perder contra la inflación.",alloc:{"Bonos / Renta fija":60,"ETF de índice (S&P 500)":25,"Acciones":10,"Efectivo / Otros":5}},
 moderado:{label:"Moderado",desc:"Buscas un equilibrio entre crecimiento y estabilidad. Una base diversificada de ETFs con una parte en acciones y bonos.",alloc:{"Bonos / Renta fija":40,"ETF de índice (S&P 500)":35,"Acciones":20,"Efectivo / Otros":5}},
 agresivo:{label:"Agresivo",desc:"Tienes horizonte largo y toleras la volatilidad a cambio de mayor crecimiento potencial. Predominan las acciones y ETFs de crecimiento.",alloc:{"Acciones":55,"ETF de índice (S&P 500)":30,"Bonos / Renta fija":10,"Efectivo / Otros":5}}
};
const quizCard=document.getElementById('quizCard');
quizCard.innerHTML='<h3>Cuestionario</h3>'+QUESTIONS.map((it,i)=>`
 <div class="question"><h4>${i+1}. ${it.q}</h4>
 ${it.o.map((op,j)=>`<label class="option"><input type="radio" name="q${i}" value="${j+1}">${op}</label>`).join('')}
 </div>`).join('')+`<button class="btn btn-primary" id="calcBtn">Calcular mi perfil</button>`;

document.getElementById('calcBtn').addEventListener('click',()=>{
 let total=0,answered=0;
 QUESTIONS.forEach((_,i)=>{const sel=document.querySelector(`input[name=q${i}]:checked`);if(sel){total+=+sel.value;answered++;}});
 if(answered<QUESTIONS.length){alert('Responde todas las preguntas por favor.');return;}
 const avg=total/QUESTIONS.length;
 const key=avg<2.1?'conservador':avg<2.7?'moderado':'agresivo';
 const p=PROFILES[key];
 document.getElementById('quizResult').innerHTML=`
  <span class="badge ${key==='conservador'?'b-blue':key==='moderado'?'b-yellow':'b-green'}" style="font-size:.95rem">Perfil: ${p.label}</span>
  <p style="margin-top:10px;color:var(--muted)">${p.desc}</p>`;
 document.getElementById('quizAlloc').innerHTML='<h4 style="margin:14px 0 6px">Asignación sugerida</h4>'+
  Object.entries(p.alloc).map(([k,v])=>barRow(k,v)).join('');
 const btn=document.getElementById('applyProfile');
 btn.style.display='inline-block';
 btn.onclick=()=>{applyProfile(key);document.getElementById('constructor').scrollIntoView({behavior:'smooth'});};
});
function barRow(label,val){
 return `<div class="bar-row"><span class="lbl">${label}</span><div class="bar-track"><div class="bar-fill" style="width:${val}%"></div></div><span class="bar-val">${val}%</span></div>`;
}
function applyProfile(key){
 portfolio=[{t:'VOO',n:'ETF S&P 500',ty:'ETF',ex:'NYSE',w:PROFILES[key].alloc['ETF de índice (S&P 500)']}];
 if(PROFILES[key].alloc['Acciones'])portfolio.push({t:'AAPL',n:'Apple Inc.',ty:'Acción',ex:'NASDAQ',w:PROFILES[key].alloc['Acciones']});
 if(PROFILES[key].alloc['Bonos / Renta fija'])portfolio.push({t:'BONOS',n:'Bonos gobierno / T-Bills',ty:'Bono',ex:'Otra',w:PROFILES[key].alloc['Bonos / Renta fija']});
 if(PROFILES[key].alloc['Efectivo / Otros'])portfolio.push({t:'CASH',n:'Efectivo / Otros',ty:'Otro',ex:'Otra',w:PROFILES[key].alloc['Efectivo / Otros']});
 saveAndRender();
}

/* ============ CONSTRUCTOR ============ */
const tbody=document.getElementById('portfolioBody');
document.getElementById('addBtn').addEventListener('click',()=>{
 const t=document.getElementById('fTicker').value.trim().toUpperCase();
 const n=document.getElementById('fName').value.trim();
 const ty=document.getElementById('fType').value;
 const ex=document.getElementById('fExch').value;
 const w=parseFloat(document.getElementById('fWeight').value);
 const msg=document.getElementById('formMsg');msg.innerHTML='';
 if(!t||!n||isNaN(w)||w<=0){msg.innerHTML='<div class="alert a-warn">Completa todos los campos y un peso mayor a 0.</div>';return;}
 if(currentSum()+w>100.02){msg.innerHTML=`<div class="alert a-warn">El total superaría 100% (actual: ${currentSum()}%). Ajusta los pesos.</div>`;return;}
 portfolio.push({t,n,ty,ex,w});
 ['fTicker','fName','fWeight'].forEach(id=>document.getElementById(id).value='');
 saveAndRender();
});
document.getElementById('clearBtn').addEventListener('click',()=>{if(confirm('¿Vaciar todo el portafolio?')){portfolio=[];saveAndRender();}});
document.getElementById('demoBtn').addEventListener('click',()=>{
 portfolio=[
  {t:'VOO',n:'ETF S&P 500',ty:'ETF',ex:'NYSE',w:35},
  {t:'QQQ',n:'ETF NASDAQ 100',ty:'ETF',ex:'NASDAQ',w:20},
  {t:'AAPL',n:'Apple Inc.',ty:'Acción',ex:'NASDAQ',w:15},
  {t:'MSFT',n:'Microsoft',ty:'Acción',ex:'NASDAQ',w:10},
  {t:'FEZ',n:'ETF Euro Stoxx 50',ty:'ETF',ex:'NYSE',w:10},
  {t:'BONOS',n:'Bonos gobierno',ty:'Bono',ex:'Otra',w:10}];
 saveAndRender();
});
function currentSum(){return portfolio.reduce((a,b)=>a+b.w,0);}
function saveAndRender(){
 localStorage.setItem(LS_PORT,JSON.stringify(portfolio));
 renderPortfolio();
}
function renderPortfolio(){
 const sum=currentSum();
 const badge=document.getElementById('sumBadge');
 badge.textContent='Suma: '+sum.toFixed(1)+'%';
 badge.className='badge '+(Math.abs(sum-100)<0.01?'b-green':sum>100?'b-red':'b-yellow');
 if(!portfolio.length){tbody.innerHTML='<tr><td colspan="5" style="color:var(--muted)">Portafolio vacío. Agrega tu primer activo ↑</td></tr>';}
 else{
  tbody.innerHTML=portfolio.map((a,i)=>`<tr>
   <td><strong>${a.t}</strong></td><td>${a.n}</td>
   <td><span class="badge b-blue">${a.ty}</span></td>
   <td><strong>${a.w}%</strong></td>
   <td><button class="btn btn-danger btn-sm" onclick="delAsset(${i})">✕</button></td></tr>`).join('');
 }
 renderChart();
}
window.delAsset=i=>{portfolio.splice(i,1);saveAndRender();};
function renderChart(){
 const ctx=document.getElementById('allocChart').getContext('2d');
 if(chart)chart.destroy();
 if(!portfolio.length){
  chart=new Chart(ctx,{type:'doughnut',data:{labels:['Sin datos'],datasets:[{data:[1],backgroundColor:['#22314f']}]},options:chartOpts()});
  return;
 }
 const colors=['#22d3ee','#34d399','#fbbf24','#f87171','#a78bfa','#f472b6','#60a5fa','#fb923c'];
 chart=new Chart(ctx,{type:'doughnut',
  data:{labels:portfolio.map(a=>a.t),datasets:[{data:portfolio.map(a=>a.w),backgroundColor:portfolio.map((_,i)=>colors[i%colors.length]),borderColor:'#131c2e',borderWidth:3}]},
  options:chartOpts()});
}
function chartOpts(){return{plugins:{legend:{position:'bottom',labels:{color:'#e6edf7',padding:14,font:{size:11}}}},cutout:'62%'};}

/* ============ COTIZACIONES ============ */
const keyInput=document.getElementById('apiKey');
keyInput.value=localStorage.getItem(LS_KEY)||'';
document.getElementById('saveKeyBtn').addEventListener('click',()=>{
 localStorage.setItem(LS_KEY,keyInput.value.trim());
 setStatus('Key guardada en tu navegador.','ok');
});
const qBody=document.getElementById('quotesBody');
function setStatus(txt,kind){document.getElementById('apiStatus').innerHTML=`<span style="color:${kind==='ok'?'var(--green)':kind==='warn'?'var(--yellow)':'var(--muted)'}">${txt}</span>`;}

document.getElementById('quoteBtn').addEventListener('click',async()=>{
 const key=keyInput.value.trim();
 if(!key){setStatus('⚠️ Necesitas una API key gratuita de twelvedata.com (o usa el modo demo).','warn');return;}
 const symbols=portfolio.filter(a=>!['CASH','BONOS'].includes(a.t)).map(a=>tickerForAPI(a.t)).slice(0,8);
 if(!symbols.length){setStatus('Agrega acciones o ETFs a tu portafolio primero.','warn');return;}
 setStatus('⏳ Consultando precios reales (plan free: máx. 8/min, se consultan en secuencia)…');
 qBody.innerHTML='<tr><td colspan="5" style="color:var(--muted)">Cargando…</td></tr>';
 const rows=[];
 for(let i=0;i<symbols.length;i++){
  try{
   const r=await fetch(`https://api.twelvedata.com/quote?symbol=${symbols[i]}&apikey=${key}`);
   const d=await r.json();
   if(d.status==='error'){rows.push({t:symbols[i],err:d.message});}
   else rows.push({t:d.symbol,n:d.name,price:+d.close,chg:+d.percent_change});
  }catch(e){rows.push({t:symbols[i],err:'Error de conexión'});}
  if(i<symbols.length-1)await new Promise(res=>setTimeout(res,8000)); // respeta límite free de 8 créditos/min
 }
 renderQuotes(rows,'Twelve Data · real');
 setStatus('✅ Datos reales obtenidos. Recuerda: plan free = 8 consultas/minuto.','ok');
});

document.getElementById('demoQuoteBtn').addEventListener('click',()=>{
 const symbols=portfolio.length?portfolio.map(a=>a.t):['AAPL','MSFT','VOO','QQQ','FEZ','NVDA'];
 const rows=symbols.filter(s=>!['CASH','BONOS'].includes(s)).map((s,i)=>{
  const base=50+((hashCode(s)%40000)/100);
  const chg=((hashCode(s+'c')%800)/100-4);
  const names={AAPL:'Apple Inc.',MSFT:'Microsoft Corp.',VOO:'ETF S&P 500',QQQ:'ETF NASDAQ 100',FEZ:'ETF Euro Stoxx 50',NVDA:'NVIDIA Corp.'};
  return {t:s,n:names[s]||'Instrumento (demo)',price:+(base*(1+chg/200)).toFixed(2),chg:+chg.toFixed(2)};
 });
 renderQuotes(rows,'Demo · simulado');
 setStatus('🎲 Modo demo activado: datos simulados con fines educativos.','warn');
});
function tickerForAPI(t){return t==='BONOS'?'TLT':t;} // bonos → ETF de bonos TLT para la demo real
function hashCode(s){let h=0;for(let i=0;i<s.length;i++){h=(h*31+s.charCodeAt(i))|0;}return Math.abs(h);}
function renderQuotes(rows,src){
 qBody.innerHTML=rows.map(r=>r.err
  ?`<tr><td><strong>${r.t}</strong></td><td colspan="3" style="color:var(--yellow)">⚠️ ${r.err}</td><td><span class="badge b-yellow">${src}</span></td></tr>`
  :`<tr><td><strong>${r.t}</strong></td><td>${r.n||'—'}</td>
    <td>$${r.price.toLocaleString('en-US',{minimumFractionDigits:2})}</td>
    <td><span class="badge ${r.chg>=0?'b-green':'b-red'}">${r.chg>=0?'▲':'▼'} ${Math.abs(r.chg).toFixed(2)}%</span></td>
    <td><span class="badge b-blue">${src}</span></td></tr>`).join('');
}

/* init */
renderPortfolio();
</script>
</body>
</html>
