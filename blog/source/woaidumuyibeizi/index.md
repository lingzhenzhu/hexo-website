---
title: 任意要求卡
date: 2023-09-24 11:03:07
---

<style>
    body {
        line-height: 1.6;
        max-width: 800px;
        margin: auto;
        padding: 20px;
    }
</style>

<!-- 顶部图片 -->
<div style="text-align:center; display:inline-block; max-width: 200px;">
    <img src="/image/card.jpg" alt="任意要求卡" 
         style="width:100%; display:block; border-radius:10px;">
</div>

<p>剩余次数：<span id="remaining-count">2</span> 次</p>
<p>签发人：朱苓榛老公</p>
<p>授予人：杜沐老婆</p>

<!-- 提交请求 -->
<div id="submit-container" style="background-color:#f5f5f5; padding:15px; margin-top:20px; border-radius:8px;">
    <h3 style="border-bottom: 2px solid #666; padding-bottom: 5px; color:#333;">提交请求</h3>
    <form id="request-form">
        <label for="request">尊敬的老婆大人，请输入你的具体要求：</label><br>
        <input type="text" id="request" required 
               style="width:85%; padding:10px; margin-top:10px; border:1px solid #ccc; border-radius:5px; font-size:16px; display:inline-block;"><br>
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
    const requestForm = document.getElementById("request-form");
    const requestInput = document.getElementById("request");
    const requestList = document.getElementById("request-list");
    const countElement = document.getElementById("remaining-count");

    const adminContainer = document.getElementById("admin-container");
    const adminLogin = document.getElementById("admin-login");
    const adminPasswordInput = document.getElementById("admin-password");
    const adminLoginBtn = document.getElementById("admin-login-btn");

    const WORKER_BASE = "https://firebase-proxy.lzzhu718.workers.dev"; // ← 替换为你的 Worker 地址

    // ✅ 获取当前状态（剩余次数 + 历史记录）
    async function refreshState() {
        const res = await fetch(`${WORKER_BASE}/get-state`);
        if (!res.ok) {
            alert("无法获取状态");
            return;
        }

        const data = await res.json();
        countElement.textContent = data.remainingCount;
        requestList.innerHTML = "";
        data.requests.forEach(text => appendRequestToList(text));
    }

    function appendRequestToList(text) {
        const li = document.createElement("li");
        li.textContent = text;
        li.style.background = "#d9d9d9";
        li.style.padding = "8px";
        li.style.margin = "5px 0";
        li.style.borderRadius = "5px";
        requestList.appendChild(li);
    }

    requestForm.addEventListener("submit", async function (event) {
        event.preventDefault();
        const requestText = requestInput.value.trim();
        if (requestText === "") {
            alert("请输入具体要求！");
            return;
        }

        const res = await fetch(`${WORKER_BASE}/write-request`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ request: requestText })
        });

        const result = await res.text();
        if (res.ok) {
            requestInput.value = "";
            refreshState();
        } else {
            alert("提交失败：" + result);
        }
    });

    // ✅ 管理员登录
    adminLoginBtn.addEventListener("click", function () {
        if (adminPasswordInput.value === "admin123") {
            adminContainer.style.display = "block";
            adminLogin.style.display = "none";
        } else {
            alert("密码错误！");
        }
    });

    // ✅ 管理员操作
    document.getElementById("increase-count").addEventListener("click", () => adjustCount(1));
    document.getElementById("decrease-count").addEventListener("click", () => adjustCount(-1));
    document.getElementById("reset-count").addEventListener("click", () => setCount(2));
    document.getElementById("clear-records").addEventListener("click", () => {
        fetch(`${WORKER_BASE}/clear`, { method: "POST" }).then(refreshState);
    });

    async function adjustCount(delta) {
        await fetch(`${WORKER_BASE}/adjust-count`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ delta })
        });
        refreshState();
    }

    async function setCount(value) {
        await fetch(`${WORKER_BASE}/set-count`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ count: value })
        });
        refreshState();
    }

    // ✅ 页面加载时获取初始状态
    refreshState();
</script>




