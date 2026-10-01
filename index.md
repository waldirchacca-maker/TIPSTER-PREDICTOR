<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>CanchaData · Analizador de fútbol</title>
<link rel="icon" href="/favicon.svg" />
<style>
  :root{
    --bg:#0b0f14; --bg-soft:#111823; --panel:#0f1620; --panel-2:#131c28;
    --border:#1f2a38; --border-soft:#1a2331;
    --text:#e6edf5; --muted:#8a99ad; --muted-2:#5d6b7e;
    --accent:#22c55e; --accent-2:#16a34a; --accent-soft:rgba(34,197,94,.12);
    --danger:#ef4444; --warn:#f59e0b;
    --radius:12px; --radius-sm:8px;
    --shadow:0 1px 0 rgba(255,255,255,.02), 0 8px 24px rgba(0,0,0,.35);
    --font:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  }
  *{box-sizing:border-box}
  html,body{margin:0;padding:0}
  body{
    background:radial-gradient(1200px 600px at 50% -200px, rgba(34,197,94,.06), transparent 60%), var(--bg);
    color:var(--text);font-family:var(--font);font-size:14px;line-height:1.5;
    min-height:100vh;-webkit-font-smoothing:antialiased;
  }
  svg{display:block}
  .site-shell{max-width:1180px;margin:0 auto;padding:20px 22px 60px}

  .topline{display:flex;align-items:center;gap:10px;padding:8px 0 18px;color:var(--muted);font-size:12.5px}
  .mark{width:30px;height:30px;display:grid;place-items:center;border-radius:8px;background:var(--accent-soft);color:var(--accent)}
  .brand{font-weight:700;color:var(--text);letter-spacing:.2px}
  .brand span{color:var(--accent)}
  .divider{width:1px;height:16px;background:var(--border);margin:0 4px}
  .topline-sub{color:var(--muted-2)}
  .topline-right{margin-left:auto;display:flex;align-items:center;gap:6px;color:var(--muted-2);font-size:12px}

  .page-header{display:grid;grid-template-columns:1fr auto;gap:24px;align-items:end;padding:22px 0 30px;border-bottom:1px solid var(--border-soft)}
  .eyebrow{display:flex;align-items:center;gap:8px;color:var(--accent);font-size:11px;font-weight:600;letter-spacing:1.4px;text-transform:uppercase;margin-bottom:10px}
  .eyebrow-line{width:22px;height:2px;background:var(--accent);border-radius:2px}
  h1{margin:0 0 10px;font-size:clamp(22px,3vw,32px);line-height:1.15;font-weight:700;letter-spacing:-.02em}
  .page-header p{margin:0;color:var(--muted);max-width:560px}
  .header-actions{display:flex;gap:10px;flex-wrap:wrap}

  .btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;height:38px;padding:0 14px;
    border-radius:var(--radius-sm);border:1px solid var(--border);background:var(--panel);color:var(--text);
    font:inherit;font-size:13.5px;font-weight:500;cursor:pointer;transition:background .15s,border-color .15s,transform .05s;white-space:nowrap}
  .btn:hover{background:var(--panel-2);border-color:#26344a}
  .btn:active{transform:translateY(1px)}
  .btn:disabled{opacity:.55;cursor:not-allowed}
  .btn-primary{background:var(--accent);border-color:var(--accent);color:#04160a;font-weight:600}
  .btn-primary:hover{background:var(--accent-2);border-color:var(--accent-2)}
  .btn-sm{height:32px;padding:0 11px;font-size:12.5px}

  .settings-panel{margin-top:26px;background:linear-gradient(180deg,var(--panel),var(--bg-soft));border:1px solid var(--border);border-radius:var(--radius);padding:20px;box-shadow:var(--shadow)}
  .settings-intro{display:flex;gap:14px;align-items:flex-start;padding-bottom:18px;border-bottom:1px dashed var(--border);margin-bottom:18px}
  .panel-icon{flex:none;width:38px;height:38px;border-radius:10px;display:grid;place-items:center;background:var(--accent-soft);color:var(--accent)}
  .settings-intro strong{display:block;margin-bottom:4px;font-size:14.5px}
  .settings-intro p{margin:0;color:var(--muted);font-size:13px;max-width:720px}
  .settings-fields{display:grid;grid-template-columns:1fr 1fr;gap:16px}
  .settings-fields label{display:flex;flex-direction:column;gap:6px;font-size:12.5px;font-weight:600;color:var(--muted);letter-spacing:.2px}
  .settings-fields label > span{font-weight:400;color:var(--muted-2);font-size:11.5px}
  .settings-fields input[type="password"]{height:38px;padding:0 12px;border-radius:var(--radius-sm);border:1px solid var(--border);
    background:#0a0f16;color:var(--text);font:inherit;font-size:13.5px;outline:none;transition:border-color .15s,box-shadow .15s}
  .settings-fields input:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(34,197,94,.15)}
  .settings-fields input::placeholder{color:var(--muted-2)}

  .toggle-row{display:flex;gap:12px;align-items:flex-start;margin-top:12px;padding:14px;border:1px solid var(--border);
    border-radius:var(--radius-sm);background:#0c131c;cursor:pointer}
  .toggle-row input{margin-top:3px;width:16px;height:16px;accent-color:var(--accent);flex:none}
  .toggle-row strong{display:block;font-size:13.5px;margin-bottom:3px}
  .toggle-row small{color:var(--muted);font-size:12px;line-height:1.45}

  .settings-bottom{display:flex;justify-content:space-between;align-items:center;gap:16px;margin-top:18px;padding-top:16px;
    border-top:1px dashed var(--border);color:var(--muted-2);font-size:12px;flex-wrap:wrap}
  .status{margin-top:12px;padding:10px 12px;border-radius:var(--radius-sm);font-size:12.5px;display:none}
  .status.show{display:block}
  .status.info{background:rgba(59,130,246,.1);border:1px solid rgba(59,130,246,.35);color:#93c5fd}
  .status.error{background:rgba(239,68,68,.1);border:1px solid rgba(239,68,68,.35);color:#fca5a5}
  .status.success{background:var(--accent-soft);border:1px solid rgba(34,197,94,.35);color:#86efac}
  .status.warn{background:rgba(245,158,11,.1);border:1px solid rgba(245,158,11,.35);color:#fcd34d}

  .metric-row{display:grid;grid-template-columns:repeat(3,minmax(0,1fr)) 1.2fr;gap:14px;margin:26px 0}
  .metric{background:var(--panel);border:1px solid var(--border);border-radius:var(--radius);padding:16px;display:flex;flex-direction:column;gap:6px}
  .metric span{font-size:10.5px;font-weight:600;letter-spacing:1.2px;color:var(--muted-2);text-transform:uppercase}
  .metric strong{font-size:24px;font-weight:700;color:var(--text)}
  .metric small{color:var(--muted);font-size:12px}
  .metric-note{flex-direction:row;align-items:center;gap:12px;background:linear-gradient(180deg,rgba(34,197,94,.06),transparent);border-color:rgba(34,197,94,.25);color:var(--accent)}
  .metric-note p{margin:0;color:var(--muted);font-size:12.5px}
  .metric-note b{color:var(--text)}

  .tabs-nav{display:flex;gap:22px;border-bottom:1px solid var(--border);margin-bottom:20px}
  .tab{display:inline-flex;align-items:center;gap:8px;background:none;border:none;color:var(--muted);padding:0 2px 12px;
    font:inherit;font-size:13.5px;font-weight:500;cursor:pointer;position:relative}
  .tab:hover{color:var(--text)}
  .tab[aria-selected="true"]{color:var(--text)}
  .tab[aria-selected="true"]::after{content:"";position:absolute;left:0;right:0;bottom:-1px;height:2px;background:var(--accent);border-radius:2px}
  .tab-count{display:inline-block;padding:1px 7px;border-radius:999px;background:var(--accent-soft);color:var(--accent);font-size:11px;font-weight:600}

  .workspace{display:grid;grid-template-columns:minmax(300px,380px) 1fr;gap:16px}
  .fixtures-panel,.detail-panel{background:var(--panel);border:1px solid var(--border);border-radius:var(--radius);min-height:460px;display:flex;flex-direction:column}
  .panel-head{padding:16px 18px;border-bottom:1px solid var(--border)}
  .panel-head h2{margin:0;font-size:14.5px;font-weight:600}
  .panel-head p{margin:2px 0 0;color:var(--muted-2);font-size:12px}

  .filters{display:flex;gap:8px;padding:12px;border-bottom:1px solid var(--border)}
  .search-box{position:relative;flex:1}
  .search-box svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);color:var(--muted-2);pointer-events:none}
  .search-box input{width:100%;height:36px;padding:0 12px 0 34px;background:#0a0f16;color:var(--text);
    border:1px solid var(--border);border-radius:var(--radius-sm);font:inherit;font-size:13px;outline:none}
  .search-box input:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(34,197,94,.15)}
  .search-box input::placeholder{color:var(--muted-2)}

  .fixtures-list{flex:1;overflow-y:auto;max-height:640px}
  .fixture{padding:12px 16px;border-bottom:1px solid var(--border-soft);cursor:pointer;transition:background .12s}
  .fixture:hover{background:var(--panel-2)}
  .fixture.selected{background:var(--accent-soft);border-left:3px solid var(--accent)}
  .fixture-time{font-size:11px;color:var(--accent);font-weight:600;letter-spacing:.5px;margin-bottom:4px}
  .fixture-teams{display:flex;align-items:center;gap:8px;font-size:13.5px;font-weight:500}
  .fixture-teams img{width:18px;height:18px;object-fit:contain}
  .fixture-teams .vs{color:var(--muted-2);font-size:11px;font-weight:400;margin:0 4px}
  .fixture-league{font-size:11px;color:var(--muted-2);margin-top:5px;display:flex;align-items:center;gap:5px}
  .fixture-score{color:var(--accent);font-weight:700;margin-left:auto;font-size:13px}

  .empty-panel{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:36px 22px;gap:8px;color:var(--muted)}
  .empty-panel svg{color:var(--muted-2);margin-bottom:6px}
  .empty-panel h3{margin:0;color:var(--text);font-size:14.5px}
  .empty-panel p{margin:0;font-size:13px;max-width:280px}

  .detail-empty{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:40px 26px}
  .detail-symbol{width:52px;height:52px;border-radius:14px;display:grid;place-items:center;background:var(--accent-soft);color:var(--accent);margin-bottom:16px}
  .detail-empty h2{margin:6px 0 8px;font-size:18px}
  .detail-empty p{margin:0;color:var(--muted);max-width:420px}
  .detail-steps{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin-top:22px}
  .detail-steps span{padding:6px 12px;border-radius:999px;background:var(--bg-soft);border:1px solid var(--border);font-size:11.5px;color:var(--muted)}

  .detail-content{padding:20px;overflow-y:auto;max-height:700px}
  .detail-header{display:flex;align-items:center;gap:14px;padding-bottom:16px;border-bottom:1px solid var(--border);margin-bottom:18px}
  .detail-team{flex:1;text-align:center}
  .detail-team img{width:42px;height:42px;object-fit:contain;margin:0 auto 6px}
  .detail-team strong{display:block;font-size:13.5px;margin-bottom:2px}
  .detail-team small{color:var(--muted-2);font-size:11px}
  .detail-vs{color:var(--muted-2);font-size:11px;font-weight:600;letter-spacing:1px}
  .detail-meta{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-bottom:18px}
  .detail-meta div{background:var(--bg-soft);padding:10px 12px;border-radius:var(--radius-sm);border:1px solid var(--border)}
  .detail-meta span{display:block;color:var(--muted-2);font-size:10.5px;letter-spacing:.6px;text-transform:uppercase;margin-bottom:3px}
  .detail-meta strong{font-size:13px;color:var(--text);font-weight:600}

  .detail-section-title{font-size:11.5px;color:var(--accent);letter-spacing:1.2px;text-transform:uppercase;font-weight:600;
    margin:0 0 12px;display:flex;align-items:center;gap:8px}
  .detail-section-title::after{content:"";flex:1;height:1px;background:var(--border)}

  .season-note{
    display:flex;align-items:center;gap:8px;
    background:rgba(245,158,11,.08);border:1px solid rgba(245,158,11,.3);color:#fcd34d;
    padding:9px 12px;border-radius:var(--radius-sm);font-size:12px;margin-bottom:14px;
  }
  .season-note svg{flex:none}

  .stats-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:18px}
  .stat-card{background:var(--bg-soft);border:1px solid var(--border);border-radius:var(--radius-sm);padding:12px;text-align:center}
  .stat-card span{display:block;font-size:10.5px;color:var(--muted-2);letter-spacing:.6px;text-transform:uppercase;margin-bottom:4px}
  .stat-card strong{font-size:20px;color:var(--accent);font-weight:700}
  .stat-card small{display:block;color:var(--muted-2);font-size:11px;margin-top:2px}

  .prob-list{display:grid;gap:8px}
  .prob-item{display:flex;align-items:center;gap:10px;background:var(--bg-soft);border:1px solid var(--border);border-radius:var(--radius-sm);padding:10px 12px}
  .prob-item .label{flex:1;font-size:12.5px;color:var(--muted)}
  .prob-item .value{font-size:14px;font-weight:700;color:var(--accent)}
  .prob-bar{height:4px;border-radius:2px;background:var(--border);flex:1;max-width:80px;overflow:hidden}
  .prob-bar i{display:block;height:100%;background:var(--accent);border-radius:2px}

  .odds-grid{display:grid;grid-template-columns:1fr;gap:10px}
  .odds-book{background:var(--bg-soft);border:1px solid var(--border);border-radius:var(--radius-sm);padding:12px}
  .odds-book h4{margin:0 0 10px;font-size:12.5px;color:var(--text)}
  .odds-outcomes{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
  .odds-cell{background:#0a0f16;border:1px solid var(--border-soft);border-radius:6px;padding:8px;text-align:center}
  .odds-cell span{display:block;font-size:10.5px;color:var(--muted-2);margin-bottom:3px}
  .odds-cell strong{font-size:14px;color:var(--accent);font-weight:700}

  .spinner{width:14px;height:14px;border:2px solid rgba(255,255,255,.2);border-top-color:currentColor;border-radius:50%;animation:spin .7s linear infinite}
  @keyframes spin{to{transform:rotate(360deg)}}

  .loading-block{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px 20px;gap:10px;color:var(--muted)}
  .loading-block .spinner{width:22px;height:22px;border-width:3px;color:var(--accent)}
  .progress-steps{display:flex;flex-direction:column;gap:6px;width:100%;max-width:340px;margin-top:10px}
  .progress-step{display:flex;align-items:center;gap:8px;font-size:12.5px;color:var(--muted-2);padding:6px 10px;border-radius:6px;background:var(--bg-soft);border:1px solid var(--border)}
  .progress-step.active{color:var(--accent);border-color:rgba(34,197,94,.35);background:var(--accent-soft)}
  .progress-step.done{color:var(--muted)}
  .progress-step .dot{width:8px;height:8px;border-radius:50%;background:var(--border);flex:none}
  .progress-step.active .dot{background:var(--accent);animation:pulse 1s ease-in-out infinite}
  .progress-step.done .dot{background:var(--accent)}
  @keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}

  .rate-badge{
    display:inline-flex;align-items:center;gap:6px;padding:4px 8px;border-radius:999px;
    background:rgba(59,130,246,.1);border:1px solid rgba(59,130,246,.3);color:#93c5fd;
    font-size:11px;font-weight:500;
  }
  .rate-badge.warn{background:rgba(245,158,11,.1);border-color:rgba(245,158,11,.3);color:#fcd34d}

  @media (max-width:900px){
    .settings-fields{grid-template-columns:1fr}
    .metric-row{grid-template-columns:repeat(2,1fr)}
    .workspace{grid-template-columns:1fr}
    .page-header{grid-template-columns:1fr}
    .stats-grid{grid-template-columns:repeat(2,1fr)}
  }
  @media (max-width:520px){
    .metric-row{grid-template-columns:1fr}
    .topline-sub{display:none}
    .topline-right{font-size:11px}
    .stats-grid{grid-template-columns:1fr}
  }
</style>
</head>
<body>
<main class="site-shell">

  <div class="topline">
    <span class="mark" aria-hidden="true">
      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round">
        <path d="M22 12h-2.48a2 2 0 0 0-1.93 1.46l-2.35 8.36a.25.25 0 0 1-.48 0L9.24 2.18a.25.25 0 0 0-.48 0l-2.35 8.36A2 2 0 0 1 4.49 12H2"/>
      </svg>
    </span>
    <span class="brand">Cancha<span>Data</span></span>
    <span class="divider"></span>
    <span class="topline-sub">Laboratorio de partidos</span>
    <span class="topline-right" id="rateBadge">
      <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <circle cx="12" cy="16" r="1"/><rect x="3" y="10" width="18" height="12" rx="2"/><path d="M7 10V7a5 5 0 0 1 10 0v3"/>
      </svg>
      Claves por sesión
    </span>
  </div>

  <header class="page-header">
    <div>
      <div class="eyebrow"><span class="eyebrow-line"></span>ANÁLISIS DE FÚTBOL</div>
      <h1>Los partidos de hoy, con datos a la vista.</h1>
      <p>Consulta resultados, historial, mercados y cobertura antes de decidir. El sistema puede abstenerse cuando la evidencia es débil.</p>
    </div>
    <div class="header-actions">
      <button class="btn" type="button" id="btnClear">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M10 5H3"/><path d="M12 19H3"/><path d="M14 3v4"/><path d="M16 17v4"/>
          <path d="M21 12h-9"/><path d="M21 19h-5"/><path d="M21 5h-7"/><path d="M8 10v4"/><path d="M8 12H3"/>
        </svg>
        Limpiar claves
      </button>
      <button class="btn btn-primary" type="button" id="btnLoadTop">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"/><path d="M21 3v5h-5"/>
          <path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"/><path d="M8 16H3v5"/>
        </svg>
        Cargar partidos
      </button>
    </div>
  </header>

  <section class="settings-panel" aria-label="Conexiones">
    <div class="settings-intro">
      <div class="panel-icon" aria-hidden="true">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <circle cx="12" cy="16" r="1"/><rect x="3" y="10" width="18" height="12" rx="2"/><path d="M7 10V7a5 5 0 0 1 10 0v3"/>
        </svg>
      </div>
      <div>
        <strong>Conecta tus dos API</strong>
        <p>Las claves se guardan únicamente en esta pestaña del navegador. El plan gratuito de API-Football limita a 10 peticiones por minuto, así que las consultas se hacen en cola con caché de 24h.</p>
      </div>
    </div>

    <div class="settings-fields">
      <label>
        API-Football <span>Calendario e historial · v3.football.api-sports.io</span>
        <input type="password" id="keyFootball" autocomplete="off" placeholder="Tu clave de API-Football" />
      </label>
      <label>
        The Odds API <span>Mercados y cuotas · the-odds-api.com</span>
        <input type="password" id="keyOdds" autocomplete="off" placeholder="Tu clave de The Odds API" />
      </label>
    </div>

    <label class="toggle-row">
      <input type="checkbox" id="useOdds" />
      <span>
        <strong>Consultar cuotas al analizar un partido</strong>
        <small>Puede consumir créditos de The Odds API. Se solicitan 1X2 solo para ese encuentro.</small>
      </span>
    </label>

    <label class="toggle-row">
      <input type="checkbox" id="useCorners" checked />
      <span>
        <strong>Calcular promedio de córners (lento)</strong>
        <small>Consume hasta 6 peticiones extra por partido. Desactívalo para análisis rápido (solo goles y tarjetas).</small>
      </span>
    </label>

    <div class="settings-bottom">
      <span>Las claves no se muestran en resultados ni se escriben en archivos del sitio.</span>
      <button class="btn btn-primary btn-sm" type="button" id="btnLoadBottom">Cargar partidos de hoy</button>
    </div>

    <div class="status" id="status"></div>
  </section>

  <div class="metric-row">
    <div class="metric">
      <span>Partidos de hoy</span>
      <strong id="mTotal">—</strong>
      <small id="mTotalSub">Esperando conexión</small>
    </div>
    <div class="metric">
      <span>Analizados</span>
      <strong id="mAnalyzed">—</strong>
      <small>Consultas dosificadas por minuto</small>
    </div>
    <div class="metric">
      <span>Señales que pasan filtros</span>
      <strong id="mSignals">—</strong>
      <small>También puede ser cero</small>
    </div>
    <div class="metric metric-note">
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/>
        <path d="m9 12 2 2 4-4"/>
      </svg>
      <p>Una probabilidad calculada <b>no garantiza</b> un resultado. Revisa el rango y la muestra.</p>
    </div>
  </div>

  <div class="tabs-nav" role="tablist" aria-label="Secciones">
    <button class="tab" role="tab" aria-selected="true">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M8 2v3"/><path d="M16 2v3"/><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M3 9h18"/>
        <path d="M8 13h.01"/><path d="M12 13h.01"/><path d="M16 13h.01"/><path d="M8 17h.01"/><path d="M12 17h.01"/><path d="M16 17h.01"/>
      </svg>
      Partidos
    </button>
    <button class="tab" role="tab" aria-selected="false">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M16 7h6v6"/><path d="m22 7-8.5 8.5-5-5L2 17"/>
      </svg>
      Señales <span class="tab-count" id="signalsCount">0</span>
    </button>
    <button class="tab" role="tab" aria-selected="false">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M3 3v16a2 2 0 0 0 2 2h16"/><path d="M18 17V9"/><path d="M13 17V5"/><path d="M8 17v-3"/>
      </svg>
      Método y límites
    </button>
  </div>

  <div class="workspace">
    <section class="fixtures-panel" aria-label="Calendario">
      <div class="panel-head">
        <h2>Calendario</h2>
        <p id="fixturesCount">0 de 0 encuentros</p>
      </div>
      <div class="filters">
        <div class="search-box">
          <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="m21 21-4.34-4.34"/><circle cx="11" cy="11" r="8"/>
          </svg>
          <input type="text" id="searchInput" aria-label="Buscar equipo o liga" placeholder="Equipo o liga" />
        </div>
      </div>
      <div class="fixtures-list" id="fixturesList">
        <div class="empty-panel">
          <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M8 2v3"/><path d="M16 2v3"/><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M3 9h18"/>
            <path d="M8 13h.01"/><path d="M12 13h.01"/><path d="M16 13h.01"/><path d="M8 17h.01"/><path d="M12 17h.01"/><path d="M16 17h.01"/>
          </svg>
          <h3>Conecta API-Football</h3>
          <p>Ingresa tu clave y carga todos los partidos programados para hoy.</p>
        </div>
      </div>
    </section>

    <section class="detail-panel" aria-label="Detalle del partido">
      <div class="detail-empty" id="detailEmpty">
        <div class="detail-symbol" aria-hidden="true">
          <svg xmlns="http://www.w3.org/2000/svg" width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10"/><circle cx="12" cy="12" r="6"/><circle cx="12" cy="12" r="2"/>
          </svg>
        </div>
        <div class="eyebrow" style="justify-content:center">
          <span class="eyebrow-line"></span> ANÁLISIS POR PARTIDO
        </div>
        <h2>Selecciona un encuentro</h2>
        <p>Verás las estadísticas históricas de ambos equipos, su cobertura, las probabilidades estimadas y, si lo autorizas, las cuotas disponibles.</p>
        <div class="detail-steps">
          <span>01 · Escoge un partido</span>
          <span>02 · Revisa la muestra</span>
          <span>03 · Compara modelo y mercado</span>
        </div>
      </div>
      <div class="detail-content" id="detailContent" style="display:none"></div>
    </section>
  </div>
</main>

<script>
/* ============================================================
   CanchaData · Lógica con rate limiting y caché persistente
   ============================================================ */

const API_FOOTBALL_URL = 'https://v3.football.api-sports.io';
const ODDS_API_URL     = 'https://api.the-odds-api.com/v4';

const FREE_SEASONS = [2024, 2023, 2022];

/* Configuración de rate limit (plan gratuito: 10/min; dejamos margen) */
const RATE_LIMIT = {
  maxRequests: 8,          // margen de seguridad (10 reales)
  windowMs: 60_000,
  timestamps: [],
  chain: Promise.resolve()
};

const CACHE_TTL_MS = 24 * 60 * 60 * 1000; // 24h
const CACHE_PREFIX = 'cd.cache.';

const STORAGE_KEYS = {
  football: 'canchadata.key.football',
  odds:     'canchadata.key.odds',
  useOdds:  'canchadata.useOdds',
  useCorners: 'canchadata.useCorners'
};

const $ = (id) => document.getElementById(id);
const keyFootball   = $('keyFootball');
const keyOdds       = $('keyOdds');
const useOdds       = $('useOdds');
const useCorners    = $('useCorners');
const statusBox     = $('status');
const fixturesList  = $('fixturesList');
const fixturesCount = $('fixturesCount');
const searchInput   = $('searchInput');
const detailEmpty   = $('detailEmpty');
const detailContent = $('detailContent');
const mTotal        = $('mTotal');
const mTotalSub     = $('mTotalSub');
const mAnalyzed     = $('mAnalyzed');
const mSignals      = $('mSignals');
const signalsCount  = $('signalsCount');
const rateBadge     = $('rateBadge');

let fixtures = [];
let selectedId = null;
let analyzedCount = 0;

/* Memoria cache (además de localStorage) */
const memCache = {};

/* ============================================================
   Helpers numéricos
   ============================================================ */
function toNum(v){
  if(v === null || v === undefined || v === '') return null;
  const n = typeof v === 'number' ? v : parseFloat(String(v).replace(',', '.'));
  return Number.isFinite(n) ? n : null;
}
function fmt1(v){ const n = toNum(v); return n === null ? '—' : n.toFixed(1); }
function fmt2(v){ const n = toNum(v); return n === null ? '—' : n.toFixed(2); }

function sleep(ms){ return new Promise(r => setTimeout(r, ms)); }

/* ============================================================
   Rate limiter (ventana deslizante)
   ============================================================ */
async function throttle(){
  const p = RATE_LIMIT.chain.then(async () => {
    while(true){
      const now = Date.now();
      RATE_LIMIT.timestamps = RATE_LIMIT.timestamps.filter(t => now - t < RATE_LIMIT.windowMs);
      if(RATE_LIMIT.timestamps.length < RATE_LIMIT.maxRequests){
        RATE_LIMIT.timestamps.push(now);
        updateRateBadge();
        return;
      }
      const oldest = RATE_LIMIT.timestamps[0];
      const waitMs = RATE_LIMIT.windowMs - (now - oldest) + 400;
      updateRateBadge();
      await sleep(waitMs);
    }
  });
  RATE_LIMIT.chain = p.catch(() => {});
  return p;
}

function updateRateBadge(){
  const now = Date.now();
  RATE_LIMIT.timestamps = RATE_LIMIT.timestamps.filter(t => now - t < RATE_LIMIT.windowMs);
  const used = RATE_LIMIT.timestamps.length;
  const remaining = Math.max(0, RATE_LIMIT.maxRequests - used);
  const isWarn = remaining <= 2;
  rateBadge.className = 'topline-right' + (isWarn ? ' rate-badge warn' : '');
  rateBadge.innerHTML = `
    <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/>
    </svg>
    API-Football: ${remaining}/${RATE_LIMIT.maxRequests} disponibles`;
}

/* fetch con throttle + retry en 429 */
async function apiFetchFootball(url, key, retries = 2){
  for(let attempt = 0; attempt <= retries; attempt++){
    await throttle();
    const res = await fetch(url, { headers: { 'x-apisports-key': key } });
    if(res.status === 429){
      const waitMs = 20000 + attempt * 15000;
      console.warn(`429 recibido. Reintentando en ${waitMs/1000}s…`);
      await sleep(waitMs);
      continue;
    }
    return res;
  }
  throw new Error('Límite de peticiones excedido. Espera un minuto e inténtalo de nuevo.');
}

/* ============================================================
   Caché persistente
   ============================================================ */
function cacheGet(key){
  if(memCache[key] !== undefined) return memCache[key];
  try{
    const raw = localStorage.getItem(CACHE_PREFIX + key);
    if(!raw) return undefined;
    const obj = JSON.parse(raw);
    if(Date.now() - obj.t > CACHE_TTL_MS){
      localStorage.removeItem(CACHE_PREFIX + key);
      return undefined;
    }
    memCache[key] = obj.v;
    return obj.v;
  }catch(e){ return undefined; }
}
function cacheSet(key, value){
  memCache[key] = value;
  try{
    localStorage.setItem(CACHE_PREFIX + key, JSON.stringify({t:Date.now(), v:value}));
  }catch(e){ /* localStorage lleno: ignorar */ }
}

/* ============================================================
   Persistencia de claves
   ============================================================ */
function saveKeys(){
  try {
    localStorage.setItem(STORAGE_KEYS.football, keyFootball.value.trim());
    localStorage.setItem(STORAGE_KEYS.odds,     keyOdds.value.trim());
    localStorage.setItem(STORAGE_KEYS.useOdds,  useOdds.checked ? '1' : '0');
    localStorage.setItem(STORAGE_KEYS.useCorners, useCorners.checked ? '1' : '0');
  } catch(e){}
}
function loadKeys(){
  try {
    keyFootball.value = localStorage.getItem(STORAGE_KEYS.football) || '';
    keyOdds.value     = localStorage.getItem(STORAGE_KEYS.odds)     || '';
    useOdds.checked   = localStorage.getItem(STORAGE_KEYS.useOdds) === '1';
    const uc = localStorage.getItem(STORAGE_KEYS.useCorners);
    useCorners.checked = uc === null ? true : uc === '1';
  } catch(e){}
}
[keyFootball, keyOdds, useOdds, useCorners].forEach(el => {
  el.addEventListener('change', saveKeys);
  el.addEventListener('blur',   saveKeys);
});

/* ============================================================
   UI helpers
   ============================================================ */
function setStatus(msg, type='info'){
  statusBox.textContent = msg;
  statusBox.className = 'status show ' + type;
}
function clearStatus(){ statusBox.className = 'status'; statusBox.textContent = ''; }

function escapeHtml(s){
  return String(s||'').replace(/[&<>"']/g, c => ({
    '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'
  })[c]);
}

function todayLocalISO(){
  const d = new Date();
  const y = d.getFullYear();
  const m = String(d.getMonth()+1).padStart(2,'0');
  const day = String(d.getDate()).padStart(2,'0');
  return `${y}-${m}-${day}`;
}

function isSeasonRestricted(msg){
  const s = String(msg||'').toLowerCase();
  return s.includes('free plans do not have access to this season')
      || s.includes('do not have access to this season')
      || s.includes('try from 20');
}

function isRateLimitError(msg){
  const s = String(msg||'').toLowerCase();
  return s.includes('too many requests') || s.includes('rate limit');
}

/* ============================================================
   1) Cargar fixtures del día
   ============================================================ */
async function loadFixtures(){
  const key = keyFootball.value.trim();
  if(!key){ setStatus('Falta la clave de API-Football.', 'error'); return; }

  const btnTop = $('btnLoadTop');
  const btnBottom = $('btnLoadBottom');
  btnTop.disabled = true; btnBottom.disabled = true;
  btnTop.innerHTML = '<span class="spinner"></span> Cargando…';
  btnBottom.innerHTML = '<span class="spinner"></span> Cargando…';
  setStatus('Consultando API-Football…', 'info');

  const date = todayLocalISO();
  const url = `${API_FOOTBALL_URL}/fixtures?date=${date}`;

  try {
    const res = await apiFetchFootball(url, key);
    if(!res.ok) throw new Error(`API-Football respondió ${res.status}`);
    const data = await res.json();
    if(data.errors && Object.keys(data.errors).length){
      throw new Error('API-Football: ' + Object.values(data.errors).join(' · '));
    }

    fixtures = (data.response || []).map(f => ({
      id: f.fixture.id,
      date: f.fixture.date,
      statusShort: f.fixture.status.short,
      statusLong: f.fixture.status.long,
      elapsed: f.fixture.status.elapsed,
      venue: f.fixture.venue?.name || '',
      city: f.fixture.venue?.city || '',
      leagueId: f.league?.id,
      league: f.league?.name || 'Liga desconocida',
      leagueCountry: f.league?.country || '',
      leagueLogo: f.league?.logo || '',
      season: f.league?.season,
      homeId: f.teams?.home?.id,
      awayId: f.teams?.away?.id,
      home: f.teams?.home?.name || 'Local',
      away: f.teams?.away?.name || 'Visitante',
      homeLogo: f.teams?.home?.logo || '',
      awayLogo: f.teams?.away?.logo || '',
      goalsHome: f.goals?.home,
      goalsAway: f.goals?.away
    }));

    analyzedCount = 0;
    renderFixtures();
    updateMetrics();
    setStatus(`Se cargaron ${fixtures.length} partidos para ${date}.`, 'success');
    clearDetail();
  } catch(err){
    console.error(err);
    const msg = isRateLimitError(err.message)
      ? 'Límite de peticiones alcanzado. Espera un minuto y vuelve a intentar.'
      : err.message;
    setStatus('Error al cargar partidos: ' + msg, 'error');
  } finally {
    btnTop.disabled = false; btnBottom.disabled = false;
    btnTop.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"/><path d="M8 16H3v5"/></svg> Cargar partidos`;
    btnBottom.innerHTML = 'Cargar partidos de hoy';
  }
}

/* ============================================================
   2) Render lista
   ============================================================ */
function renderFixtures(){
  const q = searchInput.value.trim().toLowerCase();
  const list = fixtures.filter(f => {
    if(!q) return true;
    return f.home.toLowerCase().includes(q)
        || f.away.toLowerCase().includes(q)
        || f.league.toLowerCase().includes(q);
  });

  fixturesCount.textContent = `${list.length} de ${fixtures.length} encuentros`;

  if(!list.length){
    fixturesList.innerHTML = `<div class="empty-panel"><h3>Sin resultados</h3><p>No hay partidos que coincidan con la búsqueda.</p></div>`;
    return;
  }

  fixturesList.innerHTML = list.map(f => {
    const time = new Date(f.date).toLocaleTimeString('es-PE', {hour:'2-digit', minute:'2-digit'});
    const score = (f.goalsHome !== null && f.goalsAway !== null)
      ? `<span class="fixture-score">${f.goalsHome} - ${f.goalsAway}</span>` : '';
    return `
      <div class="fixture ${f.id===selectedId?'selected':''}" data-id="${f.id}">
        <div class="fixture-time">${time} · ${f.statusShort}</div>
        <div class="fixture-teams">
          ${f.homeLogo?`<img src="${f.homeLogo}" alt="">`:''}
          <span>${escapeHtml(f.home)}</span>
          <span class="vs">vs</span>
          <span>${escapeHtml(f.away)}</span>
          ${f.awayLogo?`<img src="${f.awayLogo}" alt="">`:''}
          ${score}
        </div>
        <div class="fixture-league">${escapeHtml(f.league)}${f.leagueCountry?' · '+escapeHtml(f.leagueCountry):''}</div>
      </div>`;
  }).join('');

  fixturesList.querySelectorAll('.fixture').forEach(el => {
    el.addEventListener('click', () => selectFixture(Number(el.dataset.id)));
  });
}

/* ============================================================
   3) Selección de partido con progreso
   ============================================================ */
async function selectFixture(id){
  selectedId = id;
  renderFixtures();
  const f = fixtures.find(x => x.id === id);
  if(!f) return;

  detailEmpty.style.display = 'none';
  detailContent.style.display = 'block';

  // Estructura de progreso
  const steps = useCorners.checked
    ? ['Stats equipo local','Stats equipo visitante','Córners local','Córners visitante','Cálculo final']
    : ['Stats equipo local','Stats equipo visitante','Cálculo final'];

  detailContent.innerHTML = renderProgress(steps, 0);
  analyzedCount++;
  updateMetrics();

  try {
    await loadMatchDetail(f, (idx) => {
      detailContent.innerHTML = renderProgress(steps, idx);
    });
  } catch(err){
    console.error(err);
    const msg = isRateLimitError(err.message)
      ? 'Límite de peticiones alcanzado. Espera un minuto y vuelve a intentarlo.'
      : err.message;
    detailContent.innerHTML = `
      <div style="padding:30px;text-align:center;color:var(--muted)">
        <p style="color:#fca5a5">Error al cargar el detalle: ${escapeHtml(msg)}</p>
        <button class="btn btn-sm" onclick="selectFixture(${f.id})" style="margin-top:12px">Reintentar</button>
      </div>`;
  }
}

function renderProgress(steps, activeIdx){
  return `
    <div class="loading-block">
      <div class="spinner"></div>
      <p>Analizando partido…</p>
      <small style="color:var(--muted-2)">El plan gratuito limita a 10 peticiones/minuto. Esto puede tardar.</small>
      <div class="progress-steps">
        ${steps.map((s, i) => {
          const state = i < activeIdx ? 'done' : (i === activeIdx ? 'active' : '');
          return `<div class="progress-step ${state}"><span class="dot"></span>${escapeHtml(s)}</div>`;
        }).join('')}
      </div>
    </div>`;
}

function clearDetail(){
  detailEmpty.style.display = 'flex';
  detailContent.style.display = 'none';
  detailContent.innerHTML = '';
  selectedId = null;
}

/* ============================================================
   4) Detalle con progreso
   ============================================================ */
async function loadMatchDetail(f, onProgress){
  const key = keyFootball.value.trim();
  if(!key) throw new Error('Falta la clave de API-Football.');

  const seasonsToTry = [f.season, ...FREE_SEASONS.filter(s => s !== f.season)];

  let homeStats = null, awayStats = null;
  let seasonUsed = null;
  let lastError = null;

  // --- Stats equipo local ---
  onProgress?.(0);
  for(const season of seasonsToTry){
    try {
      homeStats = await getTeamStats(f.homeId, f.leagueId, season, key);
      seasonUsed = season;
      break;
    } catch(err){
      lastError = err;
      if(!isSeasonRestricted(err.message)) throw err;
    }
  }
  if(!homeStats) throw new Error(lastError ? lastError.message : 'No hay stats del equipo local.');

  // --- Stats equipo visitante ---
  onProgress?.(1);
  for(const season of seasonsToTry){
    try {
      awayStats = await getTeamStats(f.awayId, f.leagueId, season, key);
      if(seasonUsed && season !== seasonUsed){
        // preferimos usar la misma temporada para ambos
      }
      break;
    } catch(err){
      lastError = err;
      if(!isSeasonRestricted(err.message)) throw err;
    }
  }
  if(!awayStats) throw new Error(lastError ? lastError.message : 'No hay stats del equipo visitante.');

  // --- Córners (opcional) ---
  let homeCorners = null, awayCorners = null;
  if(useCorners.checked){
    onProgress?.(2);
    homeCorners = await getAverageCorners(f.homeId, f.leagueId, seasonUsed, key);
    onProgress?.(3);
    awayCorners = await getAverageCorners(f.awayId, f.leagueId, seasonUsed, key);
  }

  onProgress?.(useCorners.checked ? 4 : 2);

  // --- Cálculo ---
  const homeFor     = toNum(homeStats.goals?.for?.average?.total)     ?? 1.2;
  const awayFor     = toNum(awayStats.goals?.for?.average?.total)     ?? 1.0;
  const homeAgainst = toNum(homeStats.goals?.against?.average?.total) ?? 1.2;
  const awayAgainst = toNum(awayStats.goals?.against?.average?.total) ?? 1.2;

  const lambdaHomeAdj = (homeFor + awayAgainst) / 2;
  const lambdaAwayAdj = (awayFor + homeAgainst) / 2;
  const probs = calculatePoissonProbabilities(lambdaHomeAdj, lambdaAwayAdj);

  renderDetail(f, {
    homeStats, awayStats,
    homeCorners, awayCorners,
    lambdaHome: lambdaHomeAdj, lambdaAway: lambdaAwayAdj,
    probs,
    seasonUsed,
    seasonFallback: seasonUsed !== f.season
  });

  if(useOdds.checked && keyOdds.value.trim()){
    await loadOddsFor(f);
  }
}

/* ============================================================
   5) Stats de equipo con caché persistente
   ============================================================ */
async function getTeamStats(teamId, leagueId, season, key){
  const cacheKey = `stats-${leagueId}-${teamId}-${season}`;
  const cached = cacheGet(cacheKey);
  if(cached !== undefined) return cached;

  const url = `${API_FOOTBALL_URL}/teams/statistics?team=${teamId}&league=${leagueId}&season=${season}`;
  const res = await apiFetchFootball(url, key);
  if(!res.ok) throw new Error(`Stats equipo ${teamId}: HTTP ${res.status}`);
  const data = await res.json();

  if(data.errors && Object.keys(data.errors).length){
    throw new Error('API-Football stats: ' + Object.values(data.errors).join(' · '));
  }

  const stats = data.response || {};
  cacheSet(cacheKey, stats);
  return stats;
}

/* ============================================================
   6) Córners (3 partidos, con caché)
   ============================================================ */
async function getAverageCorners(teamId, leagueId, season, key){
  const cacheKey = `corners-${leagueId}-${teamId}-${season}`;
  const cached = cacheGet(cacheKey);
  if(cached !== undefined) return cached;

  try {
    const fixturesUrl = `${API_FOOTBALL_URL}/fixtures?team=${teamId}&league=${leagueId}&season=${season}&status=FT`;
    const fixRes = await apiFetchFootball(fixturesUrl, key);
    if(!fixRes.ok){ cacheSet(cacheKey, null); return null; }
    const fixData = await fixRes.json();

    if(fixData.errors && Object.keys(fixData.errors).length){
      cacheSet(cacheKey, null);
      return null;
    }

    // Muestra reducida a 3 partidos para no agotar la cuota
    const finished = (fixData.response || []).slice(0, 3);
    if(!finished.length){ cacheSet(cacheKey, null); return null; }

    let totalCorners = 0;
    let counted = 0;

    for(const f of finished){
      try {
        const statUrl = `${API_FOOTBALL_URL}/fixtures/statistics?fixture=${f.fixture.id}`;
        const stRes = await apiFetchFootball(statUrl, key);
        if(!stRes.ok) continue;
        const stData = await stRes.json();
        const teamStats = (stData.response || []).find(s => s.team?.id === teamId);
        if(!teamStats) continue;
        const cornerStat = teamStats.statistics?.find(s => s.type === 'Corner Kicks');
        const cornerVal = toNum(cornerStat?.value);
        if(cornerVal !== null){
          totalCorners += cornerVal;
          counted++;
        }
      } catch(e){}
    }

    const avg = counted > 0 ? (totalCorners / counted) : null;
    cacheSet(cacheKey, avg);
    return avg;
  } catch(e){
    cacheSet(cacheKey, null);
    return null;
  }
}

/* ============================================================
   7) Poisson
   ============================================================ */
function poissonProbability(lambda, goals){
  const l = toNum(lambda);
  if(l === null || l <= 0) return goals === 0 ? 1 : 0;
  return (Math.pow(l, goals) * Math.exp(-l)) / factorial(goals);
}
function factorial(n){
  let r = 1;
  for(let i = 2; i <= n; i++) r *= i;
  return r;
}
function calculatePoissonProbabilities(lambdaHome, lambdaAway){
  const lh = toNum(lambdaHome) ?? 1.2;
  const la = toNum(lambdaAway) ?? 1.0;
  const maxGoals = 8;
  const probHome = [], probAway = [];
  for(let i = 0; i <= maxGoals; i++){
    probHome[i] = poissonProbability(lh, i);
    probAway[i] = poissonProbability(la, i);
  }
  let homeWin = 0, draw = 0, awayWin = 0, over25 = 0, btts = 0;
  for(let h = 0; h <= maxGoals; h++){
    for(let a = 0; a <= maxGoals; a++){
      const p = probHome[h] * probAway[a];
      if(h > a) homeWin += p;
      else if(h === a) draw += p;
      else awayWin += p;
      if(h + a > 2.5) over25 += p;
      if(h > 0 && a > 0) btts += p;
    }
  }
  const total = homeWin + draw + awayWin || 1;
  return {
    homeWin: (homeWin / total) * 100,
    draw:    (draw    / total) * 100,
    awayWin: (awayWin / total) * 100,
    over25:  (over25  / total) * 100,
    btts:    (btts    / total) * 100
  };
}

/* ============================================================
   8) Render detalle
   ============================================================ */
function renderDetail(f, data){
  const { homeStats, awayStats, homeCorners, awayCorners, lambdaHome, lambdaAway, probs, seasonUsed, seasonFallback } = data;

  const dateStr = new Date(f.date).toLocaleString('es-PE', {
    weekday:'long', day:'2-digit', month:'long', hour:'2-digit', minute:'2-digit'
  });

  const homeGoalsAvg = fmt2(homeStats.goals?.for?.average?.total);
  const awayGoalsAvg = fmt2(awayStats.goals?.for?.average?.total);
  const homeYellow   = homeStats.cards?.yellow?.total ?? '—';
  const awayYellow   = awayStats.cards?.yellow?.total ?? '—';
  const homePlayed   = homeStats.fixtures?.played?.total ?? '—';
  const awayPlayed   = awayStats.fixtures?.played?.total ?? '—';

  const pct  = (v) => v != null ? v.toFixed(1) + '%' : '—';
  const barW = (v) => Math.min(100, Math.max(0, v || 0));

  const fallbackNote = seasonFallback
    ? `<div class="season-note">
         <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
           <path d="M10.29 3.86 1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/>
           <line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/>
         </svg>
         El plan gratuito no permite la temporada ${f.season}. Se muestran estadísticas de la temporada <b>${seasonUsed}</b>.
       </div>`
    : '';

  const cornersCards = useCorners.checked ? `
      <div class="stat-card">
        <span>Córners/partido ${escapeHtml(f.home)}</span>
        <strong>${fmt1(homeCorners)}</strong>
        <small>últimos 3 partidos</small>
      </div>
      <div class="stat-card">
        <span>Córners/partido ${escapeHtml(f.away)}</span>
        <strong>${fmt1(awayCorners)}</strong>
        <small>últimos 3 partidos</small>
      </div>` : '';

  detailContent.innerHTML = `
    <div class="detail-header">
      <div class="detail-team">
        ${f.homeLogo?`<img src="${f.homeLogo}" alt="">`:''}
        <strong>${escapeHtml(f.home)}</strong>
        <small>Local</small>
      </div>
      <div class="detail-vs">VS</div>
      <div class="detail-team">
        ${f.awayLogo?`<img src="${f.awayLogo}" alt="">`:''}
        <strong>${escapeHtml(f.away)}</strong>
        <small>Visitante</small>
      </div>
    </div>

    <div class="detail-meta">
      <div><span>Liga</span><strong>${escapeHtml(f.league)}</strong></div>
      <div><span>País</span><strong>${escapeHtml(f.leagueCountry)||'—'}</strong></div>
      <div><span>Fecha</span><strong>${dateStr}</strong></div>
      <div><span>Estado</span><strong>${escapeHtml(f.statusLong)} ${f.elapsed?`(${f.elapsed}')`:''}</strong></div>
      <div><span>Estadio</span><strong>${escapeHtml(f.venue)||'—'}</strong></div>
      <div><span>Ciudad</span><strong>${escapeHtml(f.city)||'—'}</strong></div>
    </div>

    ${fallbackNote}

    <h3 class="detail-section-title">Tendencias históricas (temporada ${seasonUsed})</h3>
    <div class="stats-grid">
      <div class="stat-card">
        <span>Goles/partido ${escapeHtml(f.home)}</span>
        <strong>${homeGoalsAvg}</strong>
        <small>${homePlayed} partidos</small>
      </div>
      <div class="stat-card">
        <span>Goles/partido ${escapeHtml(f.away)}</span>
        <strong>${awayGoalsAvg}</strong>
        <small>${awayPlayed} partidos</small>
      </div>
      ${cornersCards}
      <div class="stat-card">
        <span>Amarillas ${escapeHtml(f.home)}</span>
        <strong>${homeYellow}</strong>
        <small>temporada</small>
      </div>
      <div class="stat-card">
        <span>Amarillas ${escapeHtml(f.away)}</span>
        <strong>${awayYellow}</strong>
        <small>temporada</small>
      </div>
    </div>

    <h3 class="detail-section-title">Probabilidades calculadas (Poisson)</h3>
    <div class="prob-list">
      <div class="prob-item">
        <span class="label">Gana ${escapeHtml(f.home)}</span>
        <span class="value">${pct(probs.homeWin)}</span>
        <span class="prob-bar"><i style="width:${barW(probs.homeWin)}%"></i></span>
      </div>
      <div class="prob-item">
        <span class="label">Empate</span>
        <span class="value">${pct(probs.draw)}</span>
        <span class="prob-bar"><i style="width:${barW(probs.draw)}%"></i></span>
      </div>
      <div class="prob-item">
        <span class="label">Gana ${escapeHtml(f.away)}</span>
        <span class="value">${pct(probs.awayWin)}</span>
        <span class="prob-bar"><i style="width:${barW(probs.awayWin)}%"></i></span>
      </div>
      <div class="prob-item">
        <span class="label">Más de 2.5 goles</span>
        <span class="value">${pct(probs.over25)}</span>
        <span class="prob-bar"><i style="width:${barW(probs.over25)}%"></i></span>
      </div>
      <div class="prob-item">
        <span class="label">Ambos equipos anotan</span>
        <span class="value">${pct(probs.btts)}</span>
        <span class="prob-bar"><i style="width:${barW(probs.btts)}%"></i></span>
      </div>
    </div>

    <p style="color:var(--muted-2);font-size:11.5px;margin-top:14px">
      λ Local ≈ ${fmt2(lambdaHome)} · λ Visitante ≈ ${fmt2(lambdaAway)} · Muestra: temporada ${seasonUsed}
    </p>

    <div id="oddsBlock" style="margin-top:22px"></div>
  `;
}

/* ============================================================
   9) The Odds API
   ============================================================ */
async function loadOddsFor(fixture){
  const key = keyOdds.value.trim();
  if(!key) return;

  const oddsBlock = $('oddsBlock');
  if(!oddsBlock) return;

  oddsBlock.innerHTML = `
    <h3 class="detail-section-title">Cuotas de mercado</h3>
    <div class="loading-block" style="padding:20px">
      <div class="spinner"></div>
      <p>Consultando The Odds API…</p>
    </div>`;

  const url = `${ODDS_API_URL}/sports/soccer/odds/?apiKey=${encodeURIComponent(key)}&regions=eu&markets=h2h&oddsFormat=decimal`;

  try {
    const res = await fetch(url);
    if(!res.ok){
      const txt = await res.text();
      throw new Error(`Odds API respondió ${res.status}: ${txt.slice(0,120)}`);
    }
    const events = await res.json();

    const homeLc = f.home.toLowerCase();
    const awayLc = f.away.toLowerCase();
    const match = events.find(e => {
      const h = (e.home_team||'').toLowerCase();
      const a = (e.away_team||'').toLowerCase();
      return (h.includes(homeLc) || homeLc.includes(h))
          && (a.includes(awayLc) || awayLc.includes(a));
    });

    if(!match){
      oddsBlock.innerHTML = `<h3 class="detail-section-title">Cuotas de mercado</h3><p style="color:var(--muted);font-size:13px">No hay cuotas publicadas para este partido en este momento.</p>`;
      return;
    }

    const books = (match.bookmakers || []).slice(0,5);
    if(!books.length){
      oddsBlock.innerHTML = `<h3 class="detail-section-title">Cuotas de mercado</h3><p style="color:var(--muted);font-size:13px">Sin casas de apuestas disponibles en la región consultada.</p>`;
      return;
    }

    const bookHtml = books.map(b => {
      const market = (b.markets||[]).find(m => m.key === 'h2h');
      const outcomes = market?.outcomes || [];
      const cells = outcomes.map(o => `
        <div class="odds-cell">
          <span>${escapeHtml(o.name)}</span>
          <strong>${o.price != null ? Number(o.price).toFixed(2) : '—'}</strong>
        </div>`).join('');
      return `
        <div class="odds-book">
          <h4>${escapeHtml(b.title)}</h4>
          <div class="odds-outcomes">${cells || '<p style="color:var(--muted-2);font-size:12px">Sin mercados 1X2</p>'}</div>
        </div>`;
    }).join('');

    oddsBlock.innerHTML = `
      <h3 class="detail-section-title">Cuotas de mercado · 1X2 (decimal)</h3>
      <div class="odds-grid">${bookHtml}</div>
      <p style="color:var(--muted-2);font-size:11.5px;margin-top:12px">Fuente: The Odds API · Región EU · Puede consumir créditos de tu cuenta.</p>`;
  } catch(err){
    console.error(err);
    oddsBlock.innerHTML = `<h3 class="detail-section-title">Cuotas de mercado</h3><p style="color:#fca5a5;font-size:13px">Error consultando cuotas: ${escapeHtml(err.message)}</p>`;
  }
}

/* ============================================================
   10) Métricas
   ============================================================ */
function updateMetrics(){
  mTotal.textContent = fixtures.length || '—';
  mTotalSub.textContent = fixtures.length ? 'Partidos cargados' : 'Esperando conexión';
  mAnalyzed.textContent = analyzedCount || '—';
  mSignals.textContent = '0';
  signalsCount.textContent = '0';
}

/* ============================================================
   11) Eventos
   ============================================================ */
$('btnLoadTop').addEventListener('click', loadFixtures);
$('btnLoadBottom').addEventListener('click', loadFixtures);
$('btnClear').addEventListener('click', () => {
  if(!confirm('¿Borrar claves y caché guardadas en este navegador?')) return;
  try{
    Object.keys(localStorage).forEach(k => {
      if(k.startsWith(CACHE_PREFIX) || k === STORAGE_KEYS.football || k === STORAGE_KEYS.odds || k === STORAGE_KEYS.useOdds || k === STORAGE_KEYS.useCorners){
        localStorage.removeItem(k);
      }
    });
    Object.keys(memCache).forEach(k => delete memCache[k]);
    RATE_LIMIT.timestamps = [];
  }catch(e){}
  keyFootball.value = '';
  keyOdds.value = '';
  useOdds.checked = false;
  useCorners.checked = true;
  fixtures = [];
  selectedId = null;
  analyzedCount = 0;
  renderFixtures();
  updateMetrics();
  clearDetail();
  clearStatus();
  updateRateBadge();
  setStatus('Claves y caché eliminadas.', 'info');
});

searchInput.addEventListener('input', renderFixtures);

/* Refresca el badge cada 5s (va bajando el contador) */
setInterval(updateRateBadge, 5000);

/* ============================================================
   Init
   ============================================================ */
loadKeys();
updateMetrics();
updateRateBadge();
</script>
</body>
</html>
