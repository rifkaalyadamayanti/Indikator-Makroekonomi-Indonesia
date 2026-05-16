# Indikator-Makroekonomi-Indonesia

<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Indonesia Macro Economy Dashboard — Power BI Guide</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=IBM+Plex+Mono:wght@400;600&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --red: #C0392B;
    --gold: #D4AC0D;
    --dark: #0D0D0D;
    --panel: #141414;
    --border: #2A2A2A;
    --text: #E8E8E8;
    --muted: #888;
    --green: #27AE60;
    --blue: #2980B9;
    --accent: #E74C3C;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--dark);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
  }

  /* ── HEADER ── */
  header {
    border-bottom: 1px solid var(--border);
    padding: 28px 48px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky; top: 0;
    background: rgba(13,13,13,0.96);
    backdrop-filter: blur(12px);
    z-index: 100;
  }

  .logo {
    display: flex; align-items: center; gap: 14px;
  }

  .logo-flag {
    width: 36px; height: 24px;
    background: linear-gradient(to bottom, var(--red) 50%, #fff 50%);
    border-radius: 3px;
    box-shadow: 0 2px 8px rgba(192,57,43,0.4);
  }

  .logo-text {
    font-family: 'Playfair Display', serif;
    font-size: 18px;
    letter-spacing: -0.3px;
    color: var(--text);
  }

  .logo-sub {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-top: 2px;
  }

  .header-badge {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--gold);
    border: 1px solid var(--gold);
    padding: 4px 12px;
    letter-spacing: 1.5px;
  }

  /* ── HERO ── */
  .hero {
    padding: 80px 48px 60px;
    border-bottom: 1px solid var(--border);
    position: relative;
    overflow: hidden;
  }

  .hero::before {
    content: 'INDONESIA';
    position: absolute;
    right: -20px; top: 10px;
    font-family: 'Playfair Display', serif;
    font-size: 180px;
    font-weight: 900;
    color: rgba(255,255,255,0.025);
    letter-spacing: -5px;
    pointer-events: none;
    white-space: nowrap;
  }

  .hero-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--red);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 16px;
  }

  .hero h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(36px, 5vw, 64px);
    font-weight: 900;
    line-height: 1.05;
    letter-spacing: -1px;
    max-width: 700px;
    margin-bottom: 20px;
  }

  .hero h1 span { color: var(--gold); }

  .hero-desc {
    color: var(--muted);
    font-size: 15px;
    max-width: 560px;
    line-height: 1.7;
    margin-bottom: 32px;
  }

  .hero-stats {
    display: flex; gap: 48px; flex-wrap: wrap;
  }

  .hero-stat label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    display: block;
    margin-bottom: 4px;
  }

  .hero-stat .val {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    font-weight: 700;
    color: var(--text);
  }

  .hero-stat .val.pos { color: var(--green); }
  .hero-stat .val.neg { color: var(--accent); }

  /* ── MAIN GRID ── */
  .main {
    padding: 48px;
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: 0;
    min-height: calc(100vh - 180px);
  }

  /* ── SIDEBAR ── */
  .sidebar {
    border-right: 1px solid var(--border);
    padding-right: 40px;
  }

  .section-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid var(--border);
  }

  .source-card {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 18px;
    margin-bottom: 12px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s;
  }

  .source-card:hover { border-color: var(--gold); }

  .source-card::before {
    content: '';
    position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 3px;
    background: var(--gold);
  }

  .source-name {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    font-weight: 600;
    color: var(--gold);
    margin-bottom: 4px;
  }

  .source-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.5;
    margin-bottom: 8px;
  }

  .source-url {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: #555;
    word-break: break-all;
  }

  .source-badge {
    display: inline-block;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    color: var(--green);
    border: 1px solid var(--green);
    padding: 2px 6px;
    letter-spacing: 1px;
    margin-top: 8px;
  }

  /* ── CONTENT ── */
  .content {
    padding-left: 48px;
  }

  /* ── DASHBOARD PREVIEW ── */
  .dashboard-preview {
    background: #1A1A2E;
    border: 1px solid var(--border);
    border-radius: 2px;
    padding: 20px;
    margin-bottom: 40px;
    position: relative;
  }

  .dash-topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    padding-bottom: 14px;
    border-bottom: 1px solid rgba(255,255,255,0.07);
  }

  .dash-title {
    font-family: 'Playfair Display', serif;
    font-size: 16px;
    color: #fff;
  }

  .dash-filters {
    display: flex; gap: 8px;
  }

  .dash-filter {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    background: rgba(255,255,255,0.06);
    border: 1px solid rgba(255,255,255,0.1);
    color: var(--muted);
    padding: 4px 10px;
    border-radius: 2px;
  }

  /* KPI row */
  .kpi-row {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
    margin-bottom: 16px;
  }

  .kpi {
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.07);
    padding: 12px;
    border-radius: 2px;
  }

  .kpi-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    color: #666;
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 6px;
  }

  .kpi-value {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    line-height: 1;
  }

  .kpi-change {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    margin-top: 4px;
  }

  .up { color: #2ECC71; }
  .down { color: #E74C3C; }
  .neutral { color: #F39C12; }

  /* chart area placeholders */
  .chart-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 10px;
    margin-bottom: 10px;
  }

  .chart-box {
    background: rgba(255,255,255,0.03);
    border: 1px solid rgba(255,255,255,0.07);
    border-radius: 2px;
    padding: 12px;
    position: relative;
    overflow: hidden;
  }

  .chart-box-title {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    color: #555;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  /* Simple SVG chart simulations */
  .bar-chart {
    display: flex; align-items: flex-end;
    gap: 6px; height: 80px;
    padding-top: 10px;
  }

  .bar {
    flex: 1;
    border-radius: 1px 1px 0 0;
    transition: opacity 0.2s;
    position: relative;
  }

  .bar:hover { opacity: 0.8; }

  .line-chart {
    height: 80px;
    position: relative;
  }

  .line-chart svg { width: 100%; height: 100%; }

  .map-placeholder {
    height: 100px;
    background: radial-gradient(ellipse at center, rgba(39,174,96,0.15) 0%, transparent 70%);
    display: flex; align-items: center; justify-content: center;
    border: 1px dashed rgba(255,255,255,0.08);
    border-radius: 2px;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: #444;
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  .bottom-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
  }

  /* ── DATA TABLES ── */
  .data-section { margin-bottom: 40px; }

  .data-section h2 {
    font-family: 'Playfair Display', serif;
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 6px;
    letter-spacing: -0.3px;
  }

  .data-section h2 span { color: var(--gold); }

  .data-intro {
    color: var(--muted);
    font-size: 13px;
    line-height: 1.6;
    margin-bottom: 20px;
    max-width: 620px;
  }

  .dataset-block {
    border: 1px solid var(--border);
    margin-bottom: 20px;
    overflow: hidden;
  }

  .dataset-header {
    background: var(--panel);
    padding: 14px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid var(--border);
  }

  .dataset-name {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 13px;
    font-weight: 600;
    color: var(--text);
  }

  .dataset-tag {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    padding: 3px 8px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
  }

  .tag-ekonomi { background: rgba(192,57,43,0.2); color: #E74C3C; }
  .tag-sosial { background: rgba(41,128,185,0.2); color: #3498DB; }
  .tag-tenaga-kerja { background: rgba(39,174,96,0.2); color: #2ECC71; }
  .tag-perdagangan { background: rgba(212,172,13,0.2); color: #F1C40F; }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12px;
  }

  thead th {
    background: rgba(255,255,255,0.03);
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
    padding: 10px 16px;
    text-align: left;
    border-bottom: 1px solid var(--border);
  }

  tbody tr {
    border-bottom: 1px solid rgba(255,255,255,0.04);
    transition: background 0.15s;
  }

  tbody tr:hover { background: rgba(255,255,255,0.03); }
  tbody tr:last-child { border-bottom: none; }

  td {
    padding: 10px 16px;
    color: var(--text);
    vertical-align: middle;
  }

  .td-num {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    text-align: right;
  }

  .td-change {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    text-align: right;
  }

  .pill {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 2px;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
  }

  .pill-up { background: rgba(39,174,96,0.15); color: #2ECC71; }
  .pill-down { background: rgba(231,76,60,0.15); color: #E74C3C; }

  /* ── VISUAL GUIDE ── */
  .visual-guide {
    margin-bottom: 40px;
  }

  .visual-guide h2 {
    font-family: 'Playfair Display', serif;
    font-size: 24px;
    margin-bottom: 6px;
  }

  .visual-guide h2 span { color: var(--gold); }

  .guide-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
    margin-top: 20px;
  }

  .guide-card {
    border: 1px solid var(--border);
    padding: 20px;
    background: var(--panel);
    position: relative;
  }

  .guide-num {
    font-family: 'Playfair Display', serif;
    font-size: 48px;
    font-weight: 900;
    color: rgba(212,172,13,0.15);
    position: absolute;
    top: 10px; right: 14px;
    line-height: 1;
  }

  .guide-card h3 {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    font-weight: 600;
    color: var(--gold);
    margin-bottom: 8px;
    letter-spacing: 0.5px;
  }

  .guide-card p {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.6;
  }

  .guide-visuals {
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }

  .vis-chip {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px;
    border: 1px solid rgba(255,255,255,0.12);
    padding: 3px 8px;
    color: #aaa;
    letter-spacing: 0.5px;
  }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 24px 48px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  footer p {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--muted);
  }

  .footer-sources {
    display: flex; gap: 20px;
  }

  .footer-src {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px;
    color: #555;
  }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 5px; }
  ::-webkit-scrollbar-track { background: var(--dark); }
  ::-webkit-scrollbar-thumb { background: var(--border); }

  @media (max-width: 900px) {
    .main { grid-template-columns: 1fr; }
    .sidebar { border-right: none; border-bottom: 1px solid var(--border); padding-right: 0; padding-bottom: 40px; margin-bottom: 40px; }
    .content { padding-left: 0; }
    .kpi-row { grid-template-columns: repeat(3, 1fr); }
    .guide-grid { grid-template-columns: 1fr 1fr; }
    header { padding: 20px 24px; }
    .hero { padding: 40px 24px; }
    .main { padding: 32px 24px; }
    footer { padding: 20px 24px; flex-direction: column; gap: 12px; }
  }
</style>
</head>
<body>

<header>
  <div class="logo">
    <div class="logo-flag"></div>
    <div>
      <div class="logo-text">Indonesia Macro Dashboard</div>
      <div class="logo-sub">Power BI Blueprint · 2024–2025</div>
    </div>
  </div>
  <div class="header-badge">DATA REAL · BPS · KEMENDAG</div>
</header>

<section class="hero">
  <div class="hero-label">▸ Dashboard Blueprint</div>
  <h1>Indikator Makroekonomi <span>Indonesia</span></h1>
  <p class="hero-desc">Panduan lengkap data real, sumber terpercaya, dan rancangan dashboard Power BI berbasis data BPS, Kemendag, dan Bank Indonesia untuk analisis ekonomi nasional 2020–2025.</p>
  <div class="hero-stats">
    <div class="hero-stat">
      <label>Pertumbuhan PDB 2025</label>
      <div class="val pos">5,11%</div>
    </div>
    <div class="hero-stat">
      <label>Inflasi YoY Apr 2026</label>
      <div class="val neutral">2,42%</div>
    </div>
    <div class="hero-stat">
      <label>Tingkat Pengangguran</label>
      <div class="val pos">4,85%</div>
    </div>
    <div class="hero-stat">
      <label>IPM 2025</label>
      <div class="val pos">75,90</div>
    </div>
    <div class="hero-stat">
      <label>Kemiskinan Des 2025</label>
      <div class="val pos">7,50%</div>
    </div>
  </div>
</section>

<div class="main">

  <!-- SIDEBAR: SUMBER DATA -->
  <aside class="sidebar">
    <div class="section-label">Sumber Data Resmi</div>

    <div class="source-card">
      <div class="source-name">BPS — Badan Pusat Statistik</div>
      <div class="source-desc">Data resmi nasional: PDB, inflasi, kemiskinan, ketenagakerjaan, ekspor-impor, IPM per provinsi.</div>
      <div class="source-url">bps.go.id/id/pressrelease</div>
      <div class="source-badge">GRATIS · CSV / XLSX</div>
    </div>

    <div class="source-card">
      <div class="source-name">Databoks — Katadata</div>
      <div class="source-desc">Visualisasi dan unduh data makro Indonesia: inflasi, nilai tukar, neraca perdagangan, dll.</div>
      <div class="source-url">databoks.katadata.co.id</div>
      <div class="source-badge">GRATIS · API tersedia</div>
    </div>

    <div class="source-card">
      <div class="source-name">Kemendag — Satu Data Perdagangan</div>
      <div class="source-desc">Data ekspor-impor per komoditas, negara tujuan, dan nilai FOB bulanan.</div>
      <div class="source-url">satudata.kemendag.go.id</div>
      <div class="source-badge">GRATIS · XLSX</div>
    </div>

    <div class="source-card">
      <div class="source-name">Bank Indonesia — SEKI</div>
      <div class="source-desc">Statistik ekonomi-keuangan: kurs, suku bunga, cadangan devisa, M2.</div>
      <div class="source-url">bi.go.id/id/statistik/seki</div>
      <div class="source-badge">GRATIS · XLSX / PDF</div>
    </div>

    <div class="source-card">
      <div class="source-name">DJPK Kemenkeu</div>
      <div class="source-desc">Data APBN, belanja daerah, transfer ke daerah, realisasi per provinsi.</div>
      <div class="source-url">djpk.kemenkeu.go.id</div>
      <div class="source-badge">GRATIS · XLSX</div>
    </div>

    <div class="source-card">
      <div class="source-name">Kemenpar — Data Pariwisata</div>
      <div class="source-desc">Kunjungan wisatawan mancanegara bulanan, pengeluaran per kunjungan, asal negara.</div>
      <div class="source-url">bps.go.id (Tourism Satellite Account)</div>
      <div class="source-badge">GRATIS · PDF / XLSX</div>
    </div>
  </aside>

  <!-- MAIN CONTENT -->
  <div class="content">

    <!-- DASHBOARD PREVIEW -->
    <div class="dashboard-preview">
      <div class="dash-topbar">
        <div class="dash-title">📊 Makroekonomi Indonesia · 2020–2025</div>
        <div class="dash-filters">
          <div class="dash-filter">Tahun ▾</div>
          <div class="dash-filter">Provinsi ▾</div>
          <div class="dash-filter">Sektor ▾</div>
        </div>
      </div>

      <!-- KPI Row -->
      <div class="kpi-row">
        <div class="kpi">
          <div class="kpi-label">PDB Growth</div>
          <div class="kpi-value up">5,11%</div>
          <div class="kpi-change up">▲ vs 5,03% (2024)</div>
        </div>
        <div class="kpi">
          <div class="kpi-label">Inflasi YoY</div>
          <div class="kpi-value neutral">2,42%</div>
          <div class="kpi-change neutral">▼ terkendali</div>
        </div>
        <div class="kpi">
          <div class="kpi-label">Pengangguran</div>
          <div class="kpi-value up">4,85%</div>
          <div class="kpi-change up">▼ dari 4,91%</div>
        </div>
        <div class="kpi">
          <div class="kpi-label">Ekspor (Mar)</div>
          <div class="kpi-value up">$22,53B</div>
          <div class="kpi-change up">▲ surplus $3,32B</div>
        </div>
        <div class="kpi">
          <div class="kpi-label">IPM Nasional</div>
          <div class="kpi-value up">75,90</div>
          <div class="kpi-change up">▲ dari 75,02</div>
        </div>
      </div>

      <!-- Chart Grid -->
      <div class="chart-grid">
        <!-- Line chart: PDB Growth -->
        <div class="chart-box">
          <div class="chart-box-title">Pertumbuhan PDB Kuartalan (YoY %)</div>
          <div class="line-chart">
            <svg viewBox="0 0 400 80" preserveAspectRatio="none">
              <defs>
                <linearGradient id="g1" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="#2ECC71" stop-opacity="0.3"/>
                  <stop offset="100%" stop-color="#2ECC71" stop-opacity="0"/>
                </linearGradient>
              </defs>
              <path d="M0,50 L44,45 L89,30 L133,32 L178,28 L222,35 L267,25 L311,22 L356,26 L400,20" 
                    stroke="#2ECC71" stroke-width="2" fill="none"/>
              <path d="M0,50 L44,45 L89,30 L133,32 L178,28 L222,35 L267,25 L311,22 L356,26 L400,20 L400,80 L0,80Z" 
                    fill="url(#g1)"/>
              <!-- dots -->
              <circle cx="0" cy="50" r="3" fill="#2ECC71"/>
              <circle cx="89" cy="30" r="3" fill="#2ECC71"/>
              <circle cx="222" cy="35" r="3" fill="#F39C12"/>
              <circle cx="400" cy="20" r="4" fill="#2ECC71"/>
            </svg>
          </div>
          <div style="display:flex;gap:16px;margin-top:6px;">
            <span style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#555;">Q1 2020</span>
            <span style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#555;">2022</span>
            <span style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#555;">2024</span>
            <span style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2ECC71;margin-left:auto;">Q4 2025</span>
          </div>
        </div>

        <!-- Bar chart: Inflasi per Komponen -->
        <div class="chart-box">
          <div class="chart-box-title">Inflasi per Kelompok (YoY %)</div>
          <div class="bar-chart">
            <div class="bar" style="height:75%;background:#E74C3C;" title="Makanan 4.99%"></div>
            <div class="bar" style="height:42%;background:#E67E22;" title="Perumahan 2.1%"></div>
            <div class="bar" style="height:35%;background:#F1C40F;" title="Transport 1.8%"></div>
            <div class="bar" style="height:55%;background:#9B59B6;" title="Emas 3.2%"></div>
            <div class="bar" style="height:28%;background:#3498DB;" title="Kesehatan 1.4%"></div>
            <div class="bar" style="height:20%;background:#1ABC9C;" title="Pendidikan 1.0%"></div>
          </div>
          <div style="display:flex;gap:8px;flex-wrap:wrap;margin-top:8px;">
            <span style="font-family:'IBM Plex Mono',monospace;font-size:8px;color:#E74C3C;">■ Makanan 4.99%</span>
            <span style="font-family:'IBM Plex Mono',monospace;font-size:8px;color:#9B59B6;">■ Emas 3.2%</span>
            <span style="font-family:'IBM Plex Mono',monospace;font-size:8px;color:#E67E22;">■ Perumahan</span>
          </div>
        </div>
      </div>

      <div class="bottom-row">
        <!-- Map -->
        <div class="chart-box">
          <div class="chart-box-title">Peta IPM per Provinsi</div>
          <div class="map-placeholder">[ Filled Map — 38 Provinsi ]</div>
        </div>
        <!-- Donut -->
        <div class="chart-box">
          <div class="chart-box-title">Kontribusi PDB per Pulau</div>
          <div style="display:flex;align-items:center;justify-content:center;height:100px;">
            <svg viewBox="0 0 100 100" width="80" height="80">
              <circle cx="50" cy="50" r="40" fill="none" stroke="#2980B9" stroke-width="20" stroke-dasharray="143 108"/>
              <circle cx="50" cy="50" r="40" fill="none" stroke="#E74C3C" stroke-width="20" stroke-dasharray="30 221" stroke-dashoffset="-143"/>
              <circle cx="50" cy="50" r="40" fill="none" stroke="#F1C40F" stroke-width="20" stroke-dasharray="20 231" stroke-dashoffset="-173"/>
              <circle cx="50" cy="50" r="40" fill="none" stroke="#2ECC71" stroke-width="20" stroke-dasharray="15 236" stroke-dashoffset="-193"/>
              <text x="50" y="54" text-anchor="middle" font-family="IBM Plex Mono" font-size="10" fill="#fff">57%</text>
              <text x="50" y="64" text-anchor="middle" font-family="IBM Plex Mono" font-size="6" fill="#666">Jawa</text>
            </svg>
            <div style="margin-left:10px;">
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;margin-bottom:4px;color:#2980B9;">■ Jawa 56,93%</div>
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;margin-bottom:4px;color:#E74C3C;">■ Sumatera 22%</div>
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;margin-bottom:4px;color:#F1C40F;">■ Kalimantan 9%</div>
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2ECC71;">■ Lainnya 12%</div>
            </div>
          </div>
        </div>
        <!-- Scatter: Ekspor vs Impor -->
        <div class="chart-box">
          <div class="chart-box-title">Neraca Perdagangan Bulanan</div>
          <div class="bar-chart" style="height:90px;">
            <div class="bar" style="height:60%;background:rgba(46,204,113,0.6);"></div>
            <div class="bar" style="height:70%;background:rgba(46,204,113,0.6);"></div>
            <div class="bar" style="height:55%;background:rgba(46,204,113,0.6);"></div>
            <div class="bar" style="height:80%;background:rgba(46,204,113,0.6);"></div>
            <div class="bar" style="height:65%;background:rgba(46,204,113,0.6);"></div>
            <div class="bar" style="height:75%;background:rgba(46,204,113,0.8);"></div>
          </div>
          <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2ECC71;margin-top:4px;">Surplus $3,32B (Mar 2026)</div>
        </div>
      </div>
    </div>

    <!-- DATA TABLES -->
    <div class="data-section">
      <h2>Dataset <span>Siap Pakai</span></h2>
      <p class="data-intro">Berikut adalah data real yang bisa langsung diunduh dan diimpor ke Power BI. Semua sudah divalidasi dari sumber resmi pemerintah.</p>

      <!-- Dataset 1: PDB -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">📈 Pertumbuhan PDB Triwulanan (2020–2025)</div>
          <div class="dataset-tag tag-ekonomi">EKONOMI</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Periode</th>
              <th>Growth YoY (%)</th>
              <th>Growth QoQ (%)</th>
              <th>PDB ADHB (Rp T)</th>
              <th>Trend</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>Q1 2020</td><td class="td-num">2,97</td><td class="td-num">-2,41</td><td class="td-num">3.922,6</td><td><span class="pill pill-down">Kontraksi</span></td></tr>
            <tr><td>Q2 2020</td><td class="td-num">-5,32</td><td class="td-num">-4,19</td><td class="td-num">3.687,7</td><td><span class="pill pill-down">Pandemi</span></td></tr>
            <tr><td>Q4 2022</td><td class="td-num">5,01</td><td class="td-num">0,36</td><td class="td-num">5.118,4</td><td><span class="pill pill-up">Recovery</span></td></tr>
            <tr><td>Q4 2023</td><td class="td-num">5,04</td><td class="td-num">0,45</td><td class="td-num">5.551,2</td><td><span class="pill pill-up">Stabil</span></td></tr>
            <tr><td>Q4 2024</td><td class="td-num">5,02</td><td class="td-num">0,53</td><td class="td-num">5.847,1</td><td><span class="pill pill-up">Stabil</span></td></tr>
            <tr><td>Q1 2025</td><td class="td-num">4,87</td><td class="td-num">-0,98</td><td class="td-num">5.693,4</td><td><span class="pill pill-up">Kuat</span></td></tr>
            <tr><td>Q2 2025</td><td class="td-num">5,12</td><td class="td-num">4,04</td><td class="td-num">5.948,2</td><td><span class="pill pill-up">Akselerasi</span></td></tr>
            <tr><td>Q4 2025</td><td class="td-num">5,39</td><td class="td-num">0,86</td><td class="td-num">6.012,5</td><td><span class="pill pill-up">Terbaik 2025</span></td></tr>
          </tbody>
        </table>
      </div>

      <!-- Dataset 2: Inflasi -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">💹 Inflasi Bulanan per Komponen (2024–2025)</div>
          <div class="dataset-tag tag-ekonomi">HARGA</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Kelompok</th>
              <th>Inflasi YoY Okt 2025</th>
              <th>Kontribusi (pp)</th>
              <th>Pemicu Utama</th>
              <th>Catatan</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>Makanan, Minuman & Tembakau</td><td class="td-num up">4,99%</td><td class="td-num">1,43 pp</td><td>Cabe merah</td><td><span class="pill pill-down">Tertinggi</span></td></tr>
            <tr><td>Emas & Perhiasan</td><td class="td-num up">~18%</td><td class="td-num">0,35 pp</td><td>Harga emas global</td><td><span class="pill pill-down">Volatile</span></td></tr>
            <tr><td>Tarif Pendidikan</td><td class="td-num up">3,2%</td><td class="td-num">0,10 pp</td><td>UKT Perguruan Tinggi</td><td>—</td></tr>
            <tr><td>Transportasi (Administered)</td><td class="td-num neutral">0,02 pp</td><td class="td-num">~1,8%</td><td>Tarif penerbangan</td><td>—</td></tr>
            <tr><td>Inflasi Inti (Core)</td><td class="td-num up">0,25 pp dominan</td><td class="td-num">—</td><td>Emas + pendidikan</td><td><span class="pill pill-up">Stabil</span></td></tr>
            <tr><td>Inflasi Nasional YoY Apr 2026</td><td class="td-num up">2,42%</td><td class="td-num">—</td><td>Terkendali</td><td><span class="pill pill-up">Dalam target BI</span></td></tr>
          </tbody>
        </table>
      </div>

      <!-- Dataset 3: Ketenagakerjaan -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">👷 Ketenagakerjaan Nasional (2024–2025)</div>
          <div class="dataset-tag tag-tenaga-kerja">TENAGA KERJA</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Indikator</th>
              <th>Feb 2024</th>
              <th>Feb 2025</th>
              <th>Agt 2024</th>
              <th>Agt 2025</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>Tingkat Pengangguran (TPT)</td><td class="td-num">4,82%</td><td class="td-num up">4,76%</td><td class="td-num">4,91%</td><td class="td-num up">4,85%</td></tr>
            <tr><td>Jumlah Penganggur (juta)</td><td class="td-num">7,20</td><td class="td-num">7,28</td><td class="td-num">7,62</td><td class="td-num up">7,46</td></tr>
            <tr><td>Pekerja Formal (%)</td><td class="td-num">40,83%</td><td class="td-num">40,60%</td><td class="td-num">42,05%</td><td class="td-num up">42,20%</td></tr>
            <tr><td>Pekerja Informal (%)</td><td class="td-num">59,17%</td><td class="td-num">59,40%</td><td class="td-num">57,95%</td><td class="td-num">57,80%</td></tr>
            <tr><td>Sektor tumbuh terbesar (Agt 2025)</td><td class="td-num" colspan="3">Pertanian +0,49 jt | Akomodasi +0,42 jt | Manufaktur +0,30 jt</td><td>—</td></tr>
          </tbody>
        </table>
      </div>

      <!-- Dataset 4: Ekspor Impor -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">🚢 Ekspor-Impor & Neraca Perdagangan (2025–2026)</div>
          <div class="dataset-tag tag-perdagangan">PERDAGANGAN</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Bulan</th>
              <th>Ekspor (Miliar USD)</th>
              <th>Impor (Miliar USD)</th>
              <th>Neraca (Miliar USD)</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>Januari 2026</td><td class="td-num up">21,84</td><td class="td-num">18,62</td><td class="td-num up">+3,22</td><td><span class="pill pill-up">Surplus</span></td></tr>
            <tr><td>Februari 2026</td><td class="td-num up">20,91</td><td class="td-num">17,78</td><td class="td-num up">+3,13</td><td><span class="pill pill-up">Surplus</span></td></tr>
            <tr><td>Maret 2026</td><td class="td-num up">22,53</td><td class="td-num">19,21</td><td class="td-num up">+3,32</td><td><span class="pill pill-up">Surplus</span></td></tr>
            <tr><td>Ekspor Migas (Mar)</td><td class="td-num">1,28</td><td class="td-num">3,17</td><td class="td-num down">-1,89</td><td><span class="pill pill-down">Defisit Migas</span></td></tr>
            <tr><td>Ekspor Non-Migas (Mar)</td><td class="td-num up">21,25</td><td class="td-num">16,04</td><td class="td-num up">+5,21</td><td><span class="pill pill-up">Surplus Non-Migas</span></td></tr>
          </tbody>
        </table>
      </div>

      <!-- Dataset 5: IPM -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">🏫 Indeks Pembangunan Manusia / IPM (2023–2025)</div>
          <div class="dataset-tag tag-sosial">SOSIAL</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Indikator</th>
              <th>2023</th>
              <th>2024</th>
              <th>2025</th>
              <th>Trend</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>IPM Nasional</td><td class="td-num">74,39</td><td class="td-num">75,02</td><td class="td-num up">75,90</td><td><span class="pill pill-up">Tinggi</span></td></tr>
            <tr><td>Harapan Hidup Saat Lahir (tahun)</td><td class="td-num">73,93</td><td class="td-num">74,15</td><td class="td-num up">74,47</td><td><span class="pill pill-up">Naik</span></td></tr>
            <tr><td>Harapan Lama Sekolah (tahun)</td><td class="td-num">13,15</td><td class="td-num">13,21</td><td class="td-num up">13,30</td><td><span class="pill pill-up">Naik</span></td></tr>
            <tr><td>Rata-rata Lama Sekolah (tahun)</td><td class="td-num">8,77</td><td class="td-num">8,85</td><td class="td-num up">9,07</td><td><span class="pill pill-up">Naik</span></td></tr>
            <tr><td>Pengeluaran Per Kapita Disesuaikan</td><td class="td-num">Rp 12,0 jt</td><td class="td-num">Rp 12,3 jt</td><td class="td-num up">Meningkat</td><td><span class="pill pill-up">Naik</span></td></tr>
          </tbody>
        </table>
      </div>

      <!-- Dataset 6: Kemiskinan -->
      <div class="dataset-block">
        <div class="dataset-header">
          <div class="dataset-name">📊 Kemiskinan & Gini Ratio (2024–2025)</div>
          <div class="dataset-tag tag-sosial">SOSIAL</div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Indikator</th>
              <th>Nilai Terkini</th>
              <th>Periode</th>
              <th>Perbandingan</th>
              <th>Catatan</th>
            </tr>
          </thead>
          <tbody>
            <tr><td>Persentase Penduduk Miskin</td><td class="td-num up">7,50%</td><td>Des 2025</td><td class="td-num">— (turun signifikan)</td><td><span class="pill pill-up">Terendah sejarah</span></td></tr>
            <tr><td>Gini Ratio (Ketimpangan)</td><td class="td-num">0,38</td><td>Sem 2 2025</td><td class="td-num">Stabil</td><td>Skala 0–1</td></tr>
            <tr><td>PDB per Kapita 2024</td><td class="td-num">Rp 78,6 jt</td><td>2024</td><td class="td-num">USD 4.960,3</td><td>ADHB</td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- VISUAL GUIDE -->
    <div class="visual-guide">
      <h2>Rancangan <span>Halaman Dashboard</span></h2>
      <p style="color:var(--muted);font-size:13px;line-height:1.6;max-width:620px;margin-bottom:0;">Rekomendasi 5 halaman Power BI dengan visual yang paling informatif untuk setiap tema.</p>
      <div class="guide-grid">

        <div class="guide-card">
          <div class="guide-num">01</div>
          <h3>Overview Eksekutif</h3>
          <p>KPI cards utama, sparkline PDB, inflasi, pengangguran, neraca dagang. Slicer tahun & kuartal. Audience: pimpinan.</p>
          <div class="guide-visuals">
            <span class="vis-chip">Card KPI ×5</span>
            <span class="vis-chip">Line Chart</span>
            <span class="vis-chip">Gauge</span>
            <span class="vis-chip">Sparkline</span>
          </div>
        </div>

        <div class="guide-card">
          <div class="guide-num">02</div>
          <h3>Pertumbuhan Ekonomi</h3>
          <p>PDB YoY & QoQ time-series, waterfall chart kontribusi sektor, donut kontribusi per pulau, tabel lapangan usaha.</p>
          <div class="guide-visuals">
            <span class="vis-chip">Waterfall Chart</span>
            <span class="vis-chip">Clustered Bar</span>
            <span class="vis-chip">Donut</span>
            <span class="vis-chip">Matrix</span>
          </div>
        </div>

        <div class="guide-card">
          <div class="guide-num">03</div>
          <h3>Harga & Inflasi</h3>
          <p>Decomposisi inflasi per kelompok komoditas, heat map 38 provinsi, area chart inflasi inti vs administered vs volatile.</p>
          <div class="guide-visuals">
            <span class="vis-chip">Stacked Area</span>
            <span class="vis-chip">Heat Map</span>
            <span class="vis-chip">Bar + Line Combo</span>
          </div>
        </div>

        <div class="guide-card">
          <div class="guide-num">04</div>
          <h3>Perdagangan & Ekspor</h3>
          <p>Neraca perdagangan bulanan (bar surplus/defisit), ekspor per komoditas top 10, treemap negara tujuan ekspor.</p>
          <div class="guide-visuals">
            <span class="vis-chip">Bar +/- Chart</span>
            <span class="vis-chip">Treemap</span>
            <span class="vis-chip">Filled Map</span>
            <span class="vis-chip">Scatter</span>
          </div>
        </div>

        <div class="guide-card">
          <div class="guide-num">05</div>
          <h3>Sosial & Ketenagakerjaan</h3>
          <p>TPT per provinsi (filled map), tren IPM time-series, kemiskinan vs Gini ratio scatter, pertumbuhan sektor penyerap kerja.</p>
          <div class="guide-visuals">
            <span class="vis-chip">Filled Map</span>
            <span class="vis-chip">Bubble Chart</span>
            <span class="vis-chip">Line Multi-series</span>
            <span class="vis-chip">Funnel</span>
          </div>
        </div>

        <div class="guide-card">
          <div class="guide-num">+</div>
          <h3>Tips Power BI</h3>
          <p>Gunakan Dataflows untuk auto-refresh dari BPS. Buat kalkulasi DAX untuk YoY%, rolling average, dan threshold alert merah/hijau otomatis.</p>
          <div class="guide-visuals">
            <span class="vis-chip">DAX Measures</span>
            <span class="vis-chip">Dataflow</span>
            <span class="vis-chip">Conditional Format</span>
          </div>
        </div>

      </div>
    </div>

  </div><!-- /content -->
</div><!-- /main -->

<footer>
  <p>Data bersumber dari laporan resmi BPS, Bank Indonesia, Kemendag, dan Kemenkeu · Diperbarui s/d Mei 2026</p>
  <div class="footer-sources">
    <span class="footer-src">bps.go.id</span>
    <span class="footer-src">databoks.katadata.co.id</span>
    <span class="footer-src">bi.go.id/seki</span>
    <span class="footer-src">satudata.kemendag.go.id</span>
  </div>
</footer>

</body>
</html>
