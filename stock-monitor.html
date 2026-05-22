<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Real Stock Portfolio Monitor</title>

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">

  <style>
    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    :root {
      --bg-primary: #ffffff;
      --bg-secondary: #f5f5f3;
      --bg-tertiary: #eeede8;
      --text-primary: #1a1a18;
      --text-secondary: #6b6b67;
      --border-light: rgba(0,0,0,0.10);
      --border-mid: rgba(0,0,0,0.18);

      --green: #1D9E75;
      --red: #D85A30;

      --radius-md: 8px;
      --radius-lg: 12px;
    }

    body {
      font-family: system-ui;
      background: var(--bg-tertiary);
      color: var(--text-primary);
      padding: 2rem 1rem;
    }

    .container {
      max-width: 900px;
      margin: auto;
    }

    .header {
      margin-bottom: 1.5rem;
    }

    .header h1 {
      font-size: 28px;
      margin-bottom: 5px;
    }

    .header p {
      color: var(--text-secondary);
    }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(180px,1fr));
      gap: 10px;
      margin-bottom: 1.5rem;
    }

    .metric {
      background: var(--bg-primary);
      padding: 1rem;
      border-radius: var(--radius-lg);
      border: 1px solid var(--border-light);
    }

    .metric-label {
      font-size: 12px;
      color: var(--text-secondary);
      margin-bottom: 5px;
    }

    .metric-val {
      font-size: 22px;
      font-weight: bold;
    }

    .pos { color: var(--green); }
    .neg { color: var(--red); }

    .action-bar {
      display: flex;
      gap: 10px;
      margin-bottom: 1rem;
    }

    .btn {
      padding: 10px 16px;
      border-radius: var(--radius-md);
      border: none;
      cursor: pointer;
      font-size: 14px;
    }

    .btn.primary {
      background: black;
      color: white;
    }

    .btn.secondary {
      background: white;
      border: 1px solid var(--border-mid);
    }

    .add-form {
      background: white;
      padding: 1rem;
      border-radius: var(--radius-lg);
      margin-bottom: 1rem;
      display: none;
      border: 1px solid var(--border-light);
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(180px,1fr));
      gap: 10px;
      margin-bottom: 10px;
    }

    input {
      width: 100%;
      padding: 10px;
      border-radius: var(--radius-md);
      border: 1px solid #ccc;
    }

    .stocks-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .stock-card {
      background: white;
      padding: 1rem;
      border-radius: var(--radius-lg);
      border: 1px solid var(--border-light);

      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 20px;
    }

    .stock-left h2 {
      font-size: 18px;
    }

    .stock-left p {
      color: var(--text-secondary);
      font-size: 13px;
    }

    .stock-right {
      text-align: right;
    }

    .price {
      font-size: 20px;
      font-weight: bold;
    }

    .change {
      font-size: 13px;
      margin-top: 3px;
    }

    .remove-btn {
      margin-top: 8px;
      background: none;
      border: none;
      color: var(--red);
      cursor: pointer;
    }

    .status {
      margin-top: 1rem;
      font-size: 13px;
      color: var(--text-secondary);
    }

  </style>
</head>

<body>

<div class="container">

  <div class="header">
    <h1>Live Stock Portfolio Monitor</h1>
    <p>Real-time stock tracking with live market prices</p>
  </div>

  <div class="summary-grid">
    <div class="metric">
      <div class="metric-label">Portfolio Value</div>
      <div class="metric-val" id="totalValue">£0.00</div>
    </div>

    <div class="metric">
      <div class="metric-label">Total Gain/Loss</div>
      <div class="metric-val" id="totalGain">£0.00</div>
    </div>

    <div class="metric">
      <div class="metric-label">Holdings</div>
      <div class="metric-val" id="holdings">0</div>
    </div>
  </div>

  <div class="action-bar">
    <button class="btn primary" onclick="showForm()">
      Add Stock
    </button>

    <button class="btn secondary" onclick="refreshPrices()">
      Refresh Prices
    </button>
  </div>

  <div class="add-form" id="addForm">

    <div class="form-grid">

      <div>
        <input type="text" id="ticker" placeholder="Ticker (AAPL)">
      </div>

      <div>
        <input type="number" id="shares" placeholder="Shares">
      </div>

      <div>
        <input type="number" id="buyPrice" placeholder="Buy Price">
      </div>

    </div>

    <button class="btn primary" onclick="addStock()">
      Save Stock
    </button>

  </div>

  <div class="stocks-list" id="stockList"></div>

  <div class="status" id="status">
    Waiting for updates...
  </div>

</div>

<script>

const API_KEY = 'd884t8hr01qq4341kg6gd884t8hr01qq4341kg70';

let stocks = JSON.parse(localStorage.getItem('realStocks')) || [];

function saveStocks() {
  localStorage.setItem('realStocks', JSON.stringify(stocks));
}

function formatMoney(num) {
  return '£' + Number(num).toLocaleString('en-GB', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  });
}

function showForm() {
  document.getElementById('addForm').style.display = 'block';
}

function addStock() {

  const ticker = document.getElementById('ticker').value.toUpperCase().trim();
  const shares = parseFloat(document.getElementById('shares').value);
  const buyPrice = parseFloat(document.getElementById('buyPrice').value);

  if (!ticker || !shares || !buyPrice) {
    alert('Please fill everything in');
    return;
  }

  stocks.push({
    ticker,
    shares,
    buyPrice,
    currentPrice: buyPrice
  });

  saveStocks();

  document.getElementById('ticker').value = '';
  document.getElementById('shares').value = '';
  document.getElementById('buyPrice').value = '';

  renderStocks();

  refreshPrices();
}

async function refreshPrices() {

  document.getElementById('status').innerText =
    'Fetching live market prices...';

  for (const stock of stocks) {

    try {

      const response = await fetch(
        `https://finnhub.io/api/v1/quote?symbol=${stock.ticker}&token=${API_KEY}`
      );

      const data = await response.json();

      if (data.c) {
        stock.currentPrice = data.c;
      }

    } catch (err) {

      console.error(stock.ticker, err);

    }
  }

  saveStocks();

  renderStocks();

  document.getElementById('status').innerText =
    'Last updated: ' +
    new Date().toLocaleTimeString();

}

function renderStocks() {

  const stockList = document.getElementById('stockList');

  if (!stocks.length) {
    stockList.innerHTML = '<p>No stocks added yet.</p>';
    return;
  }

  let totalPortfolio = 0;
  let totalGain = 0;

  stockList.innerHTML = stocks.map(stock => {

    const value = stock.currentPrice * stock.shares;
    const gain = (stock.currentPrice - stock.buyPrice) * stock.shares;
    const gainPercent =
      ((stock.currentPrice - stock.buyPrice) / stock.buyPrice) * 100;

    totalPortfolio += value;
    totalGain += gain;

    return `
      <div class="stock-card">

        <div class="stock-left">
          <h2>${stock.ticker}</h2>
          <p>
            ${stock.shares} shares
          </p>
        </div>

        <div class="stock-right">

          <div class="price">
            ${formatMoney(stock.currentPrice)}
          </div>

          <div class="change ${gain >= 0 ? 'pos' : 'neg'}">
            ${gain >= 0 ? '+' : ''}
            ${formatMoney(gain)}
            (${gainPercent.toFixed(2)}%)
          </div>

          <button class="remove-btn" onclick="removeStock('${stock.ticker}')">
            Remove
          </button>

        </div>

      </div>
    `;

  }).join('');

  document.getElementById('totalValue').innerText =
    formatMoney(totalPortfolio);

  const gainEl = document.getElementById('totalGain');

  gainEl.innerText =
    (totalGain >= 0 ? '+' : '') +
    formatMoney(totalGain);

  gainEl.className =
    'metric-val ' + (totalGain >= 0 ? 'pos' : 'neg');

  document.getElementById('holdings').innerText =
    stocks.length;

}

function removeStock(ticker) {

  stocks = stocks.filter(s => s.ticker !== ticker);

  saveStocks();

  renderStocks();

}

renderStocks();

refreshPrices();

setInterval(refreshPrices, 30000);

</script>

</body>
</html>
