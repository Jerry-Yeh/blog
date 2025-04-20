---
title: Custom Hooks
abbrlink: 3141
date: 2022-07-12 23:38:41
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

<!-- @format -->

在前面的章節中，介紹了不少 React hooks，這章節將介紹另一個 React 核心觀念，那就是我們其實也可以客製化 hooks!

<!-- more -->

## What are Custom Hooks ?

Custom Hooks 其實就是一般的 function，只是差別在於，它是一個 stateful 的 reusable function，而且可以在其中使用其他的 React hooks，不同於 component function 在使用 hooks 時，只能於最外層直接使用，無法使用於 scope 內，如 function、for...

## Creating a Custom React Hook function

那麼在什麼時候會需要用到 custom hook 呢 ? 這個需求其實和一般開發時的思維一樣，凡是遇到相同需求的程式碼，都會盡可能的封裝它，使它可以被重複使用，也避免同一份程式碼反覆出現在不同的地方 ; 同理，custom hook 其實就是提供了一種方式，使我們可以封裝包含 React hook 的程式碼，使其在避免破壞 hook 使用規則下達到封裝的目的。

兩個非常類似的 component，一個是遞增的 counter，一個則是遞減，如下:

```js
const ForwardCounter = () => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCounter((prevCounter) => prevCounter + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return <Card>{counter}</Card>;
};

const BackwardCounter = () => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCounter((prevCounter) => prevCounter - 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return <Card>{counter}</Card>;
};
```

而如果直接封裝 component 中的程式碼，就會違背 hook 的使用守則，那就是只能在 top level 中使用，因此需要將其封裝成 custom hook，其使用方式就是透過 `use` 開頭的 function，以便讓 React 知道它是一個 hook

```js
const useCounter = () => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCounter((prevCounter) => prevCounter - 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);
};
```

### Using Custom Hook

有了 custom hook 之後，就可以透過它來取代原先的程式碼，和一般 hook 一樣，可以直接呼叫使用

```js
import useCounter from "../hooks/use-counter";

const ForwardCounter = () => {
  useCounter();

  return <Card>{counter}</Card>;
};
```

但很顯然的，如果只有做到這一步，就無法取得 custom hooks 其中的資料如 State，所以必須回傳期待在外部可以接收到的資料，回傳的格式可以是任何型別，如 object、array...，而這裡要特別注意的是，如果在多處使用同一個 custom hooks，彼此的資料是"不會共享"的，每次呼叫一次 custom hooks，就會重新產生一組新的資料

```js
const useCounter = () => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCounter((prevCounter) => prevCounter + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return counter;
};

const ForwardCounter = () => {
  const counter = useCounter();

  return <Card>{counter}</Card>;
};
```

### Configuring Custom Hooks

延續上方的範例，在 ForwardCounter & BackwardCounter 中其實有些微的差異，導致它們為一加一減 counter，custom hooks 其實就是一個 function，所以可以透過帶入參數來作為判斷的條件，上述的範例只要透過一個參數就可以判斷是否為遞增的 counter 了

```js
const useCounter = (forwards = true) => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      if (forwards) {
        setCounter((prevCounter) => prevCounter + 1);
      } else {
        setCounter((prevCounter) => prevCounter - 1);
      }
    }, 1000);

    return () => clearInterval(interval);
  }, [forwards]);

  return counter;
};

const ForwardCounter = () => {
  const counter = useCounter(true);

  return <Card>{counter}</Card>;
};

const BackCounter = () => {
  const counter = useCounter(false);

  return <Card>{counter}</Card>;
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
