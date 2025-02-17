---
title: 任意要求卡
date: 2023-09-24 11:03:07
---

<!-- 顶部图片 -->
<div style="text-align:center;">
    <img src="/images/card.jpg" alt="任意要求卡" style="width:100%; max-width:600px; display:block; margin:0 auto 20px; border-radius:10px;">
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

<script>
    document.addEventListener("DOMContentLoaded", function () {
        let remainingCount = 2;  
        const countElement = document.getElementById("remaining-count");
        const form = document.getElementById("request-form");
        const requestList = document.getElementById("request-list");

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

            // 更新剩余次数
            remainingCount--;
            countElement.textContent = remainingCount;

            // 禁用表单
            if (remainingCount === 0) {
                form.innerHTML = "<p style='color:red;'>已达到最大使用次数，无法再申请。</p>";
            }

            requestInput.value = "";
        });
    });
</script>
