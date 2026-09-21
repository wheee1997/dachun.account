# dachun.account<!doctype html>
<html lang="zh-Hant">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>每日對帳小工具</title>
  <style>
    :root{
      --bg:#f4f5f7;
      --card:#ffffff;
      --text:#1f2328;
      --muted:#6b7280;
      --line:#d9dde3;
      --accent:#111827;
      --ok:#166534;
      --bad:#b91c1c;
    }
    *{box-sizing:border-box}
    html{-webkit-text-size-adjust:100%}
    body{
      margin:0;
      background:var(--bg);
      color:var(--text);
      font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","PingFang TC","Noto Sans TC",sans-serif;
    }
    button,input{font:inherit}
    .wrap{
      width:min(100%,520px);
      margin:0 auto;
      padding:12px 10px calc(34px + env(safe-area-inset-bottom));
    }
    h1{
      font-size:22px;
      line-height:1.25;
      margin:6px 0 12px;
      text-align:center;
    }
    .card{
      background:var(--card);
      border:1px solid var(--line);
      border-radius:14px;
      padding:12px;
      margin-bottom:12px;
      box-shadow:0 1px 2px rgba(0,0,0,.03);
    }
    .section-title{
      font-size:16px;
      font-weight:800;
      margin:1px 0 10px;
      color:#374151;
    }
    .row{
      display:grid;
      grid-template-columns:minmax(0,1fr) minmax(112px,42%);
      gap:8px;
      align-items:center;
      margin:10px 0;
    }
    .label{font-size:17px;font-weight:700;line-height:1.25}
    .label small{
      display:block;
      font-size:11px;
      font-weight:500;
      color:var(--muted);
      margin-top:3px;
      line-height:1.35;
    }
    input{
      width:100%;
      min-width:0;
      height:46px;
      border:1px solid #cfd4dc;
      border-radius:10px;
      font-size:19px;
      padding:7px 9px;
      background:#fff;
      text-align:right;
      outline:none;
    }
    input:focus{border-color:#6b7280;box-shadow:0 0 0 2px rgba(17,24,39,.08)}
    .cash-source-grid{
      display:grid;
      grid-template-columns:repeat(2,minmax(0,1fr));
      gap:10px;
      margin:4px 0 12px;
      align-items:start;
    }
    .denom-group{
      display:grid;
      gap:7px;
    }
    .denom-heading{
      font-size:13px;
      font-weight:800;
      color:var(--muted);
      padding-left:2px;
      margin-bottom:1px;
    }
    .denom-item{
      display:grid;
      grid-template-columns:58px minmax(0,1fr) 22px;
      gap:5px;
      align-items:center;
      padding:7px;
      border:1px solid #e4e7ec;
      border-radius:10px;
      background:#fafafa;
    }
    .denom-item label{font-size:14px;font-weight:700;white-space:nowrap}
    .denom-item input{height:40px;font-size:17px;padding:5px 7px}
    .unit{font-size:12px;color:var(--muted);white-space:nowrap}
    .expense-name,.refund-name{text-align:left;font-size:16px}
    .expense-row,.refund-row{
      display:grid;
      grid-template-columns:minmax(0,1fr) minmax(105px,38%) 32px;
      gap:7px;
      align-items:center;
      margin:9px 0;
    }
    .remove{
      width:32px;
      height:42px;
      border:0;
      background:transparent;
      font-size:23px;
      color:#9ca3af;
      cursor:pointer;
      padding:0;
    }
    .add{
      width:100%;
      min-height:42px;
      border:1px dashed #9ca3af;
      border-radius:10px;
      background:#fff;
      font-size:15px;
      font-weight:600;
      cursor:pointer;
      margin-top:3px;
    }
    .divider{border-top:2px solid #c9ced6;margin:15px 0 11px}
    .total-row{
      display:grid;
      grid-template-columns:1fr minmax(112px,42%);
      gap:8px;
      align-items:center;
      font-size:19px;
      font-weight:800;
      margin-top:6px;
    }
    .output{
      min-height:46px;
      border:1px solid #bfc5ce;
      border-radius:10px;
      display:flex;
      align-items:center;
      justify-content:flex-end;
      padding:7px 9px;
      background:#f9fafb;
      font-size:21px;
      font-variant-numeric:tabular-nums;
      overflow-wrap:anywhere;
    }
    .cash-output{font-weight:800}
    .difference{
      background:#fff;
      border:2px solid var(--accent);
      border-radius:14px;
      padding:12px;
      margin:12px 0;
      display:grid;
      grid-template-columns:1fr minmax(118px,42%);
      gap:8px;
      align-items:center;
    }
    .difference .title{font-size:19px;font-weight:800}
    .difference .sub{font-size:11px;color:var(--muted);margin-top:2px}
    .difference .value{
      text-align:right;
      font-size:24px;
      font-weight:900;
      font-variant-numeric:tabular-nums;
      overflow-wrap:anywhere;
    }
    .card-inline{
      display:grid;
      grid-template-columns:auto 56px 18px minmax(92px,1fr);
      gap:6px;
      align-items:center;
      margin:10px 0;
    }
    .card-inline .label{font-size:17px}
    .card-inline input{font-size:18px;height:42px;padding:5px 6px}
    .card-amount{
      min-height:46px;
      border:1px solid #cfd4dc;
      border-radius:10px;
      background:#f9fafb;
      display:flex;
      align-items:center;
      justify-content:flex-end;
      padding:7px 9px;
      font-size:19px;
      font-weight:700;
      font-variant-numeric:tabular-nums;
    }
    .footer{display:flex;justify-content:center;margin-top:5px}
    .reset{
      border:0;
      background:transparent;
      color:#6b7280;
      padding:10px 14px;
      font-size:14px;
      cursor:pointer;
    }
    @media (max-width:370px){
      .wrap{padding-left:8px;padding-right:8px}
      .card{padding:10px}
      .cash-source-grid{gap:7px}
      .denom-item{grid-template-columns:52px minmax(0,1fr) 18px;padding:6px 5px}
      .denom-item label{font-size:13px}
      .denom-item input{font-size:16px;padding:4px 5px}
      .expense-row,.refund-row{
        grid-template-columns:minmax(0,1fr) 32px;
      }
      .expense-row .expense-value,.refund-row .refund-value{grid-column:1/2}
      .expense-row .remove,.refund-row .remove{grid-column:2/3;grid-row:1/3}
      .card-inline{grid-template-columns:auto 50px 16px minmax(82px,1fr)}
    }
  </style>
</head>
<body>
  <main class="wrap">
    <h1>每日對帳</h1>

    <section class="card">
      <div class="section-title">Cash 金額來源</div>
      <div class="cash-source-grid">
        <div class="denom-group">
          <div class="denom-heading">鈔票</div>
          <div class="denom-item"><label for="d1000">1000 元</label><input id="d1000" class="denom" inputmode="numeric" placeholder="0"><span class="unit">張</span></div>
          <div class="denom-item"><label for="d500">500 元</label><input id="d500" class="denom" inputmode="numeric" placeholder="0"><span class="unit">張</span></div>
          <div class="denom-item"><label for="d100">100 元</label><input id="d100" class="denom" inputmode="numeric" placeholder="0"><span class="unit">張</span></div>
        </div>
        <div class="denom-group">
          <div class="denom-heading">硬幣</div>
          <div class="denom-item"><label for="d50">50 元</label><input id="d50" class="denom" inputmode="numeric" placeholder="0"><span class="unit">枚</span></div>
          <div class="denom-item"><label for="d10">10 元</label><input id="d10" class="denom" inputmode="numeric" placeholder="0"><span class="unit">枚</span></div>
          <div class="denom-item"><label for="d5">5 元</label><input id="d5" class="denom" inputmode="numeric" placeholder="0"><span class="unit">枚</span></div>
          <div class="denom-item"><label for="d1">1 元</label><input id="d1" class="denom" inputmode="numeric" placeholder="0"><span class="unit">枚</span></div>
        </div>
      </div>

      <div class="row">
        <div class="label">Cash</div>
        <div class="output cash-output" id="cash">0</div>
      </div>
      <div class="row">
        <div class="label">LINE Pay</div>
        <input id="linepay" inputmode="decimal" placeholder="0" aria-label="LINE Pay 金額" />
      </div>

      <div id="expenses"></div>
      <button class="add" id="addExpense" type="button">＋ 增加支出欄位</button>

      <div class="divider"></div>
      <div class="total-row">
        <div>金額</div>
        <div class="output" id="topTotal">0</div>
      </div>
    </section>

    <section class="difference">
      <div>
        <div class="title">正負</div>
        <div class="sub">上方金額 − 下方金額</div>
      </div>
      <div class="value" id="difference">0</div>
    </section>

    <section class="card">
      <div class="row">
        <div class="label">財務帳金額<small>（印出財務帳結帳單上的「現金金額」）</small></div>
        <input id="finance" inputmode="decimal" placeholder="0" aria-label="財務帳金額" />
      </div>

      <div class="card-inline">
        <div class="label">集點卡</div>
        <input id="cards" inputmode="numeric" placeholder="0" aria-label="集點卡張數" />
        <div class="unit">張</div>
        <div class="card-amount" id="cardAmount">0</div>
      </div>

      <div id="refunds"></div>
      <button class="add" id="addRefund" type="button">＋ 增加退費欄位</button>

      <div class="divider"></div>
      <div class="total-row">
        <div>金額</div>
        <div class="output" id="bottomTotal">0</div>
      </div>
    </section>

    <div class="footer">
      <button class="reset" id="reset" type="button">清空全部</button>
    </div>
  </main>

  <script>
    const $ = (id) => document.getElementById(id);
    const expenses = $('expenses');
    const refunds = $('refunds');
    let expenseCount = 0;
    let refundCount = 0;

    const num = (v) => {
      const n = Number(String(v).replace(/,/g, '').trim());
      return Number.isFinite(n) ? n : 0;
    };

    const count = (v) => Math.max(0, Math.floor(num(v)));

    const money = (n) => {
      const rounded = Math.round((n + Number.EPSILON) * 100) / 100;
      return rounded.toLocaleString('zh-TW', { maximumFractionDigits: 2 });
    };

    function addExpense(name = '') {
      expenseCount += 1;
      const row = document.createElement('div');
      row.className = 'expense-row';
      row.innerHTML = `
        <input class="expense-name" type="text" placeholder="支出${expenseCount}" value="${name}" aria-label="支出名稱">
        <input class="expense-value" inputmode="decimal" placeholder="0" aria-label="支出金額">
        <button class="remove" type="button" aria-label="刪除支出">×</button>
      `;
      row.querySelector('.expense-value').addEventListener('input', calc);
      row.querySelector('.remove').addEventListener('click', () => { row.remove(); calc(); });
      expenses.appendChild(row);
    }

    function addRefund(name = '') {
      refundCount += 1;
      const row = document.createElement('div');
      row.className = 'refund-row';
      row.innerHTML = `
        <input class="refund-name" type="text" placeholder="退費${refundCount}" value="${name}" aria-label="退費名稱">
        <input class="refund-value" inputmode="decimal" placeholder="0" aria-label="退費金額">
        <button class="remove" type="button" aria-label="刪除退費">×</button>
      `;
      row.querySelector('.refund-value').addEventListener('input', calc);
      row.querySelector('.remove').addEventListener('click', () => { row.remove(); calc(); });
      refunds.appendChild(row);
    }

    function calc() {
      const cash =
        count($('d1000').value) * 1000 +
        count($('d500').value) * 500 +
        count($('d100').value) * 100 +
        count($('d50').value) * 50 +
        count($('d10').value) * 10 +
        count($('d5').value) * 5 +
        count($('d1').value);

      const linepay = num($('linepay').value);
      const expenseSum = [...document.querySelectorAll('.expense-value')]
        .reduce((sum, el) => sum + num(el.value), 0);
      const top = cash + linepay + expenseSum;

      const finance = num($('finance').value);
      const cardCount = count($('cards').value);
      const cardAmount = cardCount * 50;
      const refundSum = [...document.querySelectorAll('.refund-value')]
        .reduce((sum, el) => sum + num(el.value), 0);
      const bottom = finance - cardAmount - refundSum;

      const diff = top - bottom;

      $('cash').textContent = money(cash);
      $('topTotal').textContent = money(top);
      $('cardAmount').textContent = money(cardAmount);
      $('bottomTotal').textContent = money(bottom);
      $('difference').textContent = diff > 0 ? `+${money(diff)}` : money(diff);
      $('difference').style.color = diff === 0 ? 'var(--ok)' : 'var(--bad)';
    }

    document.querySelectorAll('.denom').forEach(el => el.addEventListener('input', calc));
    ['linepay','finance','cards'].forEach(id => $(id).addEventListener('input', calc));
    $('addExpense').addEventListener('click', () => addExpense());
    $('addRefund').addEventListener('click', () => addRefund());

    $('reset').addEventListener('click', () => {
      document.querySelectorAll('.denom').forEach(el => el.value = '');
      ['linepay','finance','cards'].forEach(id => $(id).value = '');
      expenses.innerHTML = '';
      refunds.innerHTML = '';
      expenseCount = 0;
      refundCount = 0;
      addExpense('');
      addRefund('');
      calc();
    });

    addExpense('');
    addRefund('');
    calc();
  </script>
</body>
</html>
