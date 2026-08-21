---
title: 从0到1用AI开发工具/系统部署到CF worker
date: 2026-07-01
categories:
  - vibe coding
tags:
  - AI工具
---

### 大纲
第 1 轮：贴技术要求 + 一句话业务目标 → 让 AI 出产品 spec（先不写代码）
第 2 轮：你确认/修改 spec → 让 AI 出 Phase 计划和页面清单
第 3 轮：确认后 → 从 Phase 0 + Phase 1 开始写代码
第 4 轮：MVP 能跑后 → Phase 1.5 手机预览 / Phase 2 后端
<!--more-->
### 一个示例
你对AI说
```
[粘贴：CF Worker 通用技术要求]

我要做「家庭日记」PWA，2人家庭私有使用，中文界面，手机为主。
功能：写日记、配图、标签、心情、日历回顾、搜索；支持仅自己可见 / 家庭共享。
先不做第三方登录和公开分享等。

请先输出产品 spec（不写代码），我确认后再从 Phase 0 做到 Phase 1 mock MVP。
```
AI 给出 spec 后你说：
```
可以，按这个 spec 执行 Phase 0 和 Phase 1。默认两人日记互不可见，除非标记「家庭共享」。
```
AI 做完后你说：
```
给我本地启动命令、demo 账号、手机访问方式。
```



### 附录[CF Worker通用技术要求]

```
你是一个资深全栈工程师和产品架构师。请帮我设计并从零构建一个私有部署的家庭记账 PWA。
背景：
- 我和我老婆两人使用，需要区分个人/共同账户、信用卡、投资、房产、房贷
- 私有部署到 Cloudflare（Workers、D1、R2、Pages 等），Windows 本地开发
- 手机为主，PWA，UI 参考「图图记账」的轻量清晰风格，但不要抄品牌素材
- 默认报表币种 CNY，时区 Asia/Jakarta
- 账户涉及：国内银行/支付宝/微信，以及 BCA、OCBC、Wise、Grab；投资在长桥证券
核心功能：
- 登录、概览（净资产第一屏+折线图）、快速记账、流水、账户管理
- 待确认交易（Gmail/截图/快捷指令导入默认 pending_review，人工确认后才入账）
- 投资持仓和行情、房产和房贷、AI 财务分析、设置导出备份
- iOS 快捷指令和 Action Button 截图记账
原则：
- 准确性优先于自动化；所有改动有 audit log
- 不存银行/Gmail 等明文密码；应用内账密登录
- 先做 MVP（mock Gmail/长桥/AI），再扩展真实 API
请输出一份完整、可直接交给 AI coding agent 执行的开发规格，包含：
技术栈（TypeScript monorepo、React、Vite、Hono、Drizzle、D1）、
目录结构、页面路由、领域规则、数据表、API、seed 数据、测试要求、
Phase 0-6 开发计划、文档清单和验收标准。

```