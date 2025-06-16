<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>任意要求卡</title>
  <style>
    body {
      line-height: 1.6;
      max-width: 800px;
      margin: auto;
      padding: 20px;
    }
  </style>
</head>
<body>

<!-- 顶部图片 -->
<div style="text-align:center; display:inline-block; max-width: 200px;">
  <img src="/image/card.jpg" alt="任意要求卡" 
       style="width:100%; display:block; border-radius:10px;">
</div>

<p>剩余次数：<span id="remaining-count">加载中...</span></p>
<p>签发人：朱苓榛老公</p>
<p>授予人：杜沐老婆</p>

<!-- 提交请求 -->
<div id="submit-container" style="background-color:#f5f5f5; padding:15px; margin-top:20px; border-radius:8px;">
  <h3 style="border-bottom: 2px solid #666; padding-bottom: 5px; color:#333;">提交请求</h3>
  <form id="request-form">
    <label for="request">尊敬的老婆大人，请输入你的具体要求：</label><br>
    <input type="text" id="request" required 
           style="width:85%; padding:10px; margin-top:10px; border:1px solid #ccc; border-radius:5px; font-size:16px;"><br>
    <button type="submit" style="
        background-color: #1B5E20; 
        color: white; 
        border: none; 
        padding: 12px 20px; 
        font-size: 16px; 
        font-weight: bold; 
        border-radius: 5px; 
        cursor: pointer; 
        margin-top: 10px;
        transition: all 0.3s ease;">
        提交申请
    </button>
  </form>
</div>

<!-- 申请记录 -->
<div id="history-container" style="background-color:#f5f5f5; padding:15px; margin-top:20px; border-radius:8px;">
  <h3 style="border-bottom: 2px solid #666; padding-bottom: 5px; color:#333;">申请记录</h3>
  <ul id="request-list" style="list-style:none; padding:0;"></ul>
</div>

<!-- 管理员登录 -->
<div id="admin-login" style="text-align:center; margin-top:20px;">
  <label for="admin-password">管理员密码：</label>
  <input type="password" id="admin-password" style="padding:8px; border:1px solid #aaa; border-radius:5px;">
  <button id="admin-login-btn" style="background:#FF9800; color:white; padding:8px 12px; border:none; border-radius:5px; cursor:pointer;">登录</button>
</div>

<!-- 管理员操作界面 -->
<div id="admin-container" style="display:none; margin-top:20px; padding:15px; background:#ddd; border-radius:8px;">
  <h3 style="color:#222;">管理员操作</h3>
  <button id="increase-count" style="background:#388E3C; color:white; padding:10px; border:none; border-radius:5px; cursor:pointer;">增加次数</button>
  <button id="decrease-count" style="background:#D32F2F; color:white; padding:10px; border:none; border-radius:5px; cursor:pointer;">减少次数</button>
  <button id="reset-count" style="background:#1976D2; color:white; padding:10px; border:none; border-radius:5px; cursor:pointer;">重置次数</button>
  <button id="clear-records" style="background:#616161; color:white; padding:10px; border:none; border-radius:5px; cursor:pointer;">清除申请记录</button>
</div>

<script>
const API_BASE = "https://firebase-proxy.lzzhu718.workers.dev"; // 替换为你自己的 Worker 地址

// 刷新状态
async function refreshState() {
  try {
    const res = await fetch(`${API_BASE}/get-state`);
    const data = await res.json();
    document.getElementById("remaining-count").innerText = data.count;

    const requestList = document.getElementById("request-list");
    requestList.innerHTML = "";
    data.requests.forEach((text) => {
      const li = document.createElement("li");
      li.innerText = text;
      li.style.cssText = "background:#d9d9d9;padding:8px;margin:5px 0;border-radius:5px;";
      requestList.appendChild(li);
    });
  } catch (err) {
    console.error("状态获取失败", err);
    document.getElementById("remaining-count").innerText = "加载失败";
  }
}

// 用户提交请求
document.getElementById("request-form").addEventListener("submit", async (event) => {
  event.preventDefault();
  const requestText = document.getElementById("request").value.trim();
  if (!requestText) return alert("请输入具体要求！");
  const res = await fetch(`${API_BASE}/submit`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ requestText })
  });
  if (!res.ok) {
    alert("提交失败：" + await res.text());
    return;
  }
  document.getElementById("request").value = "";
  refreshState();
});

// 管理员登录
document.getElementById("admin-login-btn").addEventListener("click", () => {
  if (document.getElementById("admin-password").value === "admin123") {
    document.getElementById("admin-container").style.display = "block";
    document.getElementById("admin-login").style.display = "none";
  } else {
    alert("密码错误！");
  }
});

// 管理员操作按钮
document.getElementById("increase-count").addEventListener("click", async () => {
  await fetch(`${API_BASE}/admin/increase`, { method: "POST" });
  refreshState();
});
document.getElementById("decrease-count").addEventListener("click", async () => {
  await fetch(`${API_BASE}/admin/decrease`, { method: "POST" });
  refreshState();
});
document.getElementById("reset-count").addEventListener("click", async () => {
  await fetch(`${API_BASE}/admin/reset`, { method: "POST" });
  refreshState();
});
document.getElementById("clear-records").addEventListener("click", async () => {
  await fetch(`${API_BASE}/admin/clear`, { method: "POST" });
  refreshState();
});

// 页面加载完成后拉取状态
refreshState();
</script>

</body>
</html>
