# reatovlogs-
<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
  <meta name="theme-color" content="#070b12">
  <meta name="description" content="reatovlogs — интерактивный каталог OSINT-инструментов и открытых источников.">
  <title>reatovlogs / OSINT Catalog</title>
  <style>
    :root {
      --bg: #070b12;
      --bg-deep: #05070b;
      --panel: rgba(11, 17, 28, .82);
      --panel-solid: #0c1421;
      --panel-soft: rgba(18, 28, 44, .72);
      --line: rgba(143, 171, 207, .16);
      --line-strong: rgba(143, 171, 207, .30);
      --text: #edf4ff;
      --muted: #8e9db2;
      --muted-2: #65758c;
      --accent: #65e7be;
      --accent-2: #7aa9ff;
      --warning: #f4c66b;
      --danger: #ff879b;
      --shadow: 0 22px 80px rgba(0, 0, 0, .38);
      --radius: 16px;
      --mono: "SFMono-Regular", Consolas, "Liberation Mono", monospace;
      --sans: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    * { box-sizing: border-box; }
    html, body { min-height: 100%; }
    body {
      margin: 0;
      color: var(--text);
      background:
        radial-gradient(circle at 78% 11%, rgba(72, 113, 186, .14), transparent 27rem),
        radial-gradient(circle at 13% 92%, rgba(47, 167, 138, .10), transparent 25rem),
        var(--bg);
      font-family: var(--sans);
      overflow-x: hidden;
    }

    button, input { font: inherit; }
    button { color: inherit; }
    a { color: inherit; text-decoration: none; }
    ::selection { color: #07120f; background: var(--accent); }

    .app-shell { min-height: 100vh; position: relative; }
    .noise {
      position: fixed; inset: 0; pointer-events: none; opacity: .025; z-index: 20;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 180 180' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.7'/%3E%3C/svg%3E");
      mix-blend-mode: screen;
    }

    .topbar {
      height: 70px; display: flex; align-items: center; justify-content: space-between;
      padding: 0 28px; border-bottom: 1px solid var(--line);
      background: rgba(6, 10, 17, .76); backdrop-filter: blur(18px);
      position: sticky; top: 0; z-index: 15;
    }
    .brand { display: flex; align-items: center; gap: 12px; min-width: 240px; }
    .brand-mark {
      width: 34px; height: 34px; border: 1px solid rgba(101, 231, 190, .7); border-radius: 10px;
      display: grid; place-items: center; color: var(--accent); background: rgba(101, 231, 190, .08);
      box-shadow: inset 0 0 20px rgba(101, 231, 190, .08), 0 0 24px rgba(101, 231, 190, .09);
    }
    .brand-mark svg { width: 19px; height: 19px; }
    .brand-name { font-weight: 800; letter-spacing: -.03em; font-size: 15px; }
    .brand-sub { color: var(--muted-2); font: 10px var(--mono); letter-spacing: .12em; text-transform: uppercase; margin-top: 3px; }
    .topnav { display: flex; gap: 5px; }
    .nav-link, .icon-btn, .ghost-btn, .primary-btn {
      border: 1px solid transparent; border-radius: 10px; background: transparent; cursor: pointer;
      transition: .2s ease; 
    }
    .nav-link { color: var(--muted); padding: 9px 12px; font-size: 13px; }
    .nav-link:hover, .nav-link.active { color: var(--text); background: rgba(255,255,255,.05); border-color: var(--line); }
    .nav-link.active { color: var(--accent); }
    .top-actions { display: flex; align-items: center; justify-content: flex-end; gap: 10px; min-width: 240px; }
    .status-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 12px var(--accent); }
    .status-text { color: var(--muted); font: 11px var(--mono); }
    .language { color: var(--muted); padding: 7px 9px; font-size: 11px; border-color: var(--line); }
    .language:hover { color: var(--text); border-color: var(--line-strong); }

    .page { width: min(1520px, calc(100% - 42px)); margin: 0 auto; padding: 42px 0 68px; }
    .hero { display: flex; align-items: flex-end; justify-content: space-between; gap: 32px; margin-bottom: 29px; }
    .eyebrow { display: flex; align-items: center; gap: 9px; color: var(--accent); font: 11px var(--mono); letter-spacing: .1em; text-transform: uppercase; margin-bottom: 15px; }
    .eyebrow-line { width: 28px; height: 1px; background: var(--accent); box-shadow: 0 0 9px var(--accent); }
    h1 { margin: 0; font-size: clamp(34px, 5.3vw, 68px); line-height: .98; letter-spacing: -.065em; font-weight: 800; }
    .hero-copy { max-width: 650px; }
    .hero-description { max-width: 580px; color: var(--muted); line-height: 1.65; margin: 20px 0 0; font-size: 14px; }
    .hero-meta { display: flex; gap: 24px; margin-top: 25px; }
    .metric b { display: block; font-size: 21px; letter-spacing: -.03em; }
    .metric span { color: var(--muted-2); font: 10px var(--mono); text-transform: uppercase; letter-spacing: .08em; }
    .hero-aside { width: 280px; padding-bottom: 4px; }
    .quote-card { border-left: 1px solid var(--accent); padding: 8px 0 8px 17px; color: var(--muted); font-size: 12px; line-height: 1.6; }
    .quote-card strong { display: block; color: var(--text); font-size: 13px; margin-bottom: 7px; }

    .workspace { display: grid; grid-template-columns: 236px minmax(0, 1fr) 280px; gap: 13px; min-height: 730px; }
    .panel { border: 1px solid var(--line); background: var(--panel); box-shadow: var(--shadow); border-radius: var(--radius); backdrop-filter: blur(14px); }
    .sidebar { padding: 17px; }
    .panel-title { color: var(--muted-2); font: 10px var(--mono); letter-spacing: .13em; text-transform: uppercase; margin-bottom: 15px; }
    .search-wrap { position: relative; margin-bottom: 21px; }
    .search-wrap svg { position: absolute; left: 11px; top: 11px; width: 15px; color: var(--muted-2); }
    .search {
      width: 100%; color: var(--text); outline: 0; border: 1px solid var(--line); background: rgba(255,255,255,.035);
      border-radius: 9px; padding: 10px 10px 10px 34px; font-size: 12px; transition: .2s;
    }
    .search:focus { border-color: rgba(101,231,190,.65); box-shadow: 0 0 0 3px rgba(101,231,190,.08); }
    .category-list { display: grid; gap: 5px; }
    .category {
      display: flex; align-items: center; justify-content: space-between; gap: 8px; width: 100%;
      text-align: left; padding: 9px 10px; border: 1px solid transparent; border-radius: 9px;
      background: transparent; color: var(--muted); cursor: pointer; font-size: 12px; transition: .18s;
    }
    .category:hover { color: var(--text); background: rgba(255,255,255,.04); }
    .category.selected { color: var(--accent); background: rgba(101,231,190,.08); border-color: rgba(101,231,190,.16); }
    .category-left { display: flex; align-items: center; gap: 9px; }
    .category-icon { width: 7px; height: 7px; border-radius: 2px; background: var(--category, var(--accent)); box-shadow: 0 0 9px var(--category, var(--accent)); }
    .category-count { font: 10px var(--mono); color: var(--muted-2); }
    .sidebar-note { border-top: 1px solid var(--line); margin-top: 22px; padding-top: 15px; color: var(--muted-2); font-size: 11px; line-height: 1.6; }
    .sidebar-note code { color: var(--accent-2); font: 10px var(--mono); }

    .graph-panel { position: relative; overflow: hidden; min-height: 730px; background: #09111d; }
    .graph-panel:before {
      content: ""; position: absolute; inset: 0; opacity: .45; pointer-events: none;
      background-image: linear-gradient(rgba(120, 157, 202, .065) 1px, transparent 1px), linear-gradient(90deg, rgba(120,157,202,.065) 1px, transparent 1px);
      background-size: 40px 40px; mask-image: linear-gradient(to bottom, black, transparent 90%);
    }
    .graph-toolbar {
      position: absolute; left: 17px; right: 17px; top: 17px; z-index: 3; display: flex; justify-content: space-between; gap: 12px; pointer-events: none;
    }
    .graph-toolbar > * { pointer-events: auto; }
    .toolbar-chip { border: 1px solid var(--line); background: rgba(8,14,24,.78); border-radius: 8px; color: var(--muted); font: 10px var(--mono); padding: 8px 10px; backdrop-filter: blur(8px); }
    .toolbar-chip span { color: var(--accent); }
    .graph-tools { display: flex; gap: 5px; }
    .icon-btn { width: 30px; height: 30px; border-color: var(--line); background: rgba(8,14,24,.78); color: var(--muted); display: grid; place-items: center; }
    .icon-btn:hover { color: var(--text); border-color: var(--line-strong); background: rgba(21,33,51,.9); }
    .icon-btn svg { width: 14px; height: 14px; }
    #graph { position: absolute; inset: 0; width: 100%; height: 100%; min-height: 730px; cursor: grab; }
    #graph.dragging { cursor: grabbing; }
    .edge { stroke: rgba(132,166,212,.26); stroke-width: 1.2; fill: none; transition: .25s; }
    .edge.hot { stroke: rgba(101,231,190,.76); stroke-width: 1.8; }
    .node-group { cursor: pointer; }
    .node-ring { fill: #0a1623; stroke: var(--node-color, #7aa9ff); stroke-width: 1.3; transition: .2s; }
    .node-halo { fill: var(--node-color, #7aa9ff); opacity: .09; transition: .2s; }
    .node-group:hover .node-ring, .node-group.active .node-ring { stroke: #fff; stroke-width: 2; }
    .node-group:hover .node-halo, .node-group.active .node-halo { opacity: .22; }
    .node-label { fill: #c7d4e7; font-size: 11px; font-family: var(--sans); pointer-events: none; text-anchor: middle; }
    .node-label.root { fill: var(--accent); font-size: 15px; font-weight: 800; }
    .node-label.category-label { fill: #eef4ff; font-size: 11px; font-weight: 700; }
    .graph-legend { position: absolute; bottom: 17px; left: 17px; z-index: 3; display: flex; gap: 14px; flex-wrap: wrap; }
    .legend-item { color: var(--muted-2); font: 10px var(--mono); display: flex; align-items: center; gap: 6px; }
    .legend-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--dot); box-shadow: 0 0 8px var(--dot); }
    .empty-state { position: absolute; inset: 0; display: none; align-items: center; justify-content: center; text-align: center; color: var(--muted); pointer-events: none; }
    .empty-state.visible { display: flex; }
    .empty-state div { max-width: 220px; font-size: 13px; line-height: 1.6; }
    .empty-state b { display: block; color: var(--text); margin-bottom: 6px; }

    .detail { padding: 17px; }
    .detail-top { display: flex; align-items: flex-start; justify-content: space-between; gap: 10px; }
    .detail-tag { color: var(--accent); font: 10px var(--mono); text-transform: uppercase; letter-spacing: .11em; margin-bottom: 10px; }
    .detail h2 { margin: 0; font-size: 23px; letter-spacing: -.04em; }
    .close-detail { color: var(--muted-2); border: 0; background: none; cursor: pointer; font-size: 19px; line-height: 1; padding: 2px; }
    .close-detail:hover { color: var(--text); }
    .detail-description { font-size: 12px; color: var(--muted); line-height: 1.7; margin: 14px 0 17px; }
    .detail-link { display: flex; align-items: center; justify-content: space-between; gap: 10px; padding: 10px 11px; border: 1px solid var(--line); border-radius: 9px; background: rgba(255,255,255,.03); color: var(--accent-2); font: 10px var(--mono); }
    .detail-link:hover { border-color: rgba(122,169,255,.55); background: rgba(122,169,255,.08); }
    .detail-section { border-top: 1px solid var(--line); margin-top: 18px; padding-top: 16px; }
    .detail-section-title { font: 10px var(--mono); text-transform: uppercase; letter-spacing: .1em; color: var(--muted-2); margin-bottom: 10px; }
    .tag-list { display: flex; flex-wrap: wrap; gap: 5px; }
    .tag { font: 10px var(--mono); border: 1px solid var(--line); color: var(--muted); border-radius: 5px; padding: 4px 6px; }
    .tip { color: var(--muted); font-size: 11px; line-height: 1.55; }
    .tip strong { color: var(--warning); font-weight: 600; }
    .detail-placeholder { color: var(--muted); font-size: 12px; line-height: 1.7; padding-top: 7px; }
    .detail-placeholder .placeholder-icon { font-size: 28px; color: var(--accent); margin-bottom: 13px; }
    .detail-placeholder b { color: var(--text); display: block; margin-bottom: 7px; font-size: 14px; }
    .quick-actions { display: grid; gap: 7px; margin-top: 21px; }
    .primary-btn, .ghost-btn { width: 100%; padding: 10px 12px; font-size: 11px; cursor: pointer; }
    .primary-btn { color: #06100d; background: var(--accent); border-color: var(--accent); font-weight: 750; }
    .primary-btn:hover { filter: brightness(1.08); box-shadow: 0 0 22px rgba(101,231,190,.18); }
    .ghost-btn { color: var(--muted); border-color: var(--line); }
    .ghost-btn:hover { color: var(--text); border-color: var(--line-strong); background: rgba(255,255,255,.04); }

    .bottom-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 13px; margin-top: 13px; }
    .info-card { padding: 17px 18px; min-height: 150px; }
    .info-card h3 { margin: 0 0 9px; font-size: 14px; letter-spacing: -.02em; }
    .info-card p { color: var(--muted); font-size: 11px; line-height: 1.65; margin: 0; }
    .info-card .small-label { color: var(--accent); font: 10px var(--mono); letter-spacing: .1em; text-transform: uppercase; margin-bottom: 12px; }
    .shortcut { display: flex; align-items: center; justify-content: space-between; color: var(--muted); font-size: 11px; padding: 5px 0; border-bottom: 1px solid rgba(143,171,207,.08); }
    kbd { color: var(--muted-2); border: 1px solid var(--line); border-radius: 4px; padding: 2px 5px; font: 9px var(--mono); }
    footer { display: flex; justify-content: space-between; align-items: center; gap: 20px; color: var(--muted-2); font: 10px var(--mono); padding-top: 28px; }
    footer a { color: var(--accent-2); }

    .toast { position: fixed; left: 50%; bottom: 24px; transform: translate(-50%, 20px); padding: 10px 15px; background: #122238; border: 1px solid var(--line-strong); color: var(--text); border-radius: 9px; font-size: 12px; box-shadow: var(--shadow); opacity: 0; pointer-events: none; transition: .25s; z-index: 50; }
    .toast.show { opacity: 1; transform: translate(-50%, 0); }
    .modal-backdrop { position: fixed; inset: 0; display: none; place-items: center; padding: 18px; background: rgba(2,5,9,.72); backdrop-filter: blur(6px); z-index: 40; }
    .modal-backdrop.open { display: grid; }
    .modal { width: min(520px, 100%); border: 1px solid var(--line-strong); border-radius: 18px; background: var(--panel-solid); box-shadow: var(--shadow); padding: 22px; }
    .modal h2 { margin: 0 0 10px; font-size: 22px; }
    .modal p { color: var(--muted); font-size: 12px; line-height: 1.65; margin: 0 0 18px; }
    .modal-actions { display: flex; gap: 8px; justify-content: flex-end; }
    .modal-actions button { width: auto; }

    @media (max-width: 1120px) {
      .workspace { grid-template-columns: 215px minmax(0, 1fr); }
      .detail { grid-column: 1 / -1; min-height: auto; }
      .detail-placeholder { padding-top: 0; }
      .detail-top { align-items: center; }
      .detail-description { max-width: 700px; }
      .detail-section { display: inline-block; vertical-align: top; width: 48%; margin-right: 1%; }
      .quick-actions { display: flex; max-width: 420px; }
    }
    @media (max-width: 760px) {
      .topbar { height: auto; min-height: 65px; padding: 12px 16px; flex-wrap: wrap; gap: 10px; }
      .brand, .top-actions { min-width: auto; }
      .topnav { order: 3; width: 100%; overflow-x: auto; padding-bottom: 1px; }
      .nav-link { white-space: nowrap; }
      .page { width: min(100% - 24px, 620px); padding-top: 28px; }
      .hero { display: block; }
      .hero-aside { display: none; }
      .workspace { display: flex; flex-direction: column; min-height: 0; }
      .sidebar { order: 1; }
      .graph-panel { order: 2; height: 590px; min-height: 590px; }
      #graph { min-height: 590px; }
      .detail { order: 3; }
      .bottom-grid { grid-template-columns: 1fr; }
      .detail-section { display: block; width: auto; margin-right: 0; }
      .hero-meta { gap: 18px; }
      footer { align-items: flex-start; flex-direction: column; }
    }
  </style>
</head>
<body>
  <div class="noise"></div>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#" aria-label="reatovlogs, на главную">
        <span class="brand-mark" aria-hidden="true">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7">
            <circle cx="12" cy="12" r="2.5"/><circle cx="5" cy="7" r="1.7"/><circle cx="19" cy="6" r="1.7"/><circle cx="18" cy="18" r="1.7"/><circle cx="6" cy="18" r="1.7"/>
            <path d="m10 10-3.6-2M14 10l3.5-2.4M14 14l2.7 2.6M10 14l-2.7 2.5"/>
          </svg>
        </span>
        <span>
          <span class="brand-name">reatovlogs</span>
          <span class="brand-sub">open intelligence index</span>
        </span>
      </a>
      <nav class="topnav" aria-label="Основная навигация">
        <button class="nav-link active" data-scroll="#catalog">Каталог</button>
        <button class="nav-link" data-scroll="#about">О проекте</button>
        <button class="nav-link" id="guideButton">Гайд</button>
      </nav>
      <div class="top-actions">
        <span class="status-dot" aria-hidden="true"></span>
        <span class="status-text">INDEX ONLINE</span>
        <button class="icon-btn language" id="langButton" title="Переключить язык">RU / EN</button>
      </div>
    </header>

    <main class="page">
      <section class="hero" id="catalog">
        <div class="hero-copy">
          <div class="eyebrow"><span class="eyebrow-line"></span><span data-i18n="eyebrow">Карта открытых источников</span></div>
          <h1>OSINT<br><span style="color:var(--accent)">CATALOG.</span></h1>
          <p class="hero-description" data-i18n="description">Интерактивный индекс инструментов для поиска, проверки и анализа информации из открытых источников. Выберите узел, чтобы увидеть краткое описание и полезные метки.</p>
          <div class="hero-meta">
            <div class="metric"><b>48</b><span data-i18n="metricTools">инструментов</span></div>
            <div class="metric"><b>12</b><span data-i18n="metricCats">категорий</span></div>
            <div class="metric"><b>07.26</b><span data-i18n="metricUpdate">обновлено</span></div>
          </div>
        </div>
        <aside class="hero-aside">
          <div class="quote-card">
            <strong>01 / Начните с контекста</strong>
            Хорошее исследование начинается с вопроса, а не с инструмента. Фиксируйте источники и проверяйте выводы.
          </div>
        </aside>
      </section>

      <section class="workspace" aria-label="Интерактивный каталог">
        <aside class="panel sidebar">
          <div class="panel-title">/ filters</div>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="7"/><path d="m20 20-4-4"/></svg>
            <input class="search" id="search" type="search" placeholder="Поиск по графу..." autocomplete="off">
          </div>
          <div class="panel-title">Категории</div>
          <div class="category-list" id="categoryList"></div>
          <div class="sidebar-note">
            <code>TIP</code><br>
            Нажмите на узел графа, чтобы открыть карточку. Граф можно перемещать мышью или пальцем.
          </div>
        </aside>

        <section class="panel graph-panel" aria-label="Граф OSINT-инструментов">
          <div class="graph-toolbar">
            <div class="toolbar-chip">NODES <span id="visibleCount">48</span> / 48</div>
            <div class="graph-tools">
              <button class="icon-btn" id="zoomOut" title="Уменьшить">−</button>
              <button class="icon-btn" id="resetGraph" title="Сбросить вид">⌂</button>
         
