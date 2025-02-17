---
title: 任意要求卡
date: 2023-09-24 11:03:07
---

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
    document.addEventListener("DOMContentLoaded", function () {
        let maxAttempts = 2;  
        let remainingCount = localStorage.getItem("remainingCount") ? parseInt(localStorage.getItem("remainingCount")) : maxAttempts;
        const countElement = document.getElementById("remaining-count");
        const form = document.getElementById("request-form");
        const requestList = document.getElementById("request-list");
        const adminContainer = document.getElementById("admin-container");
        const adminLogin = document.getElementById("admin-login");
        const adminPasswordInput = document.getElementById("admin-password");
        const adminLoginBtn = document.getElementById("admin-login-btn");

        // 读取并恢复存储的申请记录
        let savedRequests = localStorage.getItem("requestList");
        if (savedRequests) {
            requestList.innerHTML = savedRequests;
        }

        countElement.textContent = remainingCount;

        form.addEventListener("submit", function (event) {
            event.preventDefault();
            if (remainingCount <= 0) {
                alert("已达到最大使用次数，无法申请！");
                return;
            }

            const requestInput = document.getElementById("request");
            const requestText = requestInput.value.trim();
            if (requestText === "") {
                alert("请输入具体要求！");
                return;
            }

            // 添加请求记录
            const listItem = document.createElement("li");
            listItem.textContent = requestText;
            listItem.style.background = "#d9d9d9";
            listItem.style.padding = "8px";
            listItem.style.margin = "5px 0";
            listItem.style.borderRadius = "5px";
            requestList.appendChild(listItem);

            // 存储申请记录到 localStorage
            localStorage.setItem("requestList", requestList.innerHTML);

            // 更新剩余次数并存储
            remainingCount--;
            localStorage.setItem("remainingCount", remainingCount);
            countElement.textContent = remainingCount;

            // 禁用表单
            if (remainingCount === 0) {
                form.innerHTML = "<p style='color:red;'>已达到最大使用次数，无法再申请。</p>";
            }

            requestInput.value = "";
        });

        // 管理员登录
        adminLoginBtn.addEventListener("click", function () {
            if (adminPasswordInput.value === "admin123") { // 你可以修改密码
                adminContainer.style.display = "block";
                adminLogin.style.display = "none";
            } else {
                alert("密码错误！");
            }
        });

        // 管理员操作
        document.getElementById("increase-count").addEventListener("click", function () {
            remainingCount++;
            localStorage.setItem("remainingCount", remainingCount);
            countElement.textContent = remainingCount;
        });

        document.getElementById("decrease-count").addEventListener("click", function () {
            if (remainingCount > 0) {
                remainingCount--;
                localStorage.setItem("remainingCount", remainingCount);
                countElement.textContent = remainingCount;
            }
        });

        document.getElementById("reset-count").addEventListener("click", function () {
            remainingCount = maxAttempts;
            localStorage.setItem("remainingCount", remainingCount);
            countElement.textContent = remainingCount;
        });

        document.getElementById("clear-records").addEventListener("click", function () {
            requestList.innerHTML = "";
            localStorage.removeItem("requestList"); // 清除 localStorage 里的申请记录
        });
    });
</script>

