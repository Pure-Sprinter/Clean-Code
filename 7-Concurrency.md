## 동시성 Concurrency

### 1. Callback 대신 Promise를 사용하세요

Callback은 깔끔하지 않습니다. 그리고 엄청나게 많은 중괄호 중첩을 만들어냅니다. ES2015/ES6에선 Promise가 내장되어 있습니다.

**Bad**

```javascript
require("request").get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      require("fs").writeFile("article.html", response.body, (writeErr) => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**Good**

```javascript
require("request-promise")
  .get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then((response) => {
    return require("fs-promise").writeFile("article.html", response);
  })
  .then(() => {
    console.log("File written");
  })
  .catch((err) => {
    console.error(err);
  });
```

### 2. Async/Await는 Promise보다 더욱 깔끔합니다.

Promise도 Callback에 비해 정말 깔끔하지만 ES2017/ES8에선 async 와 await 가 있습니다. 이들은 Callback에 대한 더욱 깔끔한 해결책을 줍니다. 오직 필요한 것은 함수 앞에 async를 붙이는 것 뿐입니다. 그러면 함수를 논리적으로 연결하기 위해 더이상 then을 쓰지 않아도 됩니다.

**Bad**

```javascript
require("request-promise")
  .get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then((response) => {
    return require("fs-promise").writeFile("article.html", response);
  })
  .then(() => {
    console.log("File written");
  })
  .catch((err) => {
    console.error(err);
  });
```

**Good**

```javascript
async function getCleanCodeArticle() {
  try {
    const response = await require("request-promise").get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await require("fs-promise").writeFile("article.html", response);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}
```
