---
title: Testing
abbrlink: 40383
date: 2022-09-03 11:36:24
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

對於測試，一開始可能會有個疑問是，在程式碼開發時，不是就同時一時在測試了嗎 ? 為什麼還會需要特別測試 ?

前端測試其實是透過撰寫一段程式碼，透過他來"自動"測試實際開發所需的程式碼，接下來進一步了解何謂前端測試

<!-- more -->

## What & Why?

在一般傳統開發時，其實在撰寫程式碼的當下就已經同步在做測試了，但這種測試方式其實是開發者以使用者的角度去做測試，往往會忽略其他的操作情境，尤其是如果有其他功能的調整，可能也只針對當下的區塊測試，一時之間也難以涵蓋所有系統關聯

為了解決這種開發者片面的測試方式，前端自動化測試因應而生，做事是透過額外撰寫一段程式碼，這段程式碼會自動測試實際產品的程式碼，並在每一次都會對流程做完整的測試，以涵蓋所有操作情境

## Understanding Defferent Kinds of Tests

前端自動化測試 Automated Tests 又根據測試的幅度分成以下 3 種:

### Uints Tests 單元測試

單元測試 Units Tests 顧名思義就是以最小單位來對程式碼進行測試，涵蓋的範圍通常也只侷限於單一的獨立功能，也是最常見的自動化測試，一個專案可能因應功能需求包含許多 Units Tests

### Integration Tests 整合測試

整合測試 Integration Tests 相對於單元測試所涵蓋的範圍就會在大一些，會組合多個單一的獨立功能，對整體的連動功能進行測試，所以相對單元測試在專案中的數量也會比較少一些

### End-to-End (E2E) Tests

所謂的 End-to-End (E2E) Tests 指的就是端點(使用者)到端點(系統)的測試，實際上就是模擬使用者的操作流程做自動化測試，所以 QA 直接透過滑鼠操作的測試就是人工的 E2E Tests

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
