---
title: About
date: 2023-09-29 14:01:22
tags: 
layout: page
---

<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
</script>

# 关于
## 这是什么?
这里是 LessThread 的博客。


<span id="busuanzi_container_site_pv">
    这里的文章被阅读过<span id="busuanzi_value_site_pv"></span>次
</span>

<script>
      function calculateDays() {
        var currentDate = new Date();
        var targetDate = new Date("2022-10-01");
        var timeDiff = Math.abs(currentDate.getTime() - targetDate.getTime());
        var daysPassed = Math.ceil(timeDiff / (1000 * 3600 * 24));
        document.getElementById("days").innerHTML = "白驹过隙,这是开始记录的第" + daysPassed + "天";
      }
</script>
<body onload="calculateDays()">
<p id="days"></p>
</body>