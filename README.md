<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stock Portfolio Monitor</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg-primary: #ffffff;
      --bg-secondary: #f5f5f3;
      --bg-tertiary: #eeede8;
      --text-primary: #1a1a18;
      --text-secondary: #6b6b67;
      --border-light: rgba(0,0,0,0.10);
      --border-mid: rgba(0,0,0,0.18);
      --radius-md: 8px;
      --radius-lg: 12px;
      --green: #1D9E75;
      --green-bg: #E1F5EE;
      --green-text: #0F6E56;
      --red: #D85A30;
      --red-bg: #FAECE7;
      --red-text: #993C1D;
    }

    @media (prefers-color-scheme: dark) {
      :root {
        --bg-primary: #1e1e1c;
        --bg-secondary: #2a2a28;
        --bg-tertiary: #242422;
        --text-primary: #f0efe8;
        --text-secondary: #9a9993;
        --border-light: rgba(255,255,255,0.10);
        --border-mid: rgba(255,255,255,0.18);
        --green-bg: #0a2e20;
        --green-text: #5DCAA5;
        --red-bg: #2e1408;
        --red-text: #F0997B;
      }
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
      background: var(--bg-tertiary);
      color: var(--text-primary);
      min-height: 100vh;
      padding: 2rem 1rem;
    }

    .container { max-width: 860px; margin: 0 auto; }

    .header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.5rem;
    }
    .header-title {
      font-size: 22px;
      font-weight: 500;
      color: var(--text-primary);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .header-sub { font-size: 13px; color: var(--text-secondary); margin-top: 3px; }
    .last-update { font-size: 12px; color: var(--text-secondary); display: flex; align-items: center; gap: 6px; }
    .loading-dot {
      display: inline-block; width: 6px; height: 6px; border-radius: 50%;
      background: var(--green); animation: blink 1.2s infinite;
    }
    @keyframes blink { 0%,100%{opacity:0.2} 50%{opacity:1} }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: 10px;
      margin-bottom: 1.5rem;
    }
    .metric {
      background: var(--bg-secondary);
      border-radius: var(--radius-md);
      padding: 0.875rem 1rem;
    }
    .metric-label { font-size: 12px; color: var(--text-secondary); margin-bottom: 5px; }
    .metric-val { font-size: 20px; font-weight: 500; color: var(--text-primary); }
    .metric-val.pos { color: var(--green); }
    .metric-val.neg { color: var(--red); }

    .action-bar { display: flex; gap: 8px; margin-bottom: 1rem; }
    .btn {
      display: inline-flex; align-items: center; gap: 6px;
      padding: 7px 16px; border-radius: var(--radius-md); font-size: 14px;
      cursor: pointer; border: 0.5px solid var(--border-mid);
      background: var(--bg-primary); color: var(--text-primary);
      transition: background 0.15s;
    }
    .btn:hover { background: var(--bg-secondary); }
    .btn.primary {
      background: var(--text-primary); color: var(--bg-primary); border-color: transparent;
    }
    .btn.primary:hover { opacity: 0.85; }
    .btn.grow { flex: 1; justify-content: center; }

    .add-form {
      background: var(--bg-secondary);
      border-radius: var(--radius-lg);
      padding: 1.25rem;
      margin-bottom: 1rem;
      display: none;
    }
    .add-form.open { display: block; }
    .form-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 10px; }
    @media (max-width: 600px) { .form-grid { grid-template-columns: 1fr 1fr; } }
    .form-group label { display: block; font-size: 12px; color: var(--text-secondary); margin-bottom: 5px; }
    .form-group input {
      width: 100%; padding: 7px 10px; border-radius: var(--radius-md);
      border: 0.5px solid var(--border-mid); background: var(--bg-primary);
      color: var(--text-primary); font-size: 14px; outline: none;
    }
    .form-group input:focus { border-color: var(--text-secondary); box-shadow: 0 0 0 3px rgba(0,0,0,0.05); }
    .form-actions { display: flex; gap: 8px; justify-content: flex-end; }

    .stocks-list { display: flex; flex-direction: column; gap: 8px; }

    .stock-card {
      background: var(--bg-primary);
      border: 0.5px solid var(--border-light);
      border-radius: var(--radius-lg);
      padding: 0.875rem 1.25rem;
      display: grid;
      grid-template-columns: 1fr auto;
      align-items: center;
      gap: 12px;
      transition: border-color 0.15s;
    }
    .stock-card:hover { border-color: var(--border-mid); }

    .stock-left { display: flex; align-items: center; gap: 12px; }
    .stock-icon {
      width: 38px; height: 38px; border-radius: 9px;
      display: flex; align-items: center; justify-content: center;
      font-size: 11px; font-weight: 600; flex-shrink: 0;
    }
    .stock-ticker { font-size: 15px; font-weight: 500; }
    .stock-name { font-size: 12px; color: var(--text-secondary); max-width: 180px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .stock-shares { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }

    .stock-right { display: flex; align-items: center; gap: 16px; flex-wrap: wrap; justify-content: flex-end; }
    .sparkline { width: 60px; height: 28px; flex-shrink: 0; }
    .stock-price-block { text-align: right; }
    .stock-price { font-size: 15px; font-weight: 500; }
    .stock-change { font-size: 12px; margin-top: 2px; }
    .stock-change.pos { color: var(--green); }
    .stock-change.neg { color: var(--red); }
    .stock-value { text-align: right; min-width: 80px; }
    .stock-value-main { font-size: 14px; font-weight: 500; }
    .badge {
      font-size: 11px; padding: 3px 8px; border-radius: 20px;
      font-weight: 500; display: inline-block; margin-top: 3px;
    }
    .badge.pos { background: var(--green-bg); color: var(--green-text); }
    .badge.neg { background: var(--red-bg); color: var(--red-text); }
    .remove-btn {
      background: none; border: none; cursor: pointer; color: var(--text-secondary);
      padding: 4px; border-radius: 4px; display: flex; align-items: center;
      font-size: 17px; opacity: 0.45; transition: opacity 0.15s;
    }
    .remove-btn:hover { opacity: 1; }

    .empty-state {
      text-align: center; padding: 3rem 1rem; color: var(--text-secondary); font-size: 14px;
      border: 0.5px dashed var(--border-mid); border-radius: var(--radius-lg);
    }
    .empty-icon { font-size: 30px; margin-bottom: 8px; }

    .chart-section { margin-top: 1.5rem; display: none; }
    .section-label {
      font-size: 11px; color: var(--text-secondary); font-weight: 500;
      text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 10px;
    }
    .chart-wrap { position: relative; height: 220px; background: var(--bg-primary); border-radius: var(--radius-lg); border: 0.5px solid var(--border-light); padding: 1rem; }
    .chart-wrap canvas { max-height: 188px; }
  </style>
</head>
<body>
<div class="container">

  <div class="header">
    <div>
      <div class="header-title">
        <i class="ti ti-chart-candle" aria-hidden="true"></i>
        Portfolio Monitor
      </div>
      <div class="header-sub">Track your stocks and holdings</div>
    </div>
    <div class="last-update">
      <span id="lastUpdate">Prices simulated</span>
      <span class="loading-dot"></span>
    </div>
  </div>

  <div class="summary-grid">
    <div class="metric"><div class="metric-label">Portfolio value</div><div class="metric-val" id="totalValue">£0.00</div></div>
    <div class="metric"><div class="metric-label">Day's gain/loss</div><div class="metric-val" id="dayGain">£0.00</div></div>
    <div class="metric"><div class="metric-label">Total return</div><div class="metric-val" id="totalReturn">0.00%</div></div>
    <div class="metric"><div class="metric-label">Holdings</div><div class="metric-val" id="holdingsCount">0</div></div>
  </div>

  <div class="add-form" id="addForm">
    <div class="form-grid">
      <div class="form-group">
        <label>Ticker symbol</label>
        <input type="text" id="fTicker" placeholder="e.g. AAPL" style="text-transform:uppercase">
      </div>
      <div class="form-group">
        <label>Company name</label>
        <input type="text" id="fName" placeholder="e.g. Apple Inc.">
      </div>
      <div class="form-group">
        <label>Shares owned</label>
        <input type="number" id="fShares" placeholder="0" min="0" step="0.01">
      </div>
      <div class="form-group">
        <label>Avg. buy price (£)</label>
        <input type="number" id="fBuyPrice" placeholder="0.00" min="0" step="0.01">
      </div>
      <div class="form-group">
        <label>Current price (£)</label>
        <input type="number" id="fCurrentPrice" placeholder="0.00" min="0" step="0.01">
      </div>
      <div class="form-group">
        <label>Sector</label>
        <input type="text" id="fSector" placeholder="e.g. Technology">
      </div>
    </div>
    <div class="form-actions">
      <button class="btn" onclick="cancelAdd()">Cancel</button>
      <button class="btn primary" onclick="saveStock()">
        <i class="ti ti-plus" aria-hidden="true"></i> Add stock
      </button>
    </div>
  </div>

  <div class="action-bar">
    <button class="btn grow" id="addBtn" onclick="showAddForm()">
      <i class="ti ti-plus" aria-hidden="true"></i> Add stock
    </button>
    <button class="btn" onclick="refreshPrices()">
      <i class="ti ti-refresh" aria-hidden="true"></i> Refresh
    </button>
  </div>

  <div class="stocks-list" id="stockList"></div>

  <div class="chart-section" id="chartSection">
    <div class="section-label">Allocation breakdown</div>
    <div class="chart-wrap">
      <canvas id="allocationChart" role="img" aria-label="Doughnut chart showing portfolio allocation by stock"></canvas>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<script>
const COLORS = ['#378ADD','#1D9E75','#D85A30','#BA7517','#7F77DD','#D4537E','#639922','#E24B4A','#0F6E56','#993C1D'];
const ICON_COLORS = [
  ['#E6F1FB','#185FA5'],['#E1F5EE','#0F6E56'],['#FAEEDA','#854F0B'],
  ['#FAECE7','#993C1D'],['#EEEDFE','#534AB7'],['#FBEAF0','#993556']
];

let stocks = JSON.parse(localStorage.getItem('stockMonitorPro') || 'null');
if (!stocks) {
  stocks = [
    {id:1,ticker:'AAPL',name:'Apple Inc.',shares:10,buyPrice:145.00,currentPrice:178.50,sector:'Technology',history:genHistory(178.50)},
    {id:2,ticker:'TSLA',name:'Tesla Inc.',shares:5,buyPrice:220.00,currentPrice:195.30,sector:'Automotive',history:genHistory(195.30)},
    {id:3,ticker:'MSFT',name:'Microsoft Corp.',shares:8,buyPrice:310.00,currentPrice:415.20,sector:'Technology',history:genHistory(415.20)},
  ];
}
let nextId = stocks.reduce((m,s) => Math.max(m,s.id), 0) + 1;
let allocationChart = null;

function genHistory(base) {
  const h = [];
  let v = base * 0.88;
  for (let i = 0; i < 20; i++) { v += v * (Math.random() - 0.48) * 0.03; h.push(parseFloat(v.toFixed(2))); }
  h.push(base);
  return h;
}

function save() { try { localStorage.setItem('stockMonitorPro', JSON.stringify(stocks)); } catch(e) {} }
function dayChange(s) { const h = s.history || []; return h.length < 2 ? 0 : s.currentPrice - h[h.length - 2]; }
function dayChangePct(s) { const h = s.history || []; if (h.length < 2) return 0; const p = h[h.length-2]; return p ? ((s.currentPrice - p) / p) * 100 : 0; }
function totalGain(s) { return (s.currentPrice - s.buyPrice) * s.shares; }
function totalGainPct(s) { return s.buyPrice ? ((s.currentPrice - s.buyPrice) / s.buyPrice) * 100 : 0; }
function value(s) { return s.currentPrice * s.shares; }
function fmt(n) { return '£' + Math.abs(n).toLocaleString('en-GB', {minimumFractionDigits:2, maximumFractionDigits:2}); }

function sparklineSVG(history, positive) {
  const w = 60, h = 28, pts = history || [];
  if (pts.length < 2) return `<svg class="sparkline" viewBox="0 0 ${w} ${h}"><line x1="0" y1="${h/2}" x2="${w}" y2="${h/2}" stroke="#ccc" stroke-width="1"/></svg>`;
  const mn = Math.min(...pts), mx = Math.max(...pts), rng = mx - mn || 1;
  const xs = pts.map((_,i) => (i / (pts.length - 1)) * w);
  const ys = pts.map(p => h - 2 - ((p - mn) / rng) * (h - 4));
  const d = 'M' + xs.map((x,i) => `${x.toFixed(1)},${ys[i].toFixed(1)}`).join('L');
  const color = positive ? '#1D9E75' : '#D85A30';
  return `<svg class="sparkline" viewBox="0 0 ${w} ${h}" role="img" aria-label="Price trend"><path d="${d}" fill="none" stroke="${color}" stroke-width="1.5" stroke-linejoin="round" stroke-linecap="round"/></svg>`;
}

function renderStocks() {
  const list = document.getElementById('stockList');
  if (!stocks.length) {
    list.innerHTML = `<div class="empty-state"><div class="empty-icon"><i class="ti ti-chart-bar" aria-hidden="true"></i></div><div>No stocks added yet</div><div style="margin-top:4px;font-size:12px;">Click "Add stock" to start tracking your portfolio</div></div>`;
    document.getElementById('chartSection').style.display = 'none';
    updateSummary();
    return;
  }

  list.innerHTML = stocks.map((s, i) => {
    const dc = dayChange(s), dcp = dayChangePct(s), pos = dc >= 0;
    const tg = totalGain(s), tgp = totalGainPct(s), tgPos = tg >= 0;
    const ic = ICON_COLORS[i % ICON_COLORS.length];
    return `<div class="stock-card">
      <div class="stock-left">
        <div class="stock-icon" style="background:${ic[0]};color:${ic[1]};">${s.ticker.substring(0,3)}</div>
        <div class="stock-info">
          <div class="stock-ticker">${s.ticker}</div>
          <div class="stock-name">${s.name}</div>
          <div class="stock-shares">${s.shares} shares · ${s.sector || '—'}</div>
        </div>
      </div>
      <div class="stock-right">
        ${sparklineSVG(s.history, pos)}
        <div class="stock-price-block">
          <div class="stock-price">${fmt(s.currentPrice)}</div>
          <div class="stock-change ${pos ? 'pos' : 'neg'}">${pos ? '▲' : '▼'} ${Math.abs(dcp).toFixed(2)}% today</div>
        </div>
        <div class="stock-value">
          <div class="stock-value-main">${fmt(value(s))}</div>
          <span class="badge ${tgPos ? 'pos' : 'neg'}">${tgPos ? '+' : ''}${tgp.toFixed(1)}%</span>
        </div>
        <button class="remove-btn" onclick="removeStock(${s.id})" aria-label="Remove ${s.ticker}">
          <i class="ti ti-x" aria-hidden="true"></i>
        </button>
      </div>
    </div>`;
  }).join('');

  updateSummary();
  renderAllocationChart();
  document.getElementById('chartSection').style.display = 'block';
}

function updateSummary() {
  const tv = stocks.reduce((a,s) => a + value(s), 0);
  const dg = stocks.reduce((a,s) => a + dayChange(s) * s.shares, 0);
  const cost = stocks.reduce((a,s) => a + s.buyPrice * s.shares, 0);
  const trp = cost ? ((tv - cost) / cost) * 100 : 0;
  const dgEl = document.getElementById('dayGain');
  const trEl = document.getElementById('totalReturn');
  document.getElementById('totalValue').textContent = fmt(tv);
  dgEl.textContent = (dg >= 0 ? '+' : '-') + fmt(dg);
  dgEl.className = 'metric-val ' + (dg >= 0 ? 'pos' : 'neg');
  trEl.textContent = (trp >= 0 ? '+' : '') + trp.toFixed(2) + '%';
  trEl.className = 'metric-val ' + (trp >= 0 ? 'pos' : 'neg');
  document.getElementById('holdingsCount').textContent = stocks.length;
}

function renderAllocationChart() {
  const ctx = document.getElementById('allocationChart');
  if (!ctx) return;
  const labels = stocks.map(s => s.ticker);
  const data = stocks.map(s => parseFloat(value(s).toFixed(2)));
  const bgs = stocks.map((_,i) => COLORS[i % COLORS.length]);
  if (allocationChart) allocationChart.destroy();
  allocationChart = new Chart(ctx, {
    type: 'doughnut',
    data: { labels, datasets: [{ data, backgroundColor: bgs, borderWidth: 2, borderColor: 'rgba(255,255,255,0.5)' }] },
    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'right', labels: { font: { size: 12 }, padding: 14, usePointStyle: true, pointStyle: 'rect' } } } }
  });
}

function showAddForm() {
  document.getElementById('addForm').classList.add('open');
  document.getElementById('addBtn').style.display = 'none';
  document.getElementById('fTicker').focus();
}
function cancelAdd() {
  document.getElementById('addForm').classList.remove('open');
  document.getElementById('addBtn').style.display = '';
  ['fTicker','fName','fShares','fBuyPrice','fCurrentPrice','fSector'].forEach(id => document.getElementById(id).value = '');
}
function saveStock() {
  const ticker = document.getElementById('fTicker').value.trim().toUpperCase();
  if (!ticker) { document.getElementById('fTicker').focus(); return; }
  const name = document.getElementById('fName').value.trim();
  const shares = parseFloat(document.getElementById('fShares').value) || 0;
  const buyPrice = parseFloat(document.getElementById('fBuyPrice').value) || 0;
  const currentPrice = parseFloat(document.getElementById('fCurrentPrice').value) || buyPrice;
  const sector = document.getElementById('fSector').value.trim();
  stocks.push({ id: nextId++, ticker, name: name || ticker, shares, buyPrice, currentPrice, sector: sector || 'Other', history: genHistory(currentPrice) });
  save(); cancelAdd(); renderStocks();
}
function removeStock(id) {
  stocks = stocks.filter(s => s.id !== id);
  save(); renderStocks();
}
function refreshPrices() {
  stocks = stocks.map(s => {
    const drift = (Math.random() - 0.48) * 0.025;
    const newPrice = parseFloat((s.currentPrice * (1 + drift)).toFixed(2));
    const history = [...(s.history || []).slice(-20), newPrice];
    return { ...s, currentPrice: newPrice, history };
  });
  save(); renderStocks();
  document.getElementById('lastUpdate').textContent = 'Updated ' + new Date().toLocaleTimeString('en-GB', { hour: '2-digit', minute: '2-digit' });
}

renderStocks();
setInterval(refreshPrices, 30000);
</script>
</body>
</html>
