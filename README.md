<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculator — Anchal Pal</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Poppins', sans-serif;
      min-height: 100vh;
      background: #0d1b2a;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }
    .top-label {
      color: #4a90d9;
      font-size: 0.78rem;
      letter-spacing: 3px;
      text-transform: uppercase;
      margin-bottom: 18px;
      font-weight: 500;
    }
    .calc-wrap {
      background: #162032;
      border-radius: 28px;
      padding: 26px;
      width: 300px;
      box-shadow: 0 30px 70px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.05);
    }
    .screen {
      background: #0a1520;
      border-radius: 18px;
      padding: 22px 18px 14px;
      margin-bottom: 22px;
      text-align: right;
      min-height: 90px;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
    }
    .screen .hist {
      color: #3a6a9a;
      font-size: 0.82rem;
      min-height: 18px;
      margin-bottom: 8px;
      font-weight: 300;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .screen .main {
      color: #e8f4ff;
      font-size: 2.6rem;
      font-weight: 600;
      line-height: 1;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      letter-spacing: -1px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }
    .key {
      border: none;
      border-radius: 16px;
      font-family: 'Poppins', sans-serif;
      font-size: 1rem;
      font-weight: 500;
      padding: 17px 0;
      cursor: pointer;
      transition: transform 0.1s, filter 0.1s;
    }
    .key:active { transform: scale(0.91); filter: brightness(1.2); }
    .key:hover { filter: brightness(1.1); }
    .k-num { background: #1e3048; color: #d0e8ff; }
    .k-op  { background: #1a4070; color: #7ec8ff; }
    .k-eq  { background: linear-gradient(145deg, #1a6bbf, #0d4a8a); color: #fff; grid-column: span 2; font-size: 1.15rem; }
    .k-ac  { background: #3a1a1a; color: #ff7b7b; }
    .k-del { background: #2a1a2a; color: #c87bff; }
    .k-zero { grid-column: span 2; }
    .made-by {
      color: #1e3a55;
      font-size: 0.72rem;
      margin-top: 16px;
    }
  </style>
</head>
<body>
  <p class="top-label">✦ calculator</p>
  <div class="calc-wrap">
    <div class="screen">
      <div class="hist" id="hist"></div>
      <div class="main" id="main">0</div>
    </div>
    <div class="grid">
      <button class="key k-ac"  onclick="ac()">AC</button>
      <button class="key k-del" onclick="del()">⌫</button>
      <button class="key k-op"  onclick="inp('%')">%</button>
      <button class="key k-op"  onclick="inp('/')">÷</button>
      <button class="key k-num" onclick="inp('7')">7</button>
      <button class="key k-num" onclick="inp('8')">8</button>
      <button class="key k-num" onclick="inp('9')">9</button>
      <button class="key k-op"  onclick="inp('*')">×</button>
      <button class="key k-num" onclick="inp('4')">4</button>
      <button class="key k-num" onclick="inp('5')">5</button>
      <button class="key k-num" onclick="inp('6')">6</button>
      <button class="key k-op"  onclick="inp('-')">−</button>
      <button class="key k-num" onclick="inp('1')">1</button>
      <button class="key k-num" onclick="inp('2')">2</button>
      <button class="key k-num" onclick="inp('3')">3</button>
      <button class="key k-op"  onclick="inp('+')">+</button>
      <button class="key k-num k-zero" onclick="inp('0')">0</button>
      <button class="key k-num" onclick="inp('.')">.</button>
      <button class="key k-eq"  onclick="calc()">=</button>
    </div>
  </div>
  <p class="made-by">made by anchal pal</p>
  <script>
    let expr = '';
    function show() {
      document.getElementById('main').textContent = expr.replace(/\*/g,'×').replace(/\//g,'÷') || '0';
    }
    function inp(v) {
      const ops = ['+','-','*','/','%'];
      if (ops.includes(v) && ops.includes(expr.slice(-1))) expr = expr.slice(0,-1);
      expr += v;
      show();
    }
    function ac() {
      expr = '';
      document.getElementById('hist').textContent = '';
      document.getElementById('main').textContent = '0';
    }
    function del() { expr = expr.slice(0,-1); show(); }
    function calc() {
      if (!expr) return;
      try {
        let ans = Function('"use strict"; return (' + expr + ')')();
        if (!isFinite(ans)) { document.getElementById('main').textContent = 'Error'; expr=''; return; }
        if (ans % 1 !== 0) ans = parseFloat(ans.toFixed(9));
        document.getElementById('hist').textContent = expr.replace(/\*/g,'×').replace(/\//g,'÷') + ' =';
        document.getElementById('main').textContent = ans;
        expr = String(ans);
      } catch { document.getElementById('main').textContent = 'Error'; expr = ''; }
    }
    document.addEventListener('keydown', e => {
      if (e.key >= '0' && e.key <= '9') inp(e.key);
      else if (['+','-','*','/','%','.'].includes(e.key)) inp(e.key);
      else if (e.key === 'Enter' || e.key === '=') calc();
      else if (e.key === 'Backspace') del();
      else if (e.key === 'Escape') ac();
    });
  </script>
</body>
</html>
