---
title: Adjusting the Custom Hook Logic
abbrlink: 14104
date: 2022-07-16 13:17:26
tags:
  - React - The Complete Guide (Incl Hooks, React Router, Redux)
categories: React
thumbnail:
banner:
description:
---

這章節將進一步調整並優化 Custom Hook 的使用邏輯

<!-- more -->

## Custom Http Hook

有兩個送出 http request 至 server 的 component 如下:

```js
// App.js
function App() {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  const [tasks, setTasks] = useState([]);

  const fetchTasks = async (taskText) => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        '${server_url}/tasks.json'
      );

      if (!response.ok) {
        throw new Error('Request failed!');
      }

      const data = await response.json();

      const loadedTasks = [];

      for (const taskKey in data) {
        loadedTasks.push({ id: taskKey, text: data[taskKey].text });
      }

      setTasks(loadedTasks);
    } catch (err) {
      setError(err.message || 'Something went wrong!');
    }
    setIsLoading(false);
  };

  useEffect(() => {
    fetchTasks();
  }, []);

  const taskAddHandler = (task) => {
    setTasks((prevTasks) => prevTasks.concat(task));
  };

  return (
    <React.Fragment>
      <NewTask onAddTask={taskAddHandler} />
      <Tasks
        items={tasks}
        loading={isLoading}
        error={error}
        onFetch={fetchTasks}
      />
    </React.Fragment>
  );
}
```

```js
// NewTask.js
const NewTask = (props) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const enterTaskHandler = async (taskText) => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        '${server_url}/tasks.json',
        {
          method: 'POST',
          body: JSON.stringify({ text: taskText }),
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      if (!response.ok) {
        throw new Error('Request failed!');
      }

      const data = await response.json();

      const generatedId = data.name; // firebase-specific => "name" contains generated id
      const createdTask = { id: generatedId, text: taskText };

      props.onAddTask(createdTask);
    } catch (err) {
      setError(err.message || 'Something went wrong!');
    }
    setIsLoading(false);
  };

  return (
    <Section>
      <TaskForm onEnterTask={enterTaskHandler} loading={isLoading} />
      {error && <p>{error}</p>}
    </Section>
  );
};
```

很顯然地，因其中 http request 格式與 error handler 都非常相似，所以可以透過 custom hook 的方式來封裝這段程式碼

```js
// use-http.js
const useHttp = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const sendRequest = async (requestConfig, applyData) => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(requestConfig.url, {
        method: requestConfig.method ? requestConfig.method : "GET",
        headers: requestConfig.headers ? requestConfig.headers : {},
        body: requestConfig.body ? JSON.stringify(requestConfig.body) : null,
      });

      if (!response.ok) {
        throw new Error("Request failed!");
      }

      const data = await response.json();
      applyData(data);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
  };

  return {
    isLoading,
    error,
    sendRequest,
  };
};
```

接下來便可以在 App 中使用這個 custom hook

```js
const App = () => {
  const [tasks, setTasks] = useState([]);

  const transformTasks = (data) => {
    const loadedTasks = [];

    for (const taskKey in data) {
      loadedTasks.push({ id: taskKey, text: data[taskKey].text });
    }

    setTasks(loadedTasks);
  };

  const { isLoading, error, sendRequest: fetchTasks } = useHttp(
    {
      url: "https://react-http-77951-default-rtdb.firebaseio.com//tasks.json",
    },
    transformTasks
  );

  useEffect(() => {
    fetchTasks();
  }, []);

  const taskAddHandler = (task) => {
    setTasks((prevTasks) => prevTasks.concat(task));
  };

  return (
    ...
  );
};
```

但目前為止，功能看似正常了，但在 `useEffect()` 會出現一個提示，因為凡是在其中所使用到的 component 內 function ，都需要加到 dependency array 之中

```js
useEffect(() => {
  fetchTasks();
}, [fetchTasks]);
```

但這麼一來，就會出現 infinite loop 的情況，因為當 `fetchTasks` invoked，因其中改變 State，就會導致 component re-render，接著其中的 function 包含 `fetchTasks` 都會被重新產生，即便功能完全相同，因 function 也是 object，所以會在記憶體中另一個位置被新增，進而導致被判斷為不同 object 而使的這段程式碼被不斷重複執行。

為了解決這個問題，可以將 function 在 custom hook `useHttp` 中透過 `useCallback()` 來包裝它，藉此告訴 React 如果內容不變就可以不必重新生成這段 fucntion

```js
const useHttp = () => {
  ...
  const sendRequest = useCallback(async (requestConfig, applyData) => {
    ...
  }, []);

  return (...);
};
```

## Adjusting the Custom Hook Logic

到這裡其實就已經完成預期的功能了，但我們還可以進一步優化這段程式碼 ; 在前面的程式碼中，參數都透過 custom hook 直接帶入，但其實都只有在一個地方被使用，所以其實可以在 costom hook 所回傳的 fucntion 中帶入即可 

```js
const App = () => {
  const { isLoading, error, sendRequest: fetchTasks } = useHttp();

  useEffect(() => {
    const transformTasks = (data) => {
      const loadedTasks = [];

      for (const taskKey in data) {
        loadedTasks.push({ id: taskKey, text: data[taskKey].text });      }

      setTasks(loadedTasks);
    };

    fetchTasks(
      {
        url: "https://react-http-77951-default-rtdb.firebaseio.com//tasks.json",
      },
      transformTasks
    );
  }, [fetchTasks]);
};

const useHttp = () => {
  const sendRequest = useCallback(async (requestConfig, applyData) => {
    ...
  });
};
```

## Using The Custom Hook in More Components

接著這個 custom hook 使用到另外一個 component 之中，但這個 component 送出 http request 的方式不太一樣，需透過使用者手動出發，而非在 `useEffect()` 自動送出，而且後面對於資料的應用需要多一個傳入的參數 `teskText`

```js
const NewTask = (props) => {
  const { isLoading, error, sendRequest: sendTaskRequest } = useHttp();

  const createTask = async (taskData) => {
    const generatedId = taskData.name; // firebase-specific => "name" contains generated id
    const createdTask = { id: generatedId, text: taskText };
    props.onAddTask(createdTask);
  };

  const enterTaskHandler = async (taskText) => {
    sendTaskRequest(
      {
        url: "https://react-http-77951-default-rtdb.firebaseio.com//tasks.json",
        method: "POST",
        body: JSON.stringify({ text: taskText }),
        headers: {
          "Content-Type": "application/json",
        },
      },
      createTask
    );
  };
};
```

這種情況就可以透過原生 JavaScript 的 `bind()` 來帶入額外的參數

```js
const NewTask = (props) => {
  const { isLoading, error, sendRequest: sendTaskRequest } = useHttp();

  const createTask = async (taskText, taskData) => {
    const generatedId = taskData.name;
    const createdTask = { id: generatedId, text: taskText };
    props.onAddTask(createdTask);
  };

  const enterTaskHandler = async (taskText) => {
    sendTaskRequest(
      {
        url: "https://react-http-77951-default-rtdb.firebaseio.com//tasks.json",
        method: "POST",
        body: JSON.stringify({ text: taskText }),
        headers: {
          "Content-Type": "application/json",
        },
      },
      createTask.bind(null, taskText)   // bind parameter
    );
  };
};
```

## 資料參考

[React - The Complete Guide (Incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)
