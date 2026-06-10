<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>量化系统迁移公告</title>

<style>
    body {
        margin: 0;
        font-family: "PingFang SC", "Microsoft YaHei", Arial;
        background: radial-gradient(circle at top, #0b1220, #050814);
        color: #e5e7eb;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .container {
        width: 92%;
        max-width: 640px;
        background: rgba(255,255,255,0.04);
        border: 1px solid rgba(255,255,255,0.08);
        border-radius: 16px;
        padding: 34px;
        box-shadow: 0 0 40px rgba(0,0,0,0.6);
        backdrop-filter: blur(10px);
        text-align: center;
    }

    .tag {
        display: inline-block;
        padding: 4px 10px;
        background: rgba(56,189,248,0.15);
        color: #38bdf8;
        border-radius: 999px;
        font-size: 12px;
        margin-bottom: 18px;
    }

    .title {
        font-size: 24px;
        font-weight: bold;
        margin-bottom: 10px;
    }

    .desc {
        font-size: 14px;
        line-height: 1.7;
        color: #cbd5e1;
        margin-top: 10px;
    }

    .site {
        margin-top: 22px;
        font-size: 24px;
        color: #38bdf8;
        font-weight: bold;
        letter-spacing: 0.5px;
    }

    .status {
        margin-top: 18px;
        font-size: 13px;
        color: #94a3b8;
    }

    .dot {
        display: inline-block;
        width: 10px;
        height: 10px;
        background: #22c55e;
        border-radius: 50%;
        margin-right: 6px;
        animation: pulse 1s infinite;
        vertical-align: middle;
    }

    @keyframes pulse {
        0% { transform: scale(1); opacity: 1; }
        50% { transform: scale(1.5); opacity: 0.4; }
        100% { transform: scale(1); opacity: 1; }
    }

    .btn {
        margin-top: 26px;
        display: inline-block;
        padding: 12px 20px;
        background: #38bdf8;
        color: #0b1220;
        border-radius: 10px;
        font-weight: bold;
        cursor: pointer;
        user-select: none;
        transition: 0.2s;
    }

    .btn:hover {
        background: #0ea5e9;
    }

    .copy-tip {
        margin-top: 12px;
        font-size: 12px;
        color: #94a3b8;
    }

    .footer {
        margin-top: 22px;
        font-size: 12px;
        color: #64748b;
    }

    .toast {
        position: fixed;
        bottom: 30px;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.8);
        color: #fff;
        padding: 10px 16px;
        border-radius: 8px;
        font-size: 13px;
        display: none;
    }
</style>
</head>

<body>

<div class="container">

    <div class="tag">SYSTEM MIGRATION</div>

    <div class="title">量化交易系统迁移公告</div>

    <div class="desc">
        为提升系统稳定性与执行效率，本量化系统已完成全新架构升级与节点迁移。
        <br>当前所有服务已迁移至新域名，请手动复制访问。
    </div>

    <div class="site">666543.cc</div>

    <div class="status">
        <span class="dot"></span>
        系统状态：运行正常 · 已完成迁移
    </div>

    <div class="btn" onclick="copyText()">
        点击复制新域名
    </div>

    <div class="copy-tip">
        点击后自动复制到剪贴板
    </div>

    <div class="footer">
        © Quant System · Migration Completed
    </div>

</div>

<div class="toast" id="toast">已复制到剪贴板</div>

<script>
function copyText() {
    const text = "666543.cc";
    navigator.clipboard.writeText(text).then(() => {
        const toast = document.getElementById("toast");
        toast.style.display = "block";

        setTimeout(() => {
            toast.style.display = "none";
        }, 1500);
    });
}
</script>

</body>
</html>
