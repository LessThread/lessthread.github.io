---
title: vite路径过深导致打包错误
date: 2025-04-24 20:11:02
tags: ["前端"]
categories: ["踩坑合集"]
description: vite路径过深导致打包错误
---

## 系统环境
- Windows 10 / Windows 11
- Node v20.10.0
- "vue": "3.5.13"
- "vite": "6.1.0"
- "tsx": "4.19.2",

## 问题表现
```bash
> vite build --mode prod

Building [████████████████████████░░░░░░░░░░░░░░░░] 60% | Time: 5.6s
Build failed. Please check the error message

error during build:
[vite:vue-jsx] Cannot find package 'D:\code\graduation-design\code\project-vue\soybean-admin-antd\node_modules\.pnpm\@vitejs+plugin-vue-jsx@4.1.1_vite@6.1.0_@types+node@22.13.1_jiti@2.4.2_sass@1.84.0_tsx@4.19.2_eu2zzdc4k7cexl2tg3rvqfswxm\node_modules\@babel\plugin-transform-typescript\package.json' imported from D:\code\graduation-design\code\project-vue\soybean-admin-antd\node_modules\.pnpm\@vitejs+plugin-vue-jsx@4.1.1_vite@6.1.0_@types+node@22.13.1_jiti@2.4.2_sass@1.84.0_tsx@4.19.2_eu2zzdc4k7cexl2tg3rvqfswxm\node_modules\@vitejs\plugin-vue-jsx\dist\index.mjs
Did you mean to import @babel+plugin-transform-typescript@7.27.0_@babel+core@7.26.8/node_modules/@babel/plugin-transform-typescript/lib/index.js?
file: D:/code/graduation-design/code/project-vue/soybean-admin-antd/src/views/manage/menu/index.vue?vue&type=script&setup=true&lang.tsx
    at legacyMainResolve (node:internal/modules/esm/resolve:205:26)
    at packageResolve (node:internal/modules/esm/resolve:831:14)
    at moduleResolve (node:internal/modules/esm/resolve:901:20)
    at defaultResolve (node:internal/modules/esm/resolve:1121:11)
    at nextResolve (node:internal/modules/esm/hooks:865:28)
    at resolve (file:///D:/code/graduation-design/code/project-vue/soybean-admin-antd/node_modules/.pnpm/tsx@4.19.2/node_modules/tsx/dist/esm/index.mjs?1745914357203:2:4365)
    at nextResolve (node:internal/modules/esm/hooks:865:28)
    at resolve (file:///D:/code/graduation-design/code/project-vue/soybean-admin-antd/node_modules/.pnpm/tsx@4.19.2/node_modules/tsx/dist/esm/index.mjs?1745914357321:2:4365)
    at nextResolve (node:internal/modules/esm/hooks:865:28)
    at Hooks.resolve (node:internal/modules/esm/hooks:303:30)
 ELIFECYCLE  Command failed with exit code 1.
```

## 解决方案
换电脑的时候放到了`目录更浅的文件夹`,然后正常执行了;  
然后原环境换了也方到目录更浅的文件夹,也正常打包了......


## 原理
后来连同 node_modules 复制到其他文件夹的时候 Windows 出现
![文件名对目标文件夹可能太长,你可以缩短文件名重试,或者尝试路径较短的位置](https://pic.imgdd.cc/item/68138ffe3c3a6234d310af5a.png)  

应该就是Windows的问题了
