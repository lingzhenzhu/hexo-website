<!DOCTYPE html>
<html lang="zh-CN">
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
    <img src="/image/card.jpg" alt="任意要求卡" style="width:100%; display:block; border-radius:10px;">
  </div>

  <p>剩余次数：<span id="remaining-count">-</span> 次</p>
  <p>签发人：朱苓榛老公</p>
  <p>授予人：杜沐老婆</p>

  <!-- 提交请求 -->
  <div id="submit-container" style="background-color:#f5f5f5; padding:15px; margin-top:20px; border-radius:8px;">
    <h3 style="border-bottom: 2px solid #666; padding-bottom: 5px; color:#333;">提交请求</h3>
    <form id="request-form">
      <label for="request">尊敬的老婆大人，请输入你的具体要求：</label><br>
      <input type="text" id="request" required style="width:85%; padding:10px; margin-top:10px; border:1px solid #ccc; border-radius:5px; font-size:16px;"><br>
      <button type="submit" style="background-color:#1B5E20; color:white; border:none; padding:12px 20px; font-size:16px; font-weight:bold; border-radius:5px; cursor:pointer; margin-top:10px;">
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

  <!-- 管理员操作 -->
  <div id="admin-container" style="display:none; margin-top:20px; padding:15px; background:#ddd; border-radius:8px;">
    <h3 style="color:#222;">管理员操作</h3>
    <button class="admin-btn" data-action="increase" style="background:#388E3C;">增加次数</button>
    <button class="admin-btn" data-action="decrease" style="background:#D32F2F;">减少次数</button>
    <button class="admin-btn" data-action="reset" style="background:#1976D2;">重置次数</button>
    <button class="admin-btn" data-action="clear" style="background:#616161;">清除申请记录</button>
  </div>

  <script>
    const API_BASE = "https://firebase-proxy.lzzhu718.workers.dev";

    async function refreshState() {
      try {
        const res = await fetch(`${API_BASE}/get-state`);
        const data = await res.json();
        document.getElementById("remaining-count").innerText = data.remainingCount;

        const list = document.getElementById("request-list");
        list.innerHTML = "";
        data.requests.forEach(req => {
          const li = document.createElement("li");
          li.innerText = req;
          li.style.background = "#d9d9d9";
          li.style.padding = "8px";
          li.style.margin = "5px 0";
          li.style.borderRadius = "5px";
          list.appendChild(li);
        });
      } catch (err) {
        console.error("刷新失败：", err);
      }
    }

    document.getElementById("request-form").addEventListener("submit", async (e) => {
      e.preventDefault();
      const request = document.getElementById("request").value.trim();
      if (!request) return alert("请输入内容");

      const res = await fetch(`${API_BASE}/submit`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ request }),
      });

      if (res.ok) {
        document.getElementById("request").value = "";
        refreshState();
      } else {
        alert("提交失败");
      }
    });

    document.getElementById("admin-login-btn").addEventListener("click", () => {
      const pwd = document.getElementById("admin-password").value;
      if (pwd === "admin123") {
        document.getElementById("admin-container").style.display = "block";
        document.getElementById("admin-login").style.display = "none";
      } else {
        alert("密码错误！");
      }
    });

    document.querySelectorAll(".admin-btn").forEach(btn => {
      btn.addEventListener("click", async () => {
        const action = btn.getAttribute("data-action");
        const res = await fetch(`${API_BASE}/admin/${action}`, { method: "POST" });
        if (res.ok) refreshState();
        else alert("操作失败");
      });
    });

    window.addEventListener("DOMContentLoaded", refreshState);
  </script>
</body>
</html>
