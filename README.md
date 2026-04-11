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

/* SIDEBAR */
.sidebar{width:220px;background:#1a1a2e;display:flex;flex-direction:column;flex-shrink:0;transition:transform .25s;z-index:100}
.logo{padding:20px 16px;border-bottom:1px solid rgba(255,255,255,.1);display:flex;align-items:center;justify-content:space-between}
.logo-text{font-size:17px;font-weight:700;color:#fff;letter-spacing:-.3px}
.logo-sub{font-size:11px;color:rgba(255,255,255,.4);margin-top:2px}
.nav-section{padding:14px 8px 4px;font-size:10px;font-weight:600;color:rgba(255,255,255,.3);letter-spacing:.8px;text-transform:uppercase}
.nav-item{display:flex;align-items:center;gap:10px;padding:10px 12px;border-radius:8px;cursor:pointer;color:rgba(255,255,255,.6);margin:1px 8px;font-size:13px;transition:all .15s}
.nav-item:hover{background:rgba(255,255,255,.08);color:#fff}
.nav-item.active{background:rgba(83,74,183,.4);color:#fff}
.nav-icon{width:18px;text-align:center;font-size:15px}
.close-sidebar{display:none;background:none;border:none;color:rgba(255,255,255,.5);font-size:20px;cursor:pointer;padding:2px 6px}

/* MAIN */
.main{flex:1;overflow-y:auto;background:#f4f5f7;display:flex;flex-direction:column}
.topbar{background:#fff;border-bottom:1px solid #e8e8ec;padding:14px 20px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:10}
.topbar-left{display:flex;align-items:center;gap:12px}
.hamburger{display:none;background:none;border:none;font-size:22px;cursor:pointer;color:#1a1a2e;padding:2px 4px}
.page-title{font-size:15px;font-weight:600;color:#1a1a2e}
.topbar-right{display:flex;gap:8px;align-items:center}
.period{font-size:12px;color:#6b7280}
.btn{padding:7px 13px;border-radius:8px;border:1.5px solid #e0e0e8;background:#fff;cursor:pointer;font-size:13px;color:#1a1a2e;transition:all .15s;font-family:inherit;white-space:nowrap}
.btn:hover{background:#f4f5f7}
.btn-primary{background:#534AB7;color:#fff;border-color:#534AB7}
.btn-primary:hover{background:#4340a0}
.btn-danger{color:#d63031;border-color:#d63031}
.btn-success{color:#00b894;border-color:#00b894}
.btn-sm{padding:5px 10px;font-size:12px}
.content{padding:20px;flex:1}

/* OVERLAY */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:99}
.overlay.show{display:block}

/* METRICS */
.metrics-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:20px}
.metric-card{background:#fff;border-radius:12px;padding:14px 16px;border:1px solid #e8e8ec}
.metric-label{font-size:11px;color:#6b7280;font-weight:500;margin-bottom:5px}
.metric-value{font-size:22px;font-weight:700;color:#1a1a2e;letter-spacing:-.5px}
.metric-delta{font-size:11px;margin-top:3px}
.delta-up{color:#00b894}.delta-down{color:#d63031}.delta-neutral{color:#6b7280}

/* CARDS */
.card{background:#fff;border-radius:12px;border:1px solid #e8e8ec;margin-bottom:16px}
.card-header{padding:14px 18px;border-bottom:1px solid #f0f0f4;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
.card-title{font-size:14px;font-weight:600;color:#1a1a2e}
.card-body{padding:18px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.chart-container{position:relative;height:200px}

/* TABLE */
.table-wrap{overflow-x:auto;-webkit-overflow-scrolling:touch}
table{width:100%;border-collapse:collapse;min-width:580px}
th{padding:9px 14px;text-align:left;font-size:11px;font-weight:600;color:#9ca3af;background:#fafafa;text-transform:uppercase;letter-spacing:.4px;white-space:nowrap}
td{padding:11px 14px;border-bottom:1px solid #f4f5f7;font-size:13px;color:#374151}
tr:last-child td{border-bottom:none}
tr:hover td{background:#fafafe}
.badge{display:inline-flex;align-items:center;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:600}
.badge-active{background:#d1fae5;color:#065f46}
.badge-paused{background:#f3f4f6;color:#6b7280}
.badge-learning{background:#dbeafe;color:#1e40af}
.progress-bar{height:4px;background:#f0f0f4;border-radius:4px;margin-top:4px}
.progress-fill{height:100%;border-radius:4px}

/* RULES */
.rule-item{display:flex;align-items:center;gap:12px;padding:13px 0;border-bottom:1px solid #f4f5f7}
.rule-item:last-child{border-bottom:none}
.rule-icon-wrap{width:34px;height:34px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0}
.icon-red{background:#fee2e2}.icon-green{background:#d1fae5}.icon-yellow{background:#fef3c7}.icon-blue{background:#dbeafe}
.rule-info{flex:1;min-width:0}
.rule-title{font-size:13px;font-weight:600;color:#1a1a2e;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.rule-desc{font-size:11px;color:#6b7280;margin-top:2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.rule-log{font-size:10px;color:#9ca3af;margin-top:2px}
.toggle-switch{position:relative;width:38px;height:20px;flex-shrink:0}
.toggle-switch input{opacity:0;width:0;height:0}
.toggle-track{position:absolute;inset:0;background:#d1d5db;border-radius:20px;cursor:pointer;transition:background .2s}
.toggle-track:before{content:'';position:absolute;height:16px;width:16px;left:2px;top:2px;background:#fff;border-radius:50%;transition:transform .2s;box-shadow:0 1px 3px rgba(0,0,0,.2)}
.toggle-switch input:checked+.toggle-track{background:#534AB7}
.toggle-switch input:checked+.toggle-track:before{transform:translateX(18px)}

/* AI CHAT */
.ai-chat{display:flex;flex-direction:column;height:380px}
.ai-messages{flex:1;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:10px}
.msg{max-width:85%;padding:10px 13px;border-radius:12px;font-size:13px;line-height:1.55}
.msg-user{background:#534AB7;color:#fff;align-self:flex-end;border-bottom-right-radius:4px}
.msg-ai{background:#f4f5f7;color:#1a1a2e;align-self:flex-start;border-bottom-left-radius:4px}
.msg-ai.loading{color:#9ca3af}
.ai-input-row{display:flex;gap:8px;padding:10px 14px;border-top:1px solid #f0f0f4}
.ai-input{flex:1;padding:9px 13px;border-radius:8px;border:1.5px solid #e0e0e8;font-size:13px;font-family:inherit;outline:none;min-width:0}
.ai-input:focus{border-color:#534AB7}

/* OPT */
.opt-item{display:flex;align-items:flex-start;gap:12px;padding:13px 0;border-bottom:1px solid #f4f5f7;flex-wrap:wrap}
.opt-item:last-child{border-bottom:none}
.opt-priority{width:7px;height:7px;border-radius:50%;margin-top:4px;flex-shrink:0}
.priority-high{background:#ef4444}.priority-mid{background:#f59e0b}.priority-low{background:#10b981}
.opt-body{flex:1;min-width:180px}
.opt-title{font-size:13px;font-weight:600;color:#1a1a2e}
.opt-desc{font-size:12px;color:#6b7280;margin-top:3px}
.opt-impact{font-size:11px;font-weight:600;margin-top:4px}
.impact-high{color:#ef4444}.impact-mid{color:#f59e0b}.impact-low{color:#10b981}

/* ALERT */
.alert-bar{background:#fffbeb;border:1px solid #fcd34d;border-radius:10px;padding:11px 14px;display:flex;align-items:flex-start;gap:8px;margin-bottom:14px;font-size:12px;color:#92400e;line-height:1.5}

/* TABS */
.tabs{display:flex;gap:0;border-bottom:1px solid #e8e8ec;margin-bottom:16px;overflow-x:auto;-webkit-overflow-scrolling:touch}
.tab{padding:9px 16px;cursor:pointer;font-size:13px;font-weight:500;color:#6b7280;border-bottom:2px solid transparent;margin-bottom:-1px;transition:all .15s;white-space:nowrap}
.tab.active{color:#534AB7;border-bottom-color:#534AB7}
.tab-content{display:none}.tab-content.active{display:block}
.code-block{background:#1a1a2e;color:#a8b4d8;padding:14px;border-radius:10px;font-family:monospace;font-size:11px;line-height:1.8;overflow-x:auto;margin:10px 0}
.code-comment{color:#536271}.code-key{color:#81d4fa}.code-str{color:#a5d6a7}.code-num{color:#ffcc80}
.stat-row{display:flex;justify-content:space-between;align-items:center;padding:9px 0;border-bottom:1px solid #f4f5f7;font-size:13px;gap:8px}
.stat-row:last-child{border-bottom:none}
.stat-label{color:#6b7280;flex-shrink:0}
.stat-val{font-weight:600;color:#1a1a2e;text-align:right}
.quick-btns{display:flex;flex-wrap:wrap;gap:8px}
.section{display:none}.section.active{display:block}

/* MOBILE CARDS (campanhas) */
.camp-cards{display:none;flex-direction:column;gap:10px}
.camp-card{background:#fff;border:1px solid #e8e8ec;border-radius:12px;padding:14px}
.camp-card-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:10px}
.camp-card-name{font-size:13px;font-weight:600;color:#1a1a2e}
.camp-card-type{font-size:11px;color:#9ca3af;margin-top:2px}
.camp-card-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:10px}
.camp-metric{background:#f9fafb;border-radius:8px;padding:8px}
.camp-metric-label{font-size:10px;color:#9ca3af}
.camp-metric-value{font-size:14px;font-weight:700;color:#1a1a2e;margin-top:2px}
.camp-card-actions{display:flex;gap:8px}

/* RESPONSIVE */
@media(max-width:768px){
  .sidebar{position:fixed;top:0;left:0;height:100%;transform:translateX(-100%)}
  .sidebar.open{transform:translateX(0)}
  .hamburger{display:block}
  .close-sidebar{display:block}
  .period{display:none}
  .metrics-grid{grid-template-columns:repeat(2,1fr);gap:10px}
  .metric-value{font-size:18px}
  .grid2{grid-template-columns:1fr}
  .table-wrap table{display:none}
  .camp-cards{display:flex}
  .content{padding:14px}
  .card-body{padding:14px}
  .ai-chat{height:320px}
  .quick-btns .btn{font-size:12px;padding:6px 10px}
}
@media(max-width:400px){
  .metrics-grid{grid-template-columns:1fr 1fr}
  .camp-card-metrics{grid-template-columns:repeat(3,1fr)}
}
</style>
</head>
<body>
<div class="overlay" id="overlay" onclick="closeSidebar()"></div>
<div class="app">

<div class="sidebar" id="sidebar">
  <div class="logo">
    <div>
      <div class="logo-text">Metron</div>
      <div class="logo-sub">Powered by Claude AI</div>
    </div>
    <button class="close-sidebar" onclick="closeSidebar()">✕</button>
  </div>
  <div class="nav-section">Principal</div>
  <div class="nav-item active" onclick="navigate('dashboard',this)"><span class="nav-icon">📊</span>Dashboard</div>
  <div class="nav-item" onclick="navigate('campanhas',this)"><span class="nav-icon">📢</span>Campanhas</div>
  <div class="nav-section">Ferramentas</div>
  <div class="nav-item" onclick="navigate('otimizacao',this)"><span class="nav-icon">🎯</span>Otimização</div>
  <div class="nav-item" onclick="navigate('automacao',this)"><span class="nav-icon">⚡</span>Automação</div>
  <div class="nav-item" onclick="navigate('ia',this)"><span class="nav-icon">🤖</span>Assistente IA</div>
  <div class="nav-section">Dev</div>
  <div class="nav-item" onclick="navigate('api',this)"><span class="nav-icon">🔌</span>Integração API</div>
</div>

<div class="main">
  <div class="topbar">
    <div class="topbar-left">
      <button class="hamburger" onclick="openSidebar()">☰</button>
      <div class="page-title" id="page-title">Dashboard</div>
    </div>
    <div class="topbar-right">
      <span class="period">Últimos 30 dias</span>
      <button class="btn btn-primary" onclick="refreshData()">↻ Atualizar</button>
    </div>
  </div>

  <div class="content">

    <!-- DASHBOARD -->
    <div class="section active" id="sec-dashboard">
      <div class="alert-bar">⚠️ <strong>Atenção:</strong> "Banner Estático 01" com CPL 78% acima da meta — automação sugerida ativa.</div>
      <div class="metrics-grid">
        <div class="metric-card"><div class="metric-label">Total de Leads</div><div class="metric-value" id="m-leads">1.284</div><div class="metric-delta delta-up">▲ 18% vs mês anterior</div></div>
        <div class="metric-card"><div class="metric-label">CPL Médio</div><div class="metric-value">R$12,40</div><div class="metric-delta delta-up">▼ 9% — reduziu</div></div>
        <div class="metric-card"><div class="metric-label">Investimento</div><div class="metric-value">R$15,9k</div><div class="metric-delta delta-neutral">88% do orçamento</div></div>
        <div class="metric-card"><div class="metric-label">ROAS</div><div class="metric-value">4,2x</div><div class="metric-delta delta-up">▲ 0,7x vs anterior</div></div>
      </div>
      <div class="grid2">
        <div class="card">
          <div class="card-header"><div class="card-title">Leads por semana</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="leadsChart" role="img" aria-label="Leads por semana"></canvas></div></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">CPL por campanha</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="cplChart" role="img" aria-label="CPL por campanha"></canvas></div></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Resumo de performance</div></div>
        <div class="card-body">
          <div class="stat-row"><span class="stat-label">Melhor campanha</span><span class="stat-val">Captação Imóveis SP — R$9,80</span></div>
          <div class="stat-row"><span class="stat-label">Campanha com alerta</span><span class="stat-val" style="color:#ef4444">Banner Estático 01 — R$22,10</span></div>
          <div class="stat-row"><span class="stat-label">Regras ativas</span><span class="stat-val">4 de 5</span></div>
          <div class="stat-row"><span class="stat-label">Economia estimada</span><span class="stat-val" style="color:#00b894">R$2.340 este mês</span></div>
          <div class="stat-row"><span class="stat-label">Testes A/B ativos</span><span class="stat-val">3 experimentos</span></div>
        </div>
      </div>
    </div>

    <!-- CAMPANHAS -->
    <div class="section" id="sec-campanhas">
      <div class="card">
        <div class="card-header">
          <div class="card-title">Campanhas ativas</div>
          <div style="display:flex;gap:8px;flex-wrap:wrap">
            <button class="btn btn-sm" onclick="syncMeta()">Sincronizar Meta</button>
          </div>
        </div>
        <div class="card-body" style="padding:0">
          <div class="table-wrap">
            <table>
              <thead><tr><th>Campanha</th><th>Status</th><th>Leads</th><th>CPL</th><th>CTR</th><th>Orçamento</th><th>Ações</th></tr></thead>
              <tbody id="campaigns-tbody"></tbody>
            </table>
          </div>
          <div class="camp-cards" id="camp-cards" style="padding:14px"></div>
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
        <div class="card-header">
          <div class="card-title">Sugestões de otimização</div>
          <button class="btn btn-primary btn-sm" onclick="analyzeWithAI()">🤖 Analisar com IA</button>
        </div>
        <div class="card-body">
          <div class="opt-item"><div class="opt-priority priority-high"></div><div class="opt-body"><div class="opt-title">Pausar "Banner Estático 01"</div><div class="opt-desc">CPL R$22,10 — 78% acima da meta. Realocar R$1.200 para vídeos.</div><div class="opt-impact impact-high">Impacto alto — economia de R$890/mês</div></div><button class="btn btn-danger btn-sm" onclick="applyOpt(this,'pausar')">Aplicar</button></div>
          <div class="opt-item"><div class="opt-priority priority-high"></div><div class="opt-body"><div class="opt-title">Escalar "Imóvel Vídeo 30s"</div><div class="opt-desc">CPL R$7,20 — 42% abaixo da meta. Aumentar budget em 30%.</div><div class="opt-impact impact-high">Impacto alto — +120 leads estimados</div></div><button class="btn btn-success btn-sm" onclick="applyOpt(this,'escalar')">Aplicar</button></div>
          <div class="opt-item"><div class="opt-priority priority-mid"></div><div class="opt-body"><div class="opt-title">Criar Lookalike 2%</div><div class="opt-desc">Público com frequência 4,2 — sinais de saturação.</div><div class="opt-impact impact-mid">Impacto médio — +80 leads estimados</div></div><button class="btn btn-sm" onclick="applyOpt(this,'lookalike')">Criar</button></div>
          <div class="opt-item"><div class="opt-priority priority-mid"></div><div class="opt-body"><div class="opt-title">Testar horário de veiculação</div><div class="opt-desc">Picos entre 19h–22h não explorados. Criar dayparting.</div><div class="opt-impact impact-mid">Impacto médio — CPL pode cair 15%</div></div><button class="btn btn-sm" onclick="applyOpt(this,'dayparting')">Aplicar</button></div>
          <div class="opt-item"><div class="opt-priority priority-low"></div><div class="opt-body"><div class="opt-title">Atualizar headlines do carrossel</div><div class="opt-desc">CTR 1,8% abaixo da média. Teste A/B com 3 variações.</div><div class="opt-impact impact-low">Impacto baixo — CTR pode subir 20%</div></div><button class="btn btn-sm" onclick="generateCopyWithAI()">Gerar com IA</button></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">CPL por formato de anúncio</div></div>
        <div class="card-body"><div class="chart-container"><canvas id="adChart" role="img" aria-label="CPL por formato"></canvas></div></div>
      </div>
    </div>

    <!-- AUTOMAÇÃO -->
    <div class="section" id="sec-automacao">
      <div class="card">
        <div class="card-header">
          <div class="card-title">Regras de automação</div>
          <button class="btn btn-primary btn-sm" onclick="runRulesNow()">▶ Executar agora</button>
        </div>
        <div class="card-body" id="rules-list"></div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Log de execuções</div></div>
        <div class="card-body">
          <div id="exec-log" style="font-family:monospace;font-size:12px;color:#374151;line-height:2">
            <div><span style="color:#9ca3af">08/04 09:00</span> ✅ Regra "Pausar CPL alto" — nenhum anúncio pausado</div>
            <div><span style="color:#9ca3af">08/04 03:00</span> ✅ "Vídeo 30s" escalado +20% (R$1.200→R$1.440)</div>
            <div><span style="color:#9ca3af">07/04 21:00</span> ⚠️ Remarketing com frequência 4.2 — alerta enviado</div>
            <div><span style="color:#9ca3af">07/04 15:00</span> ✅ Controle orçamento — todas dentro do limite</div>
          </div>
        </div>
      </div>
    </div>

    <!-- IA -->
    <div class="section" id="sec-ia">
      <div class="card">
        <div class="card-header"><div class="card-title">🤖 Assistente Metron</div><span style="font-size:12px;color:#00b894">● Online</span></div>
        <div class="ai-chat">
          <div class="ai-messages" id="ai-messages">
            <div class="msg msg-ai">Olá! Sou o assistente Metron, especializado em Meta Ads. Posso analisar suas campanhas, sugerir otimizações e muito mais. Como posso ajudar?</div>
          </div>
          <div class="ai-input-row">
            <input class="ai-input" id="ai-input" placeholder="Como reduzir meu CPL?" onkeydown="if(event.key==='Enter')sendAI()">
            <button class="btn btn-primary" onclick="sendAI()">Enviar</button>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Sugestões rápidas</div></div>
        <div class="card-body">
          <div class="quick-btns">
            <button class="btn" onclick="quickAsk('Analise minhas campanhas e diga quais têm melhor performance')">Analisar performance</button>
            <button class="btn" onclick="quickAsk('Quais são as melhores práticas para reduzir CPL no Meta Ads?')">Reduzir CPL</button>
            <button class="btn" onclick="quickAsk('Como criar um público Lookalike eficiente para geração de leads?')">Lookalike</button>
            <button class="btn" onclick="quickAsk('Crie 3 variações de headline para anúncios de captação de leads imobiliários')">Gerar copies</button>
            <button class="btn" onclick="quickAsk('Quais métricas devo acompanhar para maximizar geração de leads?')">Métricas essenciais</button>
          </div>
        </div>
      </div>
    </div>

    <!-- API -->
    <div class="section" id="sec-api">
      <div class="card">
        <div class="card-header"><div class="card-title">Integração com Meta Marketing API</div></div>
        <div class="card-body">
          <div class="tabs">
            <div class="tab active" onclick="switchTab(this,'tab-setup')">Configuração</div>
            <div class="tab" onclick="switchTab(this,'tab-campanhas')">Campanhas</div>
            <div class="tab" onclick="switchTab(this,'tab-leads')">Leads</div>
            <div class="tab" onclick="switchTab(this,'tab-rules')">Automação</div>
          </div>
          <div class="tab-content active" id="tab-setup">
            <p style="color:#6b7280;margin-bottom:10px;font-size:13px">Configure as variáveis de ambiente:</p>
            <div class="code-block">
<span class="code-comment">// .env</span>
META_ACCESS_TOKEN=<span class="code-str">SEU_TOKEN</span>
META_AD_ACCOUNT_ID=<span class="code-str">act_123456789</span>
META_APP_ID=<span class="code-str">SEU_APP_ID</span>
META_APP_SECRET=<span class="code-str">SEU_SECRET</span>

<span class="code-comment">// Permissões necessárias:</span>
<span class="code-key">ads_management</span>
<span class="code-key">ads_read</span>
<span class="code-key">leads_retrieval</span>
            </div>
          </div>
          <div class="tab-content" id="tab-campanhas">
            <div class="code-block">
<span class="code-key">async function</span> getCampaigns() {
  <span class="code-key">const</span> { data } = <span class="code-key">await</span> axios.get(
    <span class="code-str">`https://graph.facebook.com/v19.0/${AD_ACCOUNT}/campaigns`</span>,
    { params: {
      access_token: TOKEN,
      fields: <span class="code-str">'id,name,status,insights{spend,actions,ctr}'</span>,
      date_preset: <span class="code-str">'last_30d'</span>
    }}
  );
  <span class="code-key">return</span> data.data;
}
            </div>
          </div>
          <div class="tab-content" id="tab-leads">
            <div class="code-block">
<span class="code-comment">// Webhook para capturar leads</span>
app.post(<span class="code-str">'/webhook'</span>, <span class="code-key">async</span> (req, res) => {
  <span class="code-key">const</span> { changes } = req.body.entry[<span class="code-num">0</span>];
  <span class="code-key">for</span> (<span class="code-key">const</span> c <span class="code-key">of</span> changes) {
    <span class="code-key">if</span> (c.field === <span class="code-str">'leadgen'</span>) {
      <span class="code-key">const</span> lead = <span class="code-key">await</span> fetchLead(c.value.leadgen_id);
      <span class="code-key">await</span> saveToCRM(lead);
      <span class="code-key">await</span> notifyTeam(lead);
    }
  }
  res.sendStatus(<span class="code-num">200</span>);
});
            </div>
          </div>
          <div class="tab-content" id="tab-rules">
            <div class="code-block">
<span class="code-comment">// Automação a cada 6h</span>
cron.schedule(<span class="code-str">'0 */6 * * *'</span>, <span class="code-key">async</span> () => {
  <span class="code-key">const</span> ads = <span class="code-key">await</span> getAllAds();
  <span class="code-key">for</span> (<span class="code-key">const</span> ad <span class="code-key">of</span> ads) {
    <span class="code-key">if</span> (ad.cpl > <span class="code-num">25</span>) <span class="code-key">await</span> pauseAd(ad.id);
    <span class="code-key">if</span> (ad.cpl < <span class="code-num">10</span>) <span class="code-key">await</span> scaleBudget(ad.id);
    <span class="code-key">if</span> (ad.frequency > <span class="code-num">4</span>) <span class="code-key">await</span> notify(ad);
  }
});
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</div>
</div>

<script>
const campaigns=[
  {nome:'Captação Imóveis SP',tipo:'Lead Ads · Conversão',status:'ACTIVE',leads:487,cpl:9.80,ctr:4.2,orcamento:6000,gasto:4770},
  {nome:'Curso Online — Tráfego Frio',tipo:'Lead Ads · Alcance',status:'ACTIVE',leads:312,cpl:14.20,ctr:2.9,orcamento:6000,gasto:4430},
  {nome:'Remarketing Visitantes',tipo:'Conversão · Retargeting',status:'LEARNING',leads:89,cpl:18.50,ctr:3.1,orcamento:3000,gasto:1647},
  {nome:'Black Friday Antecipada',tipo:'Lead Ads · Conversão',status:'PAUSED',leads:396,cpl:12.70,ctr:3.8,orcamento:5000,gasto:5030},
];
const rules=[
  {icon:'🛑',cls:'icon-red',title:'Pausar anúncios com CPL alto',desc:'CPL > R$25 por 3 dias → pausar e notificar',log:'Hoje 09:00 — sem ações',on:true},
  {icon:'📈',cls:'icon-green',title:'Escalar budget performático',desc:'CPL < R$10 e leads > 20/dia → +20% budget',log:'Hoje 03:00 — Vídeo 30s escalado',on:true},
  {icon:'⚠️',cls:'icon-yellow',title:'Alerta de frequência alta',desc:'Frequência > 4 → alertar para renovar público',log:'Ontem 21:00 — Remarketing freq. 4.2',on:true},
  {icon:'💸',cls:'icon-red',title:'Controle de orçamento diário',desc:'Gasto > 110% planejado → pausar até meia-noite',log:'Ontem 15:00 — sem ações',on:false},
  {icon:'🔄',cls:'icon-blue',title:'A/B test automático de criativos',desc:'A cada 14 dias, duplicar melhor anúncio com novo copy',log:'Próxima: 15/04',on:true},
];

function openSidebar(){document.getElementById('sidebar').classList.add('open');document.getElementById('overlay').classList.add('show')}
function closeSidebar(){document.getElementById('sidebar').classList.remove('open');document.getElementById('overlay').classList.remove('show')}

function navigate(id,el){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById('sec-'+id).classList.add('active');
  el.classList.add('active');
  const t={dashboard:'Dashboard',campanhas:'Campanhas',otimizacao:'Otimização',automacao:'Automação',ia:'Assistente IA',api:'Integração API'};
  document.getElementById('page-title').textContent=t[id];
  closeSidebar();
}

function switchTab(el,id){
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
  el.classList.add('active');document.getElementById(id).classList.add('active');
}

function renderCampaigns(){
  // Desktop table
  document.getElementById('campaigns-tbody').innerHTML=campaigns.map((c,i)=>{
    const pct=Math.min(100,(c.gasto/c.orcamento*100)).toFixed(0);
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const cc=c.cpl>18?'color:#ef4444':c.cpl<10?'color:#00b894':'';
    return`<tr>
      <td><div style="font-weight:600">${c.nome}</div><div style="font-size:11px;color:#9ca3af">${c.tipo}</div></td>
      <td><span class="badge ${badge}">${lbl}</span></td>
      <td><strong>${c.leads}</strong></td>
      <td style="${cc};font-weight:600">R$${c.cpl.toFixed(2)}</td>
      <td>${c.ctr}%</td>
      <td><div style="font-size:12px">R$${c.gasto.toLocaleString('pt-BR')} / R$${(c.orcamento/1000).toFixed(0)}k</div>
        <div class="progress-bar"><div class="progress-fill" style="width:${pct}%;background:${c.status==='PAUSED'?'#9ca3af':'#534AB7'}"></div></div></td>
      <td><div style="display:flex;gap:6px">
        ${c.status==='ACTIVE'?`<button class="btn btn-sm" onclick="toggleStatus(${i})">Pausar</button>`:`<button class="btn btn-success btn-sm" onclick="toggleStatus(${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">IA ↗</button>
      </div></td>
    </tr>`;
  }).join('');
  // Mobile cards
  document.getElementById('camp-cards').innerHTML=campaigns.map((c,i)=>{
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const cc=c.cpl>18?'#ef4444':c.cpl<10?'#00b894':'#1a1a2e';
    return`<div class="camp-card">
      <div class="camp-card-header">
        <div><div class="camp-card-name">${c.nome}</div><div class="camp-card-type">${c.tipo}</div></div>
        <span class="badge ${badge}">${lbl}</span>
      </div>
      <div class="camp-card-metrics">
        <div class="camp-metric"><div class="camp-metric-label">Leads</div><div class="camp-metric-value">${c.leads}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">CPL</div><div class="camp-metric-value" style="color:${cc}">R$${c.cpl.toFixed(2)}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">CTR</div><div class="camp-metric-value">${c.ctr}%</div></div>
      </div>
      <div class="camp-card-actions">
        ${c.status==='ACTIVE'?`<button class="btn btn-sm" onclick="toggleStatus(${i})">Pausar</button>`:`<button class="btn btn-success btn-sm" onclick="toggleStatus(${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">Analisar com IA ↗</button>
      </div>
    </div>`;
  }).join('');
}

function renderRules(){
  document.getElementById('rules-list').innerHTML=rules.map((r,i)=>`
    <div class="rule-item">
      <div class="rule-icon-wrap ${r.cls}">${r.icon}</div>
      <div class="rule-info">
        <div class="rule-title">${r.title}</div>
        <div class="rule-desc">${r.desc}</div>
        <div class="rule-log">${r.log}</div>
      </div>
      <label class="toggle-switch"><input type="checkbox" ${r.on?'checked':''} onchange="toggleRule(${i},this)"><span class="toggle-track"></span></label>
    </div>`).join('');
}

function toggleStatus(i){campaigns[i].status=campaigns[i].status==='ACTIVE'?'PAUSED':'ACTIVE';renderCampaigns();addLog(`${campaigns[i].status==='ACTIVE'?'▶️ Ativada':'⏸️ Pausada'}: "${campaigns[i].nome}"`);}
function toggleRule(i,el){rules[i].on=el.checked;addLog(el.checked?`✅ Regra "${rules[i].title}" ativada`:`⏸️ Regra "${rules[i].title}" desativada`);}
function addLog(msg){const log=document.getElementById('exec-log');const now=new Date();const ts=`${String(now.getDate()).padStart(2,'0')}/${String(now.getMonth()+1).padStart(2,'0')} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;log.insertAdjacentHTML('afterbegin',`<div><span style="color:#9ca3af">${ts}</span> ${msg}</div>`);}
function applyOpt(btn,t){const m={pausar:'⛔ Banner Estático 01 pausado — budget realocado',escalar:'📈 Vídeo 30s +30% de budget',lookalike:'👥 Lookalike 2% criado',dayparting:'🕐 Dayparting 19h–22h ativado'};addLog(m[t]);btn.textContent='✓ Aplicado';btn.disabled=true;btn.style.opacity='.5';}
function runRulesNow(){addLog('▶ Execução manual iniciada...');setTimeout(()=>addLog('✅ 5 regras verificadas — Vídeo 30s escalado +20%'),1200);}
function syncMeta(){alert('Em produção: buscaria campanhas reais via Meta Graph API.');}
function refreshData(){document.getElementById('m-leads').textContent=(1284+Math.floor(Math.random()*30)).toLocaleString('pt-BR');}

// CHARTS
const isDark=matchMedia('(prefers-color-scheme:dark)').matches;
const gc=isDark?'rgba(255,255,255,.07)':'rgba(0,0,0,.07)';
const tc=isDark?'#9ca3af':'#6b7280';

new Chart(document.getElementById('leadsChart'),{type:'bar',data:{labels:['S1','S2','S3','S4'],datasets:[{label:'Leads',data:[240,290,330,424],backgroundColor:'#534AB7',borderRadius:6,borderSkipped:false}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gc},ticks:{color:tc}},y:{grid:{color:gc},ticks:{color:tc}}}}});

new Chart(document.getElementById('cplChart'),{type:'line',data:{labels:['S1','S2','S3','S4'],datasets:[{label:'Imóveis SP',data:[13.2,11.8,10.5,9.8],borderColor:'#534AB7',tension:.4,fill:false,pointBackgroundColor:'#534AB7'},{label:'Curso Online',data:[18.1,16.4,15.2,14.2],borderColor:'#00b894',tension:.4,fill:false,pointBackgroundColor:'#00b894'},{label:'Meta',data:[12.4,12.4,12.4,12.4],borderColor:'#ef4444',borderDash:[6,3],tension:0,fill:false,pointRadius:0}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{font:{size:11},color:tc,boxWidth:10}}},scales:{x:{grid:{color:gc},ticks:{color:tc}},y:{grid:{color:gc},ticks:{color:tc,callback:v=>'R$'+v}}}}});

new Chart(document.getElementById('adChart'),{type:'bar',data:{labels:['Vídeo 30s','Carrossel','Imagem A','Imagem B','Banner'],datasets:[{label:'CPL (R$)',data:[7.20,10.40,14.80,19.30,22.10],backgroundColor:['#3B6D11','#639922','#BA7517','#D85A30','#A32D2D'],borderRadius:4,borderSkipped:false}]},options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gc},ticks:{color:tc,callback:v=>'R$'+v}},y:{grid:{display:false},ticks:{color:tc}}}}});

renderCampaigns();
renderRules();

// AI
const campCtx=JSON.stringify(campaigns.map(c=>({nome:c.nome,leads:c.leads,cpl:'R$'+c.cpl,status:c.status})));
async function callAI(prompt){
  const msgs=document.getElementById('ai-messages');
  const load=document.createElement('div');load.className='msg msg-ai loading';load.textContent='Analisando...';
  msgs.appendChild(load);msgs.scrollTop=msgs.scrollHeight;
  try{
    const r=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:1000,system:`Você é o assistente Metron, especialista em Meta Ads e geração de leads. Responda em português, seja direto e prático. Campanhas atuais: ${campCtx}`,messages:[{role:'user',content:prompt}]})});
    const d=await r.json();load.remove();
    const ai=document.createElement('div');ai.className='msg msg-ai';ai.textContent=d.content?.[0]?.text||'Erro ao obter resposta.';
    msgs.appendChild(ai);msgs.scrollTop=msgs.scrollHeight;
  }catch(e){load.textContent='Erro de conexão.';}
}
function sendAI(){const inp=document.getElementById('ai-input');const v=inp.value.trim();if(!v)return;const msgs=document.getElementById('ai-messages');const u=document.createElement('div');u.className='msg msg-user';u.textContent=v;msgs.appendChild(u);inp.value='';callAI(v);}
function quickAsk(q){navigate('ia',document.querySelectorAll('.nav-item')[4]);setTimeout(()=>{document.getElementById('ai-input').value=q;sendAI();},100);}
function analyzeWithAI(){quickAsk('Com base nas campanhas atuais, quais são as 3 ações mais urgentes para maximizar leads?');}
function generateCopyWithAI(){quickAsk('Crie 3 variações de headline para anúncios de carrossel de captação de leads imobiliários');}
function analyzecamp(i){quickAsk(`Analise a campanha "${campaigns[i].nome}" com CPL R$${campaigns[i].cpl} e ${campaigns[i].leads} leads. O que devo fazer?`);}
</script>
</body>
</html>
