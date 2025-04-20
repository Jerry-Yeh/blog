---
title: Adding a Chart
abbrlink: 34522
date: 2022-06-12 15:25:47
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這堂課將加上圖表來統計每個月份的總開銷，並根據資料動態改變 CSS Style

<!-- more -->

首先，新增一組 Component Bar.js，並加上基本結構如下:

```js
// Bar.js
const Chart = (props) => {
  return (
    <div className="chart">
      {props.dataPoints.map((dataPoint) => (
        <ChartBar
          key={dataPoint.label}
          value={dataPoint.value}
          label={dataPoint.label}
          maxValue={null}
        />
      ))}
    </div>
  );
};
```

## Adding Dynamic Styles

接著新增一組 Component ChartBar.js，其基本結構如下:

```js
const ChartBar = (props) => {
  return (
    <div className="chart-bar">
      <div className="chart-bar__inner">
        <div className="chart-bar__fill"></div>
      </div>
      <div className="chart-bar__label">{props.label}</div>
    </div>
  );
};
```

接著我們需要計算 props 進來的值，藉此動態顯示對應的 CSS Style ; 在 React 中，如果要在 JSX 加上 `style`，其中的值為一個 object {}

```js
const ChartBar = (props) => {
  let barFillHeight = '0%';

  if (props.max > 0) {
    barFillHeight = Math.round((props.value / props.max) * 100) + '%';
  }

  return (
    <div className="chart-bar">
      <div className="chart-bar__inner">
        <div className="chart-bar__fill" style={{height: barFillHeight}}></div>
      </div>
      <div className="chart-bar__label">{props.label}</div>
    </div>
  );
};
```

如果遇到 `background-color` 這種多字元的屬性，則須要透過引號 '' 來包住它，或是透過駝峰式 Camel Case 來表示

```js
// Quatation Marks
<div className="chart-bar__fill" style={{'background-color': 'red'}}></div>

// Camel Case
<div className="chart-bar__fill" style={{backgroundColor: 'red'}}></div>
```

## Wrap Up & Next Steps

接著，我們可以在 Expense 資料節中新增一個 Component ExpenseChar 來導入 Component Chart，並帶入過濾後的資料

```js
// Expenses.js

return (
  <ExpenseChart expenses={filteredExpenses}>
);
```

有了這些資料後，便可以對資料進行加總統計，並將資料整理成 Component Chart 需要的格式

```js
// ExpenseChart.js

const ExpenseChart = (props) => {
  const chartDataPoints = [
    { label: "Jan", value: 0 },
    { label: "Feb", value: 0 },
    { label: "Mar", value: 0 },
    { label: "Apr", value: 0 },
    { label: "May", value: 0 },
    { label: "Jun", value: 0 },
    { label: "Jul", value: 0 },
    { label: "Aug", value: 0 },
    { label: "Sep", value: 0 },
    { label: "Oct", value: 0 },
    { label: "Nov", value: 0 },
    { label: "Dec", value: 0 },
  ];

  for (const expense of props.expenses) {
    const expenseMonth = expense.date.getMonth(); // starting at 0 => January => 0
    chartDataPoints[expenseMonth].value += expense.amount; 
  }

  return <Chart dataPoints={chartDataPoints} />;
};
```

最後就可藉由這些資料取得最大值作為 100% 的基準了

```js
// Chart.js
const Chart = (props) => {
  const dataPointValues = props.dataPoints.map(dataPoint => dataPoint.value);
  const totalMaximum = Math.max(...dataPointValues);

  return (
    <div className="chart">
      {props.dataPoints.map((dataPoint) => (
        <ChartBar
          key={dataPoint.label}
          value={dataPoint.value}
          label={dataPoint.label}
          maxValue={totalMaximum}
        />
      ))}
    </div>
  );
};
```
## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
[Github](https://github.com/Jerry-Yeh/react-complete-guide/commit/d1a9c9979618b9f30b7e8a19c956a9b6de0f384a)