<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Банк медиаматериалов</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&family=Unbounded:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --teal: #1D9E75;
    --teal-light: #E1F5EE;
    --teal-mid: #5DCAA5;
    --amber: #EF9F27;
    --amber-light: #FAEEDA;
    --coral: #D85A30;
    --coral-light: #FAECE7;
    --purple: #534AB7;
    --purple-light: #EEEDFE;
    --blue: #378ADD;
    --blue-light: #E6F1FB;
    --bg: #FAFDF9;
    --sidebar-w: 300px;
    --header-h: 68px;
  }

  body {
    font-family: 'Nunito', sans-serif;
    background: var(--bg);
    color: #2C2C2A;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ===== OVERLAY ===== */
  #overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.35);
    z-index: 90;
    backdrop-filter: blur(2px);
  }
  #overlay.open { display: block; }

  /* ===== SIDEBAR ===== */
  #sidebar {
    position: fixed;
    top: 0; left: 0;
    width: var(--sidebar-w);
    height: 100vh;
    background: #fff;
    z-index: 100;
    transform: translateX(-100%);
    transition: transform 0.32s cubic-bezier(.4,0,.2,1);
    display: flex;
    flex-direction: column;
    border-right: 1.5px solid #E1F5EE;
    overflow: hidden;
  }
  #sidebar.open { transform: translateX(0); }

  .sidebar-header {
    padding: 22px 20px 16px;
    background: linear-gradient(135deg, #1D9E75 0%, #0F6E56 100%);
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-shrink: 0;
  }
  .sidebar-header h2 {
    font-family: 'Unbounded', sans-serif;
    font-size: 13px;
    color: #fff;
    letter-spacing: 0.5px;
    line-height: 1.4;
  }
  .sidebar-close {
    background: rgba(255,255,255,0.2);
    border: none;
    color: #fff;
    border-radius: 8px;
    width: 32px; height: 32px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer;
    font-size: 18px;
    transition: background 0.2s;
  }
  .sidebar-close:hover { background: rgba(255,255,255,0.35); }

  .sidebar-scroll {
    overflow-y: auto;
    flex: 1;
    padding: 12px 0 20px;
  }

  .sidebar-section-title {
    font-size: 10px;
    font-weight: 800;
    color: #888780;
    text-transform: uppercase;
    letter-spacing: 1.2px;
    padding: 16px 20px 6px;
  }

  .age-item {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 20px;
    cursor: pointer;
    border-radius: 0;
    transition: background 0.15s;
    font-size: 15px;
    font-weight: 600;
    color: #2C2C2A;
    position: relative;
  }
  .age-item:hover { background: var(--teal-light); }
  .age-item.active { background: var(--teal-light); color: var(--teal); }
  .age-item .age-dot {
    width: 32px; height: 32px;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 11px;
    font-weight: 800;
    flex-shrink: 0;
  }
  .age-item .chevron {
    margin-left: auto;
    font-size: 14px;
    color: #B4B2A9;
    transition: transform 0.2s;
  }
  .age-item.active .chevron { transform: rotate(90deg); color: var(--teal); }

  .subjects-panel {
    overflow: hidden;
    max-height: 0;
    transition: max-height 0.3s ease;
    background: #FAFDF9;
    border-top: 1px solid #E1F5EE;
    border-bottom: 1px solid #E1F5EE;
  }
  .subjects-panel.open { max-height: 300px; }

  .subject-link {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 9px 20px 9px 32px;
    font-size: 14px;
    font-weight: 600;
    color: #444441;
    cursor: pointer;
    transition: background 0.15s, color 0.15s;
  }
  .subject-link:hover { background: var(--teal-light); color: var(--teal); }
  .subject-link.active { color: var(--teal); }
  .subject-link i { font-size: 16px; }

  .sidebar-divider {
    height: 1px;
    background: #E1F5EE;
    margin: 8px 20px;
  }

  /* ===== HEADER ===== */
  header {
    position: sticky;
    top: 0;
    height: var(--header-h);
    background: rgba(255,255,255,0.95);
    backdrop-filter: blur(8px);
    border-bottom: 1.5px solid #E1F5EE;
    display: flex;
    align-items: center;
    padding: 0 28px;
    gap: 16px;
    z-index: 50;
  }

  .menu-btn {
    background: none;
    border: 1.5px solid #D3D1C7;
    border-radius: 10px;
    width: 40px; height: 40px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer;
    font-size: 20px;
    color: #5F5E5A;
    transition: border-color 0.2s, background 0.2s;
    flex-shrink: 0;
  }
  .menu-btn:hover { border-color: var(--teal); color: var(--teal); background: var(--teal-light); }

  .logo {
    font-family: 'Unbounded', sans-serif;
    font-size: 14px;
    font-weight: 700;
    color: var(--teal);
    line-height: 1.3;
    flex: 1;
  }
  .logo span { color: #2C2C2A; font-weight: 400; font-family: 'Nunito', sans-serif; font-size: 12px; display: block; }

  .header-search {
    display: flex;
    align-items: center;
    gap: 8px;
    background: #F1EFE8;
    border-radius: 10px;
    padding: 8px 14px;
    max-width: 240px;
    flex: 1;
    border: 1.5px solid transparent;
    transition: border-color 0.2s, background 0.2s;
  }
  .header-search:focus-within { border-color: var(--teal); background: #fff; }
  .header-search i { color: #888780; font-size: 16px; }
  .header-search input {
    background: none;
    border: none;
    outline: none;
    font-family: 'Nunito', sans-serif;
    font-size: 14px;
    color: #2C2C2A;
    width: 100%;
  }
  .header-search input::placeholder { color: #B4B2A9; }

  /* ===== MAIN ===== */
  main { padding: 0; }

  /* ===== HERO ===== */
  .hero {
    background: linear-gradient(160deg, #0F6E56 0%, #1D9E75 55%, #5DCAA5 100%);
    padding: 64px 40px 80px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 300px; height: 300px;
    border-radius: 50%;
    background: rgba(255,255,255,0.06);
  }
  .hero::after {
    content: '';
    position: absolute;
    bottom: -80px; left: -40px;
    width: 220px; height: 220px;
    border-radius: 50%;
    background: rgba(255,255,255,0.05);
  }
  .hero-emoji {
    font-size: 56px;
    margin-bottom: 20px;
    display: block;
    filter: drop-shadow(0 4px 12px rgba(0,0,0,0.15));
  }
  .hero h1 {
    font-family: 'Unbounded', sans-serif;
    font-size: clamp(22px, 4vw, 36px);
    font-weight: 700;
    color: #fff;
    line-height: 1.3;
    margin-bottom: 16px;
    position: relative;
  }
  .hero h1 em {
    font-style: normal;
    color: #9FE1CB;
  }
  .hero p {
    font-size: 16px;
    color: rgba(255,255,255,0.85);
    max-width: 560px;
    margin: 0 auto 40px;
    line-height: 1.7;
    position: relative;
  }

  .hero-stats {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
    position: relative;
  }
  .hero-stat {
    background: rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.25);
    border-radius: 12px;
    padding: 14px 22px;
    text-align: center;
    backdrop-filter: blur(4px);
  }
  .hero-stat strong {
    font-family: 'Unbounded', sans-serif;
    font-size: 22px;
    color: #fff;
    display: block;
  }
  .hero-stat span {
    font-size: 12px;
    color: rgba(255,255,255,0.75);
  }

  /* ===== AGE SECTION ===== */
  .section { padding: 52px 40px; }
  .section-title {
    font-family: 'Unbounded', sans-serif;
    font-size: 18px;
    font-weight: 700;
    color: #2C2C2A;
    margin-bottom: 8px;
  }
  .section-sub {
    font-size: 14px;
    color: #888780;
    margin-bottom: 32px;
  }

  .age-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
  }

  .age-card {
    background: #fff;
    border-radius: 20px;
    padding: 32px 24px;
    border: 2px solid transparent;
    cursor: pointer;
    transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .age-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 12px 32px rgba(0,0,0,0.10);
  }
  .age-card.active { border-color: var(--teal); }

  .age-card .card-emoji {
    font-size: 44px;
    display: block;
    margin-bottom: 16px;
  }
  .age-card .card-age {
    font-family: 'Unbounded', sans-serif;
    font-size: 15px;
    font-weight: 700;
    margin-bottom: 6px;
  }
  .age-card .card-label {
    font-size: 12px;
    color: #888780;
    margin-bottom: 14px;
  }
  .age-card .card-count {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    font-size: 12px;
    font-weight: 700;
    border-radius: 20px;
    padding: 4px 12px;
  }

  .age-card:nth-child(1) { background: #FFFBF0; }
  .age-card:nth-child(1) .card-age { color: #BA7517; }
  .age-card:nth-child(1) .card-count { background: var(--amber-light); color: #BA7517; }

  .age-card:nth-child(2) { background: #F0FDF8; }
  .age-card:nth-child(2) .card-age { color: var(--teal); }
  .age-card:nth-child(2) .card-count { background: var(--teal-light); color: var(--teal); }

  .age-card:nth-child(3) { background: #F0F4FF; }
  .age-card:nth-child(3) .card-age { color: var(--purple); }
  .age-card:nth-child(3) .card-count { background: var(--purple-light); color: var(--purple); }

  .age-card:nth-child(4) { background: #FFF1EE; }
  .age-card:nth-child(4) .card-age { color: var(--coral); }
  .age-card:nth-child(4) .card-count { background: var(--coral-light); color: var(--coral); }

  /* ===== MATERIALS SECTION ===== */
  .materials-section {
    padding: 0 40px 60px;
    display: none;
  }
  .materials-section.visible { display: block; }

  .materials-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 24px;
    flex-wrap: wrap;
    gap: 12px;
  }
  .materials-title {
    font-family: 'Unbounded', sans-serif;
    font-size: 16px;
    font-weight: 700;
    color: #2C2C2A;
  }
  .materials-badge {
    background: var(--teal-light);
    color: var(--teal);
    font-size: 12px;
    font-weight: 700;
    padding: 4px 12px;
    border-radius: 20px;
  }

  .subject-tabs {
    display: flex;
    gap: 8px;
    margin-bottom: 28px;
    flex-wrap: wrap;
  }
  .subject-tab {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 8px 18px;
    border-radius: 40px;
    border: 1.5px solid #D3D1C7;
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    background: #fff;
    color: #5F5E5A;
    transition: all 0.2s;
  }
  .subject-tab:hover { border-color: var(--teal); color: var(--teal); }
  .subject-tab.active { background: var(--teal); border-color: var(--teal); color: #fff; }

  .materials-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 18px;
  }

  .material-card {
    background: #fff;
    border-radius: 16px;
    border: 1.5px solid #E1F5EE;
    overflow: hidden;
    transition: transform 0.2s, box-shadow 0.2s;
    cursor: pointer;
  }
  .material-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.09);
  }

  .material-thumb {
    height: 110px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 36px;
    position: relative;
  }
  .material-type-badge {
    position: absolute;
    top: 10px; right: 10px;
    font-size: 10px;
    font-weight: 800;
    padding: 3px 8px;
    border-radius: 6px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  .material-body { padding: 16px; }
  .material-body h3 { font-size: 14px; font-weight: 700; color: #2C2C2A; margin-bottom: 6px; line-height: 1.4; }
  .material-body p { font-size: 12px; color: #888780; line-height: 1.5; margin-bottom: 12px; }
  .material-meta {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .material-level {
    font-size: 11px;
    font-weight: 700;
    padding: 3px 8px;
    border-radius: 6px;
  }
  .level-base { background: var(--teal-light); color: var(--teal); }
  .level-adv { background: var(--purple-light); color: var(--purple); }
  .material-duration { font-size: 11px; color: #B4B2A9; display: flex; align-items: center; gap: 4px; }

  /* type colors */
  .type-video { background: #FBEAF0; color: #993556; }
  .type-audio { background: var(--amber-light); color: #854F0B; }
  .type-img { background: var(--blue-light); color: #185FA5; }
  .type-inter { background: var(--teal-light); color: var(--teal); }
  .type-pres { background: var(--purple-light); color: var(--purple); }
  .type-doc { background: #F1EFE8; color: #5F5E5A; }

  .thumb-video { background: #FBEAF0; }
  .thumb-audio { background: var(--amber-light); }
  .thumb-img { background: var(--blue-light); }
  .thumb-inter { background: var(--teal-light); }
  .thumb-pres { background: var(--purple-light); }
  .thumb-doc { background: #F1EFE8; }

  /* ===== FEATURES ===== */
  .features-section {
    background: #fff;
    padding: 52px 40px;
    border-top: 1.5px solid #E1F5EE;
  }
  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 24px;
    margin-top: 32px;
  }
  .feature-item {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .feature-icon {
    width: 48px; height: 48px;
    border-radius: 14px;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px;
  }
  .feature-item h4 { font-size: 15px; font-weight: 700; color: #2C2C2A; }
  .feature-item p { font-size: 13px; color: #888780; line-height: 1.6; }

  /* ===== FOOTER ===== */
  footer {
    background: #0F6E56;
    padding: 32px 40px;
    text-align: center;
    color: rgba(255,255,255,0.7);
    font-size: 13px;
  }
  footer strong { color: #fff; }
</style>
</head>
<body>

<div id="overlay"></div>

<!-- SIDEBAR -->
<nav id="sidebar">
  <div class="sidebar-header">
    <h2>📚 Банк<br>медиаматериалов</h2>
    <button class="sidebar-close" onclick="closeSidebar()" aria-label="Закрыть меню"><i class="ti ti-x"></i></button>
  </div>
  <div class="sidebar-scroll">

    <div class="sidebar-section-title">Возрастные группы</div>

    <!-- 3-4 -->
    <div class="age-item" id="nav-3-4" onclick="toggleAge('3-4')">
      <div class="age-dot" style="background:#FAEEDA;color:#BA7517;">3–4</div>
      <span>3–4 года</span>
      <i class="ti ti-chevron-right chevron"></i>
    </div>
    <div class="subjects-panel" id="panel-3-4">
      <div class="subject-link" onclick="goTo('3-4','math')"><i class="ti ti-math-symbols" style="color:#BA7517;"></i>Математика 3–4</div>
      <div class="subject-link" onclick="goTo('3-4','russian')"><i class="ti ti-alphabet-cyrillic" style="color:#BA7517;"></i>Русский язык</div>
      <div class="subject-link" onclick="goTo('3-4','biology')"><i class="ti ti-leaf" style="color:#BA7517;"></i>Биология</div>
    </div>

    <!-- 4-5 -->
    <div class="age-item" id="nav-4-5" onclick="toggleAge('4-5')">
      <div class="age-dot" style="background:#E1F5EE;color:#0F6E56;">4–5</div>
      <span>4–5 лет</span>
      <i class="ti ti-chevron-right chevron"></i>
    </div>
    <div class="subjects-panel" id="panel-4-5">
      <div class="subject-link" onclick="goTo('4-5','math')"><i class="ti ti-math-symbols" style="color:#1D9E75;"></i>Математика 4–5</div>
      <div class="subject-link" onclick="goTo('4-5','russian')"><i class="ti ti-alphabet-cyrillic" style="color:#1D9E75;"></i>Русский язык</div>
      <div class="subject-link" onclick="goTo('4-5','biology')"><i class="ti ti-leaf" style="color:#1D9E75;"></i>Биология</div>
    </div>

    <!-- 5-6 -->
    <div class="age-item" id="nav-5-6" onclick="toggleAge('5-6')">
      <div class="age-dot" style="background:#EEEDFE;color:#534AB7;">5–6</div>
      <span>5–6 лет</span>
      <i class="ti ti-chevron-right chevron"></i>
    </div>
    <div class="subjects-panel" id="panel-5-6">
      <div class="subject-link" onclick="goTo('5-6','math')"><i class="ti ti-math-symbols" style="color:#534AB7;"></i>Математика 5–6</div>
      <div class="subject-link" onclick="goTo('5-6','russian')"><i class="ti ti-alphabet-cyrillic" style="color:#534AB7;"></i>Русский язык</div>
      <div class="subject-link" onclick="goTo('5-6','biology')"><i class="ti ti-leaf" style="color:#534AB7;"></i>Биология</div>
    </div>

    <!-- 6-7 -->
    <div class="age-item" id="nav-6-7" onclick="toggleAge('6-7')">
      <div class="age-dot" style="background:#FAECE7;color:#993C1D;">6–7</div>
      <span>6–7 лет</span>
      <i class="ti ti-chevron-right chevron"></i>
    </div>
    <div class="subjects-panel" id="panel-6-7">
      <div class="subject-link" onclick="goTo('6-7','math')"><i class="ti ti-math-symbols" style="color:#D85A30;"></i>Математика 6–7</div>
      <div class="subject-link" onclick="goTo('6-7','russian')"><i class="ti ti-alphabet-cyrillic" style="color:#D85A30;"></i>Русский язык</div>
      <div class="subject-link" onclick="goTo('6-7','biology')"><i class="ti ti-leaf" style="color:#D85A30;"></i>Биология</div>
    </div>

    <div class="sidebar-divider"></div>
    <div class="sidebar-section-title">Предметы</div>

    <div class="subject-link" onclick="goBySubject('math')"><i class="ti ti-math-symbols" style="color:#BA7517;"></i>Вся математика</div>
    <div class="subject-link" onclick="goBySubject('russian')"><i class="ti ti-alphabet-cyrillic" style="color:#378ADD;"></i>Весь русский язык</div>
    <div class="subject-link" onclick="goBySubject('biology')"><i class="ti ti-leaf" style="color:#1D9E75;"></i>Вся биология</div>

  </div>
</nav>

<!-- HEADER -->
<header>
  <button class="menu-btn" onclick="openSidebar()" aria-label="Открыть меню">
    <i class="ti ti-menu-2"></i>
  </button>
  <div class="logo">
    Банк медиаматериалов
    <span>для дошкольного образования</span>
  </div>
  <div class="header-search">
    <i class="ti ti-search"></i>
    <input type="text" placeholder="Найти материал...">
  </div>
</header>

<!-- MAIN -->
<main>

  <!-- HERO -->
  <section class="hero">
    <span class="hero-emoji">📖</span>
    <h1>Добро пожаловать в <em>«Банк медиаматериалов»</em></h1>
    <p>Здесь мы собрали лучшую коллекцию материалов для проведения занятий с детьми. Видео, аудио, игры, карточки — всё под рукой!</p>
    <div class="hero-stats">
      <div class="hero-stat"><strong>200+</strong><span>материалов</span></div>
      <div class="hero-stat"><strong>4</strong><span>возрастные группы</span></div>
      <div class="hero-stat"><strong>3</strong><span>предмета</span></div>
      <div class="hero-stat"><strong>6</strong><span>форматов</span></div>
    </div>
  </section>

  <!-- AGE CARDS -->
  <section class="section">
    <h2 class="section-title">Выберите возрастную группу</h2>
    <p class="section-sub">Нажмите на карточку, чтобы посмотреть подходящие материалы</p>
    <div class="age-cards">
      <div class="age-card" onclick="showMaterials('3-4', 'all')" id="card-3-4">
        <span class="card-emoji">🐣</span>
        <div class="card-age">3–4 года</div>
        <div class="card-label">Ранний дошкольный</div>
        <span class="card-count"><i class="ti ti-files"></i> 48 материалов</span>
      </div>
      <div class="age-card" onclick="showMaterials('4-5', 'all')" id="card-4-5">
        <span class="card-emoji">🌱</span>
        <div class="card-age">4–5 лет</div>
        <div class="card-label">Средний дошкольный</div>
        <span class="card-count"><i class="ti ti-files"></i> 62 материала</span>
      </div>
      <div class="age-card" onclick="showMaterials('5-6', 'all')" id="card-5-6">
        <span class="card-emoji">🌿</span>
        <div class="card-age">5–6 лет</div>
        <div class="card-label">Старший дошкольный</div>
        <span class="card-count"><i class="ti ti-files"></i> 74 материала</span>
      </div>
      <div class="age-card" onclick="showMaterials('6-7', 'all')" id="card-6-7">
        <span class="card-emoji">🌟</span>
        <div class="card-age">6–7 лет</div>
        <div class="card-label">Подготовительный</div>
        <span class="card-count"><i class="ti ti-files"></i> 81 материал</span>
      </div>
    </div>
  </section>

  <!-- MATERIALS SECTION -->
  <section class="materials-section" id="materials-section">
    <div class="materials-header">
      <div>
        <div class="materials-title" id="materials-title">Материалы</div>
      </div>
      <span class="materials-badge" id="materials-badge"></span>
    </div>

    <div class="subject-tabs" id="subject-tabs">
      <div class="subject-tab active" onclick="switchSubject('all', this)"><i class="ti ti-layout-grid"></i> Все</div>
      <div class="subject-tab" onclick="switchSubject('math', this)"><i class="ti ti-math-symbols"></i> Математика</div>
      <div class="subject-tab" onclick="switchSubject('russian', this)"><i class="ti ti-alphabet-cyrillic"></i> Русский язык</div>
      <div class="subject-tab" onclick="switchSubject('biology', this)"><i class="ti ti-leaf"></i> Биология</div>
    </div>

    <div class="materials-grid" id="materials-grid"></div>
  </section>

  <!-- FEATURES -->
  <section class="features-section">
    <h2 class="section-title">Возможности банка</h2>
    <p class="section-sub">Всё необходимое для качественного занятия</p>
    <div class="features-grid">
      <div class="feature-item">
        <div class="feature-icon" style="background:var(--teal-light);"><i class="ti ti-video" style="color:var(--teal);"></i></div>
        <h4>Видеоматериалы</h4>
        <p>Анимации, опыты, обучающие мультфильмы для наглядного объяснения</p>
      </div>
      <div class="feature-item">
        <div class="feature-icon" style="background:var(--amber-light);"><i class="ti ti-headphones" style="color:#BA7517;"></i></div>
        <h4>Аудио и музыка</h4>
        <p>Считалки, песенки, аудиосказки для развития речи и слуха</p>
      </div>
      <div class="feature-item">
        <div class="feature-icon" style="background:var(--purple-light);"><i class="ti ti-device-gamepad-2" style="color:var(--purple);"></i></div>
        <h4>Интерактивные игры</h4>
        <p>Тренажёры, пазлы и задания для увлекательного обучения</p>
      </div>
      <div class="feature-item">
        <div class="feature-icon" style="background:var(--blue-light);"><i class="ti ti-photo" style="color:var(--blue);"></i></div>
        <h4>Карточки и схемы</h4>
        <p>Наглядные иллюстрации, инфографика и демонстрационный материал</p>
      </div>
      <div class="feature-item">
        <div class="feature-icon" style="background:var(--coral-light);"><i class="ti ti-presentation" style="color:var(--coral);"></i></div>
        <h4>Презентации</h4>
        <p>Готовые слайды по всем темам программы с методическими советами</p>
      </div>
      <div class="feature-item">
        <div class="feature-icon" style="background:#F1EFE8;"><i class="ti ti-file-text" style="color:#5F5E5A;"></i></div>
        <h4>Рабочие листы</h4>
        <p>Задания для самостоятельной работы и занятий с родителями</p>
      </div>
    </div>
  </section>

</main>

<footer>
  <strong>📚 Банк медиаматериалов</strong> — для дошкольного образования<br>
  <span style="margin-top:6px;display:block;">Все материалы проверены методистами и соответствуют ФГОС ДО</span>
</footer>

<script>
const DATA = {
  '3-4': {
    math: [
      { title: 'Счёт до 5', desc: 'Анимация с весёлыми зверятами, учим считать до пяти', type: 'video', emoji: '🎬', level: 'base', dur: '4 мин' },
      { title: 'Формы и фигуры', desc: 'Карточки с геометрическими фигурами и заданиями', type: 'img', emoji: '🖼', level: 'base', dur: '12 карт' },
      { title: 'Больше и меньше', desc: 'Интерактивная игра на сравнение групп предметов', type: 'inter', emoji: '🎮', level: 'base', dur: '5 мин' },
      { title: 'Один и много', desc: 'Рабочий лист с раскраской и заданиями', type: 'doc', emoji: '📄', level: 'base', dur: '1 лист' },
    ],
    russian: [
      { title: 'Буква А', desc: 'Весёлая песенка про букву А с мультиком', type: 'audio', emoji: '🎵', level: 'base', dur: '2 мин' },
      { title: 'Моя семья', desc: 'Иллюстрации и карточки с подписями для расширения словаря', type: 'img', emoji: '🖼', level: 'base', dur: '8 карт' },
      { title: 'Слова и звуки', desc: 'Видеоурок — учимся различать звуки речи', type: 'video', emoji: '🎬', level: 'base', dur: '5 мин' },
    ],
    biology: [
      { title: 'Части растения', desc: 'Красочная схема с корнем, стеблем, листьями и цветком', type: 'img', emoji: '🖼', level: 'base', dur: '1 схема' },
      { title: 'Животные леса', desc: 'Видеопрогулка по зимнему лесу с рассказом о зверях', type: 'video', emoji: '🎬', level: 'base', dur: '6 мин' },
      { title: 'Сезоны года', desc: 'Презентация с фотографиями природы в разные времена года', type: 'pres', emoji: '📊', level: 'base', dur: '10 слайдов' },
    ],
  },
  '4-5': {
    math: [
      { title: 'Счёт до 10', desc: 'Анимация с загадками и весёлыми задачами до десяти', type: 'video', emoji: '🎬', level: 'base', dur: '6 мин' },
      { title: 'Геометрия вокруг нас', desc: 'Ищем фигуры в предметах: квадрат, круг, треугольник', type: 'inter', emoji: '🎮', level: 'base', dur: '7 мин' },
      { title: 'Порядковый счёт', desc: 'Рабочий лист с заданиями «Который по счёту?»', type: 'doc', emoji: '📄', level: 'base', dur: '2 листа' },
      { title: 'Сложение до 5', desc: 'Интерактивный тренажёр с яблоками и корзинками', type: 'inter', emoji: '🎮', level: 'adv', dur: '10 мин' },
    ],
    russian: [
      { title: 'Гласные звуки', desc: 'Видеоурок с упражнениями на А, О, У, Э, И, Ы', type: 'video', emoji: '🎬', level: 'base', dur: '7 мин' },
      { title: 'Слог за слогом', desc: 'Аудиоупражнение на деление слов на слоги', type: 'audio', emoji: '🎵', level: 'base', dur: '5 мин' },
      { title: 'Моё тело', desc: 'Карточки-иллюстрации с названиями частей тела', type: 'img', emoji: '🖼', level: 'base', dur: '10 карт' },
    ],
    biology: [
      { title: 'Жизнь насекомых', desc: 'Анимация про бабочек, жучков и их жизненный цикл', type: 'video', emoji: '🎬', level: 'base', dur: '5 мин' },
      { title: 'Что едят животные', desc: 'Интерактивная игра на сортировку травоядных и хищников', type: 'inter', emoji: '🎮', level: 'base', dur: '6 мин' },
      { title: 'Вода и её свойства', desc: 'Презентация с фото опытов: лёд, пар, жидкость', type: 'pres', emoji: '📊', level: 'base', dur: '8 слайдов' },
    ],
  },
  '5-6': {
    math: [
      { title: 'Счёт до 20', desc: 'Видео с задачами на сложение и вычитание в пределах 20', type: 'video', emoji: '🎬', level: 'adv', dur: '8 мин' },
      { title: 'Измерение длины', desc: 'Практические задания с линейкой и сравнением предметов', type: 'doc', emoji: '📄', level: 'base', dur: '2 листа' },
      { title: 'Ориентация в пространстве', desc: 'Интерактивная игра «Лабиринт»: лево, право, вперёд', type: 'inter', emoji: '🎮', level: 'base', dur: '8 мин' },
      { title: 'Задачи на вычитание', desc: 'Рабочие листы с иллюстрированными задачами', type: 'doc', emoji: '📄', level: 'adv', dur: '3 листа' },
    ],
    russian: [
      { title: 'Алфавит', desc: 'Плакат-интерактив с буквами и примерами слов', type: 'pres', emoji: '📊', level: 'base', dur: '33 слайда' },
      { title: 'Пересказываем сказки', desc: 'Аудиозапись с паузами для развития связной речи', type: 'audio', emoji: '🎵', level: 'adv', dur: '10 мин' },
      { title: 'Составляем предложение', desc: 'Карточки-слова для составления простых предложений', type: 'img', emoji: '🖼', level: 'base', dur: '20 карт' },
    ],
    biology: [
      { title: 'Строение цветка', desc: 'Схема и видео: тычинки, пестик, лепестки, чашелистик', type: 'video', emoji: '🎬', level: 'adv', dur: '6 мин' },
      { title: 'Птицы нашего края', desc: 'Фотокарточки с названиями и звуками птиц', type: 'img', emoji: '🖼', level: 'base', dur: '15 карт' },
      { title: 'Откуда берётся хлеб', desc: 'Документальный видеоряд от зерна до булки', type: 'video', emoji: '🎬', level: 'base', dur: '9 мин' },
    ],
  },
  '6-7': {
    math: [
      { title: 'Числа до 100', desc: 'Интерактивный тренажёр на счёт и порядок числа до 100', type: 'inter', emoji: '🎮', level: 'adv', dur: '12 мин' },
      { title: 'Состав числа', desc: 'Анимация с наглядной декомпозицией чисел 1–10', type: 'video', emoji: '🎬', level: 'adv', dur: '7 мин' },
      { title: 'Задачи в 2 действия', desc: 'Рабочие листы с сюжетными задачами на два действия', type: 'doc', emoji: '📄', level: 'adv', dur: '4 листа' },
      { title: 'Схемы и чертежи', desc: 'Вводный курс чтения простейших схем и планов', type: 'pres', emoji: '📊', level: 'adv', dur: '12 слайдов' },
    ],
    russian: [
      { title: 'Чтение по слогам', desc: 'Видеоурок с систематическими упражнениями', type: 'video', emoji: '🎬', level: 'adv', dur: '10 мин' },
      { title: 'Ударение в словах', desc: 'Интерактивный тест — расставь ударение', type: 'inter', emoji: '🎮', level: 'adv', dur: '8 мин' },
      { title: 'Печатные буквы', desc: 'Прописи и рабочие листы с прописью печатных букв', type: 'doc', emoji: '📄', level: 'base', dur: '5 листов' },
    ],
    biology: [
      { title: 'Тело человека', desc: 'Интерактивная схема с органами и их функциями', type: 'inter', emoji: '🎮', level: 'adv', dur: '10 мин' },
      { title: 'Экосистема пруда', desc: 'Видеоэкскурсия с рассказом о жителях пруда', type: 'video', emoji: '🎬', level: 'adv', dur: '8 мин' },
      { title: 'Звёздное небо', desc: 'Презентация о созвездиях и планетах для детей', type: 'pres', emoji: '📊', level: 'adv', dur: '14 слайдов' },
      { title: 'Как растут растения', desc: 'Видео-эксперимент: от семечка до ростка за 30 дней', type: 'video', emoji: '🎬', level: 'base', dur: '5 мин' },
    ],
  },
};

const typeLabels = { video:'Видео', audio:'Аудио', img:'Изображения', inter:'Интерактив', pres:'Презентация', doc:'Документ' };
const ageLabels = { '3-4':'3–4 года','4-5':'4–5 лет','5-6':'5–6 лет','6-7':'6–7 лет' };
const subjectLabels = { math:'Математика', russian:'Русский язык', biology:'Биология' };

let currentAge = null;
let currentSubject = 'all';

function openSidebar() {
  document.getElementById('sidebar').classList.add('open');
  document.getElementById('overlay').classList.add('open');
}
function closeSidebar() {
  document.getElementById('sidebar').classList.remove('open');
  document.getElementById('overlay').classList.remove('open');
}
document.getElementById('overlay').addEventListener('click', closeSidebar);

function toggleAge(age) {
  const panel = document.getElementById('panel-' + age);
  const item = document.getElementById('nav-' + age);
  const isOpen = panel.classList.contains('open');
  document.querySelectorAll('.subjects-panel').forEach(p => p.classList.remove('open'));
  document.querySelectorAll('.age-item').forEach(i => i.classList.remove('active'));
  if (!isOpen) {
    panel.classList.add('open');
    item.classList.add('active');
  }
}

function goTo(age, subject) {
  closeSidebar();
  showMaterials(age, subject);
}

function goBySubject(subject) {
  closeSidebar();
  if (!currentAge) currentAge = '3-4';
  showMaterials(currentAge, subject);
}

function showMaterials(age, subject) {
  currentAge = age;
  currentSubject = subject;

  document.querySelectorAll('.age-card').forEach(c => c.classList.remove('active'));
  const card = document.getElementById('card-' + age);
  if (card) card.classList.add('active');

  const section = document.getElementById('materials-section');
  section.classList.add('visible');
  section.scrollIntoView({ behavior: 'smooth', block: 'start' });

  document.getElementById('materials-title').textContent =
    'Материалы · ' + ageLabels[age] + (subject !== 'all' ? ' · ' + subjectLabels[subject] : '');

  document.querySelectorAll('.subject-tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.subject-tab')[['all','math','russian','biology'].indexOf(subject)].classList.add('active');

  renderMaterials(age, subject);
}

function switchSubject(subject, el) {
  currentSubject = subject;
  document.querySelectorAll('.subject-tab').forEach(t => t.classList.remove('active'));
  el.classList.add('active');
  document.getElementById('materials-title').textContent =
    'Материалы · ' + ageLabels[currentAge] + (subject !== 'all' ? ' · ' + subjectLabels[subject] : '');
  renderMaterials(currentAge, subject);
}

function renderMaterials(age, subject) {
  const grid = document.getElementById('materials-grid');
  const ageData = DATA[age];
  let items = [];

  if (subject === 'all') {
    ['math','russian','biology'].forEach(s => {
      (ageData[s] || []).forEach(m => items.push({ ...m, subject: s }));
    });
  } else {
    items = (ageData[subject] || []).map(m => ({ ...m, subject }));
  }

  document.getElementById('materials-badge').textContent = items.length + ' материалов';

  grid.innerHTML = items.map(m => `
    <div class="material-card">
      <div class="material-thumb thumb-${m.type}">
        <span style="font-size:40px;">${m.emoji}</span>
        <span class="material-type-badge type-${m.type}">${typeLabels[m.type]}</span>
      </div>
      <div class="material-body">
        <h3>${m.title}</h3>
        <p>${m.desc}</p>
        <div class="material-meta">
          <span class="material-level ${m.level === 'base' ? 'level-base' : 'level-adv'}">
            ${m.level === 'base' ? 'Базовый' : 'Углублённый'}
          </span>
          <span class="material-duration"><i class="ti ti-clock" style="font-size:13px;"></i>${m.dur}</span>
        </div>
      </div>
    </div>
  `).join('');
}
</script>
</body>
</html>
