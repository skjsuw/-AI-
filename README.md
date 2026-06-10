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
        text-align: center;
        box-shadow: 0 0 40px rgba(0,0,0,0.6);
        backdrop-filter: blur(10px);
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
    }

    .site {
        margin-top: 25px;
        font-size: 26px;
        color: #38bdf8;
        font-weight: bold;
        user-select: all;
        padding: 10px;
        border: 1px dashed rgba(56,189,248,0.5);
        border-radius: 10px;
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
    }

    .tip {
        margin-top: 10px;
        font-size: 12px;
        color: #94a3b8;
    }

    .toast {
        position: fixed;
        bottom: 30px;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.85);
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
        系统已完成升级与节点迁移，请复制下方新域名访问。
    </div>

    <!-- 可手动长按复制（手机兜底方案） -->
    <div class="site" id="text">666543.cc</div>

    <div class="status">
        <span class="dot"></span>
        系统状态：运行正常 · 已迁移完成
    </div>

    <div class="btn" onclick="copyText()">点击复制域名</div>

    <div class="tip">如果复制失败，可长按上方域名手动复制</div>

</div>

<div class="toast" id="toast">复制成功</div>

<script>
function copyText() {
    const text = "666543.cc";

    // 方案1：现代浏览器
    if (navigator.clipboard) {
        navigator.clipboard.writeText(text).then(showToast)
        .catch(fallbackCopy);
    } else {
        fallbackCopy();
    }
}

// 方案2：兼容微信/旧浏览器
function fallbackCopy() {
    const input = document.createElement("input");
    input.value = "666543.cc";
    document.body.appendChild(input);
    input.select();
    input.setSelectionRange(0, 99999);
    document.execCommand("copy");
    document.body.removeChild(input);
    showToast();
}

function showToast() {
    const toast = document.getElementById("toast");
    toast.style.display = "block";

    setTimeout(() => {
        toast.style.display = "none";
    }, 1500);
}
</script>

</body>
</html>
