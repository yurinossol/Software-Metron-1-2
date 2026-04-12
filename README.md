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
.logo-text{font-size:17px;font-weight:700;color:#fff}
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
.page-title{font-size:15px;font-weight:600}
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

/* STATUS BAR */
.status-bar{display:flex;align-items:center;gap:8px;padding:8px 14px;border-radius:8px;font-size:12px;margin-bottom:14px}
.status-connected{background:#d1fae5;color:#065f46}
.status-error{background:#fee2e2;color:#991b1b}
.status-loading{background:#dbeafe;color:#1e40af}
.status-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}
.dot-green{background:#00b894}.dot-red{background:#ef4444}.dot-blue{background:#534AB7;animation:pulse 1.2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}

/* CONFIG PANEL */
.config-panel{background:#fff;border:1px solid #e8e8ec;border-radius:12px;padding:20px;margin-bottom:20px}
.config-title{font-size:14px;font-weight:600;margin-bottom:14px;color:#1a1a2e}
.config-row{display:flex;flex-direction:column;gap:6px;margin-bottom:12px}
.config-label{font-size:12px;font-weight:500;color:#6b7280}
.config-input{padding:9px 12px;border-radius:8px;border:1.5px solid #e0e0e8;font-size:13px;font-family:inherit;outline:none;width:100%}
.config-input:focus{border-color:#534AB7}
.config-input[type=password]{letter-spacing:2px}
.config-hint{font-size:11px;color:#9ca3af;margin-top:2px}
.config-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}

/* METRICS */
.metrics-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:18px}
.metric-card{background:#fff;border-radius:12px;padding:14px 16px;border:1px solid #e8e8ec}
.metric-label{font-size:11px;color:#6b7280;font-weight:500;margin-bottom:4px}
.metric-value{font-size:21px;font-weight:700;color:#1a1a2e;letter-spacing:-.5px}
.metric-delta{font-size:11px;margin-top:3px}
.delta-up{color:#00b894}.delta-down{color:#d63031}.delta-neutral{color:#6b7280}
.skeleton{background:linear-gradient(90deg,#f0f0f4 25%,#e0e0e8 50%,#f0f0f4 75%);background-size:200% 100%;animation:shimmer 1.4s infinite;border-radius:6px;height:24px;width:80%}
@keyframes shimmer{0%{background-position:200% 0}100%{background-position:-200% 0}}

/* CARDS */
.card{background:#fff;border-radius:12px;border:1px solid #e8e8ec;margin-bottom:16px}
.card-header{padding:14px 18px;border-bottom:1px solid #f0f0f4;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
.card-title{font-size:14px;font-weight:600}
.card-body{padding:16px 18px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
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
.progress-bar{height:4px;background:#f0f0f4;border-radius:4px;margin-top:4px}
.progress-fill{height:100%;border-radius:4px}
.stat-row{display:flex;justify-content:space-between;align-items:center;padding:9px 0;border-bottom:1px solid #f4f5f7;font-size:13px;gap:8px}
.stat-row:last-child{border-bottom:none}
.stat-label{color:#6b7280}.stat-val{font-weight:600;text-align:right}
.section{display:none}.section.active{display:block}
.alert-bar{background:#fffbeb;border:1px solid #fcd34d;border-radius:10px;padding:10px 14px;display:flex;align-items:flex-start;gap:8px;margin-bottom:14px;font-size:12px;color:#92400e;line-height:1.5}

/* LEADS */
.lead-item{display:flex;align-items:center;gap:12px;padding:11px 0;border-bottom:1px solid #f4f5f7}
.lead-item:last-child{border-bottom:none}
.lead-avatar{width:34px;height:34px;border-radius:50%;background:#ede9fe;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:600;color:#534AB7;flex-shrink:0}
.lead-info{flex:1;min-width:0}
.lead-name{font-size:13px;font-weight:600;color:#1a1a2e;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.lead-contact{font-size:11px;color:#6b7280;margin-top:2px}
.lead-time{font-size:11px;color:#9ca3af;flex-shrink:0}

/* RULES */
.rule-item{display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid #f4f5f7}
.rule-item:last-child{border-bottom:none}
.rule-icon-wrap{width:34px;height:34px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0}
.icon-red{background:#fee2e2}.icon-green{background:#d1fae5}.icon-yellow{background:#fef3c7}.icon-purple{background:#ede9fe}.icon-blue{background:#dbeafe}
.rule-info{flex:1;min-width:0}
.rule-title{font-size:13px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.rule-desc{font-size:11px;color:#6b7280;margin-top:2px}
.toggle-switch{position:relative;width:38px;height:20px;flex-shrink:0}
.toggle-switch input{opacity:0;width:0;height:0}
.toggle-track{position:absolute;inset:0;background:#d1d5db;border-radius:20px;cursor:pointer;transition:background .2s}
.toggle-track:before{content:'';position:absolute;height:16px;width:16px;left:2px;top:2px;background:#fff;border-radius:50%;transition:transform .2s;box-shadow:0 1px 3px rgba(0,0,0,.2)}
.toggle-switch input:checked+.toggle-track{background:#534AB7}
.toggle-switch input:checked+.toggle-track:before{transform:translateX(18px)}

/* AI CHAT */
.ai-chat{display:flex;flex-direction:column;height:360px}
.ai-messages{flex:1;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:10px}
.msg{max-width:85%;padding:10px 13px;border-radius:12px;font-size:13px;line-height:1.55}
.msg-user{background:#534AB7;color:#fff;align-self:flex-end;border-bottom-right-radius:4px}
.msg-ai{background:#f4f5f7;color:#1a1a2e;align-self:flex-start;border-bottom-left-radius:4px}
.msg-ai.loading{color:#9ca3af}
.ai-input-row{display:flex;gap:8px;padding:10px 14px;border-top:1px solid #f0f0f4}
.ai-input{flex:1;padding:9px 13px;border-radius:8px;border:1.5px solid #e0e0e8;font-size:13px;font-family:inherit;outline:none;min-width:0}
.ai-input:focus{border-color:#534AB7}

/* LOGS */
.log-line{font-family:monospace;font-size:12px;color:#374151;line-height:2;padding:2px 0;border-bottom:1px solid #f9fafb}
.log-time{color:#9ca3af;margin-right:8px}

/* CAMP MOBILE CARDS */
.camp-cards{display:none;flex-direction:column;gap:10px}
.camp-card{background:#fff;border:1px solid #e8e8ec;border-radius:12px;padding:14px}
.camp-card-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:10px}
.camp-card-name{font-size:13px;font-weight:600}
.camp-card-type{font-size:11px;color:#9ca3af;margin-top:1px}
.camp-card-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:10px}
.camp-metric{background:#f9fafb;border-radius:8px;padding:8px}
.camp-metric-label{font-size:10px;color:#9ca3af}
.camp-metric-value{font-size:14px;font-weight:700;margin-top:2px}
.camp-card-actions{display:flex;gap:8px}

/* OPT */
.opt-item{display:flex;align-items:flex-start;gap:12px;padding:12px 0;border-bottom:1px solid #f4f5f7;flex-wrap:wrap}
.opt-item:last-child{border-bottom:none}
.opt-priority{width:7px;height:7px;border-radius:50%;margin-top:4px;flex-shrink:0}
.priority-high{background:#ef4444}.priority-mid{background:#f59e0b}.priority-low{background:#10b981}
.opt-body{flex:1;min-width:160px}
.opt-title{font-size:13px;font-weight:600}
.opt-desc{font-size:12px;color:#6b7280;margin-top:3px}
.opt-impact{font-size:11px;font-weight:600;margin-top:4px}
.impact-high{color:#ef4444}.impact-mid{color:#f59e0b}.impact-low{color:#10b981}
.quick-btns{display:flex;flex-wrap:wrap;gap:8px}

/* EMPTY */
.empty-state{text-align:center;padding:40px 20px;color:#9ca3af}
.empty-icon{font-size:36px;margin-bottom:12px}
.empty-text{font-size:14px}

@media(max-width:768px){
  .sidebar{position:fixed;top:0;left:0;height:100%;transform:translateX(-100%)}
  .sidebar.open{transform:translateX(0)}
  .hamburger{display:block}
  .close-sb{display:block}
  .metrics-grid{grid-template-columns:repeat(2,1fr);gap:10px}
  .metric-value{font-size:18px}
  .grid2{grid-template-columns:1fr}
  .table-wrap table{display:none}
  .camp-cards{display:flex}
  .content{padding:12px}
  .card-body{padding:14px}
  .config-grid{grid-template-columns:1fr}
}
</style>
</head>
<body>
<div class="overlay" id="overlay" onclick="closeSB()"></div>
<div class="app">

<!-- SIDEBAR -->
<div class="sidebar" id="sidebar">
  <div class="logo">
    <div><div class="logo-text">Metron</div><div class="logo-sub">Powered by Claude AI</div></div>
    <button class="close-sb" onclick="closeSB()">✕</button>
  </div>
  <div class="nav-section">Principal</div>
  <div class="nav-item active" onclick="nav('config',this)"><span class="nav-icon">⚙️</span>Configuração</div>
  <div class="nav-item" onclick="nav('dashboard',this)"><span class="nav-icon">📊</span>Dashboard</div>
  <div class="nav-item" onclick="nav('campanhas',this)"><span class="nav-icon">📢</span>Campanhas</div>
  <div class="nav-item" onclick="nav('leads',this)"><span class="nav-icon">🎯</span>Leads</div>
  <div class="nav-section">Inteligência</div>
  <div class="nav-item" onclick="nav('ml',this)"><span class="nav-icon">🧠</span>Machine Learning</div>
  <div class="nav-item" onclick="nav('otimizacao',this)"><span class="nav-icon">🎯</span>Otimização</div>
  <div class="nav-item" onclick="nav('automacao',this)"><span class="nav-icon">🔁</span>Automação</div>
  <div class="nav-item" onclick="nav('ia',this)"><span class="nav-icon">🤖</span>Assistente IA</div>
</div>

<div class="main">
  <div class="topbar">
    <div class="topbar-left">
      <button class="hamburger" onclick="openSB()">☰</button>
      <div class="page-title" id="page-title">Configuração</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center">
      <div id="conn-indicator" style="font-size:12px;color:#9ca3af">Não conectado</div>
      <button class="btn btn-primary btn-sm" onclick="syncAll()">↻ Sincronizar</button>
    </div>
  </div>

  <div class="content">

    <!-- CONFIGURAÇÃO -->
    <div class="section active" id="sec-config">
      <div class="config-panel">
        <div class="config-title">Conectar ao Backend Metron</div>
        <div class="config-row">
          <label class="config-label">URL do Backend</label>
          <input class="config-input" id="cfg-url" placeholder="http://localhost:3000" value="http://localhost:3000">
          <div class="config-hint">Endereço onde seu servidor Node.js está rodando (ex: Railway, Render)</div>
        </div>
        <button class="btn btn-primary" onclick="testConnection()" style="width:100%;justify-content:center;padding:10px">Testar conexão</button>
        <div id="conn-result" style="margin-top:12px"></div>
      </div>

      <div class="config-panel">
        <div class="config-title">Assistente IA (Claude API)</div>
        <div class="config-row">
          <label class="config-label">Anthropic API Key</label>
          <input class="config-input" id="cfg-apikey" type="password" placeholder="sk-ant-...">
          <div class="config-hint">Necessária para o chat com o Assistente IA. Obtenha em console.anthropic.com</div>
        </div>
        <button class="btn btn-primary" onclick="saveConfig()" style="width:100%;padding:10px">Salvar configurações</button>
      </div>

      <div class="config-panel">
        <div class="config-title">Como colocar em produção</div>
        <div style="display:flex;flex-direction:column;gap:10px;font-size:13px;color:#374151;line-height:1.7">
          <div>1. Baixe o código do backend (artifact "Metron Backend")</div>
          <div>2. Configure o arquivo <code style="background:#f4f5f7;padding:2px 6px;border-radius:4px">.env</code> com seus tokens da Meta e banco de dados</div>
          <div>3. Faça deploy no Railway (<a href="https://railway.app" style="color:#534AB7">railway.app</a>) ou Render</div>
          <div>4. Cole a URL gerada no campo acima e clique em "Testar conexão"</div>
          <div>5. Configure o webhook no Meta for Developers apontando para <code style="background:#f4f5f7;padding:2px 6px;border-radius:4px">sua-url/webhook</code></div>
        </div>
      </div>
    </div>

    <!-- DASHBOARD -->
    <div class="section" id="sec-dashboard">
      <div id="dash-alert" style="display:none" class="alert-bar"></div>
      <div class="metrics-grid" id="dash-metrics">
        <div class="metric-card"><div class="metric-label">Total de Leads</div><div class="skeleton"></div></div>
        <div class="metric-card"><div class="metric-label">CPL Médio</div><div class="skeleton"></div></div>
        <div class="metric-card"><div class="metric-label">Score ML Médio</div><div class="skeleton"></div></div>
        <div class="metric-card"><div class="metric-label">Campanhas Ativas</div><div class="skeleton"></div></div>
      </div>
      <div class="grid2">
        <div class="card">
          <div class="card-header"><div class="card-title">Leads por campanha</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="leadsChart" role="img" aria-label="Leads por campanha"></canvas></div></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">CPL por campanha</div></div>
          <div class="card-body"><div class="chart-container"><canvas id="cplChart" role="img" aria-label="CPL por campanha"></canvas></div></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Resumo</div></div>
        <div class="card-body" id="dash-summary"><div class="skeleton" style="margin-bottom:10px"></div><div class="skeleton" style="margin-bottom:10px"></div><div class="skeleton"></div></div>
      </div>
    </div>

    <!-- CAMPANHAS -->
    <div class="section" id="sec-campanhas">
      <div class="card">
        <div class="card-header">
          <div class="card-title">Campanhas</div>
          <div style="display:flex;gap:8px">
            <button class="btn btn-sm" onclick="loadCampaigns()">↻ Atualizar</button>
            <button class="btn btn-primary btn-sm" onclick="syncAll()">Sincronizar Meta</button>
          </div>
        </div>
        <div class="card-body" style="padding:0">
          <div class="table-wrap"><table>
            <thead><tr><th>Campanha</th><th>Status</th><th>Leads</th><th>CPL</th><th>Score ML</th><th>Budget</th><th>Ações</th></tr></thead>
            <tbody id="camp-tbody"></tbody>
          </table></div>
          <div class="camp-cards" id="camp-cards" style="padding:14px"></div>
        </div>
      </div>
    </div>

    <!-- LEADS -->
    <div class="section" id="sec-leads">
      <div class="metrics-grid" id="leads-metrics">
        <div class="metric-card"><div class="metric-label">Total de leads</div><div class="metric-value" id="leads-total">—</div></div>
        <div class="metric-card"><div class="metric-label">Hoje</div><div class="metric-value" id="leads-today">—</div></div>
        <div class="metric-card"><div class="metric-label">Esta semana</div><div class="metric-value" id="leads-week">—</div></div>
        <div class="metric-card"><div class="metric-label">Este mês</div><div class="metric-value" id="leads-month">—</div></div>
      </div>
      <div class="card">
        <div class="card-header">
          <div class="card-title">Leads capturados</div>
          <button class="btn btn-sm" onclick="loadLeads()">↻ Atualizar</button>
        </div>
        <div class="card-body" id="leads-list">
          <div class="empty-state"><div class="empty-icon">🎯</div><div class="empty-text">Conecte ao backend para ver os leads</div></div>
        </div>
      </div>
    </div>

    <!-- ML -->
    <div class="section" id="sec-ml">
      <div class="card">
        <div class="card-header">
          <div><div class="card-title">Previsões de performance</div><div style="font-size:12px;color:#6b7280;margin-top:2px">Score 0–100 baseado em CPL, CTR, frequência e leads</div></div>
          <button class="btn btn-primary btn-sm" onclick="retrainML()">🔄 Retreinar modelo</button>
        </div>
        <div class="card-body" id="ml-body">
          <div class="empty-state"><div class="empty-icon">🧠</div><div class="empty-text">Conecte ao backend para ver as previsões</div></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Insights do modelo</div></div>
        <div class="card-body" id="ml-insights">
          <div class="empty-state"><div class="empty-icon">💡</div><div class="empty-text">Aguardando dados do ML</div></div>
        </div>
      </div>
    </div>

    <!-- OTIMIZAÇÃO -->
    <div class="section" id="sec-otimizacao">
      <div class="card">
        <div class="card-header"><div class="card-title">Sugestões de otimização</div><button class="btn btn-primary btn-sm" onclick="analyzeWithAI()">🤖 Analisar com IA</button></div>
        <div class="card-body" id="opt-list">
          <div class="empty-state"><div class="empty-icon">🎯</div><div class="empty-text">Conecte ao backend para gerar sugestões</div></div>
        </div>
      </div>
    </div>

    <!-- AUTOMAÇÃO -->
    <div class="section" id="sec-automacao">
      <div class="card">
        <div class="card-header"><div class="card-title">Regras de automação</div><button class="btn btn-primary btn-sm" onclick="runAutomation()">▶ Executar agora</button></div>
        <div class="card-body">
          <div class="rule-item"><div class="rule-icon-wrap icon-red">🛑</div><div class="rule-info"><div class="rule-title">Pausar anúncios com CPL alto</div><div class="rule-desc">CPL > R$25 por 3 dias → pausar e notificar</div></div><label class="toggle-switch"><input type="checkbox" checked><span class="toggle-track"></span></label></div>
          <div class="rule-item"><div class="rule-icon-wrap icon-green">📈</div><div class="rule-info"><div class="rule-title">Escalar budget performático</div><div class="rule-desc">CPL < R$10 e leads > 20/dia → +20% budget</div></div><label class="toggle-switch"><input type="checkbox" checked><span class="toggle-track"></span></label></div>
          <div class="rule-item"><div class="rule-icon-wrap icon-yellow">⚠️</div><div class="rule-info"><div class="rule-title">Alerta de frequência alta</div><div class="rule-desc">Frequência > 4 → alertar e reduzir lance</div></div><label class="toggle-switch"><input type="checkbox" checked><span class="toggle-track"></span></label></div>
          <div class="rule-item"><div class="rule-icon-wrap icon-purple">🧠</div><div class="rule-info"><div class="rule-title">Ação automática por ML</div><div class="rule-desc">Score ML < 30 por 2 dias → pausar campanha</div></div><label class="toggle-switch"><input type="checkbox" checked><span class="toggle-track"></span></label></div>
          <div class="rule-item"><div class="rule-icon-wrap icon-blue">⚡</div><div class="rule-info"><div class="rule-title">Ajuste de lances por horário</div><div class="rule-desc">+30% de lance nos horários de pico</div></div><label class="toggle-switch"><input type="checkbox" checked><span class="toggle-track"></span></label></div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Log de execuções</div><button class="btn btn-sm" onclick="loadLogs()">↻ Atualizar</button></div>
        <div class="card-body" id="logs-body">
          <div class="empty-state"><div class="empty-icon">📋</div><div class="empty-text">Conecte ao backend para ver os logs</div></div>
        </div>
      </div>
    </div>

    <!-- IA -->
    <div class="section" id="sec-ia">
      <div class="card">
        <div class="card-header"><div class="card-title">🤖 Assistente Metron</div><span style="font-size:12px;color:#00b894" id="ia-status">● Aguardando API Key</span></div>
        <div class="ai-chat">
          <div class="ai-messages" id="ai-messages">
            <div class="msg msg-ai">Olá! Sou o assistente Metron. Configure sua Anthropic API Key na tela de Configuração para ativar as respostas com IA real.</div>
          </div>
          <div class="ai-input-row">
            <input class="ai-input" id="ai-input" placeholder="Pergunte sobre suas campanhas..." onkeydown="if(event.key==='Enter')sendAI()">
            <button class="btn btn-primary" onclick="sendAI()">Enviar</button>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header"><div class="card-title">Sugestões rápidas</div></div>
        <div class="card-body"><div class="quick-btns">
          <button class="btn" onclick="quickAsk('Analise minhas campanhas e diga quais têm melhor performance')">Analisar performance</button>
          <button class="btn" onclick="quickAsk('Quais campanhas devo pausar e quais devo escalar agora?')">Pausar ou escalar</button>
          <button class="btn" onclick="quickAsk('Como reduzir meu CPL médio nos próximos 30 dias?')">Reduzir CPL</button>
          <button class="btn" onclick="quickAsk('Crie 3 headlines para anúncios de captação de leads')">Gerar copies</button>
          <button class="btn" onclick="quickAsk('Quais são as 3 ações mais urgentes para maximizar leads agora?')">Ações urgentes</button>
        </div></div>
      </div>
    </div>

  </div>
</div>
</div>

<script>
// ── Estado global ──────────────────────────────────────────
const STATE = {
  backendUrl: localStorage.getItem('metron_url') || 'http://localhost:3000',
  apiKey:     localStorage.getItem('metron_key') || '',
  campaigns:  [],
  leads:      [],
  connected:  false,
};

// Restaurar config salva
document.getElementById('cfg-url').value = STATE.backendUrl;
document.getElementById('cfg-apikey').value = STATE.apiKey ? '••••••••' : '';

// ── Navegação ──────────────────────────────────────────────
function openSB(){document.getElementById('sidebar').classList.add('open');document.getElementById('overlay').classList.add('show')}
function closeSB(){document.getElementById('sidebar').classList.remove('open');document.getElementById('overlay').classList.remove('show')}
function nav(id,el){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById('sec-'+id).classList.add('active');
  el.classList.add('active');
  const t={config:'Configuração',dashboard:'Dashboard',campanhas:'Campanhas',leads:'Leads',ml:'Machine Learning',otimizacao:'Otimização',automacao:'Automação',ia:'Assistente IA'};
  document.getElementById('page-title').textContent=t[id];
  closeSB();
  if(id==='dashboard' && STATE.connected) loadDashboard();
  if(id==='campanhas' && STATE.connected) loadCampaigns();
  if(id==='leads' && STATE.connected) loadLeads();
  if(id==='ml' && STATE.connected) loadML();
  if(id==='otimizacao' && STATE.connected) loadOptimization();
  if(id==='automacao' && STATE.connected) loadLogs();
}

// ── Config & Conexão ───────────────────────────────────────
function saveConfig(){
  const url = document.getElementById('cfg-url').value.trim();
  const key = document.getElementById('cfg-apikey').value.trim();
  if(url) { STATE.backendUrl = url; localStorage.setItem('metron_url', url); }
  if(key && key !== '••••••••') { STATE.apiKey = key; localStorage.setItem('metron_key', key); }
  document.getElementById('ia-status').textContent = STATE.apiKey ? '● Online' : '● Aguardando API Key';
  document.getElementById('ia-status').style.color = STATE.apiKey ? '#00b894' : '#9ca3af';
  showResult('conn-result','✅ Configurações salvas!','#d1fae5','#065f46');
}

async function testConnection(){
  const url = document.getElementById('cfg-url').value.trim();
  showResult('conn-result','Testando conexão...','#dbeafe','#1e40af');
  try {
    const r = await fetch(`${url}/api/campaigns`, {signal: AbortSignal.timeout(5000)});
    if(!r.ok) throw new Error(`Status ${r.status}`);
    const data = await r.json();
    STATE.backendUrl = url;
    STATE.connected = true;
    STATE.campaigns = data.data || [];
    localStorage.setItem('metron_url', url);
    showResult('conn-result',`✅ Conectado com sucesso! ${STATE.campaigns.length} campanhas encontradas.`,'#d1fae5','#065f46');
    document.getElementById('conn-indicator').textContent = '● Conectado';
    document.getElementById('conn-indicator').style.color = '#00b894';
  } catch(e) {
    showResult('conn-result',`❌ Erro: ${e.message}. Verifique se o backend está rodando.`,'#fee2e2','#991b1b');
    STATE.connected = false;
  }
}

function showResult(id, msg, bg, color){
  const el = document.getElementById(id);
  el.style.cssText = `padding:10px 14px;border-radius:8px;font-size:13px;background:${bg};color:${color}`;
  el.textContent = msg;
}

// ── API Helper ─────────────────────────────────────────────
async function api(path, method='GET', body=null){
  const opts = { method, headers: {'Content-Type':'application/json'} };
  if(body) opts.body = JSON.stringify(body);
  const r = await fetch(`${STATE.backendUrl}${path}`, opts);
  return r.json();
}

async function syncAll(){
  if(!STATE.connected){ alert('Configure e teste a conexão primeiro.'); return; }
  document.getElementById('conn-indicator').textContent = '⟳ Sincronizando...';
  try {
    const data = await api('/api/campaigns');
    STATE.campaigns = data.data || [];
    loadDashboard();
    document.getElementById('conn-indicator').textContent = '● Conectado';
    document.getElementById('conn-indicator').style.color = '#00b894';
  } catch(e) {
    document.getElementById('conn-indicator').textContent = '● Erro na sincronização';
    document.getElementById('conn-indicator').style.color = '#ef4444';
  }
}

// ── Cálculo de score ML local ──────────────────────────────
function calcScore(c){
  const cplScore  = Math.max(0,Math.min(100,((30-c.cpl)/20)*100));
  const ctrScore  = Math.max(0,Math.min(100,(c.ctr/4)*100));
  const freqScore = Math.max(0,Math.min(100,((5-(c.frequencia||1))/3)*100));
  const ldScore   = Math.max(0,Math.min(100,(c.leads/50)*100));
  return Math.round(cplScore*.4 + ctrScore*.25 + freqScore*.2 + ldScore*.15);
}

// ── Dashboard ──────────────────────────────────────────────
let leadsChartInst, cplChartInst;
function loadDashboard(){
  if(!STATE.campaigns.length) return;
  const cs = STATE.campaigns;
  const totalLeads = cs.reduce((s,c)=>s+c.leads,0);
  const avgCPL = cs.filter(c=>c.cpl>0).reduce((s,c,_,a)=>s+c.cpl/a.length,0);
  const avgScore = Math.round(cs.reduce((s,c)=>s+calcScore(c),0)/cs.length);
  const active = cs.filter(c=>c.status==='ACTIVE').length;

  document.getElementById('dash-metrics').innerHTML = `
    <div class="metric-card"><div class="metric-label">Total de Leads</div><div class="metric-value">${totalLeads.toLocaleString('pt-BR')}</div><div class="metric-delta delta-neutral">Últimos 30 dias</div></div>
    <div class="metric-card"><div class="metric-label">CPL Médio</div><div class="metric-value">R$${avgCPL.toFixed(2)}</div><div class="metric-delta delta-neutral">Todas as campanhas</div></div>
    <div class="metric-card"><div class="metric-label">Score ML Médio</div><div class="metric-value">${avgScore}<span style="font-size:14px">/100</span></div><div class="metric-delta delta-neutral">Saúde geral</div></div>
    <div class="metric-card"><div class="metric-label">Campanhas Ativas</div><div class="metric-value">${active}</div><div class="metric-delta delta-neutral">de ${cs.length} total</div></div>`;

  const risk = cs.filter(c=>calcScore(c)<40);
  if(risk.length){
    const al = document.getElementById('dash-alert');
    al.style.display='flex';
    al.innerHTML=`⚠️ <strong>ML detectou risco:</strong> ${risk.map(c=>c.nome).join(', ')} com score abaixo de 40. Ação recomendada.`;
  }

  const summary = document.getElementById('dash-summary');
  const best = cs.sort((a,b)=>a.cpl-b.cpl)[0];
  const worst = cs.sort((a,b)=>b.cpl-a.cpl)[0];
  summary.innerHTML = `
    <div class="stat-row"><span class="stat-label">Melhor CPL</span><span class="stat-val" style="color:#00b894">${best?.nome} — R$${best?.cpl?.toFixed(2)}</span></div>
    <div class="stat-row"><span class="stat-label">Maior CPL</span><span class="stat-val" style="color:#ef4444">${worst?.nome} — R$${worst?.cpl?.toFixed(2)}</span></div>
    <div class="stat-row"><span class="stat-label">Total investido</span><span class="stat-val">R$${cs.reduce((s,c)=>s+c.gasto,0).toLocaleString('pt-BR',{minimumFractionDigits:2})}</span></div>
    <div class="stat-row"><span class="stat-label">Total de campanhas</span><span class="stat-val">${cs.length} campanhas</span></div>`;

  const isDark=matchMedia('(prefers-color-scheme:dark)').matches;
  const gc=isDark?'rgba(255,255,255,.07)':'rgba(0,0,0,.07)';
  const tc=isDark?'#9ca3af':'#6b7280';
  const labels = cs.map(c=>c.nome.split(' ').slice(0,2).join(' '));

  if(leadsChartInst) leadsChartInst.destroy();
  leadsChartInst = new Chart(document.getElementById('leadsChart'),{
    type:'bar',
    data:{labels,datasets:[{label:'Leads',data:cs.map(c=>c.leads),backgroundColor:'#534AB7',borderRadius:6,borderSkipped:false}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gc},ticks:{color:tc,maxRotation:30}},y:{grid:{color:gc},ticks:{color:tc}}}}
  });
  if(cplChartInst) cplChartInst.destroy();
  cplChartInst = new Chart(document.getElementById('cplChart'),{
    type:'bar',
    data:{labels,datasets:[{label:'CPL (R$)',data:cs.map(c=>c.cpl),backgroundColor:cs.map(c=>c.cpl<10?'#00b894':c.cpl>20?'#ef4444':'#f59e0b'),borderRadius:6,borderSkipped:false}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:gc},ticks:{color:tc,maxRotation:30}},y:{grid:{color:gc},ticks:{color:tc,callback:v=>'R$'+v}}}}
  });
}

// ── Campanhas ──────────────────────────────────────────────
async function loadCampaigns(){
  if(!STATE.connected){ showEmpty('camp-tbody','Conecte ao backend primeiro'); return; }
  try {
    const data = await api('/api/campaigns');
    STATE.campaigns = data.data || [];
    renderCampaigns();
  } catch(e){ showEmpty('camp-tbody','Erro ao carregar campanhas'); }
}

function renderCampaigns(){
  const cs = STATE.campaigns;
  if(!cs.length){ showEmpty('camp-tbody','Nenhuma campanha encontrada'); return; }
  document.getElementById('camp-tbody').innerHTML = cs.map((c,i)=>{
    const pct=Math.min(100,(c.gasto/c.orcamento*100)).toFixed(0);
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const score=calcScore(c);
    const sc=score>75?'#00b894':score>50?'#f59e0b':'#ef4444';
    const cc=c.cpl>20?'color:#ef4444':c.cpl<10?'color:#00b894':'';
    return`<tr>
      <td><div style="font-weight:600">${c.nome}</div><div style="font-size:11px;color:#9ca3af">${c.objetivo||''}</div></td>
      <td><span class="badge ${badge}">${lbl}</span></td>
      <td><strong>${c.leads}</strong></td>
      <td style="${cc};font-weight:600">R$${c.cpl.toFixed(2)}</td>
      <td><div style="display:flex;align-items:center;gap:6px"><div style="flex:1;height:5px;background:#f0f0f4;border-radius:5px"><div style="height:100%;width:${score}%;background:${sc};border-radius:5px"></div></div><span style="font-size:12px;font-weight:600;color:${sc}">${score}</span></div></td>
      <td><div style="font-size:12px">R$${c.gasto.toLocaleString('pt-BR',{minimumFractionDigits:0})} / R$${c.orcamento.toLocaleString('pt-BR')}</div><div class="progress-bar"><div class="progress-fill" style="width:${pct}%;background:#534AB7"></div></div></td>
      <td><div style="display:flex;gap:6px">
        ${c.status==='ACTIVE'
          ?`<button class="btn btn-sm btn-danger" onclick="toggleCamp('${c.id}','PAUSED',${i})">Pausar</button>`
          :`<button class="btn btn-sm btn-success" onclick="toggleCamp('${c.id}','ACTIVE',${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">IA ↗</button>
      </div></td>
    </tr>`;
  }).join('');

  document.getElementById('camp-cards').innerHTML = cs.map((c,i)=>{
    const badge=c.status==='ACTIVE'?'badge-active':c.status==='PAUSED'?'badge-paused':'badge-learning';
    const lbl=c.status==='ACTIVE'?'Ativo':c.status==='PAUSED'?'Pausado':'Aprendendo';
    const score=calcScore(c);
    const sc=score>75?'#00b894':score>50?'#f59e0b':'#ef4444';
    const cc=c.cpl>20?'#ef4444':c.cpl<10?'#00b894':'#1a1a2e';
    return`<div class="camp-card">
      <div class="camp-card-header"><div><div class="camp-card-name">${c.nome}</div><div class="camp-card-type">${c.objetivo||''}</div></div><span class="badge ${badge}">${lbl}</span></div>
      <div class="camp-card-metrics">
        <div class="camp-metric"><div class="camp-metric-label">Leads</div><div class="camp-metric-value">${c.leads}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">CPL</div><div class="camp-metric-value" style="color:${cc}">R$${c.cpl.toFixed(2)}</div></div>
        <div class="camp-metric"><div class="camp-metric-label">Score ML</div><div class="camp-metric-value" style="color:${sc}">${score}</div></div>
      </div>
      <div class="camp-card-actions">
        ${c.status==='ACTIVE'
          ?`<button class="btn btn-sm btn-danger" onclick="toggleCamp('${c.id}','PAUSED',${i})">Pausar</button>`
          :`<button class="btn btn-sm btn-success" onclick="toggleCamp('${c.id}','ACTIVE',${i})">Ativar</button>`}
        <button class="btn btn-primary btn-sm" onclick="analyzecamp(${i})">Analisar IA ↗</button>
      </div>
    </div>`;
  }).join('');
}

async function toggleCamp(id, status, idx){
  try {
    const path = status==='PAUSED' ? `/api/campaigns/${id}/pause` : `/api/campaigns/${id}/activate`;
    await api(path, 'POST');
    STATE.campaigns[idx].status = status;
    renderCampaigns();
  } catch(e){ alert('Erro ao atualizar campanha: '+e.message); }
}

// ── Leads ──────────────────────────────────────────────────
async function loadLeads(){
  if(!STATE.connected) return;
  try {
    const data = await api('/api/leads');
    STATE.leads = data.data || [];
    renderLeads();
  } catch(e){ document.getElementById('leads-list').innerHTML = '<div class="empty-state"><div class="empty-icon">❌</div><div class="empty-text">Erro ao carregar leads</div></div>'; }
}

function renderLeads(){
  const ls = STATE.leads;
  const now = new Date();
  const today = ls.filter(l=>new Date(l.criado_em).toDateString()===now.toDateString()).length;
  const week  = ls.filter(l=>(now-new Date(l.criado_em))<7*86400000).length;
  const month = ls.filter(l=>(now-new Date(l.criado_em))<30*86400000).length;
  document.getElementById('leads-total').textContent = ls.length;
  document.getElementById('leads-today').textContent = today;
  document.getElementById('leads-week').textContent = week;
  document.getElementById('leads-month').textContent = month;

  if(!ls.length){ document.getElementById('leads-list').innerHTML='<div class="empty-state"><div class="empty-icon">🎯</div><div class="empty-text">Nenhum lead capturado ainda. Configure o webhook.</div></div>'; return; }
  document.getElementById('leads-list').innerHTML = ls.slice(0,20).map(l=>{
    const initials = (l.nome||'?').split(' ').map(n=>n[0]).slice(0,2).join('').toUpperCase();
    const time = new Date(l.criado_em).toLocaleString('pt-BR',{day:'2-digit',month:'2-digit',hour:'2-digit',minute:'2-digit'});
    return`<div class="lead-item">
      <div class="lead-avatar">${initials}</div>
      <div class="lead-info"><div class="lead-name">${l.nome||'Sem nome'}</div><div class="lead-contact">${l.email||''} ${l.telefone?'· '+l.telefone:''}</div></div>
      <div class="lead-time">${time}</div>
    </div>`;
  }).join('');
}

// ── ML ─────────────────────────────────────────────────────
async function loadML(){
  if(!STATE.connected || !STATE.campaigns.length) return;
  const cs = STATE.campaigns.map(c=>({...c,score:calcScore(c)}));
  document.getElementById('ml-body').innerHTML = cs.map(c=>{
    const sc=c.score>75?'#00b894':c.score>50?'#f59e0b':'#ef4444';
    const risk=100-c.score;
    return`<div style="display:flex;align-items:center;gap:14px;padding:12px 0;border-bottom:1px solid #f4f5f7">
      <div style="flex:1"><div style="font-size:13px;font-weight:600">${c.nome}</div><div style="height:6px;background:#f0f0f4;border-radius:6px;margin:6px 0"><div style="height:100%;width:${c.score}%;background:${sc};border-radius:6px"></div></div><div style="font-size:11px;color:${sc}">${c.score>75?'Saudável':c.score>50?'Atenção necessária':'Alto risco'} — ${risk}% chance de piora</div></div>
      <div style="font-size:24px;font-weight:700;color:${sc}">${c.score}</div>
    </div>`;
  }).join('');

  const insights = cs.filter(c=>c.score<50).map(c=>`
    <div style="display:flex;gap:10px;padding:10px 0;border-bottom:1px solid #f4f5f7">
      <div style="width:8px;height:8px;border-radius:50%;background:#ef4444;margin-top:4px;flex-shrink:0"></div>
      <div style="font-size:13px;color:#374151">${c.nome}: score ${c.score}/100 — CPL R$${c.cpl.toFixed(2)}, CTR ${c.ctr}%. Ação recomendada.</div>
    </div>`).join('') || '<div style="font-size:13px;color:#374151;padding:12px 0">Todas as campanhas estão saudáveis. Continue monitorando.</div>';
  document.getElementById('ml-insights').innerHTML = insights;
}

async function retrainML(){
  try {
    const r = await api('/api/ml/retrain','POST');
    alert(r.data?.message || 'Modelo retreinado com sucesso!');
  } catch(e){ alert('Erro ao retreinar: '+e.message); }
}

// ── Otimização ─────────────────────────────────────────────
function loadOptimization(){
  if(!STATE.campaigns.length){ document.getElementById('opt-list').innerHTML='<div class="empty-state"><div class="empty-icon">🎯</div><div class="empty-text">Carregue as campanhas primeiro</div></div>'; return; }
  const cs = STATE.campaigns;
  const suggestions = [];

  cs.forEach(c=>{
    if(c.cpl>20) suggestions.push({priority:'high',title:`Pausar "${c.nome}"`,desc:`CPL R$${c.cpl.toFixed(2)} está acima da meta. Realocar budget.`,impact:`Economia estimada: R$${(c.gasto*.3).toFixed(0)}/mês`,btn:'Pausar',action:()=>api(`/api/campaigns/${c.id}/pause`,'POST')});
    if(c.cpl<10 && c.leads>30) suggestions.push({priority:'high',title:`Escalar "${c.nome}"`,desc:`CPL R$${c.cpl.toFixed(2)} excelente. Aumentar budget em 20% agora.`,impact:`+${Math.round(c.leads*.2)} leads estimados`,btn:'Escalar',action:()=>api(`/api/campaigns/${c.id}/budget`,'POST',{daily_budget:c.orcamento*1.2})});
    if((c.frequencia||0)>3.5) suggestions.push({priority:'mid',title:`Renovar público de "${c.nome}"`,desc:`Frequência ${c.frequencia?.toFixed(1)} — sinais de saturação.`,impact:'Impacto médio — evitar saturação',btn:'Ver IA',action:()=>quickAsk(`Como renovar o público da campanha ${c.nome} com frequência ${c.frequencia}?`)});
  });

  if(!suggestions.length){ document.getElementById('opt-list').innerHTML='<div class="empty-state"><div class="empty-icon">✅</div><div class="empty-text">Nenhuma sugestão urgente no momento. Campanhas saudáveis!</div></div>'; return; }
  document.getElementById('opt-list').innerHTML = suggestions.map((s,i)=>`
    <div class="opt-item">
      <div class="opt-priority priority-${s.priority}"></div>
      <div class="opt-body"><div class="opt-title">${s.title}</div><div class="opt-desc">${s.desc}</div><div class="opt-impact impact-${s.priority}">${s.impact}</div></div>
      <button class="btn btn-sm" id="opt-btn-${i}" onclick="applyOpt(${i})">${s.btn}</button>
    </div>`).join('');
  window._optSuggestions = suggestions;
}

async function applyOpt(i){
  const s = window._optSuggestions[i];
  try { await s.action(); document.getElementById(`opt-btn-${i}`).textContent='✓ Aplicado'; document.getElementById(`opt-btn-${i}`).disabled=true; }
  catch(e){ alert('Erro: '+e.message); }
}

// ── Automação ──────────────────────────────────────────────
async function runAutomation(){
  try {
    const r = await api('/api/automation/run','POST');
    await loadLogs();
    alert(r.data?.length ? `${r.data.length} ações executadas.` : 'Regras verificadas — nenhuma ação necessária.');
  } catch(e){ alert('Erro: '+e.message); }
}

async function loadLogs(){
  if(!STATE.connected) return;
  try {
    const data = await api('/api/automation/logs');
    const logs = data.data || [];
    if(!logs.length){ document.getElementById('logs-body').innerHTML='<div class="empty-state"><div class="empty-icon">📋</div><div class="empty-text">Nenhum log ainda. Execute as regras.</div></div>'; return; }
    document.getElementById('logs-body').innerHTML = logs.map(l=>{
      const t = new Date(l.created_at).toLocaleString('pt-BR',{day:'2-digit',month:'2-digit',hour:'2-digit',minute:'2-digit'});
      const icon = l.action.includes('PAUSED')?'⏸️':l.action.includes('BUDGET')?'📈':l.action.includes('ALERT')?'⚠️':'✅';
      return`<div class="log-line"><span class="log-time">${t}</span>${icon} ${l.rule} — ${l.campanha}: ${l.detalhes||l.action}</div>`;
    }).join('');
  } catch(e){}
}

// ── Utilitários ────────────────────────────────────────────
function showEmpty(id, msg){ document.getElementById(id).innerHTML=`<tr><td colspan="7" style="text-align:center;padding:32px;color:#9ca3af">${msg}</td></tr>`; }

// ── AI ─────────────────────────────────────────────────────
async function callAI(prompt){
  const key = STATE.apiKey || localStorage.getItem('metron_key');
  if(!key){ addMsg('Configure sua Anthropic API Key na tela de Configuração.','ai'); return; }
  const load = addMsg('Analisando...','ai loading');
  const ctx = JSON.stringify(STATE.campaigns.map(c=>({nome:c.nome,leads:c.leads,cpl:'R$'+c.cpl?.toFixed(2),ctr:c.ctr+'%',score:calcScore(c)+'/100',status:c.status})));
  try {
    const r = await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json','x-api-key':key,'anthropic-version':'2023-06-01','anthropic-dangerous-direct-browser-access':'true'},
      body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:1000,
        system:`Você é o Metron, assistente especialista em Meta Ads e geração de leads. Responda em português, seja direto e prático. Campanhas: ${ctx}`,
        messages:[{role:'user',content:prompt}]})
    });
    const d = await r.json();
    load.remove();
    addMsg(d.content?.[0]?.text||'Erro na resposta.','ai');
  } catch(e){ load.textContent='Erro de conexão. Verifique sua API Key.'; }
}

function addMsg(text, type){
  const msgs = document.getElementById('ai-messages');
  const el = document.createElement('div');
  el.className = `msg msg-${type.includes('ai')?'ai':'user'}${type.includes('loading')?' loading':''}`;
  el.textContent = text;
  msgs.appendChild(el);
  msgs.scrollTop = msgs.scrollHeight;
  return el;
}

function sendAI(){ const inp=document.getElementById('ai-input'); const v=inp.value.trim(); if(!v)return; addMsg(v,'user'); inp.value=''; callAI(v); }
function quickAsk(q){ nav('ia',document.querySelectorAll('.nav-item')[7]); setTimeout(()=>{ addMsg(q,'user'); callAI(q); },100); }
function analyzeWithAI(){ quickAsk('Com base nos dados atuais, quais são as 3 ações mais urgentes para maximizar leads?'); }
function analyzecamp(i){ const c=STATE.campaigns[i]; quickAsk(`Analise a campanha "${c.nome}" com CPL R$${c.cpl?.toFixed(2)}, score ML ${calcScore(c)}/100 e ${c.leads} leads. O que devo fazer?`); }
</script>
</body>
</html>
