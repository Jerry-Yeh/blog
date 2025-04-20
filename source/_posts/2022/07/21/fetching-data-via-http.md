---
title: Fetching Data via Http
abbrlink: 1097
date: 2022-07-21 00:10:54
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將透過 firebase 所提供的簡易資料庫與 API 來實際在 React 專案中串接應用

<!-- more -->

一筆簡單的陣列資料，並透過這筆資料產生 JSX，如下:

```js
const AvailableMeals = () => {
  const meals = [...];

  const mealsList = meals.map((meal) => (
    <MealItem
      key={meals.id}
      id={meals.id}
      name={meal.name}
      description={meal.description}
      price={meal.price}
    />
  ));

  return (
    <section className={classes.meals}>
      <Card>
        <ul>{mealsList}</ul>
      </Card>
    </section>
  );
};
```

## Fetching Data

在串接 firebase 所提供的 API 時，需使用 http 或是 在 url 最後方加上 .json，否則會出現 CORS error，而因為 http request 是一個 async event，所以須透過 State 來存取資料 ; 而像是這種在頁面載入時，就會自動送出 request 取得資料的情況，就可以使用 `useEffect()`，並使 dependency array 為 empty array，這麼一來就只有在 component first render 時會觸發

```js
const [meals, setMeals] = useState([]);

useEffect(async () => {
  const response = await fetch(
    "https://react-http-77951-default-rtdb.firebaseio.com/meals.json"
  );
  const data = await response.json();
  const loadedMeals = [];

  for (const key in data) {
    loadedMeals.push({
      id: key,
      name: data[key].name,
      description: data[key].description,
      price: data[key].price,
    });
  }

  setMeals(loadedMeals);
}, []);
```

但這裡要特別注意的是，`useEffect()` 的 callback function 只能是一個 sync funcion，如果要在其中做到 async event，就必須透過另一層 async function 來封裝

```js
const [meals, setMeals] = useState([]);

useEffect(() => {
  const fetchMeals = () => {
    const response = await fetch(
      "https://react-http-77951-default-rtdb.firebaseio.com/meals.json"
    );
    const data = await response.json();
    const loadedMeals = [];

    for (const key in data) {
      loadedMeals.push({
        id: key,
        name: data[key].name,
        description: data[key].description,
        price: data[key].price,
      });
    }

    setMeals(loadedMeals);
  }

  fetchMeals();
}, []);
```

而如果這個 async function 其實只有在 `useEffect()` 中呼叫一次，也可以改寫成 IIFE

```js
useEffect(() => {
  (async () => {
    const response = await fetch(
      "https://react-http-77951-default-rtdb.firebaseio.com/meals.json"
    );
    const data = await response.json();
    const loadedMeals = [];

    for (const key in data) {
      loadedMeals.push({
        id: key,
        name: data[key].name,
        description: data[key].description,
        price: data[key].price,
      });
    }

    setMeals(loadedMeals);
  })();
}, []);
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
