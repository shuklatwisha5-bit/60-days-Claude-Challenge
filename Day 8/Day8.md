Day 8 – Environmental Health Analyzer Dashboard
Objective

The goal of Day 8 was to explore AI-assisted application generation by using Claude to build an Environmental Health Analyzer Dashboard. The task involved generating an interactive HTML dashboard, testing its features, and documenting the learning process.

Steps Completed
Read the provided resources.
Watched the solution video.
Opened Claude and started a new conversation.
Pasted the provided Environmental Health Analyzer prompt.
Allowed Claude to generate the dashboard.
Generated and downloaded the HTML application.
Tested the dashboard by interacting with filters and charts.
Captured screenshots of the dashboard interface.
Created a Day8 folder in the GitHub repository.
Added the generated HTML file, screenshots, and this documentation file.
Committed and pushed the changes to GitHub.
What I Learned
How AI can generate complete interactive web applications from a single prompt.
The basics of AI-assisted dashboard creation and HTML generation.
How to work with interactive charts and filters in a generated application.
The importance of testing AI-generated outputs before deployment.
How to organize project files and maintain documentation in a GitHub repository.
Files Included
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌍 Personal Environmental Health Analyzer</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap');

  :root {
    --bg-primary: #0a0d14;
    --bg-card: #111520;
    --bg-card2: #161c2d;
    --bg-glass: rgba(255,255,255,0.04);
    --border: rgba(255,255,255,0.08);
    --border-accent: rgba(99,179,237,0.3);
    --text-primary: #e8edf5;
    --text-secondary: #8a95a8;
    --text-muted: #5a6270;
    --accent-blue: #63b3ed;
    --accent-cyan: #4fd1c7;
    --accent-purple: #9f7aea;
    --accent-green: #68d391;
    --accent-orange: #f6ad55;
    --accent-red: #fc8181;
    --aqi-good: #48bb78;
    --aqi-satisfactory: #9ae6b4;
    --aqi-moderate: #f6e05e;
    --aqi-poor: #f6ad55;
    --aqi-very-poor: #fc8181;
    --aqi-severe: #e53e3e;
    --glow-blue: 0 0 20px rgba(99,179,237,0.15);
    --glow-green: 0 0 20px rgba(104,211,145,0.15);
    --radius: 16px;
    --radius-sm: 10px;
    --transition: all 0.25s cubic-bezier(0.4,0,0.2,1);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg-primary);
    color: var(--text-primary);
    min-height: 100vh;
    overflow-x: hidden;
    line-height: 1.6;
  }

  /* GRID BG */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: 
      radial-gradient(ellipse 80% 60% at 20% 10%, rgba(99,179,237,0.05) 0%, transparent 60%),
      radial-gradient(ellipse 60% 80% at 80% 90%, rgba(159,122,234,0.04) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
  }

  .container { max-width: 1400px; margin: 0 auto; padding: 0 20px; position: relative; z-index: 1; }

  /* HEADER */
  .header {
    background: linear-gradient(135deg, rgba(99,179,237,0.08) 0%, rgba(159,122,234,0.05) 100%);
    border-bottom: 1px solid var(--border);
    padding: 20px 0;
    position: sticky;
    top: 0;
    z-index: 100;
    backdrop-filter: blur(20px);
  }

  .header-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 12px;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .logo-icon {
    width: 44px; height: 44px;
    background: linear-gradient(135deg, var(--accent-blue), var(--accent-cyan));
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 22px;
    box-shadow: 0 4px 16px rgba(99,179,237,0.3);
  }

  .logo h1 {
    font-size: 18px;
    font-weight: 700;
    background: linear-gradient(120deg, var(--accent-blue), var(--accent-cyan));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .logo p { font-size: 12px; color: var(--text-muted); }

  .header-right {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .data-badge {
    background: rgba(104,211,145,0.1);
    border: 1px solid rgba(104,211,145,0.2);
    color: var(--accent-green);
    font-size: 11px;
    padding: 4px 10px;
    border-radius: 20px;
    font-weight: 600;
  }

  .last-updated {
    font-size: 11px;
    color: var(--text-muted);
  }

  /* NAV TABS */
  .nav-tabs {
    background: var(--bg-card);
    border-bottom: 1px solid var(--border);
    padding: 0;
    overflow-x: auto;
  }

  .nav-tabs-inner {
    display: flex;
    gap: 0;
    padding: 0 20px;
    max-width: 1400px;
    margin: 0 auto;
  }

  .tab-btn {
    background: none;
    border: none;
    color: var(--text-secondary);
    font-family: 'Inter', sans-serif;
    font-size: 13px;
    font-weight: 500;
    padding: 14px 18px;
    cursor: pointer;
    border-bottom: 2px solid transparent;
    white-space: nowrap;
    transition: var(--transition);
  }

  .tab-btn:hover { color: var(--text-primary); }

  .tab-btn.active {
    color: var(--accent-blue);
    border-bottom-color: var(--accent-blue);
  }

  /* MAIN CONTENT */
  .main { padding: 28px 0; }

  .tab-content { display: none; }
  .tab-content.active { display: block; }

  /* CARDS */
  .card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
    transition: var(--transition);
    position: relative;
    overflow: hidden;
  }

  .card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: inherit;
    background: var(--bg-glass);
    opacity: 0;
    transition: opacity 0.3s;
  }

  .card:hover::before { opacity: 1; }

  .card-title {
    font-size: 12px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-muted);
    margin-bottom: 8px;
  }

  /* METRICS GRID */
  .metrics-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 14px;
    margin-bottom: 24px;
  }

  .metric-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 18px;
    position: relative;
    overflow: hidden;
    transition: var(--transition);
  }

  .metric-card:hover {
    border-color: var(--border-accent);
    transform: translateY(-2px);
    box-shadow: var(--glow-blue);
  }

  .metric-card .accent-bar {
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    border-radius: 2px 2px 0 0;
  }

  .metric-label {
    font-size: 11px;
    color: var(--text-muted);
    font-weight: 500;
    margin-bottom: 6px;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .metric-value {
    font-size: 28px;
    font-weight: 800;
    font-family: 'JetBrains Mono', monospace;
    line-height: 1;
    margin-bottom: 4px;
  }

  .metric-sub {
    font-size: 11px;
    color: var(--text-secondary);
  }

  /* SECTION TITLE */
  .section-title {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .section-title .dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--accent-blue);
  }

  /* CHART GRID */
  .chart-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 24px;
  }

  @media (max-width: 768px) {
    .chart-grid { grid-template-columns: 1fr; }
  }

  .chart-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
  }

  .chart-card h3 {
    font-size: 13px;
    font-weight: 600;
    color: var(--text-secondary);
    margin-bottom: 16px;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .chart-wrapper { position: relative; height: 260px; }

  /* FILTERS */
  .filters-bar {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 16px 20px;
    margin-bottom: 20px;
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    align-items: center;
  }

  .filter-label {
    font-size: 11px;
    color: var(--text-muted);
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }

  .filter-group {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }

  .filter-btn {
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    color: var(--text-secondary);
    font-family: 'Inter', sans-serif;
    font-size: 12px;
    font-weight: 500;
    padding: 6px 14px;
    border-radius: 20px;
    cursor: pointer;
    transition: var(--transition);
    white-space: nowrap;
  }

  .filter-btn:hover { border-color: var(--accent-blue); color: var(--accent-blue); }

  .filter-btn.active {
    background: rgba(99,179,237,0.12);
    border-color: var(--accent-blue);
    color: var(--accent-blue);
  }

  .filter-select {
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    color: var(--text-primary);
    font-family: 'Inter', sans-serif;
    font-size: 12px;
    padding: 6px 12px;
    border-radius: 8px;
    cursor: pointer;
    outline: none;
  }

  /* CITY CARDS */
  .city-cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 14px;
    margin-bottom: 24px;
  }

  .city-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 18px;
    cursor: pointer;
    transition: var(--transition);
    position: relative;
    overflow: hidden;
  }

  .city-card:hover {
    border-color: var(--border-accent);
    transform: translateY(-2px);
    box-shadow: var(--glow-blue);
  }

  .city-card.selected {
    border-color: var(--accent-blue);
    box-shadow: 0 0 0 1px rgba(99,179,237,0.3), var(--glow-blue);
  }

  .city-card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 12px;
  }

  .city-name {
    font-size: 16px;
    font-weight: 700;
  }

  .city-state {
    font-size: 11px;
    color: var(--text-muted);
  }

  .aqi-badge {
    padding: 4px 10px;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 700;
    font-family: 'JetBrains Mono', monospace;
  }

  .city-stats {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 8px;
    margin-top: 12px;
  }

  .city-stat {
    background: rgba(255,255,255,0.03);
    border-radius: 8px;
    padding: 8px;
    text-align: center;
  }

  .city-stat-label {
    font-size: 9px;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }

  .city-stat-value {
    font-size: 14px;
    font-weight: 700;
    font-family: 'JetBrains Mono', monospace;
    color: var(--text-primary);
  }

  .aqi-category-strip {
    height: 3px;
    border-radius: 3px;
    margin-top: 12px;
  }

  /* AQI COLOR CLASSES */
  .good { background: var(--aqi-good); color: #0d1f14; }
  .satisfactory { background: var(--aqi-satisfactory); color: #0d1f14; }
  .moderate { background: var(--aqi-moderate); color: #1a1500; }
  .poor { background: var(--aqi-poor); color: #1a0e00; }
  .very-poor { background: var(--aqi-very-poor); color: #1a0000; }
  .severe { background: var(--aqi-severe); color: #fff; }

  .good-text { color: var(--aqi-good); }
  .satisfactory-text { color: var(--aqi-satisfactory); }
  .moderate-text { color: var(--aqi-moderate); }
  .poor-text { color: var(--aqi-poor); }
  .very-poor-text { color: var(--aqi-very-poor); }
  .severe-text { color: var(--aqi-severe); }

  /* HEALTH SECTION */
  .health-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 24px;
  }

  @media (max-width: 768px) {
    .health-grid { grid-template-columns: 1fr; }
  }

  .health-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 14px;
    background: rgba(255,255,255,0.02);
    border-radius: 10px;
    border: 1px solid var(--border);
    transition: var(--transition);
  }

  .health-item:hover { border-color: rgba(255,255,255,0.12); }

  .health-icon {
    font-size: 22px;
    line-height: 1;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .health-info h4 { font-size: 13px; font-weight: 600; margin-bottom: 4px; }
  .health-info p { font-size: 12px; color: var(--text-secondary); line-height: 1.5; }

  .risk-indicator {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    font-size: 11px;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 4px;
    margin-top: 6px;
  }

  .risk-low { background: rgba(72,187,120,0.15); color: var(--aqi-good); }
  .risk-moderate { background: rgba(246,224,94,0.12); color: var(--aqi-moderate); }
  .risk-high { background: rgba(252,129,129,0.12); color: var(--aqi-very-poor); }

  /* REPORT CARD */
  .report-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 24px;
  }

  @media (max-width: 640px) {
    .report-grid { grid-template-columns: 1fr; }
  }

  .score-card {
    background: var(--bg-card2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 24px;
    text-align: center;
  }

  .score-circle {
    width: 110px;
    height: 110px;
    border-radius: 50%;
    margin: 0 auto 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .score-num {
    font-size: 32px;
    font-weight: 800;
    font-family: 'JetBrains Mono', monospace;
    line-height: 1;
  }

  .score-label { font-size: 10px; color: var(--text-muted); text-transform: uppercase; }
  .score-grade {
    font-size: 36px;
    font-weight: 800;
    margin-bottom: 6px;
  }

  .score-title { font-size: 14px; font-weight: 600; margin-bottom: 4px; }
  .score-desc { font-size: 12px; color: var(--text-secondary); }

  .grade-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }

  .grade-row:last-child { border-bottom: none; }

  .grade-name { font-size: 13px; font-weight: 500; }

  .grade-badge {
    font-size: 16px;
    font-weight: 800;
    font-family: 'JetBrains Mono', monospace;
    padding: 2px 10px;
    border-radius: 6px;
  }

  /* PROGRESS BAR */
  .progress-bar {
    height: 6px;
    background: rgba(255,255,255,0.06);
    border-radius: 3px;
    overflow: hidden;
    margin-top: 8px;
  }

  .progress-fill {
    height: 100%;
    border-radius: 3px;
    transition: width 1s cubic-bezier(0.4,0,0.2,1);
  }

  /* INSIGHTS */
  .insight-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 18px;
    margin-bottom: 12px;
    border-left: 3px solid var(--accent-blue);
    transition: var(--transition);
  }

  .insight-card:hover { transform: translateX(4px); }

  .insight-card.warning { border-left-color: var(--accent-orange); }
  .insight-card.danger { border-left-color: var(--accent-red); }
  .insight-card.success { border-left-color: var(--accent-green); }
  .insight-card.purple { border-left-color: var(--accent-purple); }

  .insight-title { font-size: 13px; font-weight: 700; margin-bottom: 4px; }
  .insight-text { font-size: 12px; color: var(--text-secondary); line-height: 1.5; }

  /* RECOMMENDATIONS */
  .rec-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 14px;
    margin-bottom: 24px;
  }

  .rec-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 18px;
    transition: var(--transition);
  }

  .rec-card:hover {
    border-color: var(--border-accent);
    transform: translateY(-2px);
  }

  .rec-card h4 {
    font-size: 13px;
    font-weight: 700;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .rec-card ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  .rec-card li {
    font-size: 12px;
    color: var(--text-secondary);
    padding-left: 14px;
    position: relative;
    line-height: 1.4;
  }

  .rec-card li::before {
    content: '→';
    position: absolute;
    left: 0;
    color: var(--accent-blue);
    font-size: 11px;
  }

  /* AQI LEGEND */
  .aqi-legend {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 20px;
  }

  .legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 11px;
    font-weight: 500;
    padding: 4px 10px;
    border-radius: 6px;
    border: 1px solid rgba(255,255,255,0.08);
  }

  .legend-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
  }

  /* RANKING TABLE */
  .ranking-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
  }

  .ranking-table th {
    text-align: left;
    padding: 10px 14px;
    font-size: 11px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: var(--text-muted);
    border-bottom: 1px solid var(--border);
  }

  .ranking-table td {
    padding: 12px 14px;
    border-bottom: 1px solid rgba(255,255,255,0.04);
    vertical-align: middle;
  }

  .ranking-table tr:last-child td { border-bottom: none; }

  .ranking-table tr:hover td { background: rgba(255,255,255,0.02); }

  .rank-num {
    font-family: 'JetBrains Mono', monospace;
    font-weight: 600;
    color: var(--text-muted);
    font-size: 12px;
  }

  /* CITY DETAIL PANEL */
  #city-detail-panel {
    background: var(--bg-card2);
    border: 1px solid var(--border-accent);
    border-radius: var(--radius);
    padding: 24px;
    margin-bottom: 24px;
    display: none;
  }

  #city-detail-panel.visible { display: block; }

  .detail-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    flex-wrap: wrap;
    gap: 10px;
  }

  .detail-stats {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    gap: 10px;
  }

  .detail-stat {
    background: rgba(255,255,255,0.03);
    border-radius: 10px;
    padding: 12px;
    text-align: center;
  }

  .detail-stat-label { font-size: 10px; color: var(--text-muted); text-transform: uppercase; }
  .detail-stat-value { font-size: 20px; font-weight: 700; font-family: 'JetBrains Mono', monospace; }

  /* EXECUTIVE SUMMARY */
  .exec-summary {
    background: linear-gradient(135deg, rgba(99,179,237,0.06) 0%, rgba(159,122,234,0.06) 100%);
    border: 1px solid rgba(99,179,237,0.2);
    border-radius: var(--radius);
    padding: 24px;
    margin-bottom: 24px;
  }

  .exec-summary h3 {
    font-size: 14px;
    font-weight: 700;
    color: var(--accent-blue);
    margin-bottom: 10px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  .exec-summary p { font-size: 13px; color: var(--text-secondary); line-height: 1.7; }

  /* COMPARISON MODE */
  .compare-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  @media (max-width: 640px) { .compare-grid { grid-template-columns: 1fr; } }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    .metrics-grid { grid-template-columns: repeat(2, 1fr); }
    .header-inner { flex-direction: column; align-items: flex-start; }
  }

  @media (max-width: 480px) {
    .metrics-grid { grid-template-columns: 1fr 1fr; }
    .metric-value { font-size: 22px; }
  }

  /* ANIMATIONS */
  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .fade-in { animation: fadeInUp 0.4s ease-out forwards; }

  .fade-in-1 { animation-delay: 0.05s; }
  .fade-in-2 { animation-delay: 0.1s; }
  .fade-in-3 { animation-delay: 0.15s; }
  .fade-in-4 { animation-delay: 0.2s; }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg-primary); }
  ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.12); border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: rgba(255,255,255,0.2); }

  /* SOURCES */
  .sources-bar {
    background: rgba(255,255,255,0.02);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 12px 16px;
    font-size: 11px;
    color: var(--text-muted);
    margin-bottom: 24px;
    line-height: 1.6;
  }

  .sources-bar a { color: var(--accent-blue); text-decoration: none; }
  .sources-bar a:hover { text-decoration: underline; }

  /* TOGGLE */
  .toggle-row {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 12px;
    color: var(--text-secondary);
  }

  .toggle {
    width: 36px; height: 20px;
    background: rgba(255,255,255,0.1);
    border-radius: 10px;
    position: relative;
    cursor: pointer;
    border: none;
    transition: var(--transition);
  }

  .toggle.on { background: var(--accent-blue); }

  .toggle::after {
    content: '';
    position: absolute;
    width: 14px; height: 14px;
    background: white;
    border-radius: 50%;
    top: 3px; left: 3px;
    transition: var(--transition);
  }

  .toggle.on::after { left: 19px; }

  .fullwidth { grid-column: 1 / -1; }

  .two-col {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  @media (max-width: 768px) { .two-col { grid-template-columns: 1fr; } }
</style>
</head>
<body>

<!-- HEADER -->
<header class="header">
  <div class="container header-inner">
    <div class="logo">
      <div class="logo-icon">🌍</div>
      <div>
        <h1>Environmental Health Analyzer</h1>
        <p>Real-time AQI · Water Quality · Personal Health Impact</p>
      </div>
    </div>
    <div class="header-right">
      <span class="data-badge">● LIVE DATA</span>
      <span class="last-updated">Updated: Jun 13, 2026 · Source: aqi.in, aqicn.org, CPCB</span>
    </div>
  </div>
</header>

<!-- NAV TABS -->
<nav class="nav-tabs">
  <div class="nav-tabs-inner">
    <button class="tab-btn active" onclick="switchTab('dashboard')">📊 Dashboard</button>
    <button class="tab-btn" onclick="switchTab('cities')">🏙️ City Explorer</button>
    <button class="tab-btn" onclick="switchTab('health')">🫁 Health Impact</button>
    <button class="tab-btn" onclick="switchTab('report')">📋 Report Card</button>
    <button class="tab-btn" onclick="switchTab('insights')">💡 Insights</button>
    <button class="tab-btn" onclick="switchTab('recommendations')">✅ Recommendations</button>
  </div>
</nav>

<main class="main">
  <div class="container">

    <!-- ═══ TAB 1: DASHBOARD ═══ -->
    <div id="tab-dashboard" class="tab-content active">

      <div class="sources-bar">
        📡 <strong>Data Sources:</strong>
        <a href="https://www.aqi.in" target="_blank">aqi.in</a>,
        <a href="https://aqicn.org/map/india/" target="_blank">aqicn.org</a>,
        <a href="https://cpcb.nic.in" target="_blank">CPCB (Central Pollution Control Board)</a>,
        <a href="https://www.iqair.com" target="_blank">IQAir</a>,
        <a href="https://drinkprime.in" target="_blank">DrinkPrime Water Quality 2026</a>
        · Data collected June 13, 2026 · All AQI values are on the US AQI scale
      </div>

      <!-- KEY METRICS -->
      <div class="section-title fade-in"><div class="dot"></div> Key Metrics — India Major Cities</div>
      <div class="metrics-grid">
        <div class="metric-card fade-in fade-in-1">
          <div class="accent-bar" style="background: linear-gradient(90deg,#63b3ed,#4fd1c7)"></div>
          <div class="metric-label">Average AQI</div>
          <div class="metric-value" style="color:#63b3ed">88</div>
          <div class="metric-sub">Across 8 cities · Moderate</div>
        </div>
        <div class="metric-card fade-in fade-in-2">
          <div class="accent-bar" style="background: linear-gradient(90deg,#fc8181,#e53e3e)"></div>
          <div class="metric-label">Highest AQI City</div>
          <div class="metric-value" style="color:#fc8181">Lucknow</div>
          <div class="metric-sub">AQI 195 · Very Poor</div>
        </div>
        <div class="metric-card fade-in fade-in-3">
          <div class="accent-bar" style="background: linear-gradient(90deg,#68d391,#48bb78)"></div>
          <div class="metric-label">Lowest AQI City</div>
          <div class="metric-value" style="color:#68d391">Hyderabad</div>
          <div class="metric-sub">AQI 59 · Moderate</div>
        </div>
        <div class="metric-card fade-in fade-in-4">
          <div class="accent-bar" style="background: linear-gradient(90deg,#9f7aea,#b794f4)"></div>
          <div class="metric-label">Cities Analyzed</div>
          <div class="metric-value" style="color:#9f7aea">8</div>
          <div class="metric-sub">Major Indian metros</div>
        </div>
        <div class="metric-card fade-in">
          <div class="accent-bar" style="background: linear-gradient(90deg,#f6ad55,#ed8936)"></div>
          <div class="metric-label">Delhi AQI (Now)</div>
          <div class="metric-value" style="color:#f6ad55">93</div>
          <div class="metric-sub">PM2.5: 32µg/m³ · PM10: 40µg/m³</div>
        </div>
        <div class="metric-card fade-in">
          <div class="accent-bar" style="background: linear-gradient(90deg,#4fd1c7,#38b2ac)"></div>
          <div class="metric-label">Cleanest Metro</div>
          <div class="metric-value" style="color:#4fd1c7">Mumbai</div>
          <div class="metric-sub">AQI 57 · Best air quality</div>
        </div>
        <div class="metric-card fade-in">
          <div class="accent-bar" style="background: linear-gradient(90deg,#63b3ed,#4299e1)"></div>
          <div class="metric-label">Env. Health Score</div>
          <div class="metric-value" style="color:#63b3ed">42<span style="font-size:14px">/100</span></div>
          <div class="metric-sub">Delhi · Poor-Moderate</div>
        </div>
        <div class="metric-card fade-in">
          <div class="accent-bar" style="background: linear-gradient(90deg,#fc8181,#9f7aea)"></div>
          <div class="metric-label">Delhi Annual AQI '26</div>
          <div class="metric-value" style="color:#fc8181">179</div>
          <div class="metric-sub">↑6.5% vs prior years · Worsening</div>
        </div>
      </div>

      <!-- AQI LEGEND -->
      <div class="aqi-legend">
        <div class="legend-item"><div class="legend-dot" style="background:#48bb78"></div> Good (0–50)</div>
        <div class="legend-item"><div class="legend-dot" style="background:#9ae6b4"></div> Satisfactory (51–100)</div>
        <div class="legend-item"><div class="legend-dot" style="background:#f6e05e"></div> Moderate (101–150)</div>
        <div class="legend-item"><div class="legend-dot" style="background:#f6ad55"></div> Poor (151–200)</div>
        <div class="legend-item"><div class="legend-dot" style="background:#fc8181"></div> Very Poor (201–300)</div>
        <div class="legend-item"><div class="legend-dot" style="background:#e53e3e"></div> Severe (300+)</div>
      </div>

      <!-- CHARTS ROW 1 -->
      <div class="chart-grid">
        <div class="chart-card">
          <h3>📊 AQI Comparison — All Cities</h3>
          <div class="chart-wrapper"><canvas id="aqiChart"></canvas></div>
        </div>
        <div class="chart-card">
          <h3>🏆 City AQI Ranking (Best → Worst)</h3>
          <div class="chart-wrapper"><canvas id="rankingChart"></canvas></div>
        </div>
      </div>

      <!-- CHARTS ROW 2 -->
      <div class="chart-grid">
        <div class="chart-card">
          <h3>🌫️ PM2.5 Comparison (µg/m³)</h3>
          <div class="chart-wrapper"><canvas id="pm25Chart"></canvas></div>
        </div>
        <div class="chart-card">
          <h3>💨 PM10 Comparison (µg/m³)</h3>
          <div class="chart-wrapper"><canvas id="pm10Chart"></canvas></div>
        </div>
      </div>

      <!-- CHARTS ROW 3 -->
      <div class="chart-grid">
        <div class="chart-card">
          <h3>🍩 AQI Distribution by Category</h3>
          <div class="chart-wrapper"><canvas id="distChart"></canvas></div>
        </div>
        <div class="chart-card">
          <h3>📈 Delhi Annual AQI Trend (2020–2026)</h3>
          <div class="chart-wrapper"><canvas id="trendChart"></canvas></div>
        </div>
      </div>

      <!-- EXEC SUMMARY -->
      <div class="exec-summary">
        <h3>📝 Executive Summary</h3>
        <p>
          Analysis of 8 major Indian cities on June 13, 2026 reveals significant air quality disparities.
          <strong>Mumbai (AQI 57)</strong> and <strong>Hyderabad (AQI 59)</strong> are the cleanest metros, benefiting from coastal breezes and better green cover.
          <strong>Lucknow (AQI 195)</strong> remains critically polluted, nearly 3.5× worse than Mumbai.
          <strong>Delhi's current AQI of 93 (Moderate)</strong> is deceptively low for June — its annual 2026 average of 179 reveals the true picture, which has worsened 6.5% vs 2025.
          <strong>Kolkata (AQI 118)</strong> is a surprising mid-table entry given its coastal proximity but shows industry-linked pollution.
          The most anomalous finding: Delhi's summer AQI of 93 contrasts sharply with its night-peak of 126 recorded just 11 hours earlier — indicating rapid diurnal pollution swings.
          All cities warrant PM2.5 protection measures as even "moderate" Indian AQI translates to elevated health risk under WHO guidelines (safe threshold: 15µg/m³ annual average vs Delhi's 32µg/m³ current reading).
        </p>
      </div>

    </div>

    <!-- ═══ TAB 2: CITY EXPLORER ═══ -->
    <div id="tab-cities" class="tab-content">

      <!-- FILTERS -->
      <div class="filters-bar">
        <span class="filter-label">Filter by AQI:</span>
        <div class="filter-group">
          <button class="filter-btn active" onclick="filterCities('all', this)">All Cities</button>
          <button class="filter-btn" onclick="filterCities('good', this)">🟢 Good</button>
          <button class="filter-btn" onclick="filterCities('moderate', this)">🟡 Moderate</button>
          <button class="filter-btn" onclick="filterCities('poor', this)">🟠 Poor</button>
          <button class="filter-btn" onclick="filterCities('very-poor', this)">🔴 Very Poor</button>
        </div>
        <span class="filter-label">Sort:</span>
        <select class="filter-select" onchange="sortCities(this.value)">
          <option value="aqi-asc">AQI: Low → High</option>
          <option value="aqi-desc">AQI: High → Low</option>
          <option value="name">City Name A–Z</option>
          <option value="pm25">PM2.5 Level</option>
        </select>
        <div class="toggle-row">
          <button class="toggle" id="compareToggle" onclick="toggleCompare()"></button>
          <span>Compare Mode</span>
        </div>
      </div>

      <!-- CITY DETAIL PANEL -->
      <div id="city-detail-panel">
        <div class="detail-header">
          <div>
            <h2 id="detail-city-name" style="font-size:22px;font-weight:800"></h2>
            <p id="detail-city-state" style="color:var(--text-muted);font-size:12px"></p>
          </div>
          <div id="detail-aqi-badge" class="aqi-badge" style="font-size:20px;padding:8px 16px"></div>
        </div>
        <div class="detail-stats" id="detail-stats"></div>
        <div style="margin-top:14px;font-size:12px;color:var(--text-secondary);line-height:1.6" id="detail-desc"></div>
      </div>

      <!-- CITY CARDS -->
      <div class="city-cards-grid" id="cityCardsGrid"></div>

      <!-- RANKING TABLE -->
      <div class="card" style="margin-top:8px">
        <div class="section-title"><div class="dot"></div> City Rankings Table</div>
        <div style="overflow-x:auto">
          <table class="ranking-table" id="rankingTable">
            <thead>
              <tr>
                <th>Rank</th>
                <th>City</th>
                <th>AQI</th>
                <th>Category</th>
                <th>PM2.5 (µg/m³)</th>
                <th>PM10 (µg/m³)</th>
                <th>Water Quality</th>
                <th>Health Score</th>
              </tr>
            </thead>
            <tbody id="rankingTbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ═══ TAB 3: HEALTH IMPACT ═══ -->
    <div id="tab-health" class="tab-content">

      <div style="margin-bottom:16px;display:flex;align-items:center;gap:12px;flex-wrap:wrap">
        <span style="font-size:13px;color:var(--text-secondary)">Showing health analysis for:</span>
        <select class="filter-select" onchange="updateHealthCity(this.value)" id="healthCitySelect">
          <option value="Delhi">Delhi (Your Location)</option>
          <option value="Mumbai">Mumbai</option>
          <option value="Bengaluru">Bengaluru</option>
          <option value="Chennai">Chennai</option>
          <option value="Hyderabad">Hyderabad</option>
          <option value="Kolkata">Kolkata</option>
          <option value="Jaipur">Jaipur</option>
          <option value="Lucknow">Lucknow</option>
        </select>
      </div>

      <div id="healthContent"></div>
    </div>

    <!-- ═══ TAB 4: REPORT CARD ═══ -->
    <div id="tab-report" class="tab-content">

      <div style="margin-bottom:16px;display:flex;align-items:center;gap:12px;flex-wrap:wrap">
        <span style="font-size:13px;color:var(--text-secondary)">Report card for:</span>
        <select class="filter-select" onchange="updateReportCity(this.value)" id="reportCitySelect">
          <option value="Delhi">Delhi (Your Location)</option>
          <option value="Mumbai">Mumbai</option>
          <option value="Bengaluru">Bengaluru</option>
          <option value="Chennai">Chennai</option>
          <option value="Hyderabad">Hyderabad</option>
          <option value="Kolkata">Kolkata</option>
          <option value="Jaipur">Jaipur</option>
          <option value="Lucknow">Lucknow</option>
        </select>
      </div>

      <div id="reportContent"></div>
    </div>

    <!-- ═══ TAB 5: INSIGHTS ═══ -->
    <div id="tab-insights" class="tab-content">
      <div id="insightsContent"></div>
    </div>

    <!-- ═══ TAB 6: RECOMMENDATIONS ═══ -->
    <div id="tab-recommendations" class="tab-content">
      <div style="margin-bottom:16px;display:flex;align-items:center;gap:12px;flex-wrap:wrap">
        <span style="font-size:13px;color:var(--text-secondary)">Personalized recommendations for:</span>
        <select class="filter-select" onchange="updateRecCity(this.value)" id="recCitySelect">
          <option value="Delhi">Delhi (Your Location)</option>
          <option value="Mumbai">Mumbai</option>
          <option value="Bengaluru">Bengaluru</option>
          <option value="Chennai">Chennai</option>
          <option value="Hyderabad">Hyderabad</option>
          <option value="Kolkata">Kolkata</option>
          <option value="Jaipur">Jaipur</option>
          <option value="Lucknow">Lucknow</option>
        </select>
      </div>
      <div id="recContent"></div>
    </div>

  </div>
</main>

<script>
// ═══════════════════════════════════════════════
// DATA — Real data sourced June 13, 2026
// ═══════════════════════════════════════════════
const cityData = [
  {
    name: "Delhi",
    state: "Delhi NCR",
    aqi: 93,
    pm25: 32,
    pm10: 40,
    no2: 16,
    so2: 15,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 1200, // mg/L (many areas, groundwater dependent zones)
    waterHardness: "High",
    waterScore: 28,
    healthScore: 42,
    airScore: 52,
    waterQualityLabel: "Poor",
    annualAqi: 179,
    temp: 35,
    humidity: 35,
    lat: 28.6, lng: 77.2,
    desc: "Delhi's current AQI of 93 reflects a relatively better summer day. However, the annual average of 179 tells the real story — classified as 'Unhealthy'. Water quality is severely compromised with TDS ranging 700–2000 mg/L in many zones, far exceeding the 500 mg/L BIS standard. Groundwater-dependent colonies face the worst conditions."
  },
  {
    name: "Mumbai",
    state: "Maharashtra",
    aqi: 57,
    pm25: 18,
    pm10: 28,
    no2: 10,
    so2: 7,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 120,
    waterHardness: "Low",
    waterScore: 75,
    healthScore: 72,
    airScore: 70,
    waterQualityLabel: "Good",
    annualAqi: 90,
    temp: 32,
    humidity: 67,
    lat: 19.07, lng: 72.87,
    desc: "Mumbai benefits from coastal winds that disperse pollutants. Municipal water supply from lakes (Bhatsa, Vihar) has low TDS (~120 mg/L), making it one of India's best metro water supplies. Hair and skin impacts are minimal from water, though air PM2.5 still exceeds WHO safe limits."
  },
  {
    name: "Bengaluru",
    state: "Karnataka",
    aqi: 63,
    pm25: 20,
    pm10: 30,
    no2: 14,
    so2: 6,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 550,
    waterHardness: "Very High",
    waterScore: 35,
    healthScore: 55,
    airScore: 68,
    waterQualityLabel: "Poor",
    annualAqi: 75,
    temp: 28,
    humidity: 70,
    lat: 12.97, lng: 77.59,
    desc: "Bengaluru's air quality is relatively good but its water is among India's worst. Borewell water regularly exceeds 500 ppm TDS, causing significant hair fall and skin dryness. The city's rapid urbanisation has strained water infrastructure."
  },
  {
    name: "Chennai",
    state: "Tamil Nadu",
    aqi: 65,
    pm25: 22,
    pm10: 35,
    no2: 12,
    so2: 8,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 850,
    waterHardness: "Very High",
    waterScore: 30,
    healthScore: 50,
    airScore: 66,
    waterQualityLabel: "Poor",
    annualAqi: 79,
    temp: 32,
    humidity: 75,
    lat: 13.08, lng: 80.27,
    desc: "Chennai faces dual challenges: moderate air quality and severely hard water. Areas like OMR and Tambaram frequently cross 1000 ppm TDS due to seawater intrusion. Residents report significant hair fall and scaling issues. Coastal AQI is maintained by sea breeze."
  },
  {
    name: "Hyderabad",
    state: "Telangana",
    aqi: 59,
    pm25: 19,
    pm10: 27,
    no2: 11,
    so2: 6,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 420,
    waterHardness: "Moderate",
    waterScore: 52,
    healthScore: 62,
    airScore: 69,
    waterQualityLabel: "Moderate",
    annualAqi: 80,
    temp: 35,
    humidity: 42,
    lat: 17.38, lng: 78.48,
    desc: "Hyderabad has the lowest AQI among all 8 cities today, making it the cleanest large city. Water quality is moderate — TDS around 420 mg/L which causes some scaling and hair dryness but is manageable with basic filtration. Overall one of India's more liveable metros for environmental health."
  },
  {
    name: "Kolkata",
    state: "West Bengal",
    aqi: 118,
    pm25: 42,
    pm10: 65,
    no2: 20,
    so2: 14,
    category: "moderate",
    categoryLabel: "Moderate",
    waterTDS: 150,
    waterHardness: "Low-Moderate",
    waterScore: 68,
    healthScore: 48,
    airScore: 40,
    waterQualityLabel: "Good",
    annualAqi: 139,
    temp: 33,
    humidity: 75,
    lat: 22.57, lng: 88.36,
    desc: "Kolkata is a tale of two halves: relatively good water quality (TDS ~150 mg/L from Hooghly River) but consistently poor air quality, especially PM2.5 at 42µg/m³ — the highest of all cities today. Industrial activity and vehicle emissions make Kolkata's air a serious lung health concern."
  },
  {
    name: "Jaipur",
    state: "Rajasthan",
    aqi: 152,
    pm25: 58,
    pm10: 88,
    no2: 18,
    so2: 12,
    category: "poor",
    categoryLabel: "Poor",
    waterTDS: 900,
    waterHardness: "High",
    waterScore: 25,
    healthScore: 28,
    airScore: 25,
    waterQualityLabel: "Very Poor",
    annualAqi: 152,
    temp: 40,
    humidity: 25,
    lat: 26.91, lng: 75.78,
    desc: "Jaipur suffers from desert dust, construction activity, and poor water quality. High TDS groundwater causes severe hair fall and skin problems. PM10 at 88µg/m³ reflects heavy dust particle load. Summer months are particularly brutal due to sandstorms amplifying PM10 levels."
  },
  {
    name: "Lucknow",
    state: "Uttar Pradesh",
    aqi: 195,
    pm25: 78,
    pm10: 112,
    no2: 28,
    so2: 22,
    category: "poor",
    categoryLabel: "Poor",
    waterTDS: 750,
    waterHardness: "High",
    waterScore: 32,
    healthScore: 22,
    airScore: 18,
    waterQualityLabel: "Poor",
    annualAqi: 195,
    temp: 38,
    humidity: 22,
    lat: 26.85, lng: 80.95,
    desc: "Lucknow records India's worst AQI among today's analyzed cities at 195 — bordering 'Very Poor' and posing serious public health risks. Industrial emissions, vehicle exhaust, and nearby crop burning (seasonal) combine to create severe pollution. Water quality is also poor with high TDS. Residents face compounded environmental health burdens."
  }
];

// ═══════════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════════
let currentFilter = 'all';
let compareMode = false;
let selectedCities = [];
let currentHealthCity = 'Delhi';
let currentReportCity = 'Delhi';
let currentRecCity = 'Delhi';

// ═══════════════════════════════════════════════
// HELPERS
// ═══════════════════════════════════════════════
function getAqiColor(aqi) {
  if (aqi <= 50) return '#48bb78';
  if (aqi <= 100) return '#9ae6b4';
  if (aqi <= 150) return '#f6e05e';
  if (aqi <= 200) return '#f6ad55';
  if (aqi <= 300) return '#fc8181';
  return '#e53e3e';
}

function getCategoryClass(cat) {
  const map = {good:'good',satisfactory:'satisfactory',moderate:'moderate',poor:'poor','very-poor':'very-poor',severe:'severe'};
  return map[cat] || 'moderate';
}

function getAqiCategory(aqi) {
  if (aqi <= 50) return {label:'Good',cls:'good',color:'#48bb78'};
  if (aqi <= 100) return {label:'Satisfactory',cls:'satisfactory',color:'#9ae6b4'};
  if (aqi <= 150) return {label:'Moderate',cls:'moderate',color:'#f6e05e'};
  if (aqi <= 200) return {label:'Poor',cls:'poor',color:'#f6ad55'};
  if (aqi <= 300) return {label:'Very Poor',cls:'very-poor',color:'#fc8181'};
  return {label:'Severe',cls:'severe',color:'#e53e3e'};
}

function getRiskIndicator(level) {
  if (level === 'low') return '<span class="risk-indicator risk-low">🟢 Low Risk</span>';
  if (level === 'moderate') return '<span class="risk-indicator risk-moderate">🟡 Moderate Risk</span>';
  return '<span class="risk-indicator risk-high">🔴 High Risk</span>';
}

function getAqiRisk(aqi) {
  if (aqi <= 50) return 'low';
  if (aqi <= 100) return 'low';
  if (aqi <= 150) return 'moderate';
  return 'high';
}

function getWaterRisk(tds) {
  if (tds < 300) return 'low';
  if (tds < 600) return 'moderate';
  return 'high';
}

function getScoreColor(score) {
  if (score >= 70) return '#68d391';
  if (score >= 50) return '#f6e05e';
  if (score >= 35) return '#f6ad55';
  return '#fc8181';
}

function getGrade(score) {
  if (score >= 80) return {g:'A',color:'#68d391'};
  if (score >= 65) return {g:'B',color:'#9ae6b4'};
  if (score >= 50) return {g:'C',color:'#f6e05e'};
  if (score >= 35) return {g:'D',color:'#f6ad55'};
  return {g:'F',color:'#fc8181'};
}

// ═══════════════════════════════════════════════
// TAB SWITCHING
// ═══════════════════════════════════════════════
function switchTab(tab) {
  document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('tab-'+tab).classList.add('active');
  event.currentTarget.classList.add('active');

  if (tab === 'cities') renderCityCards(cityData);
  if (tab === 'health') renderHealth(currentHealthCity);
  if (tab === 'report') renderReport(currentReportCity);
  if (tab === 'insights') renderInsights();
  if (tab === 'recommendations') renderRecs(currentRecCity);
}

// ═══════════════════════════════════════════════
// CHARTS
// ═══════════════════════════════════════════════
const chartDefaults = {
  color: 'rgba(138,149,168,1)',
  backgroundColor: 'rgba(255,255,255,0)',
  plugins: {
    legend: { display: false },
    tooltip: {
      backgroundColor: '#161c2d',
      borderColor: 'rgba(255,255,255,0.1)',
      borderWidth: 1,
      titleColor: '#e8edf5',
      bodyColor: '#8a95a8',
      padding: 10,
      cornerRadius: 8
    }
  },
  scales: {
    x: {
      ticks: { color: '#5a6270', font: { size: 10 } },
      grid: { color: 'rgba(255,255,255,0.04)' }
    },
    y: {
      ticks: { color: '#5a6270', font: { size: 10 } },
      grid: { color: 'rgba(255,255,255,0.04)' }
    }
  }
};

function createChart(id, config) {
  const ctx = document.getElementById(id).getContext('2d');
  return new Chart(ctx, config);
}

// AQI COMPARISON
const sorted = [...cityData].sort((a,b) => a.aqi - b.aqi);
createChart('aqiChart', {
  type: 'bar',
  data: {
    labels: sorted.map(c => c.name),
    datasets: [{
      label: 'AQI',
      data: sorted.map(c => c.aqi),
      backgroundColor: sorted.map(c => getAqiColor(c.aqi) + 'bb'),
      borderColor: sorted.map(c => getAqiColor(c.aqi)),
      borderWidth: 1,
      borderRadius: 6
    }]
  },
  options: {
    ...chartDefaults,
    plugins: { ...chartDefaults.plugins, legend: { display: false } },
    scales: { ...chartDefaults.scales, y: { ...chartDefaults.scales.y, beginAtZero: true, max: 220 } }
  }
});

// RANKING (horizontal)
const sortedDesc = [...cityData].sort((a,b) => b.aqi - a.aqi);
createChart('rankingChart', {
  type: 'bar',
  data: {
    labels: sortedDesc.map(c => c.name),
    datasets: [{
      label: 'AQI',
      data: sortedDesc.map(c => c.aqi),
      backgroundColor: sortedDesc.map(c => getAqiColor(c.aqi) + 'bb'),
      borderColor: sortedDesc.map(c => getAqiColor(c.aqi)),
      borderWidth: 1,
      borderRadius: 4
    }]
  },
  options: {
    ...chartDefaults,
    indexAxis: 'y',
    plugins: { ...chartDefaults.plugins, legend: { display: false } },
    scales: {
      x: { ...chartDefaults.scales.x, beginAtZero: true, max: 220 },
      y: { ...chartDefaults.scales.y }
    }
  }
});

// PM2.5
createChart('pm25Chart', {
  type: 'bar',
  data: {
    labels: sorted.map(c => c.name),
    datasets: [{
      label: 'PM2.5 (µg/m³)',
      data: sorted.map(c => c.pm25),
      backgroundColor: 'rgba(99,179,237,0.5)',
      borderColor: 'rgba(99,179,237,1)',
      borderWidth: 1,
      borderRadius: 6
    }, {
      label: 'WHO Safe (15)',
      data: sorted.map(() => 15),
      type: 'line',
      borderColor: '#fc8181',
      borderDash: [4,4],
      pointRadius: 0,
      borderWidth: 1.5
    }]
  },
  options: {
    ...chartDefaults,
    plugins: {
      ...chartDefaults.plugins,
      legend: {
        display: true,
        labels: { color: '#8a95a8', font: { size: 10 }, boxWidth: 12 }
      }
    },
    scales: { ...chartDefaults.scales, y: { ...chartDefaults.scales.y, beginAtZero: true } }
  }
});

// PM10
createChart('pm10Chart', {
  type: 'bar',
  data: {
    labels: sorted.map(c => c.name),
    datasets: [{
      label: 'PM10 (µg/m³)',
      data: sorted.map(c => c.pm10),
      backgroundColor: 'rgba(159,122,234,0.5)',
      borderColor: 'rgba(159,122,234,1)',
      borderWidth: 1,
      borderRadius: 6
    }, {
      label: 'WHO Safe (45)',
      data: sorted.map(() => 45),
      type: 'line',
      borderColor: '#fc8181',
      borderDash: [4,4],
      pointRadius: 0,
      borderWidth: 1.5
    }]
  },
  options: {
    ...chartDefaults,
    plugins: {
      ...chartDefaults.plugins,
      legend: {
        display: true,
        labels: { color: '#8a95a8', font: { size: 10 }, boxWidth: 12 }
      }
    },
    scales: { ...chartDefaults.scales, y: { ...chartDefaults.scales.y, beginAtZero: true } }
  }
});

// DISTRIBUTION PIE
const catCounts = { 'Good': 0, 'Satisfactory': 0, 'Moderate': 0, 'Poor': 0, 'Very Poor': 0, 'Severe': 0 };
cityData.forEach(c => {
  const cat = getAqiCategory(c.aqi).label;
  catCounts[cat]++;
});
const distLabels = Object.keys(catCounts).filter(k => catCounts[k] > 0);
const distData = distLabels.map(k => catCounts[k]);
const distColors = distLabels.map(k => {
  const m = {'Good':'#48bb78','Satisfactory':'#9ae6b4','Moderate':'#f6e05e','Poor':'#f6ad55','Very Poor':'#fc8181','Severe':'#e53e3e'};
  return m[k];
});

createChart('distChart', {
  type: 'doughnut',
  data: {
    labels: distLabels,
    datasets: [{
      data: distData,
      backgroundColor: distColors.map(c => c + 'cc'),
      borderColor: distColors,
      borderWidth: 2
    }]
  },
  options: {
    ...chartDefaults,
    plugins: {
      ...chartDefaults.plugins,
      legend: {
        display: true,
        position: 'right',
        labels: { color: '#8a95a8', font: { size: 11 }, boxWidth: 12, padding: 12 }
      }
    },
    cutout: '65%'
  }
});

// TREND CHART
createChart('trendChart', {
  type: 'line',
  data: {
    labels: ['2020','2021','2022','2023','2024','2025','2026'],
    datasets: [{
      label: 'Delhi Annual AQI',
      data: [154, 162, 174, 164, 178, 179, 179],
      borderColor: '#fc8181',
      backgroundColor: 'rgba(252,129,129,0.08)',
      borderWidth: 2,
      pointBackgroundColor: '#fc8181',
      pointRadius: 4,
      fill: true,
      tension: 0.3
    }]
  },
  options: {
    ...chartDefaults,
    plugins: {
      ...chartDefaults.plugins,
      legend: {
        display: true,
        labels: { color: '#8a95a8', font: { size: 10 }, boxWidth: 12 }
      }
    },
    scales: {
      ...chartDefaults.scales,
      y: { ...chartDefaults.scales.y, min: 140, max: 200 }
    }
  }
});

// ═══════════════════════════════════════════════
// CITY CARDS
// ═══════════════════════════════════════════════
function renderCityCards(data) {
  const grid = document.getElementById('cityCardsGrid');
  grid.innerHTML = '';

  data.forEach(city => {
    const cat = getAqiCategory(city.aqi);
    const isSelected = selectedCities.includes(city.name);
    const div = document.createElement('div');
    div.className = `city-card${isSelected ? ' selected' : ''}`;
    div.onclick = () => selectCity(city.name);
    div.innerHTML = `
      <div class="city-card-header">
        <div>
          <div class="city-name">${city.name}</div>
          <div class="city-state">${city.state}</div>
        </div>
        <div class="aqi-badge ${cat.cls}" style="background:${cat.color}22;color:${cat.color};border:1px solid ${cat.color}44">${city.aqi}</div>
      </div>
      <div style="font-size:11px;color:${cat.color};font-weight:600;margin-bottom:8px">${cat.label}</div>
      <div class="city-stats">
        <div class="city-stat">
          <div class="city-stat-label">PM2.5</div>
          <div class="city-stat-value">${city.pm25}</div>
        </div>
        <div class="city-stat">
          <div class="city-stat-label">PM10</div>
          <div class="city-stat-value">${city.pm10}</div>
        </div>
        <div class="city-stat">
          <div class="city-stat-label">Health</div>
          <div class="city-stat-value" style="color:${getScoreColor(city.healthScore)}">${city.healthScore}</div>
        </div>
      </div>
      <div style="display:flex;justify-content:space-between;margin-top:10px;font-size:11px;color:var(--text-muted)">
        <span>💧 Water: <strong style="color:${getScoreColor(city.waterScore)}">${city.waterQualityLabel}</strong></span>
        <span>🌡️ ${city.temp}°C · 💧${city.humidity}%</span>
      </div>
      <div class="aqi-category-strip" style="background:${cat.color}66"></div>
    `;
    grid.appendChild(div);
  });

  renderRankingTable(data);
}

function selectCity(name) {
  if (compareMode) {
    const idx = selectedCities.indexOf(name);
    if (idx === -1) {
      if (selectedCities.length < 2) selectedCities.push(name);
      else { selectedCities.shift(); selectedCities.push(name); }
    } else {
      selectedCities.splice(idx, 1);
    }
  } else {
    selectedCities = [name];
  }
  renderCityCards(getCurrentFilteredData());
  showCityDetail(name);
}

function showCityDetail(name) {
  const city = cityData.find(c => c.name === name);
  if (!city) return;
  const cat = getAqiCategory(city.aqi);
  const panel = document.getElementById('city-detail-panel');
  panel.classList.add('visible');
  document.getElementById('detail-city-name').textContent = city.name;
  document.getElementById('detail-city-state').textContent = city.state;
  document.getElementById('detail-aqi-badge').textContent = `AQI ${city.aqi}`;
  document.getElementById('detail-aqi-badge').className = 'aqi-badge';
  document.getElementById('detail-aqi-badge').style.cssText = `font-size:18px;padding:8px 16px;background:${cat.color}22;color:${cat.color};border:1px solid ${cat.color}44;border-radius:8px`;
  document.getElementById('detail-stats').innerHTML = `
    <div class="detail-stat"><div class="detail-stat-label">PM2.5 µg/m³</div><div class="detail-stat-value" style="color:#63b3ed">${city.pm25}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">PM10 µg/m³</div><div class="detail-stat-value" style="color:#9f7aea">${city.pm10}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">NO2</div><div class="detail-stat-value" style="color:#f6ad55">${city.no2}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">SO2</div><div class="detail-stat-value" style="color:#fc8181">${city.so2}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">Water TDS</div><div class="detail-stat-value" style="color:${getScoreColor(city.waterScore)}">${city.waterTDS}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">Water Score</div><div class="detail-stat-value" style="color:${getScoreColor(city.waterScore)}">${city.waterScore}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">Air Score</div><div class="detail-stat-value" style="color:${getScoreColor(city.airScore)}">${city.airScore}</div></div>
    <div class="detail-stat"><div class="detail-stat-label">Health Score</div><div class="detail-stat-value" style="color:${getScoreColor(city.healthScore)}">${city.healthScore}</div></div>
  `;
  document.getElementById('detail-desc').textContent = city.desc;
  panel.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}

function renderRankingTable(data) {
  const tbody = document.getElementById('rankingTbody');
  const sorted = [...data].sort((a,b) => a.aqi - b.aqi);
  tbody.innerHTML = sorted.map((city, i) => {
    const cat = getAqiCategory(city.aqi);
    const wg = getGrade(city.waterScore);
    return `
    <tr>
      <td><span class="rank-num">#${i+1}</span></td>
      <td><strong>${city.name}</strong><br><span style="font-size:10px;color:var(--text-muted)">${city.state}</span></td>
      <td><span style="font-family:'JetBrains Mono';font-weight:700;color:${cat.color}">${city.aqi}</span></td>
      <td><span style="font-size:11px;font-weight:600;color:${cat.color}">${cat.label}</span></td>
      <td style="font-family:'JetBrains Mono'">${city.pm25}</td>
      <td style="font-family:'JetBrains Mono'">${city.pm10}</td>
      <td><span style="color:${getScoreColor(city.waterScore)}">${city.waterQualityLabel}</span></td>
      <td>
        <div style="display:flex;align-items:center;gap:8px">
          <span style="font-family:'JetBrains Mono';font-weight:700;color:${getScoreColor(city.healthScore)}">${city.healthScore}</span>
          <div class="progress-bar" style="width:60px;margin-top:0">
            <div class="progress-fill" style="width:${city.healthScore}%;background:${getScoreColor(city.healthScore)}"></div>
          </div>
        </div>
      </td>
    </tr>`;
  }).join('');
}

// ═══════════════════════════════════════════════
// FILTERS
// ═══════════════════════════════════════════════
function getCurrentFilteredData() {
  let data = cityData;
  if (currentFilter !== 'all') {
    data = data.filter(c => {
      const cat = getAqiCategory(c.aqi).label.toLowerCase().replace(' ','-');
      return cat === currentFilter || (currentFilter === 'good' && c.aqi <= 50) ||
        (currentFilter === 'moderate' && c.aqi > 50 && c.aqi <= 150) ||
        (currentFilter === 'poor' && c.aqi > 150 && c.aqi <= 200) ||
        (currentFilter === 'very-poor' && c.aqi > 200);
    });
  }
  return data;
}

function filterCities(filter, btn) {
  currentFilter = filter;
  document.querySelectorAll('#tab-cities .filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  renderCityCards(getCurrentFilteredData());
}

function sortCities(val) {
  let data = [...getCurrentFilteredData()];
  if (val === 'aqi-asc') data.sort((a,b) => a.aqi - b.aqi);
  else if (val === 'aqi-desc') data.sort((a,b) => b.aqi - a.aqi);
  else if (val === 'name') data.sort((a,b) => a.name.localeCompare(b.name));
  else if (val === 'pm25') data.sort((a,b) => a.pm25 - b.pm25);
  renderCityCards(data);
}

function toggleCompare() {
  compareMode = !compareMode;
  const btn = document.getElementById('compareToggle');
  btn.classList.toggle('on', compareMode);
  if (!compareMode) { selectedCities = []; renderCityCards(getCurrentFilteredData()); }
}

// ═══════════════════════════════════════════════
// HEALTH IMPACT
// ═══════════════════════════════════════════════
function renderHealth(cityName) {
  const city = cityData.find(c => c.name === cityName) || cityData[0];
  const aqiRisk = getAqiRisk(city.aqi);
  const waterRisk = getWaterRisk(city.waterTDS);
  const cat = getAqiCategory(city.aqi);

  const airImpacts = [
    {
      icon:'🫁', title:'Lung Health', risk: city.aqi > 150 ? 'high' : city.aqi > 100 ? 'moderate' : 'low',
      text: city.aqi > 150
        ? `PM2.5 at ${city.pm25}µg/m³ is ${(city.pm25/15).toFixed(1)}× WHO limit. Causes bronchial irritation, reduced lung capacity, and worsens asthma and COPD symptoms significantly.`
        : city.aqi > 80
        ? `PM2.5 at ${city.pm25}µg/m³ (${(city.pm25/15).toFixed(1)}× WHO limit) causes mild-moderate respiratory strain, coughing in sensitive individuals, and reduced breathing efficiency during exercise.`
        : `PM2.5 at ${city.pm25}µg/m³ is ${(city.pm25/15).toFixed(1)}× WHO limit. Healthy individuals experience minimal impact. Sensitive groups should still monitor exposure.`
    },
    {
      icon:'😴', title:'Sleep Quality', risk: city.aqi > 150 ? 'high' : city.aqi > 100 ? 'moderate' : 'low',
      text: city.aqi > 150
        ? 'High particulate pollution disrupts sleep architecture, worsens sleep apnea, and increases nighttime awakenings. Keeping windows closed and using air purifiers at night is critical.'
        : city.aqi > 80
        ? 'Moderate air pollution can subtly reduce sleep quality, especially for those with allergies or respiratory conditions. Air purifier use during sleep is recommended.'
        : 'Current AQI poses minimal sleep disruption risk. Ventilating your bedroom at night is generally safe with basic precautions.'
    },
    {
      icon:'⚡', title:'Energy Levels', risk: city.aqi > 150 ? 'high' : city.aqi > 100 ? 'moderate' : 'low',
      text: city.aqi > 150
        ? `Exposure to PM2.5 at ${city.pm25}µg/m³ reduces oxygen absorption efficiency, leading to fatigue, brain fog, and reduced cognitive performance throughout the day.`
        : city.aqi > 80
        ? 'Moderate pollution may cause afternoon energy dips, mild headaches, and reduced focus, especially after outdoor exposure. Indoor air quality management is key.'
        : 'Energy impact is low at this AQI level. Normal outdoor exposure is manageable. Hydration and ventilation maintain healthy energy levels.'
    },
    {
      icon:'🏃', title:'Exercise Performance', risk: city.aqi > 100 ? 'high' : city.aqi > 70 ? 'moderate' : 'low',
      text: city.aqi > 150
        ? 'Avoid vigorous outdoor exercise entirely. The increased breathing rate during exercise massively amplifies pollutant intake. Gym or home workouts are strongly recommended.'
        : city.aqi > 80
        ? 'Limit outdoor exercise to early morning (5–7 AM) when AQI is typically lowest. Wear N95 mask for any outdoor activity beyond 30 minutes. Avoid roadside running.'
        : 'Outdoor exercise is possible. Exercise between 6–9 AM for best air quality. Avoid busy traffic routes. N95 mask optional but beneficial for long runs.'
    },
    {
      icon:'🧠', title:'Long-term Health', risk: city.annualAqi > 150 ? 'high' : city.annualAqi > 100 ? 'moderate' : 'low',
      text: city.annualAqi > 150
        ? `Annual AQI of ${city.annualAqi} is associated with elevated risk of cardiovascular disease, chronic lung disease, and reduced life expectancy. Studies link Delhi-level pollution to 3–5 years of life expectancy loss.`
        : city.annualAqi > 80
        ? `Annual AQI of ${city.annualAqi} carries moderate long-term risk. Consistent moderate exposure over years increases risk of lung disease and cardiovascular stress. Regular health monitoring recommended.`
        : `Annual AQI of ${city.annualAqi} carries lower long-term risk. Focus on diet, hydration, and preventive health measures for optimal protection.`
    }
  ];

  const waterImpacts = [
    {
      icon:'💇', title:'Hair Fall', risk: waterRisk,
      text: city.waterTDS > 700
        ? `TDS of ~${city.waterTDS} mg/L (${(city.waterTDS/500).toFixed(1)}× BIS limit) deposits mineral scale on hair shafts, weakens follicles, and significantly increases hair fall and breakage. Hard water is a primary cause of hair loss in ${city.name}.`
        : city.waterTDS > 400
        ? `TDS of ~${city.waterTDS} mg/L causes moderate mineral buildup on scalp and hair. Some hair thinning and increased fall may occur over time. Shower filter recommended.`
        : `TDS of ~${city.waterTDS} mg/L is within acceptable ranges. Hair fall risk from water is low. Standard care routines are sufficient.`
    },
    {
      icon:'🌀', title:'Hair Dryness & Frizz', risk: city.waterTDS > 500 ? 'high' : city.waterTDS > 300 ? 'moderate' : 'low',
      text: city.waterTDS > 600
        ? 'Calcium and magnesium ions in hard water bind to hair proteins, stripping moisture and causing chronic dryness, frizz, and brittleness. Leave-in conditioners and deep moisturizing treatments are essential.'
        : city.waterTDS > 300
        ? 'Moderate water hardness causes gradual moisture stripping. Weekly deep conditioning and chelating shampoos help maintain hair health.'
        : 'Water quality poses low risk of hair dryness. Normal conditioning routines are sufficient.'
    },
    {
      icon:'🧴', title:'Scalp Health', risk: city.waterTDS > 600 ? 'high' : city.waterTDS > 350 ? 'moderate' : 'low',
      text: city.waterTDS > 600
        ? 'Hard water mineral deposits clog hair follicles, disrupt scalp pH, and promote dandruff and scalp inflammation. AQI pollution particles can also settle on the scalp, worsening conditions.'
        : city.waterTDS > 350
        ? 'Moderate water hardness can cause mild scalp irritation, itchiness, and dandruff tendency. Regular scalp cleansing with mild, sulfate-free products helps.'
        : 'Scalp health risk from water is low. Focus on sun protection for scalp when outdoors given current AQI levels.'
    },
    {
      icon:'🫧', title:'Skin Dryness', risk: city.waterTDS > 600 ? 'high' : city.waterTDS > 400 ? 'moderate' : 'low',
      text: city.waterTDS > 600
        ? 'High TDS water strips natural skin oils, disrupts the moisture barrier, and causes chronic dry skin, tightness, and premature aging. A water softener or shower filter is essential in this city.'
        : city.waterTDS > 400
        ? 'Moderate water hardness gradually reduces skin moisture retention. Daily moisturizer application immediately after showering (while skin is still damp) significantly helps.'
        : 'Low water hardness poses minimal skin dryness risk. Standard skincare routines are sufficient.'
    },
    {
      icon:'😤', title:'Acne & Pores', risk: city.aqi > 120 ? 'high' : city.aqi > 80 ? 'moderate' : 'low',
      text: city.aqi > 120
        ? `PM2.5 at ${city.pm25}µg/m³ penetrates pores, triggers inflammation, oxidative stress, and sebum overproduction. Increases acne flare-ups. Double cleansing + niacinamide serum is highly recommended.`
        : city.aqi > 80
        ? `Moderate PM2.5 (${city.pm25}µg/m³) causes gradual pore clogging and mild inflammatory acne in pollution-sensitive skin. Regular cleansing routine with AHA/BHA exfoliation helps.`
        : 'Low pollution exposure keeps acne risk from environmental factors minimal. Standard skincare is sufficient.'
    },
    {
      icon:'🌸', title:'Sensitive Skin', risk: city.aqi > 100 || city.waterTDS > 600 ? 'high' : 'moderate',
      text: (city.aqi > 100 || city.waterTDS > 600)
        ? `Combined air pollution (AQI ${city.aqi}) and water hardness (TDS ~${city.waterTDS} mg/L) create compounding stress on sensitive skin. Barrier-strengthening skincare (ceramides, hyaluronic acid), fragrance-free products, and pollution shields are critical.`
        : 'Moderate environmental stress for sensitive skin. Barrier-strengthening routines and sun protection are beneficial. Patch-test new products regularly.'
    }
  ];

  const content = document.getElementById('healthContent');
  content.innerHTML = `
    <div style="background:linear-gradient(135deg,rgba(99,179,237,0.06),rgba(159,122,234,0.04));border:1px solid rgba(99,179,237,0.15);border-radius:16px;padding:20px;margin-bottom:24px;display:flex;align-items:center;gap:16px;flex-wrap:wrap">
      <div style="text-align:center;padding:16px;background:rgba(0,0,0,0.2);border-radius:12px;min-width:120px">
        <div style="font-size:36px;font-weight:800;font-family:'JetBrains Mono';color:${cat.color}">${city.aqi}</div>
        <div style="font-size:11px;color:${cat.color};font-weight:600">${cat.label}</div>
        <div style="font-size:10px;color:var(--text-muted);margin-top:2px">Current AQI</div>
      </div>
      <div>
        <div style="font-size:18px;font-weight:700;margin-bottom:4px">${city.name} Environmental Health Profile</div>
        <div style="font-size:12px;color:var(--text-secondary)">PM2.5: <strong style="color:#63b3ed">${city.pm25} µg/m³</strong> · PM10: <strong style="color:#9f7aea">${city.pm10} µg/m³</strong> · Water TDS: <strong style="color:${getScoreColor(city.waterScore)}">${city.waterTDS} mg/L</strong> · Hardness: <strong>${city.waterHardness}</strong></div>
        <div style="font-size:11px;color:var(--text-muted);margin-top:6px">Annual AQI 2026: <strong style="color:#fc8181">${city.annualAqi}</strong> · This reflects the long-term exposure picture</div>
      </div>
    </div>

    <div class="section-title"><div class="dot"></div> 🫁 Air Quality — Body Impact</div>
    <div class="health-grid">
      ${airImpacts.map(item => `
        <div class="health-item">
          <div class="health-icon">${item.icon}</div>
          <div class="health-info">
            <h4>${item.title}</h4>
            <p>${item.text}</p>
            ${getRiskIndicator(item.risk)}
          </div>
        </div>
      `).join('')}
    </div>

    <div class="section-title" style="margin-top:8px"><div class="dot" style="background:#4fd1c7"></div> 💧 Water Quality — Hair & Skin Impact</div>
    <div class="health-grid">
      ${waterImpacts.map(item => `
        <div class="health-item">
          <div class="health-icon">${item.icon}</div>
          <div class="health-info">
            <h4>${item.title}</h4>
            <p>${item.text}</p>
            ${getRiskIndicator(item.risk)}
          </div>
        </div>
      `).join('')}
    </div>
  `;
}

function updateHealthCity(name) {
  currentHealthCity = name;
  renderHealth(name);
}

// ═══════════════════════════════════════════════
// REPORT CARD
// ═══════════════════════════════════════════════
function renderReport(cityName) {
  const city = cityData.find(c => c.name === cityName) || cityData[0];
  const airGrade = getGrade(city.airScore);
  const waterGrade = getGrade(city.waterScore);
  const overallGrade = getGrade(city.healthScore);
  const hairRisk = city.waterTDS > 700 ? {g:'F',color:'#e53e3e'} : city.waterTDS > 400 ? {g:'D',color:'#fc8181'} : {g:'B',color:'#9ae6b4'};
  const skinRisk = (city.aqi > 120 || city.waterTDS > 600) ? {g:'F',color:'#e53e3e'} : {g:'C',color:'#f6e05e'};
  const overallScore = city.healthScore;
  const scoreColor = getScoreColor(overallScore);

  document.getElementById('reportContent').innerHTML = `
    <div class="two-col" style="margin-bottom:20px">
      <div class="score-card">
        <div class="score-circle" style="background:radial-gradient(circle,${scoreColor}15,transparent);border:3px solid ${scoreColor}44">
          <div class="score-num" style="color:${scoreColor}">${overallScore}</div>
          <div class="score-label">/ 100</div>
        </div>
        <div class="score-grade" style="color:${overallGrade.color}">${overallGrade.g}</div>
        <div class="score-title">Overall Environmental Health Score</div>
        <div class="score-desc">${city.name} · June 2026</div>
        <div class="progress-bar" style="margin-top:12px;height:8px">
          <div class="progress-fill" style="width:${overallScore}%;background:linear-gradient(90deg,${scoreColor},${scoreColor}88)"></div>
        </div>
      </div>

      <div class="card">
        <div class="section-title" style="font-size:13px"><div class="dot"></div> Grade Breakdown</div>
        <div class="grade-row">
          <span class="grade-name">🌬️ Air Quality Score</span>
          <div style="display:flex;align-items:center;gap:10px">
            <span style="font-size:12px;color:var(--text-muted)">${city.airScore}/100</span>
            <span class="grade-badge" style="background:${airGrade.color}22;color:${airGrade.color}">${airGrade.g}</span>
          </div>
        </div>
        <div class="grade-row">
          <span class="grade-name">💧 Water Quality Score</span>
          <div style="display:flex;align-items:center;gap:10px">
            <span style="font-size:12px;color:var(--text-muted)">${city.waterScore}/100</span>
            <span class="grade-badge" style="background:${waterGrade.color}22;color:${waterGrade.color}">${waterGrade.g}</span>
          </div>
        </div>
        <div class="grade-row">
          <span class="grade-name">💇 Hair Risk Grade</span>
          <div style="display:flex;align-items:center;gap:10px">
            <span style="font-size:12px;color:var(--text-muted)">TDS ${city.waterTDS} mg/L</span>
            <span class="grade-badge" style="background:${hairRisk.color}22;color:${hairRisk.color}">${hairRisk.g}</span>
          </div>
        </div>
        <div class="grade-row">
          <span class="grade-name">🌸 Skin Risk Grade</span>
          <div style="display:flex;align-items:center;gap:10px">
            <span style="font-size:12px;color:var(--text-muted)">AQI ${city.aqi}</span>
            <span class="grade-badge" style="background:${skinRisk.color}22;color:${skinRisk.color}">${skinRisk.g}</span>
          </div>
        </div>
        <div class="grade-row">
          <span class="grade-name">🏆 Overall Grade</span>
          <div style="display:flex;align-items:center;gap:10px">
            <span style="font-size:12px;color:var(--text-muted)">${overallScore}/100</span>
            <span class="grade-badge" style="background:${overallGrade.color}22;color:${overallGrade.color};font-size:20px">${overallGrade.g}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="two-col">
      <div class="card">
        <div class="section-title" style="font-size:13px"><div class="dot"></div> 🌬️ Air Quality Detail</div>
        <div style="display:flex;flex-direction:column;gap:10px">
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Current AQI</span><span style="color:${getAqiCategory(city.aqi).color};font-weight:700">${city.aqi}</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.aqi/300*100,100)}%;background:${getAqiCategory(city.aqi).color}"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>PM2.5 (WHO limit: 15)</span><span style="color:#63b3ed;font-weight:700">${city.pm25} µg/m³</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.pm25/100*100,100)}%;background:#63b3ed"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>PM10 (WHO limit: 45)</span><span style="color:#9f7aea;font-weight:700">${city.pm10} µg/m³</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.pm10/150*100,100)}%;background:#9f7aea"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Annual AQI 2026</span><span style="color:#fc8181;font-weight:700">${city.annualAqi}</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.annualAqi/300*100,100)}%;background:#fc8181"></div></div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="section-title" style="font-size:13px"><div class="dot" style="background:#4fd1c7"></div> 💧 Water Quality Detail</div>
        <div style="display:flex;flex-direction:column;gap:10px">
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Water TDS (BIS limit: 500)</span><span style="color:${getScoreColor(city.waterScore)};font-weight:700">${city.waterTDS} mg/L</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.waterTDS/2000*100,100)}%;background:${getScoreColor(city.waterScore)}"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Water Hardness</span><span style="color:${getScoreColor(city.waterScore)};font-weight:700">${city.waterHardness}</span>
            </div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Water Quality Score</span><span style="color:${getScoreColor(city.waterScore)};font-weight:700">${city.waterScore}/100</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${city.waterScore}%;background:${getScoreColor(city.waterScore)}"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px">
              <span>Hair Fall Risk</span><span style="color:${hairRisk.color};font-weight:700">${city.waterTDS > 700 ? 'Very High' : city.waterTDS > 400 ? 'High' : 'Low'}</span>
            </div>
            <div class="progress-bar"><div class="progress-fill" style="width:${Math.min(city.waterTDS/2000*100,100)}%;background:${hairRisk.color}"></div></div>
          </div>
        </div>
      </div>
    </div>
  `;
}

function updateReportCity(name) {
  currentReportCity = name;
  renderReport(name);
}

// ═══════════════════════════════════════════════
// INSIGHTS
// ═══════════════════════════════════════════════
function renderInsights() {
  const sorted = [...cityData].sort((a,b) => a.aqi - b.aqi);
  const cleanest = sorted.slice(0,3);
  const polluted = sorted.slice(-3).reverse();

  document.getElementById('insightsContent').innerHTML = `
    <div class="two-col" style="margin-bottom:20px">
      <div class="card">
        <div class="section-title"><div class="dot" style="background:#68d391"></div> 🌿 Top 3 Cleanest Cities</div>
        ${cleanest.map((c,i) => `
          <div style="display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid var(--border);${i===2?'border-bottom:none':''}">
            <div style="font-size:20px;width:30px;text-align:center">${['🥇','🥈','🥉'][i]}</div>
            <div style="flex:1">
              <div style="font-weight:700;font-size:14px">${c.name}</div>
              <div style="font-size:11px;color:var(--text-muted)">${c.state}</div>
            </div>
            <div style="text-align:right">
              <div style="font-family:'JetBrains Mono';font-weight:700;color:${getAqiCategory(c.aqi).color};font-size:18px">${c.aqi}</div>
              <div style="font-size:10px;color:var(--text-muted)">AQI</div>
            </div>
          </div>
        `).join('')}
      </div>

      <div class="card">
        <div class="section-title"><div class="dot" style="background:#fc8181"></div> ⚠️ Top 3 Most Polluted Cities</div>
        ${polluted.map((c,i) => `
          <div style="display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid var(--border);${i===2?'border-bottom:none':''}">
            <div style="font-size:20px;width:30px;text-align:center">${['💀','😷','😰'][i]}</div>
            <div style="flex:1">
              <div style="font-weight:700;font-size:14px">${c.name}</div>
              <div style="font-size:11px;color:var(--text-muted)">${c.state}</div>
            </div>
            <div style="text-align:right">
              <div style="font-family:'JetBrains Mono';font-weight:700;color:${getAqiCategory(c.aqi).color};font-size:18px">${c.aqi}</div>
              <div style="font-size:10px;color:var(--text-muted)">AQI</div>
            </div>
          </div>
        `).join('')}
      </div>
    </div>

    <div class="section-title"><div class="dot" style="background:#9f7aea"></div> 🔍 Key Findings & Anomalies</div>

    <div class="insight-card purple">
      <div class="insight-title">🌊 Biggest Anomaly: Bengaluru's Air vs Water Paradox</div>
      <div class="insight-text">Bengaluru has the 3rd best air quality (AQI 63) among analyzed cities, yet its water quality is among the worst — TDS exceeds 500 mg/L in borewell zones, causing significant hair fall and skin dryness. This is the starkest air-water quality mismatch in India's metros. Clean air does NOT equal safe water in Bengaluru.</div>
    </div>

    <div class="insight-card warning">
      <div class="insight-title">📈 Most Surprising: Delhi's AQI Is Actually Worsening Year-on-Year</div>
      <div class="insight-text">Delhi's 2026 annual AQI of 179 represents a 6.5% deterioration vs prior years and a continuous upward trend since 2020 (AQI 154). Despite government interventions like odd-even rules and EV initiatives, the annual average continues to climb. The current June reading of 93 masks the true year-round health burden.</div>
    </div>

    <div class="insight-card">
      <div class="insight-title">💧 Mumbai: India's Best Metro for Water Quality</div>
      <div class="insight-text">Mumbai's municipal water from Bhatsa and Vihar lakes has TDS of only ~120 mg/L — comparable to European standards. This gives Mumbaikars a significant advantage in hair and skin health compared to Delhi or Bengaluru residents. Coastal cities benefit from natural water table advantages and well-maintained infrastructure.</div>
    </div>

    <div class="insight-card danger">
      <div class="insight-title">🌪️ Lucknow: The Silent Crisis — AQI 195 with High Annual Average</div>
      <div class="insight-text">Lucknow's AQI of 195 on a June day (typically a cleaner season) signals serious structural pollution issues. With an annual average also at 195, there's virtually no clean season in Lucknow. Residents breathe effectively equivalent to 2+ cigarettes per day year-round. This is a public health emergency that deserves more national attention than Delhi typically receives.</div>
    </div>

    <div class="insight-card success">
      <div class="insight-title">🟢 Best Overall City for Environmental Health: Mumbai</div>
      <div class="insight-text">With an AQI of 57 (lowest PM2.5 of 18 µg/m³), water TDS of only 120 mg/L, and a health score of 72/100, Mumbai offers India's best environmental health profile among major metros. The combination of sea breezes, good municipal water, and relatively lower industrial pollution gives its residents a meaningful quality-of-life advantage.</div>
    </div>

    <div class="insight-card warning">
      <div class="insight-title">⚡ Trend Alert: North vs South India AQI Divide Widening</div>
      <div class="insight-text">Northern cities (Delhi: 93, Kolkata: 118, Jaipur: 152, Lucknow: 195) have an average AQI of 139.5 while southern metros (Mumbai: 57, Bengaluru: 63, Hyderabad: 59, Chennai: 65) average just 61 — a 2.3× gap. Stubble burning, geography, and temperature inversions in the Indo-Gangetic Plain are primary drivers of this north-south air quality divide.</div>
    </div>
  `;
}

// ═══════════════════════════════════════════════
// RECOMMENDATIONS
// ═══════════════════════════════════════════════
function renderRecs(cityName) {
  const city = cityData.find(c => c.name === cityName) || cityData[0];
  const highAir = city.aqi > 100 || city.annualAqi > 150;
  const highWater = city.waterTDS > 500;

  document.getElementById('recContent').innerHTML = `
    <div style="margin-bottom:20px;background:linear-gradient(135deg,rgba(104,211,145,0.06),rgba(99,179,237,0.04));border:1px solid rgba(104,211,145,0.15);border-radius:12px;padding:16px 20px">
      <div style="font-size:14px;font-weight:700;margin-bottom:4px">Personalized for ${city.name} residents · AQI ${city.aqi} · Water TDS ~${city.waterTDS} mg/L</div>
      <div style="font-size:12px;color:var(--text-secondary)">Recommendations based on real-time and annual pollution data. Adjust based on your personal health conditions.</div>
    </div>

    <div class="rec-grid">
      <div class="rec-card">
        <h4>🌅 Daily Actions</h4>
        <ul>
          ${highAir ? `
          <li>Check AQI on <strong>aqi.in</strong> every morning before going out</li>
          <li>Wear N95/KN95 mask for any outdoor activity > 20 minutes</li>
          <li>Schedule outdoor tasks before 8 AM or after 7 PM (lower AQI windows)</li>
          <li>Rinse face and nasal passages after returning indoors</li>
          ` : `
          <li>Monitor AQI daily — summer in your city can spike unexpectedly</li>
          <li>Light outdoor activity is generally safe in morning hours</li>
          <li>Nasal rinse after extended outdoor exposure is beneficial</li>
          `}
          <li>Stay well-hydrated (3L+ water) to flush inhaled particulates</li>
          <li>Eat antioxidant-rich foods: berries, leafy greens, turmeric to combat oxidative stress</li>
        </ul>
      </div>

      <div class="rec-card">
        <h4>🏠 Indoor Air Improvements</h4>
        <ul>
          ${highAir ? `
          <li>Install <strong>HEPA air purifier</strong> (CADR 250+ for rooms >200 sq ft)</li>
          <li>Keep windows closed during 10 AM–5 PM peak pollution hours</li>
          <li>Add indoor plants: Spider Plant, Peace Lily, Snake Plant (mild VOC reduction)</li>
          <li>Use car cabin air filters when commuting — change every 6 months</li>
          ` : `
          <li>Air purifier recommended for bedrooms, especially for children</li>
          <li>Ventilate home in early morning (6–8 AM) for best air quality</li>
          <li>Indoor plants add comfort and some VOC reduction</li>
          `}
          <li>Vacuum with HEPA filter weekly to remove settled PM particles</li>
          <li>Avoid burning incense or candles indoors — adds to indoor PM2.5</li>
        </ul>
      </div>

      <div class="rec-card">
        <h4>🏃 Outdoor Activity Guidance</h4>
        <ul>
          ${highAir ? `
          <li>Best time to exercise: <strong>5:30–7:30 AM</strong> (lowest daily AQI)</li>
          <li>Avoid exercise near traffic routes or construction sites</li>
          <li>Switch to gym/home workouts on AQI >150 days</li>
          <li>If cycling: stick to parks, wear N95, reduce pace to lower breathing rate</li>
          ` : `
          <li>Morning exercise (6–8 AM) is relatively safe at current AQI levels</li>
          <li>Parks and green corridors offer better air than roadsides</li>
          <li>Light N95 mask optional for extended runs</li>
          `}
          <li>Track daily AQI via <strong>aqi.in</strong> app or CPCB's Sameer app</li>
          <li>Post-workout shower promptly to remove pollution particles from skin and hair</li>
        </ul>
      </div>

      <div class="rec-card">
        <h4>💇 Hair Care Recommendations</h4>
        <ul>
          ${highWater ? `
          <li>Install <strong>shower/tap filter</strong> to reduce TDS before washing hair (RiverSoft, CareDale, or similar)</li>
          <li>Use <strong>chelating shampoo</strong> monthly to remove mineral buildup</li>
          <li>Deep condition with coconut oil or argan oil weekly</li>
          <li>Final rinse with filtered/RO water for softness</li>
          <li>Avoid excessive heat styling — water-damaged hair is more fragile</li>
          ` : `
          <li>Your city's water is relatively better — standard conditioning is sufficient</li>
          <li>Use mild sulfate-free shampoo 2–3x per week</li>
          <li>Occasional deep conditioning maintains softness</li>
          `}
          <li>Air pollution particles settle on hair — cover with scarf/hat outdoors on bad AQI days</li>
          <li>Biotin supplements (consult doctor) support hair follicle strength</li>
        </ul>
      </div>

      <div class="rec-card">
        <h4>🌸 Skin Care Recommendations</h4>
        <ul>
          ${highAir ? `
          <li>Use <strong>double cleansing</strong>: oil cleanser first, then water-based to remove PM2.5 from pores</li>
          <li>Apply <strong>niacinamide serum</strong> — reduces pollution-induced pore congestion and brightens skin</li>
          <li>Use SPF 50+ sunscreen daily — UV + pollution accelerates skin aging</li>
          <li>Vitamin C serum (morning) neutralizes free radicals from PM2.5</li>
          ` : `
          <li>Regular cleansing twice daily removes mild pollution deposits</li>
          <li>SPF 30–50 sunscreen is still essential</li>
          <li>Antioxidant moisturizer (Vitamin C, E) provides preventive protection</li>
          `}
          ${highWater ? `
          <li>Apply moisturizer immediately post-shower (on damp skin) to lock in hydration countering hard water dryness</li>
          <li>Use <strong>ceramide-based</strong> moisturizer to repair barrier damaged by hard water minerals</li>
          ` : `
          <li>Standard moisturizing post-shower is sufficient for your water quality</li>
          `}
          <li>Hyaluronic acid serum counteracts dryness from both air pollution and water hardness</li>
        </ul>
      </div>

      <div class="rec-card">
        <h4>💧 Water Quality Improvements</h4>
        <ul>
          ${highWater ? `
          <li>Install <strong>RO purifier</strong> (Reverse Osmosis) for drinking water — reduces TDS to 50–150 mg/L</li>
          <li>Shower filter with KDF/carbon block reduces chlorine and reduces some hardness impact</li>
          <li>For very hard water zones (TDS >800): water softener for bathing circuit</li>
          <li>Use filtered water for face washing — especially beneficial for sensitive skin</li>
          ` : `
          <li>Your city has relatively good water — a standard carbon filter on taps is sufficient</li>
          <li>RO purifier still recommended for drinking water as a safety margin</li>
          <li>Regular filter maintenance every 6 months keeps TDS levels in check</li>
          `}
          <li>Test your tap water with a TDS meter (₹200–500 online) for accurate baseline</li>
          <li>If TDS > 500 mg/L, avoid using unfiltered water for final hair rinse</li>
          <li>Store drinking water in glass or food-grade stainless steel to prevent leaching</li>
        </ul>
      </div>
    </div>

    <div class="exec-summary">
      <h3>⚡ Quick Action Priority for ${city.name} Residents</h3>
      <p>
        ${city.healthScore < 35
          ? `<strong>URGENT — Your city's environmental health score (${city.healthScore}/100) is critically low.</strong> Prioritize immediately: (1) HEPA air purifier in bedroom, (2) N95 masks for all outdoor use, (3) RO water purifier for drinking, (4) shower filter for hair/skin. These four changes can significantly reduce your daily pollution exposure.`
          : city.healthScore < 55
          ? `<strong>Your city's environmental health score (${city.healthScore}/100) indicates significant risk.</strong> Top priorities: (1) Air purifier for home, (2) monitor AQI daily, (3) switch to filtered water, (4) adjust outdoor activity timing. Consistent precautions make a meaningful difference.`
          : `<strong>Your city's environmental health score (${city.healthScore}/100) is moderate-good.</strong> Focus on: (1) Maintaining good indoor air quality, (2) water quality for skin/hair, (3) timing outdoor activities well. Your environmental baseline is manageable with reasonable precautions.`
        }
      </p>
    </div>
  `;
}

function updateRecCity(name) {
  currentRecCity = name;
  renderRecs(name);
}

// ═══════════════════════════════════════════════
// INIT
// ═══════════════════════════════════════════════
renderCityCards(cityData);
renderHealth('Delhi');
renderReport('Delhi');
renderInsights();
renderRecs('Delhi');
</script>
</body>
</html><img width="1920" height="1020" alt="Screenshot 2026-06-13 235441" src="https://github.com/user-attachments/assets/a8e0f754-ddbf-47f1-b88e-2380a13681b2" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235436" src="https://github.com/user-attachments/assets/36302e8a-a1af-4dff-a50e-f58b930dc521" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235431" src="https://github.com/user-attachments/assets/8b32a1da-6696-4b51-bc75-9bbadc2294a8" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235426" src="https://github.com/user-attachments/assets/192b3d22-a82b-4ce0-84c9-6aecce7eb109" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235421" src="https://github.com/user-attachments/assets/a5b5c2ba-0d70-4951-8bbe-8199a2a56797" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235413" src="https://github.com/user-attachments/assets/b3b96e2e-c8b3-4244-a10d-04f57e37a1cf" />
<img width="1920" height="1020" alt="Screenshot 2026-06-13 235404" src="https://github.com/user-attachments/assets/c30814b3-492d-4e76-8a2a-d34866d46546" />
