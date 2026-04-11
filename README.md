<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Metron</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f4f5f7;color:#1a1a2e;font-size:14px}
.app{display:flex;height:100vh;overflow:hidden}
.sidebar{width:224px;background:#1a1a2e;display:flex;flex-direction:column;flex-shrink:0;transition:transform .25s;z-index:100;overflow-y:auto}
.logo{padding:18px 16px;border-bottom:1px solid rgba(255,255,255,.1);display:flex;align-items:center;justify-content:space-between}
.logo-text{font-size:17px;font-weight:700;color:#fff;letter-spacing:-.3px}
.logo-sub{font-size:10px;color:rgba(255,255,255,.4);margin-top:2px}
.nav-section{padding:14px 8px 4px;font-size:10px;font-weight:600;color:rgba(255,255,255,.3);letter-spacing:.8px;text-transform:uppercase}
.nav-item{display:flex;align-items:center;gap:10px;padding:9px 12px;border-radius:8px;cursor:pointer;color:rgba(255,255,255,.6);margin:1px 8px;font-size:13px;transition:all .15s}
.nav-item:hover{background:rgba(255,255,255,.08);color:#fff}
.nav-item.active{background:rgba(83,74,183,.4);color:#fff}
.nav-icon{width:18px;text-align:center;font-size:14px}
.close-sb{display:none;background:none;border:none;color:rgba(255,255,255,.5);font-size:20px;cursor:pointer}
.main{flex:1;overflow-y:auto;display:flex;flex-direction:column}
.topbar{background:#fff;border-bottom:1px solid #e8e8ec;padding:13px 20px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:10}
.topbar-left{display:flex;align-items:center;gap:10px}
.hamburger{display:none;background:none;border:none;font-size:22px;cursor:pointer;color:#1a1a2e}
.page-title{font-size:15px;font-weight:600;color:#1a1a2e}
.btn{padding:7px 13px;border-radius:8px;border:1.5px solid #e0e0e8;background:#fff;cursor:pointer;font-size:13px;color:#1a1a2e;transition:all .15s;font-family:inherit;white-space:nowrap}
.btn:hover{background:#f4f5f7}
.btn-primary{background:#534AB7;color:#fff;border-color:#534AB7}
.btn-primary:hover{background:#4340a0}
.btn-danger{color:#d63031;border-color:#d63031}
.btn-success{color:#00b894;border-color:#00b894}
.btn-sm{padding:5px 10px;font-size:12px}
.content{padding:18px;flex:1}
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:99}
.overlay.show{display:block}
.metrics-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:18px}
.metric-card{background:#fff;border-radius:12px;padding:14px 16px;border:1px solid #e8e8ec}
.metric-label{font-size:11px;color:#6b7280;font-weight:500;margin-bottom:4px}
.metric-value{font-size:21px;font-weight:700;color:#1a1a2e;letter-spacing:-.5px}
.metric-delta{font-size:11px;margin-top:3px}
.delta-up{color:#00b894}.delta-down{color:#d63031}.delta-neutral{color:#6b7280}
.card{background:#fff;border-radius:12px;border:1px solid #e8e8ec;margin-bottom:16px}
.card-header{padding:14px 18px;border-bottom:1px solid #f0f0f4;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
.card-title{font-size:14px;font-weight:600;color:#1a1a2e}
.card-sub{font-size:12px;color:#6b7280;margin-top:2px}
.card-body{padding:16px 18px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
.chart-container{position:relative;height:200px}
.table-wrap{overflow-x:auto;-webkit-overflow-scrolling:touch}
table{width:100%;border-collapse:collapse;min-width:560px}
th{padding:9px 14px;text-align:left;font-size:11px;font-weight:600;color:#9ca3af;background:#fafafa;text-transform:uppercase;letter-spacing:.4px;white-space:nowrap}
td{padding:11px 14px;border-bottom:1px solid #f4f5f7;font-size:13px;color:#374151}
tr:last-child td{border-bottom:none}
tr:hover td{background:#fafafe}
.badge{display:inline-flex;align-items:center;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:600}
.badge-active{background:#d1fae5;color:#065f46}
.badge-paused{background:#f3f4f6;color:#6b7280}
.badge-learning{background:#dbeafe;color:#1e40af}
.badge-risk{background:#fee2e2;color:#991b1b}
.badge-ok{background:#d1fae5;color:#065f46}
.badge-warn{background:#fef3c7;color:#92400e}
.progress-bar{height:4px;background:#f0f0f4;border-radius:4px;margin-top:4px}
.progress-fill{height:100%;border-radius:4px}
.rule-item{display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid #f4f5f7}
.rule-item:last-child{border-bottom:none}
.rule-icon-wrap{width:34px;height:34px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0}
.icon-red{background:#fee2e2}.icon-green{background:#d1fae5}.icon-yellow{background:#fef3c7}.icon-blue{background:#dbeafe}.icon-purple{background:#ede9fe}
.rule-info{flex:1;min-width:0}
.rule-title{font-size:13px;font-weight:600;color:#1a1a2e;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.rule-desc{font-size:11px;color:#6b7280;margin-top:2px}
.rule-log{font-size:10px;color:#9ca3af;margin-top:2px}
.toggle-switch{position:relative;width:38px;height:20px;flex-shrink:0}
.toggle-switch input{opacity:0;width:0;height:0}
.toggle-track{position:absolute;inset:0;background:#d1d5db;border-radius:20px;cursor:pointer;transition:background .2s}
.toggle-track:before{content:'';position:absolute;height:16px;width:16px;left:2px;top:2px;background:#fff;border-radius:50%;transition:transform .2s;box-shadow:0 1px 3px rgba(0,0,0,.2)}
.toggle-switch input:checked+.toggle-track{background:#534AB7}
.toggle-switch input:checked+.toggle-track:before{transform:translateX(18px)}
.ai-chat{display:flex;flex-direction:column;height:360px}
.ai-messages{flex:1;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:10px}
.msg{max-width:85%;padding:10px 13px;border-radius:12px;font-size:13px;line-height:1.55}
.msg-user{background:#534AB7;color:#fff;align-self:flex-end;border-bottom-right-radius:4px}
.msg-ai{background:#f4f5f7;color:#1a1a2e;align-self:flex-start;border-bottom-left-radius:4px}
.msg-ai.loading{color:#9ca3af}
.ai-input-row{display:flex;gap:8px;padding:10px 14px;border-top:1px solid #f0f0f4}
.ai-input{flex:1;padding:9px 13px;border-radius:8px;border:1.5px solid #e0e0e8;font-size:13px;font-family:inherit;outline:none;min-width:0}
.ai-input:focus{border-color:#534AB7}
.opt-item{display:flex;align-items:flex-start;gap:12px;padding:12px 0;border-bottom:1px solid #f4f5f7;flex-wrap:wrap}
.opt-item:last-child{border-bottom:none}
.opt-priority{width:7px;height:7px;border-radius:50%;margin-top:4px;flex-shrink:0}
.priority-high{background:#ef4444}.priority-mid{background:#f59e0b}.priority-low{background:#10b981}
.opt-body{flex:1;min-width:160px}
.opt-title{font-size:13px;font-weight:600;color:#1a1a2e}
.opt-desc{font-size:12px;color:#6b7280;margin-top:3px}
.opt-impact{font-size:11px;font-weight:600;margin-top:4px}
.impact-high{color:#ef4444}.impact-mid{color:#f59e0b}.impact-low{color:#10b981}
.alert-bar{background:#fffbeb;border:1px solid #fcd34d;border-radius:10px;padding:10px 14px;display:flex;align-items:flex-start;gap:8px;margin-bottom:14px;font-size:12px;color:#92400e;line-height:1.5}
.stat-row{display:flex;justify-content:space-between;align-items:center;padding:9px 0;border-bottom:1px solid #f4f5f7;font-size:13px;gap:8px}
.stat-row:last-child{border-bottom:none}
.stat-label{color:#6b7280}
.stat-val{font-weight:600;color:#1a1a2e;text-align:right}
.quick-btns{display:flex;flex-wrap:wrap;gap:8px}
.section{display:none}.section.active{display:block}

/* ML CARDS */
.pred-card{border:1px solid #e8e8ec;border-radius:12px;padding:14px;background:#fff}
.pred-score{font-size:28px;font-weight:700;letter-spacing:-.5px}
.pred-label{font-size:11px;color:#6b7280;margin-top:2px}
.pred-bar{height:6px;border-radius:6px;background:#f0f0f4;margin:10px 0}
.pred-fill{height:100%;border-radius:6px}
.risk-high{color:#ef4444}.risk-mid{color:#f59e0b}.risk-low{color:#10b981}
.insight-item{display:flex;gap:10px;padding:10px 0;border-bottom:1px solid #f4f5f7;align-items:flex-start}
.insight-item:last-child{border-bottom:none}
.insight-dot{width:8px;height:8px;border-radius:50%;margin-top:4px;flex-shrink:0}

/* CREATIVE */
.creative-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.creative-card{border:1px solid #e8e8ec;border-radius:12px;overflow:hidden;background:#fff}
.creative-thumb{height:100px;display:flex;align-items:center;justify-content:center;font-size:32px}
.creative-body{padding:12px}
.creative-name{font-size:12px;font-weight:600;color:#1a1a2e;margin-bottom:6px}
.creative-score{display:flex;align-items:center;gap:6px;margin-bottom:8px}
.score-bar{flex:1;height:5px;background:#f0f0f4;border-radius:5px}
.score-fill{height:100%;border-radius:5px}
.creative-tags{display:flex;flex-wrap:wrap;gap:4px}
.tag{padding:2px 7px;background:#f3f4f6;border-radius:6px;font-size:10px;color:#6b7280}
.tag-good{background:#d1fae5;color:#065f46}
.tag-bad{background:#fee2e2;color:#991b1b}
.upload-zone{border:2px dashed #e0e0e8;border-radius:12px;padding:32px;text-align:center;cursor:pointer;transition:all .2s}
.upload-zone:hover{border-color:#534AB7;background:#f8f7ff}
.upload-icon{font-size:32px;margin-bottom:8px}
.upload-text{font-size:13px;color:#6b7280}

/* LANCES */
.lance-card{border:1px solid #e8e8ec;border-radius:12px;padding:16px;background:#fff;margin-bottom:12px}
.lance-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
.lance-name{font-size:13px;font-weight:600;color:#1a1a2e}
.lance-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:12px}
.lance-metric{background:#f9fafb;border-radius:8px;padding:8px;text-align:center}
.lance-metric-val{font-size:15px;font-weight:700;color:#1a1a2e}
.lance-metric-label{font-size:10px;color:#9ca3af;margin-top:2px}
.daypart-grid{display:grid;grid-template-columns:repeat(8,1fr);gap:4px;margin-top:8px}
.hour-cell{height:28px;border-radius:4px;display:flex;align-items:center;justify-content:center;font-size:9px;color:#fff;font-weight:600;cursor:pointer}

/* MOBILE CAMPS */
.camp-cards{display:none;flex-direction:column;gap:10px}
.camp-card{background:#fff;border:1px solid #e8e8ec;border-radius:12px;padding:14px}
.camp-card-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:10px}
.camp-card-name{font-size:13px;font-weight:600}
.camp-card-type{font-size:11px;color:#9ca3af;margin-top:1px}
.camp-card-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:10px}
.camp-metric{background:#f9fafb;border-radius:8px;padding:8px}
.camp-metric-label{font-size:10px;color:#9ca3af}
.camp-metric-value{font-size:14px;font-weight:700;color:#1a1a2e;margin-top:2px}
.camp-card-actions{display:flex;gap:8px}

@media(max-width:768px){
  .sidebar{position:fixed;top:0;left:0;height:100%;transform:translateX(-100%)}
  .sidebar.open{transform:translateX(0)}
  .hamburger{display:block}
  .close-sb{display:block}
  .metrics-grid{grid-template-columns:repeat(2,1fr);gap:10px}
  .metric-value{font-size:18px}
  .grid2,.grid3{grid-template-columns:1fr}
  .creative-grid{grid-template-columns:repeat(2,1fr)}
  .table-wrap table{display:none}
  .camp-cards{display:flex}
  .content{padding:12px}
  .card-body{padding:14px}
  .daypart-grid{grid-template-columns:repeat(6,1fr)}
}
@media(max-width:400px){
  .creative-grid{grid-template-columns:1fr 1fr}
  .lance-metrics{grid-template-columns:repeat(3,1fr)}
}
</style>
</head>
<body>
<div class="overlay" id="overlay" onclick="closeSB()"></div>
<div class="app">
<div class="sidebar" id="sidebar">
  <div class="logo">
    <div><div class="logo-text">Metron</div><div class="logo-sub">Powered by Claude AI</div></div>
    <button class="close-sb" onclick="closeSB()">✕</button>
  </div>
  <div class="nav-section">Principal</div>
  <div class="nav-item active" onclick="nav('dashboard',this)"><span class="nav-icon">📊</span>Dashboard</div>
  <div class="nav-item" onclick="nav('campanhas',this)"><span class="nav-icon">📢</span>Campanhas</div>
  <div class="nav-section">Inteligência</div>
  <div class="nav-item" onclick="nav('ml',this)"><span class="nav-icon">🧠</span>Machine Learning</div>
  <div class="nav-item" onclick="nav('criativos',this)"><span class="nav-icon">🎨</span>Análise de Criativos</div>
  <div class="nav-item" onclick="nav('lances',this)"><span class="nav-icon">⚡</span>Otimização de Lances</div>
  <div class="nav-section">Ferramentas</div>
  <div class="nav-item" onclick="nav('otimizacao',this)"><span class="nav-icon">🎯</span>Otimização</div>
  <div class="nav-item" onclick="nav('automacao',this)"><span class="nav-icon">🔁</span>Automação</div>
  <div class="nav-item" onclick="nav('ia',this)"><span class="nav-icon">🤖</span>Assistente IA</div>
</div>

<div class="main">
  <div class="topbar">
    <div class="topbar-left">
      <button class="hamburger" onclick="openSB()">☰</button>
      <div class="page-title" id="page-title">Dashboard</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center">
      <span style="font-size:12px;color:#6b7280">Últimos 30 dias</span>
      <button class="btn btn-primary" onclick="refreshData()">↻ Atualizar</button>
    </div>
  </div>
  <div class="content">

    <!-- DASHBOARD -->
    <div class="section active" id="sec-dashboard">
      <div class="alert-bar">⚠️ <strong>ML detectou:</strong> "Banner Estático 01" com 89% de chance de CPL acima da meta nos próximos 3 dias. Ação automática sugerida.</div>
      <div class="metrics-grid">
        <div class="metric-card"><div class="metric-label">Total de Leads</div><div class="metric-value" id="m-leads">1.284</div><div class="metric-delta delta-up">▲ 18% vs mês anterior</div></div>
        <div class="metric-card"><div class="metric-label">CPL Médio</div><div class="metric-value">R$12,40</div><div class="metric-delta delta-up">▼ 9% — reduziu</div></div>
        <div class="metric-card"><div class="metric-label">Score ML Médio</div><div class="metric-value">74<span style="font-size:14px">/100</span></div><div class="metric-delta delta-neutral">Saúde das campanhas</div></div>
        <div class="metric-card"><div class="metric-label">ROAS</div><div class="metric-value">4,2x</div><div class="metric-delta delta-up">▲ 0,7x vs anterior</div></div>
      </div>
      <div class="grid2">
        <div class="card">
          <div class="card-header"><div class="card-title">Leads por semana</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="leadsChart" role="img" aria-label="Leads por semana"></canvas></div></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">CPL real vs previsão ML</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="cplChart" role="img" aria-label="CPL real vs previsão"></canvas></div></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Resumo geral</div></div>
        <div class="card-body">
          <div class="stat-row"><span class="stat-label">Melhor campanha</span><span class="stat-val">Captação Imóveis SP — R$9,80 CPL</span></div>
          <div class="stat-row"><span class="stat-label">Campanha em risco (ML)</span><span class="stat-val" style="color:#ef4444">Banner Estático 01 — 89% risco</span></div>
          <div class="stat-row"><span class="stat-label">Melhor criativo (IA)</span><span class="stat-val">Vídeo 30s — Score 91/100</span></div>
          <div class="stat-row"><span class="stat-label">Economia por otimização de lances</span><span class="stat-val" style="color:#00b894">R$1.840 este mês</span></div>
          <div class="stat-row"><span class="stat-label">Regras de automação ativas</span><span class="stat-val">5 de 6</span></div>
        </div>
      </div>
    </div>

    <!-- CAMPANHAS -->
    <div class="section" id="sec-campanhas">
      <div class="card">
        <div class="card-header"><div class="card-title">Campanhas</div><button class="btn btn-primary btn-sm" onclick="syncMeta()">Sincronizar Meta API</button></div>
        <div class="card-body" style="padding:0">
          <div class="table-wrap">
            <table>
              <thead><tr><th>Campanha</th><th>Status</th><th>Leads</th><th>CPL</th><th>Score ML</th><th>Orçamento</th><th>Ações</th></tr></thead>
              <tbody id="campaigns-tbody"></tbody>
            </table>
          </div>
          <div class="camp-cards" id="camp-cards" style="padding:14px"></div>
        </div>
      </div>
    </div>

    <!-- MACHINE LEARNING -->
    <div class="section" id="sec-ml">
      <div class="card">
        <div class="card-header">
          <div><div class="card-title">Previsão de performance — próximos 7 dias</div><div class="card-sub">Modelo treinado com 90 dias de histórico</div></div>
          <button class="btn btn-primary btn-sm" onclick="retrainML()">🔄 Retreinar modelo</button>
        </div>
        <div class="card-body">
          <div class="grid2" style="margin-bottom:16px">
            <div><div class="chart-container" style="height:220px"><canvas id="mlChart" role="img" aria-label="Previsão ML de leads"></canvas></div></div>
            <div style="display:flex;flex-direction:column;gap:10px" id="pred-cards"></div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Insights detectados pelo modelo</div></div>
        <div class="card-body" id="ml-insights"></div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Fatores de risco identificados</div></div>
        <div class="card-body">
          <div class="chart-container" style="height:180px"><canvas id="riskChart" role="img" aria-label="Fatores de risco"></canvas></div>
        </div>
      </div>
    </div>

    <!-- CRIATIVOS -->
    <div class="section" id="sec-criativos">
      <div class="card">
        <div class="card-header">
          <div><div class="card-title">Análise de criativos com IA</div><div class="card-sub">Score baseado em CTR, CPL, relevância e elementos visuais</div></div>
          <button class="btn btn-primary btn-sm" onclick="analyzeAllCreatives()">🤖 Analisar todos</button>
        </div>
        <div class="card-body">
          <div class="creative-grid" id="creative-grid"></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Enviar criativo para análise</div></div>
        <div class="card-body">
          <div class="upload-zone" onclick="analyzeNewCreative()">
            <div class="upload-icon">🖼️</div>
            <div class="upload-text" style="font-weight:600;color:#1a1a2e;margin-bottom:4px">Clique para analisar um criativo</div>
            <div class="upload-text">A IA analisa elementos visuais, texto, CTA e prevê o score de performance</div>
          </div>
          <div id="creative-analysis" style="margin-top:16px"></div>
        </div>
      </div>
    </div>

    <!-- LANCES -->
    <div class="section" id="sec-lances">
      <div class="metrics-grid">
        <div class="metric-card"><div class="metric-label">Economia gerada</div><div class="metric-value" style="color:#00b894">R$1.840</div><div class="metric-delta delta-neutral">vs lances manuais</div></div>
        <div class="metric-card"><div class="metric-label">Leads extras</div><div class="metric-value" style="color:#534AB7">+218</div><div class="metric-delta delta-neutral">pelo dayparting</div></div>
        <div class="metric-card"><div class="metric-label">Redução de CPL</div><div class="metric-value">-23%</div><div class="metric-delta delta-up">vs 30 dias atrás</div></div>
        <div class="metric-card"><div class="metric-label">Lances otimizados</div><div class="metric-value">4</div><div class="metric-delta delta-neutral">campanhas ativas</div></div>
      </div>
      <div id="lances-list"></div>
      <div class="card">
        <div class="card-header"><div class="card-title">Mapa de calor — melhor horário para leads</div><div class="card-sub">Mais escuro = melhor performance</div></div>
        <div class="card-body">
          <div style="font-size:11px;color:#9ca3af;margin-bottom:8px">Hora do dia →</div>
          <div id="heatmap-wrap"></div>
          <div style="display:flex;align-items:center;gap:8px;margin-top:10px">
            <div style="width:12px;height:12px;background:#E8E8F0;border-radius:3px"></div><span style="font-size:11px;color:#9ca3af">Baixo</span>
            <div style="width:12px;height:12px;background:#534AB7;border-radius:3px;margin-left:8px"></div><span style="font-size:11px;color:#9ca3af">Alto</span>
          </div>
        </div>
      </div>
    </div>

    <!-- OTIMIZAÇÃO -->
    <div class="section" id="sec-otimizacao">
      <div class="metrics-grid">
        <div class="metric-card"><div class="metric-label">Economia potencial</div><div class="metric-value" style="color:#00b894">R$2.340</div><div class="metric-delta delta-neutral">pausando ineficientes</div></div>
        <div class="metric-card"><div class="metric-label">Leads extras estimados</div><div class="metric-value" style="color:#534AB7">+340</div><div class="metric-delta delta-neutral">aplicando sugestões</div></div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Sugestões de otimização</div><button class="btn btn-primary btn-sm" onclick="analyzeWithAI()">🤖 Analisar com IA</button></div>
        <div class="card-body">
          <div class="opt-item"><div class="opt-priority priority-high"></div><div class="opt-body"><div class="opt-title">Pausar "Banner Estático 01"</div><div class="opt-desc">CPL R$22,10 — 78% acima da meta. ML prevê piora.</div><div class="opt-impact impact-high">Impacto alto — economia R$890/mês</div></div><button class="btn btn-danger btn-sm" onclick="applyOpt(this,'pausar')">Aplicar</button></div>
          <div class="opt-item"><div class="opt-priority priority-high"></div><div class="opt-body"><div class="opt-title">Escalar "Imóvel Vídeo 30s"</div><div class="opt-desc">CPL R$7,20 — score ML 91. Janela ideal agora.</div><div class="opt-impact impact-high">Impacto alto — +120 leads estimados</div></div><button class="btn btn-success btn-sm" onclick="applyOpt(this,'escalar')">Aplicar</button></div>
          <div class="opt-item"><div class="opt-priority priority-mid"></div><div class="opt-body"><div class="opt-title">Criar Lookalike 2%</div><div class="opt-desc">Público atual com frequência 4,2 — saturando.</div><div class="opt-impact impact-mid">Impacto médio — +80 leads</div></div><button class="btn btn-sm" onclick="applyOpt(this,'lookalike')">Criar</button></div>
          <div class="opt-item"><div class="opt-priority priority-mid"></div><div class="opt-body"><div class="opt-title">Ativar dayparting 19h–22h</div><div class="opt-desc">Mapa de calor mostra pico de conversão não explorado.</div><div class="opt-impact impact-mid">Impacto médio — CPL pode cair 15%</div></div><button class="btn btn-sm" onclick="applyOpt(this,'dayparting')">Aplicar</button></div>
        </div>
      </div>
    </div>

    <!-- AUTOMAÇÃO -->
    <div class="section" id="sec-automacao">
      <div class="card">
        <div class="card-header"><div class="card-title">Regras de automação</div><button class="btn btn-primary btn-sm" onclick="runRulesNow()">▶ Executar agora</button></div>
        <div class="card-body" id="rules-list"></div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Log de execuções</div></div>
        <div class="card-body"><div id="exec-log" style="font-family:monospace;font-size:12px;color:#374151;line-height:2">
          <div><span style="color:#9ca3af">08/04 09:00</span> ✅ ML retreinado — 94% de acurácia nas previsões</div>
          <div><span style="color:#9ca3af">08/04 03:00</span> 📈 Vídeo 30s escalado +20% — lances ajustados por dayparting</div>
          <div><span style="color:#9ca3af">07/04 21:00</span> ⚠️ Remarketing freq. 4.2 — alerta enviado + lance reduzido 15%</div>
          <div><span style="color:#9ca3af">07/04 15:00</span> ✅ Controle orçamento — todas dentro do limite</div>
        </div></div>
      </div>
    </div>

    <!-- IA -->
    <div class="section" id="sec-ia">
      <div class="card">
        <div class="card-header"><div class="card-title">🤖 Assistente Metron</div><span style="font-size:12px;color:#00b894">● Online</span></div>
        <div class="ai-chat">
          <div class="ai-messages" id="ai-messages">
            <div class="msg msg-ai">Olá! Sou o Metron, seu assistente com IA, ML e análise de criativos integrados. Posso analisar suas campanhas, prever performance, avaliar anúncios e muito mais. Como posso ajudar?</div>
          </div>
          <div class="ai-input-row">
            <input class="ai-input" id="ai-input" placeholder="Pergunte qualquer coisa sobre suas campanhas..." onkeydown="if(event.key==='Enter')sendAI()">
            <button class="btn btn-primary" onclick="sendAI()">Enviar</button>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Sugestões rápidas</div></div>
        <div class="card-body"><div class="quick-btns">
          <button class="btn" onclick="quickAsk('Analise minhas campanhas e diga quais têm melhor performance')">Analisar performance</button>
          <button class="btn" onclick="quickAsk('O que o modelo de ML está prevendo para os próximos 7 dias?')">Previsão ML</button>
          <button class="btn" onclick="quickAsk('Quais horários do dia geram mais leads com menor CPL?')">Melhor horário</button>
          <button class="btn" onclick="quickAsk('Crie 3 variações de headline para anúncios de captação de leads imobiliários')">Gerar copies</button>
          <button class="btn" onclick="quickAsk('Como melhorar o score dos meus criativos para aumentar o CTR?')">Melhorar criativos</button>
          <button class="btn" onclick="quickAsk('Quais são as 3 ações mais urgentes para maximizar leads agora?')">Ações urgentes</button>
        </div></div>
      </div>
    </div>

  </div>
</div>
</div>

<script>
const campaigns=[
  {nome:'Captação Imóveis SP',tipo:'Lead Ads · Conversão',status:'ACTIVE',leads:487,cpl:9.80,ctr:4.2,score:88,orcamento:6000,gasto:4770},
  {nome:'Curso Online — Tráfego Frio',tipo:'Lead Ads · Alcance',status:'ACTIVE',leads:312,cpl:14.20,ctr:2.9,score:62,orcamento:6000,gasto:4430},
  {nome:'Remarketing Visitantes',tipo:'Conversão · Retargeting',status:'LEARNING',leads:89,cpl:18.50,ctr:3.1,score:55,orcamento:3000,gasto:1647},
  {nome:'Banner Estático 01',tipo:'Lead Ads · Conversão',status:'ACTIVE',leads:198,cpl:22.10,ctr:1.8,score:21,orcamento:4000,gasto:4376},
];
const rules=[
  {icon:'🛑',cls:'icon-red',title:'Pausar anúncios com CPL alto',desc:'CPL > R$25 por 3 dias → pausar e notificar',log:'Hoje 09:00 — sem ações',on:true},
  {icon:'📈',cls:'icon-green',title:'Escalar budget performático',desc:'CPL < R$10 e leads > 20/dia → +20% budget',log:'Hoje 03:00 — Vídeo 30s escalado',on:true},
  {icon:'⚠️',cls:'icon-yellow',title:'Alerta de frequência alta',desc:'Frequência > 4 → reduzir lance e alertar',log:'Ontem 21:00 — Remarketing freq. 4.2',on:true},
  {icon:'💸',cls:'icon-red',title:'Controle de orçamento diário',desc:'Gasto > 110% planejado → pausar até meia-noite',log:'Ontem 15:00 — sem ações',on:false},
  {icon:'🧠',cls:'icon-purple',title:'Ação automática por ML',desc:'Score ML < 30 por 2 dias → pausar e notificar equipe',log:'Ativo — monitorando Banner Estático 01',on:true},
  {icon:'⚡',cls:'icon-blue',title:'Ajuste de lances por dayparting',desc:'Aumentar lance +30% nos horários de pico identificados',log:'Executando diariamente às 06:00',on:true},
];
const criativos=[
  {icon:'🎬',nome:'Vídeo 30s — Imóvel',score:91,ctr:4.8,cpl:7.2,tags:[{t:'Vídeo',g:true},{t:'CTA forte',g:true},{t:'Alta retenção',g:true}]},
  {icon:'🖼️',nome:'Carrossel Produto',score:74,ctr:3.2,cpl:10.4,tags:[{t:'Carrossel',g:true},{t:'Texto longo',g:false}]},
  {icon:'📷',nome:'Imagem Estática A',score:58,ctr:2.4,cpl:14.8,tags:[{t:'Sem vídeo',g:false},{t:'CTA fraco',g:false},{t:'Boa imagem',g:true}]},
  {icon:'📋',nome:'Banner Estático 01',score:22,ctr:1.8,cpl:22.1,tags:[{t:'Baixo CTR',g:false},{t:'Sem CTA',g:false},{t:'Saturado',g:false}]},
  {icon:'🎥',nome:'Reels — Depoimento',score:83,ctr:4.1,cpl:8.9,tags:[{t:'Prova social',g:true},{t:'Vídeo curto',g:true}]},
  {icon:'🖼️',nome:'Imagem Estática B',score:45,ctr:2.0,cpl:19.3,tags:[{t:'Copy fraco',g:false},{t:'Visual ok',g:true}]},
];

function openSB(){document.getElementById('sidebar').classList.add('open');document.getElementById('overlay').classList.add('show')}
function closeSB(){document.getElementById('sidebar').classList.remove('open');document.getElementById('overlay').classList.remove('show')}
function nav(id,el){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById('sec-'+id).classList.add('active');
  el.classList.add('active');
  const t={dashboard:'Dashboard',campanhas:'Campanhas',ml:'Machine Learning',criativos:'Análise de Criativos',lances:'Otimização de Lances',otimizacao:'Otimização',automacao:'Automação',ia:'Assistente IA'};
  document.getElementById('page-title').textContent=t[id];
  closeSB();
}

function renderCampaigns(){
  document.getElementById('campaigns-tbody').innerHTML=campaigns.map((c,i)=>{
    const pct=Math.min(100,(c.gasto/c.orcamento*100)).toFixed(0);
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const cc=c.cpl>18?'color:#ef4444':c.cpl<10?'color:#00b894':'';
    const sc=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    return`<tr>
      <td><div style="font-weight:600">${c.nome}</div><div style="font-size:11px;color:#9ca3af">${c.tipo}</div></td>
      <td><span class="badge ${badge}">${lbl}</span></td>
      <td><strong>${c.leads}</strong></td>
      <td style="${cc};font-weight:600">R$${c.cpl.toFixed(2)}</td>
      <td><div style="display:flex;align-items:center;gap:6px"><div style="flex:1;height:5px;background:#f0f0f4;border-radius:5px"><div style="height:100%;width:${c.score}%;background:${sc};border-radius:5px"></div></div><span style="font-size:12px;font-weight:600;color:${sc}">${c.score}</span></div></td>
      <td><div style="font-size:12px">R$${c.gasto.toLocaleString('pt-BR')} / R$${(c.orcamento/1000).toFixed(0)}k</div><div class="progress-bar"><div class="progress-fill" style="width:${pct}%;background:#534AB7"></div></div></td>
      <td><div style="display:flex;gap:6px">
        ${c.status==='ACTIVE'?`<button class="btn btn-sm" onclick="toggleStatus(${i})">Pausar</button>`:`<button class="btn btn-success btn-sm" onclick="toggleStatus(${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">IA ↗</button>
      </div></td>
    </tr>`;
  }).join('');
  document.getElementById('camp-cards').innerHTML=campaigns.map((c,i)=>{
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const cc=c.cpl>18?'#ef4444':c.cpl<10?'#00b894':'#1a1a2e';
    const sc=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    return`<div class="camp-card">
      <div class="camp-card-header"><div><div class="camp-card-name">${c.nome}</div><div class="camp-card-type">${c.tipo}</div></div><span class="badge ${badge}">${lbl}</span></div>
      <div class="camp-card-metrics">
        <div class="camp-metric"><div class="camp-metric-label">Leads</div><div class="camp-metric-value">${c.leads}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">CPL</div><div class="camp-metric-value" style="color:${cc}">R$${c.cpl.toFixed(2)}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">Score ML</div><div class="camp-metric-value" style="color:${sc}">${c.score}</div></div>
      </div>
      <div class="camp-card-actions">
        ${c.status==='ACTIVE'?`<button class="btn btn-sm" onclick="toggleStatus(${i})">Pausar</button>`:`<button class="btn btn-success btn-sm" onclick="toggleStatus(${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">Analisar IA ↗</button>
      </div>
    </div>`;
  }).join('');
}

function renderRules(){
  document.getElementById('rules-list').innerHTML=rules.map((r,i)=>`
    <div class="rule-item">
      <div class="rule-icon-wrap ${r.cls}">${r.icon}</div>
      <div class="rule-info"><div class="rule-title">${r.title}</div><div class="rule-desc">${r.desc}</div><div class="rule-log">${r.log}</div></div>
      <label class="toggle-switch"><input type="checkbox" ${r.on?'checked':''} onchange="toggleRule(${i},this)"><span class="toggle-track"></span></label>
    </div>`).join('');
}

function renderCreatives(){
  document.getElementById('creative-grid').innerHTML=criativos.map((c,i)=>{
    const sc=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    const tags=c.tags.map(t=>`<span class="tag ${t.g?'tag-good':'tag-bad'}">${t.t}</span>`).join('');
    return`<div class="creative-card">
      <div class="creative-thumb" style="background:${c.score>75?'#d1fae5':c.score>50?'#fef3c7':'#fee2e2'}">${c.icon}</div>
      <div class="creative-body">
        <div class="creative-name">${c.nome}</div>
        <div class="creative-score">
          <span style="font-size:16px;font-weight:700;color:${sc}">${c.score}</span>
          <div class="score-bar"><div class="score-fill" style="width:${c.score}%;background:${sc}"></div></div>
        </div>
        <div style="font-size:11px;color:#6b7280;margin-bottom:8px">CTR ${c.ctr}% · CPL R$${c.cpl}</div>
        <div class="creative-tags">${tags}</div>
        <button class="btn btn-sm" style="width:100%;margin-top:10px;font-size:11px" onclick="analyzeCreative(${i})">Analisar com IA ↗</button>
      </div>
    </div>`;
  }).join('');
}

function renderLances(){
  document.getElementById('lances-list').innerHTML=campaigns.filter(c=>c.status==='ACTIVE').map((c,i)=>{
    const sc=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    const lanceAtual=(c.cpl*0.8).toFixed(2);
    const lanceSug=(c.cpl*0.65).toFixed(2);
    return`<div class="lance-card">
      <div class="lance-header">
        <div><div class="lance-name">${c.nome}</div><div style="font-size:11px;color:#6b7280;margin-top:2px">Otimização automática ativa</div></div>
        <span class="badge ${c.score>75?'badge-ok':c.score>50?'badge-warn':'badge-risk'}">${c.score>75?'Saudável':c.score>50?'Atenção':'Risco'}</span>
      </div>
      <div class="lance-metrics">
        <div class="lance-metric"><div class="lance-metric-val">R$${lanceAtual}</div><div class="lance-metric-label">Lance atual</div></div>
        <div class="lance-metric"><div class="lance-metric-val" style="color:#534AB7">R$${lanceSug}</div><div class="lance-metric-label">Lance sugerido ML</div></div>
        <div class="lance-metric"><div class="lance-metric-val" style="color:${sc}">${c.score}/100</div><div class="lance-metric-label">Score ML</div></div>
      </div>
      <button class="btn btn-primary btn-sm" onclick="applyLance(this,'${c.nome}','${lanceSug}')">Aplicar lance R$${lanceSug}</button>
    </div>`;
  }).join('');
}

function renderML(){
  document.getElementById('pred-cards').innerHTML=campaigns.map(c=>{
    const risk=c.score>75?{l:'Baixo risco',cl:'risk-low',pct:100-c.score}:c.score>50?{l:'Risco médio',cl:'risk-mid',pct:100-c.score}:{l:'Alto risco',cl:'risk-high',pct:100-c.score};
    const barColor=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    return`<div class="pred-card">
      <div style="display:flex;justify-content:space-between;align-items:flex-start">
        <div><div style="font-size:12px;font-weight:600;color:#1a1a2e">${c.nome.split('—')[0].trim()}</div><div class="pred-label">${c.tipo}</div></div>
        <div class="pred-score" style="color:${barColor}">${c.score}</div>
      </div>
      <div class="pred-bar"><div class="pred-fill" style="width:${c.score}%;background:${barColor}"></div></div>
      <div style="font-size:11px" class="${risk.cl}">${risk.l} — ${risk.pct}% chance de piora</div>
    </div>`;
  }).join('');

  document.getElementById('ml-insights').innerHTML=[
    {dot:'#ef4444',txt:'Banner Estático 01: modelo prevê CPL acima de R$28 nos próximos 3 dias com 89% de confiança. Recomenda-se pausar.'},
    {dot:'#00b894',txt:'Captação Imóveis SP: tendência de melhora contínua. Melhor janela para escalar budget: terça a quinta, 19h–22h.'},
    {dot:'#f59e0b',txt:'Curso Online: CTR caindo 0,3pp por semana — possível saturação de criativo. Renovar anúncio em até 7 dias.'},
    {dot:'#534AB7',txt:'Padrão identificado: leads gerados entre 19h–22h têm CPL 34% menor em todas as campanhas. Dayparting recomendado.'},
  ].map(i=>`<div class="insight-item"><div class="insight-dot" style="background:${i.dot}"></div><div style="font-size:13px;color:#374151;line-height:1.5">${i.txt}</div></div>`).join('');
}

function renderHeatmap(){
  const days=['Seg','Ter','Qua','Qui','Sex','Sáb','Dom'];
  const hours=['0h','3h','6h','9h','12h','15h','18h','21h'];
  const data=[
    [1,1,1,2,3,4,6,8],[1,1,1,3,4,5,8,9],[1,1,1,3,5,6,9,10],
    [1,1,1,3,5,6,9,10],[1,1,1,2,4,5,8,9],[2,2,2,4,6,7,8,7],[2,2,1,3,5,5,7,6]
  ];
  const max=10;
  let html=`<div style="display:grid;grid-template-columns:36px repeat(8,1fr);gap:4px;align-items:center">`;
  html+=`<div></div>${hours.map(h=>`<div style="font-size:10px;color:#9ca3af;text-align:center">${h}</div>`).join('')}`;
  days.forEach((d,i)=>{
    html+=`<div style="font-size:11px;color:#6b7280;text-align:right;padding-right:6px">${d}</div>`;
    data[i].forEach(v=>{
      const alpha=(v/max);
      const r=Math.round(83+(255-83)*(1-alpha));
      const g=Math.round(74+(255-74)*(1-alpha));
      const b=Math.round(183+(255-183)*(1-alpha));
      html+=`<div style="height:24px;border-radius:4px;background:rgb(${r},${g},${b})" title="${v} leads/h"></div>`;
    });
  });
  html+=`</div>`;
  document.getElementById('heatmap-wrap').innerHTML=html;
}

function toggleStatus(i){campaigns[i].status=campaigns[i].status==='ACTIVE'?'PAUSED':'ACTIVE';renderCampaigns();renderLances();addLog(`${campaigns[i].status==='ACTIVE'?'▶️ Ativada':'⏸️ Pausada'}: "${campaigns[i].nome}"`);}
function toggleRule(i,el){rules[i].on=el.checked;addLog(el.checked?`✅ Regra "${rules[i].title}" ativada`:`⏸️ "${rules[i].title}" desativada`);}
function addLog(msg){const log=document.getElementById('exec-log');const now=new Date();const ts=`${String(now.getDate()).padStart(2,'0')}/${String(now.getMonth()+1).padStart(2,'0')} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;log.insertAdjacentHTML('afterbegin',`<div><span style="color:#9ca3af">${ts}</span> ${msg}</div>`);}
function applyOpt(btn,t){const m={pausar:'⛔ Banner Estático 01 pausado — budget realocado',escalar:'📈 Vídeo 30s +30% de budget',lookalike:'👥 Lookalike 2% criado',dayparting:'🕐 Dayparting 19h–22h ativado'};addLog(m[t]);btn.textContent='✓ Aplicado';btn.disabled=true;btn.style.opacity='.5';}
function applyLance(btn,nome,val){addLog(`⚡ Lance de "${nome}" ajustado para R$${val} pelo ML`);btn.textContent='✓ Lance aplicado';btn.disabled=true;btn.style.opacity='.5';}
function runRulesNow(){addLog('▶ Execução manual iniciada...');setTimeout(()=>addLog('✅ 6 regras verificadas — Vídeo 30s escalado, lance Banner reduzido'),1200);}
function syncMeta(){alert('Em produção: buscaria campanhas reais via Meta Graph API com seu Access Token.');}
function refreshData(){document.getElementById('m-leads').textContent=(1284+Math.floor(Math.random()*30)).toLocaleString('pt-BR');}
function retrainML(){addLog('🧠 Retreinamento do modelo iniciado com últimos 90 dias...');setTimeout(()=>addLog('✅ Modelo retreinado — acurácia: 94%'),2000);}
function analyzeAllCreatives(){quickAsk('Analise todos os criativos ativos e diga quais devo pausar, melhorar ou escalar. Seja específico.');}
function analyzeCreative(i){quickAsk(`Analise o criativo "${criativos[i].nome}" com score ${criativos[i].score}/100, CTR ${criativos[i].ctr}% e CPL R$${criativos[i].cpl}. O que devo melhorar?`);}
function analyzeNewCreative(){quickAsk('Quais são os elementos visuais e de copy que mais aumentam o score de um criativo no Meta Ads para geração de leads?');}

// CHARTS
const isDark=matchMedia('(prefers-color-scheme:dark)').matches;
const gc=isDark?'rgba(255,255,255,.07)':'rgba(0,0,0,.07)';
const tc=isDark?'#9ca3af':'#6b7280';

new Chart(document.getElementById('leadsChart'),{type:'bar',data:{labels:['S1','S2','S3','S4'],datasets:[{label:'Leads',data:[240,290,330,424],backgroundColor:'#534AB7',borderRadius:6,borderSkipped:false}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gc},ticks:{color:tc}},y:{grid:{color:gc},ticks:{color:tc}}}}});

new Chart(document.getElementById('cplChart'),{type:'line',data:{labels:['S1','S2','S3','S4','S5 (prev)','S6 (prev)'],datasets:[
  {label:'CPL real',data:[14.8,13.5,12.9,12.4,null,null],borderColor:'#534AB7',tension:.4,fill:false,pointBackgroundColor:'#534AB7'},
  {label:'Previsão ML',data:[null,null,null,12.4,11.8,11.2],borderColor:'#00b894',borderDash:[5,4],tension:.4,fill:false,pointBackgroundColor:'#00b894'},
  {label:'Meta',data:[12.4,12.4,12.4,12.4,12.4,12.4],borderColor:'#ef4444',borderDash:[6,3],tension:0,fill:false,pointRadius:0}
]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{font:{size:11},color:tc,boxWidth:10}}},scales:{x:{grid:{color:gc},ticks:{color:tc}},y:{grid:{color:gc},ticks:{color:tc,callback:v=>'R$'+v}}}}});

new Chart(document.getElementById('mlChart'),{type:'line',data:{labels:['Hoje','D+1','D+2','D+3','D+4','D+5','D+6'],datasets:[
  {label:'Leads previstos',data:[42,45,48,44,50,53,58],borderColor:'#534AB7',backgroundColor:'rgba(83,74,183,.08)',fill:true,tension:.4},
  {label:'Intervalo ML',data:[38,41,44,40,46,49,54],borderColor:'rgba(83,74,183,.3)',borderDash:[4,3],tension:.4,fill:false,pointRadius:0}
]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{font:{size:11},color:tc,boxWidth:10}}},scales:{x:{grid:{color:gc},ticks:{color:tc}},y:{grid:{color:gc},ticks:{color:tc}}}}});

new Chart(document.getElementById('riskChart'),{type:'bar',data:{labels:['Saturação público','CPL acima meta','Frequência alta','Budget esgotando','CTR caindo'],datasets:[{label:'Nível de risco',data:[72,89,68,45,55],backgroundColor:['#f59e0b','#ef4444','#f59e0b','#00b894','#f59e0b'],borderRadius:6,borderSkipped:false}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{max:100,grid:{color:gc},ticks:{color:tc,callback:v=>v+'%'}},y:{grid:{display:false},ticks:{color:tc}}}}});

renderCampaigns();renderRules();renderCreatives();renderLances();renderML();renderHeatmap();

// AI
const campCtx=JSON.stringify(campaigns.map(c=>({nome:c.nome,leads:c.leads,cpl:'R$'+c.cpl,score:c.score+'/100',status:c.status})));
async function callAI(prompt){
  const msgs=document.getElementById('ai-messages');
  const load=document.createElement('div');load.className='msg msg-ai loading';load.textContent='Analisando...';
  msgs.appendChild(load);msgs.scrollTop=msgs.scrollHeight;
  try{
    const r=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:1000,system:`Você é o assistente Metron, especialista em Meta Ads com capacidades de ML, análise de criativos e otimização de lances. Responda em português de forma direta e prática. Dados atuais das campanhas: ${campCtx}. Insights ML: Banner Estático 01 com 89% chance de piora. Melhor horário: 19h-22h com CPL 34% menor. Score médio de criativos: 62/100.`,messages:[{role:'user',content:prompt}]})});
    const d=await r.json();load.remove();
    const ai=document.createElement('div');ai.className='msg msg-ai';ai.textContent=d.content?.[0]?.text||'Erro ao obter resposta.';
    msgs.appendChild(ai);msgs.scrollTop=msgs.scrollHeight;
  }catch(e){load.textContent='Erro de conexão. Verifique a API.';}
}
function sendAI(){const inp=document.getElementById('ai-input');const v=inp.value.trim();if(!v)return;const msgs=document.getElementById('ai-messages');const u=document.createElement('div');u.className='msg msg-user';u.textContent=v;msgs.appendChild(u);inp.value='';callAI(v);}
function quickAsk(q){nav('ia',document.querySelectorAll('.nav-item')[8]);setTimeout(()=>{document.getElementById('ai-input').value=q;sendAI();},100);}
function analyzeWithAI(){quickAsk('Com base nas campanhas atuais e nos dados de ML, quais são as 3 ações mais urgentes para maximizar leads?');}
function analyzecamp(i){quickAsk(`Analise a campanha "${campaigns[i].nome}" com CPL R$${campaigns[i].cpl}, score ML ${campaigns[i].score}/100 e ${campaigns[i].leads} leads. O que devo fazer?`);}
</script>
</body>
</html>
