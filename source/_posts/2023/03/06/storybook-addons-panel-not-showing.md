---
title: Storybook addons panel not showing
abbrlink: 60961
date: 2023-03-06 21:54:33
tags: Bug
categories: Storybook
thumbnail:
banner:
description:
---

<!-- @format -->

截至目前為止 Storybook 7.0.0-beta.58 仍會出現此 bug

<!-- more -->

Storybook 會突然出現其餘功能正常，但無法顯示 addons panel 的情況，如下:

![](addons-panel-not-show.png)

可透過以下指令於 Devtools Console 清除 localStorage

```js
localStorage.clear();
```

## 參考資料

[Addons panel not showing - but knobs are still working(?)](https://github.com/storybookjs/storybook/issues/8383)
[Features and behavior](https://storybook.js.org/docs/7.0/react/configure/features-and-behavior)
