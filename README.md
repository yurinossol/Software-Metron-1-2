<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>Madeireira Rossi | Cambará e Eucalipto — Joinville</title>
  <meta name="description" content="Madeireira em Joinville: venda de madeira Cambará e Eucalipto. Tábuas, vigas, caibros, ripas e pranchas. Orçamento rápido no WhatsApp."/>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700;900&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg:       #050805;
      --bg2:      #080d08;
      --surface:  #0e160e;
      --surface2: #131c13;
      --border:   rgba(100,180,100,.10);
      --border2:  rgba(100,180,100,.20);
      --text:     #e8f2e8;
      --muted:    #7aaa7a;
      --faint:    #4a784a;
      --gold:     #5ebf5e;
      --gold2:    #82d882;
      --green:    #3a8a3a;
      --green2:   #5ebf5e;
      --radius:   16px;
      --max:      1100px;
    }
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0 }
    html { scroll-behavior: smooth }
    body {
      font-family: 'DM Sans', sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
      overflow-x: hidden;
    }
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background:
        radial-gradient(ellipse 900px 500px at 10% -10%, rgba(80,180,80,.10), transparent 60%),
        radial-gradient(ellipse 700px 400px at 90% 80%, rgba(40,120,40,.08), transparent 55%);
      pointer-events: none;
      z-index: 0;
    }
    a { color: inherit; text-decoration: none }
    .wrap { max-width: var(--max); margin: 0 auto; padding: 0 24px; position: relative; z-index: 1 }

    /* ── NAV ── */
    nav {
      position: sticky; top: 0; z-index: 100;
      background: rgba(5,8,5,.88);
      backdrop-filter: blur(14px);
      border-bottom: 1px solid var(--border);
    }
    .nav-inner {
      display: flex; align-items: center; justify-content: space-between;
      padding: 16px 0; gap: 16px; flex-wrap: wrap;
    }
    .brand {
      display: flex; align-items: center; gap: 12px;
    }
    .logo-mark {
      height: 44px;
      display: flex; align-items: center;
    }
    .logo-mark img { height: 44px; width: auto; display: block; border-radius: 0 }
    .brand-name {
      font-family: 'Playfair Display', serif;
      font-weight: 700;
      font-size: 1.1rem;
      letter-spacing: .2px;
    }
    .brand-sub {
      font-size: .78rem;
      color: var(--muted);
      font-weight: 400;
      letter-spacing: .5px;
      text-transform: uppercase;
    }
    .navlinks {
      display: flex; gap: 4px; align-items: center;
    }
    .navlinks a {
      font-size: .9rem;
      color: var(--muted);
      padding: 8px 12px;
      border-radius: 10px;
      transition: color .2s, background .2s;
    }
    .navlinks a:hover { color: var(--text); background: rgba(255,220,140,.07) }
    .nav-cta { display: flex; gap: 8px; align-items: center }
    .btn {
      display: inline-flex; align-items: center; gap: 7px;
      padding: 10px 16px;
      border-radius: 12px;
      font-family: 'DM Sans', sans-serif;
      font-size: .88rem;
      font-weight: 600;
      cursor: pointer;
      border: 1px solid var(--border2);
      background: rgba(255,220,140,.06);
      color: var(--text);
      transition: all .15s ease;
      white-space: nowrap;
      letter-spacing: .2px;
    }
    .btn:hover { background: rgba(255,220,140,.12); transform: translateY(-1px) }
    .btn:active { transform: translateY(0) }
    .btn-gold {
      background: linear-gradient(135deg, #3a8a3a 0%, #266026 100%);
      border-color: #3a8a3a;
      color: #e8f2e8;
    }
    .btn-gold:hover { background: linear-gradient(135deg, #4aaa4a, #338833); border-color: #4aaa4a }
    .btn-wa {
      background: rgba(37,211,102,.14);
      border-color: rgba(37,211,102,.28);
      color: #7de8a0;
    }
    .btn-wa:hover { background: rgba(37,211,102,.22) }

    /* ── HERO ── */
    .hero {
      padding: 72px 0 56px;
      position: relative; z-index: 1;
    }
    .hero-grid {
      display: grid;
      grid-template-columns: 1fr 420px;
      gap: 32px;
      align-items: start;
    }
    .eyebrow {
      display: inline-flex; align-items: center; gap: 8px;
      font-size: .8rem;
      font-weight: 600;
      letter-spacing: 1.5px;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 20px;
      padding: 7px 14px;
      border: 1px solid rgba(232,160,32,.2);
      border-radius: 999px;
      background: rgba(232,160,32,.07);
    }
    h1 {
      font-family: 'Playfair Display', serif;
      font-size: clamp(38px, 5vw, 64px);
      line-height: 1.04;
      letter-spacing: -1px;
      font-weight: 900;
      margin-bottom: 20px;
    }
    h1 em { font-style: normal; color: var(--gold2) }
    h1 strong { font-style: normal; color: var(--green2) }
    .hero-sub {
      font-size: 1.08rem;
      color: var(--muted);
      max-width: 52ch;
      margin-bottom: 32px;
      line-height: 1.7;
    }
    .hero-actions {
      display: flex; gap: 10px; flex-wrap: wrap;
    }
    .hero-note {
      margin-top: 28px;
      padding: 14px 16px;
      border: 1px solid var(--border);
      border-left: 3px solid var(--gold);
      border-radius: 0 12px 12px 0;
      background: rgba(232,160,32,.05);
      font-size: .875rem;
      color: var(--muted);
    }
    .hero-note b { color: var(--text) }

    /* Side card */
    .info-stack { display: flex; flex-direction: column; gap: 16px }
    .info-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 22px;
    }
    .info-card h3 {
      font-family: 'Playfair Display', serif;
      font-size: 1.05rem;
      font-weight: 700;
      margin-bottom: 12px;
      color: var(--gold2);
    }
    .info-card p, .info-card li {
      font-size: .9rem;
      color: var(--muted);
      line-height: 1.7;
    }
    .info-card ul { padding-left: 18px }
    .hours-grid { display: grid; grid-template-columns: auto 1fr; gap: 4px 12px; font-size: .9rem }
    .hours-grid dt { color: var(--muted); font-weight: 600 }
    .hours-grid dd { color: var(--text) }

    /* Feature badges */
    .badge-row {
      display: flex; flex-wrap: wrap; gap: 8px;
      margin: 28px 0 0;
    }
    .badge {
      display: inline-flex; align-items: center; gap: 6px;
      font-size: .82rem;
      padding: 7px 12px;
      border-radius: 999px;
      border: 1px solid var(--border2);
      background: rgba(255,220,140,.05);
      color: var(--muted);
      letter-spacing: .2px;
    }
    .badge-dot {
      width: 6px; height: 6px;
      border-radius: 50%;
      background: var(--gold);
      flex-shrink: 0;
    }
    .badge-dot.green { background: var(--green2) }

    /* ── DIVIDER ── */
    .divider {
      height: 1px;
      background: linear-gradient(90deg, transparent, var(--border2) 40%, var(--border2) 60%, transparent);
      margin: 8px 0;
      position: relative; z-index: 1;
    }

    /* ── SECTIONS ── */
    section { padding: 64px 0; position: relative; z-index: 1 }
    .section-label {
      display: inline-flex; align-items: center; gap: 8px;
      font-size: .75rem;
      font-weight: 600;
      letter-spacing: 1.8px;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 12px;
    }
    .section-label::before {
      content: '';
      display: inline-block;
      width: 20px; height: 2px;
      background: var(--gold);
      border-radius: 2px;
    }
    h2 {
      font-family: 'Playfair Display', serif;
      font-size: clamp(26px, 3.2vw, 38px);
      font-weight: 700;
      letter-spacing: -.5px;
      margin-bottom: 8px;
    }
    .lead {
      color: var(--muted);
      font-size: 1rem;
      margin-bottom: 40px;
      max-width: 52ch;
    }

    /* ── PRODUCTS ── */
    .product-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    .product-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 28px;
      display: flex;
      flex-direction: column;
      gap: 18px;
      transition: border-color .2s;
    }
    .product-card:hover { border-color: var(--border2) }
    .product-header {
      display: flex; justify-content: space-between; align-items: flex-start; gap: 12px;
    }
    .product-title {
      font-family: 'Playfair Display', serif;
      font-size: 1.5rem;
      font-weight: 700;
    }
    .product-title.camb { color: var(--gold2) }
    .product-title.eucal { color: var(--green2) }
    .product-chip {
      font-size: .75rem;
      padding: 5px 10px;
      border-radius: 999px;
      border: 1px solid var(--border2);
      color: var(--muted);
      background: rgba(255,220,140,.04);
      white-space: nowrap;
    }
    .product-desc {
      font-size: .92rem;
      color: var(--muted);
      line-height: 1.65;
    }
    .tag-row { display: flex; flex-wrap: wrap; gap: 7px }
    .tag {
      font-size: .8rem;
      padding: 5px 10px;
      border-radius: 8px;
      border: 1px solid var(--border);
      color: var(--faint);
      background: rgba(255,255,255,.03);
    }
    .product-footer {
      border-top: 1px dashed var(--border);
      padding-top: 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
    }
    .product-footer-text strong { font-size: 1rem; color: var(--text) }
    .product-footer-text small { display: block; font-size: .8rem; color: var(--faint); margin-top: 2px }

    /* ── SERVICES ── */
    .service-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
    }
    .service-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 24px;
    }
    .service-icon {
      width: 40px; height: 40px;
      border-radius: 12px;
      background: rgba(80,180,80,.10);
      border: 1px solid rgba(80,180,80,.16);
      display: grid; place-items: center;
      margin-bottom: 16px;
    }
    .service-card h3 {
      font-family: 'Playfair Display', serif;
      font-size: 1.05rem;
      font-weight: 700;
      margin-bottom: 8px;
    }
    .service-card p { font-size: .88rem; color: var(--muted); line-height: 1.65 }

    /* ── FORM ── */
    .quote-grid {
      display: grid;
      grid-template-columns: 1fr 360px;
      gap: 20px;
      align-items: start;
    }
    .form-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 28px;
    }
    .form-card h3 {
      font-family: 'Playfair Display', serif;
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 20px;
      color: var(--gold2);
    }
    form { display: grid; gap: 12px }
    .row2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px }
    label {
      display: block;
      font-size: .8rem;
      font-weight: 600;
      letter-spacing: .4px;
      text-transform: uppercase;
      color: var(--muted);
      margin-bottom: 6px;
    }
    input, select, textarea {
      width: 100%;
      padding: 12px 14px;
      border-radius: 12px;
      border: 1px solid var(--border2);
      background: rgba(0,0,0,.2);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-size: .92rem;
      outline: none;
      transition: border-color .2s, box-shadow .2s;
    }
    input:focus, select:focus, textarea:focus {
      border-color: rgba(232,160,32,.45);
      box-shadow: 0 0 0 4px rgba(232,160,32,.1);
    }
    select option { background: #2c1f0e }
    textarea { min-height: 110px; resize: vertical }
    .fine { font-size: .78rem; color: var(--faint); margin-top: 4px }

    .side-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 24px;
    }
    .side-card h3 {
      font-family: 'Playfair Display', serif;
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 8px;
      color: var(--gold2);
    }
    .side-card p { font-size: .88rem; color: var(--muted); margin-bottom: 16px }
    .wa-buttons { display: flex; flex-direction: column; gap: 8px }
    .contact-info {
      margin-top: 20px;
      padding-top: 16px;
      border-top: 1px solid var(--border);
    }
    .contact-row {
      display: flex; gap: 10px; align-items: flex-start;
      font-size: .88rem; margin-bottom: 10px;
    }
    .contact-row span:first-child {
      color: var(--gold);
      font-weight: 600;
      min-width: 60px;
    }
    .contact-row span:last-child { color: var(--muted) }

    /* ── MAP ── */
    .map-grid {
      display: grid;
      grid-template-columns: 280px 1fr;
      gap: 20px;
      align-items: start;
    }
    .map-info {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 22px;
    }
    .map-info h3 {
      font-family: 'Playfair Display', serif;
      font-size: 1.05rem;
      font-weight: 700;
      color: var(--gold2);
      margin-bottom: 12px;
    }
    .map-info p { font-size: .88rem; color: var(--muted); line-height: 1.8; margin-bottom: 14px }
    .map-embed {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
    }
    .map-embed iframe { display: block }

    /* ── FOOTER ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 28px 0 40px;
      position: relative; z-index: 1;
    }
    .footer-inner {
      display: flex; justify-content: space-between; align-items: center;
      gap: 12px; flex-wrap: wrap;
    }
    .footer-brand {
      font-family: 'Playfair Display', serif;
      font-weight: 700;
      font-size: 1.05rem;
    }
    .footer-brand span { color: var(--gold2) }
    .footer-right { font-size: .82rem; color: var(--faint) }

    /* ── TOAST ── */
    #toast {
      position: fixed; left: 50%; bottom: 22px;
      transform: translateX(-50%) translateY(20px);
      background: var(--surface2);
      border: 1px solid var(--border2);
      padding: 12px 18px;
      border-radius: 14px;
      display: none;
      gap: 10px; align-items: center;
      z-index: 200;
      font-size: .9rem;
      box-shadow: 0 12px 40px rgba(0,0,0,.5);
      animation: slideUp .25s ease forwards;
    }
    #toast.show { display: flex }
    @keyframes slideUp {
      from { transform: translateX(-50%) translateY(20px); opacity: 0 }
      to   { transform: translateX(-50%) translateY(0);  opacity: 1 }
    }
    .toast-dot {
      width: 8px; height: 8px; border-radius: 50%;
      background: #25d366; flex-shrink: 0;
    }

    /* ── RESPONSIVE ── */
    /* ── HAMBURGER ── */
    .hamburger {
      display: none;
      flex-direction: column;
      gap: 5px;
      cursor: pointer;
      padding: 8px;
      border-radius: 10px;
      border: 1px solid var(--border2);
      background: rgba(100,180,100,.06);
    }
    .hamburger span {
      display: block;
      width: 22px; height: 2px;
      background: var(--text);
      border-radius: 2px;
      transition: all .25s ease;
    }
    .hamburger.open span:nth-child(1) { transform: translateY(7px) rotate(45deg) }
    .hamburger.open span:nth-child(2) { opacity: 0 }
    .hamburger.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg) }

    .mobile-menu {
      display: none;
      flex-direction: column;
      gap: 4px;
      padding: 12px 0 16px;
      border-top: 1px solid var(--border);
    }
    .mobile-menu.open { display: flex }
    .mobile-menu a {
      font-size: 1rem;
      color: var(--muted);
      padding: 12px 16px;
      border-radius: 12px;
      transition: background .15s, color .15s;
    }
    .mobile-menu a:hover { background: rgba(100,180,100,.07); color: var(--text) }
    .mobile-menu .btn {
      margin-top: 8px;
      justify-content: center;
      font-size: .95rem;
      padding: 14px 20px;
    }

    @media (max-width: 860px) {
      .hero-grid, .product-grid, .service-grid,
      .quote-grid, .map-grid { grid-template-columns: 1fr }
      .row2 { grid-template-columns: 1fr }
      .navlinks { display: none }
      .nav-cta { display: none }
      .hamburger { display: flex }
      .hero { padding: 40px 0 28px }
      h1 { font-size: clamp(30px, 8vw, 44px) }
      .hero-sub { font-size: 1rem }
      .hero-actions { gap: 8px }
      .hero-actions .btn { flex: 1; justify-content: center; padding: 13px 10px; font-size: .88rem }
      .badge-row { gap: 6px }
      .badge { font-size: .78rem; padding: 6px 10px }
      section { padding: 44px 0 }
      h2 { font-size: clamp(22px, 6vw, 32px) }
      .product-card { padding: 20px }
      .product-footer { flex-direction: column; align-items: stretch }
      .product-footer .btn { justify-content: center; padding: 13px }
      .form-card { padding: 20px }
      .service-grid { grid-template-columns: 1fr }
      .map-grid { grid-template-columns: 1fr }
      .info-stack { display: grid; grid-template-columns: 1fr 1fr; gap: 12px }
      .footer-inner { flex-direction: column; text-align: center; gap: 6px }
      .wrap { padding: 0 16px }
    }
    @media (max-width: 480px) {
      .info-stack { grid-template-columns: 1fr }
      .hero-actions { flex-direction: column }
      .hero-actions .btn { width: 100% }
      .product-chip { display: none }
    }
  </style>
</head>
<body>

  <!-- NAV -->
  <nav>
    <div class="wrap">
      <div class="nav-inner">
        <a class="brand" href="#topo">
          <div class="logo-mark"><img src="data:image/png;base64,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" alt="Madeireira Rossi"/></div>
          <div>
            <div class="brand-name">Madeireira Rossi</div>
            <div class="brand-sub">Cambará &amp; Eucalipto</div>
          </div>
        </a>
        <nav class="navlinks">
          <a href="#produtos">Produtos</a>
          <a href="#servicos">Serviços</a>
          <a href="#localizacao">Localização</a>
        </nav>
        <div class="nav-cta">
          <a class="btn btn-wa" id="btnWhatsTop" href="https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento" target="_blank" rel="noopener noreferrer">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"/><path d="M12 0C5.373 0 0 5.373 0 12c0 2.126.553 4.122 1.52 5.857L0 24l6.335-1.51C8.063 23.447 10.003 24 12 24c6.627 0 12-5.373 12-12S18.627 0 12 0zm0 22c-1.881 0-3.651-.504-5.177-1.383l-.371-.22-3.763.897.914-3.661-.242-.381C2.504 15.658 2 13.88 2 12 2 6.477 6.477 2 12 2s10 4.477 10 10-4.477 10-10 10z"/></svg>
            WhatsApp
          </a>
        </div>
        <button class="hamburger" id="hamburger" aria-label="Menu">
          <span></span><span></span><span></span>
        </button>
      </div>
      <div class="mobile-menu" id="mobileMenu">
        <a href="#produtos" class="mobile-link">Produtos</a>
        <a href="#servicos" class="mobile-link">Serviços</a>
        <a href="#localizacao" class="mobile-link">Localização</a>
        <a class="btn btn-wa" id="btnWhatsMobile" href="https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento" target="_blank" rel="noopener noreferrer">WhatsApp</a>
      </div>
    </div>
  </nav>

  <main id="topo">
    <!-- HERO -->
    <section class="hero">
      <div class="wrap">
        <div class="hero-grid">
          <div>
            <div class="eyebrow">
              <span style="width:6px;height:6px;border-radius:50%;background:var(--gold);flex-shrink:0"></span>
              Joinville &amp; Região — SC
            </div>
            <h1>
              Madeira de qualidade,<br>
              entregue na sua <em>obra.</em>
            </h1>
            <p class="hero-sub">
              Cambará e Eucalipto em tábuas, vigas, caibros, ripas e pranchas.
              Atendimento direto pelo WhatsApp, orçamento rápido.
            </p>
            <div class="hero-actions">
              <a class="btn btn-wa" id="btnWhatsHero" href="https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento" target="_blank" rel="noopener noreferrer">
                <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347z"/><path d="M12 0C5.373 0 0 5.373 0 12c0 2.126.553 4.122 1.52 5.857L0 24l6.335-1.51C8.063 23.447 10.003 24 12 24c6.627 0 12-5.373 12-12S18.627 0 12 0zm0 22c-1.881 0-3.651-.504-5.177-1.383l-.371-.22-3.763.897.914-3.661-.242-.381C2.504 15.658 2 13.88 2 12 2 6.477 6.477 2 12 2s10 4.477 10 10-4.477 10-10 10z"/></svg>
                Pedir no WhatsApp
              </a>
              <a class="btn" href="#produtos">Ver produtos</a>
            </div>

            <div class="hero-note">
              <b>Dica para orçamento rápido:</b> informe espessura × largura × comprimento e quantidade de cada peça.
            </div>

            <div class="badge-row">
              <span class="badge"><span class="badge-dot green"></span>Entrega em Joinville e região</span>
              <span class="badge"><span class="badge-dot"></span>Corte sob medida (consultar)</span>
              <span class="badge"><span class="badge-dot"></span>Nota fiscal (opcional)</span>
              <span class="badge"><span class="badge-dot green"></span>Pronta entrega Eucalipto</span>
            </div>
          </div>

          <div class="info-stack">
            <div class="info-card">
              <h3>Horário de atendimento</h3>
              <dl class="hours-grid">
                <dt>Seg – Sex</dt><dd>08:00 – 18:00</dd>
                <dt>Sábado</dt><dd>Fechado</dd>
                <dt>Domingo</dt><dd>Fechado</dd>
              </dl>
            </div>
            <div class="info-card">
              <h3>Quem atendemos</h3>
              <ul>
                <li>Construtoras e obras</li>
                <li>Marceneiros e serralherias</li>
                <li>Clientes finais (pequenas compras)</li>
                <li>Reformas residenciais</li>
              </ul>
            </div>
            <div class="info-card" style="background: linear-gradient(135deg, rgba(90,138,60,.12), rgba(232,160,32,.08)); border-color: rgba(90,138,60,.22)">
              <h3 style="color:var(--green2)">Cambará — Destaque</h3>
              <p style="font-size:.88rem; color:var(--muted); margin:0">Boa resistência e acabamento. Indicada para estruturas, caibros e aplicações externas conforme tratamento.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div class="divider"></div>

    <!-- PRODUTOS -->
    <section id="produtos">
      <div class="wrap">
        <div class="section-label">Produtos</div>
        <h2>Cambará e Eucalipto</h2>
        <p class="lead">Informe tipo, medidas e quantidade para receber um orçamento por WhatsApp.</p>

        <div class="product-grid">
          <!-- Cambará -->
          <div class="product-card">
            <div class="product-header">
              <div class="product-title camb">Cambará</div>
              <span class="product-chip">Sob consulta</span>
            </div>
            <p class="product-desc">
              Estruturas, caibros, vigas e aplicações que pedem resistência e estabilidade dimensional. 
              Boa trabalhabilidade e durabilidade.
            </p>
            <div class="tag-row">
              <span class="tag">Tábuas</span>
              <span class="tag">Vigas</span>
              <span class="tag">Caibros</span>
              <span class="tag">Ripas</span>
              <span class="tag">Pranchas</span>
            </div>
            <div class="product-footer">
              <div class="product-footer-text">
                <strong>Orçamento sob medida</strong>
                <small>Varia por bitola, comprimento e estoque</small>
              </div>
              <a class="btn btn-wa" href="https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento" target="_blank" rel="noopener noreferrer" data-whats="cambara">Orçar Cambará</a>
            </div>
          </div>

          <!-- Eucalipto -->
          <div class="product-card">
            <div class="product-header">
              <div class="product-title eucal">Eucalipto</div>
              <span class="product-chip" style="border-color:rgba(90,138,60,.3);color:var(--green2)">Pronta entrega ✓</span>
            </div>
            <p class="product-desc">
              Custo-benefício e alta disponibilidade em várias bitolas.
              Versátil para construção civil, cercas, estruturas e usos gerais.
            </p>
            <div class="tag-row">
              <span class="tag">Tábuas</span>
              <span class="tag">Vigas</span>
              <span class="tag">Caibros</span>
              <span class="tag">Ripas</span>
              <span class="tag">Pranchas</span>
            </div>
            <div class="product-footer">
              <div class="product-footer-text">
                <strong>Orçamento sob medida</strong>
                <small>Informe medidas e quantidade</small>
              </div>
              <a class="btn btn-wa" href="https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento" target="_blank" rel="noopener noreferrer" data-whats="eucalipto">Orçar Eucalipto</a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div class="divider"></div>

    <!-- SERVIÇOS -->
    <section id="servicos">
      <div class="wrap">
        <div class="section-label">Serviços</div>
        <h2>Do pedido à entrega</h2>
        <p class="lead">Facilitamos cada etapa para que sua obra não pare.</p>

        <div class="service-grid">

          <div class="service-card">
            <div class="service-icon">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="var(--gold)" stroke-width="1.8">
                <rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/>
              </svg>
            </div>
            <h3>Separação e conferência</h3>
            <p>Separamos por bitola e quantidade. Reduzimos erro e retrabalho: você recebe exatamente o que pediu.</p>
          </div>
          <div class="service-card">
            <div class="service-icon">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="var(--gold)" stroke-width="1.8">
                <path d="M5 17H3a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11a2 2 0 0 1 2 2v3"/><rect x="9" y="11" width="14" height="10" rx="1"/><path d="M13 16h4"/>
              </svg>
            </div>
            <h3>Entrega programada</h3>
            <p>Agende por região. Ideal para cronogramas de obra e compras recorrentes com fretes planejados.</p>
          </div>
        </div>
      </div>
    </section>


    <div class="divider"></div>

    <!-- LOCALIZAÇÃO -->
    <section id="localizacao">
      <div class="wrap">
        <div class="section-label">Localização</div>
        <h2>Onde estamos</h2>
        <p class="lead">Venha retirar no pátio ou solicite entrega para Joinville e região.</p>

        <div class="map-grid">
          <div class="map-info">
            <h3>Endereço</h3>
            <p>
              R. Antônio João de Borba, 222<br>
              Paranaguamirim<br>
              Joinville – SC<br>
              CEP 89234-015
            </p>
            <a class="btn btn-gold" href="https://www.google.com/maps/search/?api=1&query=R.+Ant%C3%B4nio+Jo%C3%A3o+de+Borba+222+Paranaguamirim+Joinville+SC" id="btnMaps" target="_blank" rel="noopener noreferrer" style="width:100%; justify-content:center">
              Abrir no Google Maps
            </a>
            <div class="contact-info" style="border-top:1px solid var(--border); padding-top:14px; margin-top:16px">
              <div class="contact-row"><span>Seg–Sex</span><span>08:00 – 18:00</span></div>
              <div class="contact-row" style="margin-bottom:0"><span>Sábado</span><span>Fechado</span></div>
            </div>
          </div>
          <div class="map-embed">
            <iframe
              title="Mapa - Joinville SC"
              width="100%"
              height="320"
              loading="lazy"
              referrerpolicy="no-referrer-when-downgrade"
              src="https://www.google.com/maps?q=R.+Ant%C3%B4nio+Jo%C3%A3o+de+Borba+222+Paranaguamirim+Joinville+SC&output=embed"
              style="display:block;border:0">
            </iframe>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="wrap">
      <div class="footer-inner">
        <div class="footer-brand">Madeireira <span>Rossi</span> — Cambará &amp; Eucalipto</div>
        <div class="footer-right">© <span id="year"></span> · Joinville, SC</div>
      </div>
    </div>
  </footer>

  <!-- TOAST -->
  <div id="toast">
    <div class="toast-dot"></div>
    <span>Abrindo WhatsApp com a mensagem…</span>
  </div>

    <script>
    const WA_LINK = "https://api.whatsapp.com/send?phone=5547996440039&text=Ol%C3%A1,%20vim%20das%20redes%20sociais%20e%20gostaria%20de%20fazer%20um%20or%C3%A7amento";
    const MAPS_URL = "https://www.google.com/maps/search/?api=1&query=R.+Ant%C3%B4nio+Jo%C3%A3o+de+Borba+222+Paranaguamirim+Joinville+SC";

    function openWhatsApp() {
      window.open(WA_LINK, "_blank", "noopener,noreferrer");
    }

    function showToast() {
      const t = document.getElementById("toast");
      t.classList.add("show");
      clearTimeout(window.__tt);
      window.__tt = setTimeout(() => t.classList.remove("show"), 2800);
    }

    document.getElementById("year").textContent = new Date().getFullYear();
    document.getElementById("btnMaps").href = MAPS_URL;

    ["btnWhatsTop","btnWhatsHero"].forEach(id => {
      document.getElementById(id).addEventListener("click", e => {
        e.preventDefault(); showToast(); openWhatsApp();
      });
    });

    document.querySelectorAll("[data-whats]").forEach(el => {
      el.addEventListener("click", e => {
        e.preventDefault(); showToast(); openWhatsApp();
      });
    });



    // Hamburger menu
    const hamburger = document.getElementById("hamburger");
    const mobileMenu = document.getElementById("mobileMenu");
    hamburger.addEventListener("click", () => {
      hamburger.classList.toggle("open");
      mobileMenu.classList.toggle("open");
    });
    document.querySelectorAll(".mobile-link").forEach(link => {
      link.addEventListener("click", () => {
        hamburger.classList.remove("open
