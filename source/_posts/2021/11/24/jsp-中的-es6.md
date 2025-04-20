---
title: .jsp 中的 ES6+
abbrlink: 45063
date: 2021-11-24 17:44:36
tags:
  - JSP
  - JAVA
categories: ES6+
thumbnail:
banner:
description:
---

因公司現行專案架構為 JAVA Base 的前後端整合架構，因此前端開發都會寫在 `.jsp` 檔案內，而該檔案類型本來就有 JAVA 相關語法可以使用，所以在此紀錄與純前端開發語法上的不同之處

<!-- more -->

## Literals Template

在 ES6 的模板字串中，我們可以使用 \` 包住 `${}` 將變數帶入字串中，但剛好 `${}` 在 `.jsp` 檔案中也是帶入參數的保留符號，所以在 `.jsp` 中要使用模板字串時要在前方加上反斜線 `\` 以做區別

```jsp
// *.jsp

<script>
  const test = "123";
  console.log(`\${test}`);   // 123
</script>
```
