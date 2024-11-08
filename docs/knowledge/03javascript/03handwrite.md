---
sidebar_position: 3
---

# 手写题 🌟

参考： [https://github.com/Sunny-117/js-challenges](https://github.com/Sunny-117/js-challenges)

## Promise 相关

### 1. 完整实现 Promise A+

```js

```

### 2. Promise.all

```js
let tasks = [promise...]

function PromiseAll(tasks) {
    // 参数检查
    if(tasks === null || typeof tasks[Symbol.iterator] !== 'function'){
        throw new TypeError(`Parameter ${tasks} is not iterable`)
    }
    return new Promise((resolve, reject) => {
        let results = [];
        let cnt = 0;  // 注意 控制返回的
        for(let i = 0; i < tasks.length; ++i) {
          // 转成Promise
          Promise.solve(tasks[i])
          .then((res)=> {
            results[i] = res;
            cnt++;
            if(cnt === tasks.length){
                resolve(results)
            }
          })
          .catch(reject)
        }

    })
}
```

### 3. Promise.allSettled

```js
function PromiseAllSettled(tasks) {
    if(tasks === null || tasks[Symbol.iterator]!=='function') {
        throw new TypeError(`Parameter ${tasks} is not iterable.`);
    }
    return new Promise((resolve, reject) => {
        let results = [];
        let cnt = 0;
        for(let i = 0; i<tasks.length; ++i) {
            Promise.resolve(tasks[i])
              .then(value => {
                // 注意返回的字段，都有staus
                // 成功时有value属性
                results[i] = {
                    status: "fullfilled",
                    value
                }
              })
              .catch(reason => {
                // 失败时有reason属性
                results[i] = {
                    status: "rejected",
                    reason
                }
              })
              .finally(() => [
                cnt++;
                if(cnt === tasks.length) {
                    resolve(results)
                }
              ])
        }
    })
}
```

### 4. Promise.race

```js
function PromiseRace(tasks) {
  if (tasks === null || tasks[Symbol.iterator] !== "function") {
    throw new TypeError(`Parameter ${tasks} is not iterable. `);
  }
  return new Promise((resolve, reject) => {
    for (let i = 0; i < tasks.length; ++i) {
      Promise.resolve(tasks[i]).then(resolve).catch(reject);
    }
  });
}
```

### 5. Promise.resolve

```javascript
function isPromise(value) {
  return (
    value !== null && typeof value === "object" && value.constructor === Promise
  );
}
function PromiseResolve(value) {
  return new Promise((resolve, reject) => {
    // 检查参数类型
    if (isPromise(value)) {
      // 如果本身是promise，直接调用它的then
      return value.then(resolve, reject);
    } else {
      // 如果是其他类型，交给resolve转换
      resolve(value);
    }
  });
}
```

### 6. Promise.reject

```js
function PromiseReject(value) {
  return new Promise((resolve, reject) => {
    reject(value);
  });
}
```

### 7. Promise.prototype.finally

```js
// 在prototype上的方法，记得this是当前实例
function promisePrototypeFinally(callback) {
  this.then(
    async (res) => {
      await callback();
      return res; // 向后链式抛
    },
    async (e) => {
      await callback();
      throw e;
    }
  );
}
```

### 8. Promise.prototype.catch

```js
function promiseProtypeCatch(callback) {
  this.then(undefined, (e) => {
    callback(e);
  });
}
```

### 经典题（包括场景题）

#### 💿 HardMan/LazyMan

> source: 腾讯 WXG、字节

##### 题目描述：

实现一个 HardMan:

```md
HardMan("jack")
// 输出 I am jack

HardMan("jack").rest(10).learn("computer")
// 输出 I am jack
// 等待 10 秒
Start learning after 10 seconds
Learning computer

HardMan("jack").restFirst(5).learn("chinese")
// 输出
// 等待 5 秒
Start learning after 5 seconds
I am jack
Learning chinese
```

##### 参考解答：

- 考点 1：事件循环 同步异步
- 考点 2：promise 串行化
- 考点 3：this 有点像建造者模式
- 考点 4：事件队列思想

```javascript
// 可以包装一层，改进成class
function Hardman(name) {
  let tasks = []; // 事件队列
  // 下面这个也可以用等待时间为0的setTimeout转为异步任务
  new Promise((resolve) => {
    tasks.push(() => {
      return new Promise((resolve2) => {
        console.log(`I am ${name}`);
        resolve2();
      });
    });
    resolve();
  }).then(async () => {
    for (let i = 0; i < tasks.length; ++i) {
      await tasks[i]();
    }
  });

  return {
    rest: function (seconds) {
      tasks.push(() => {
        return new Promise((resolve2) => {
          setTimeout(() => {
            console.log(`Start learning after ${seconds} seconds`);
            resolve2();
          }, seconds * 1000);
        });
      });
      return this;
    },
    restFirst: function (seconds) {
      // 队头入队
      tasks.unshift(() => {
        return new Promise((resolve2) => {
          setTimeout(() => {
            console.log(`Start learning after ${seconds} seconds`);
            resolve2();
          }, seconds * 1000);
        });
      });
      return this;
    },
    learn: function (subject) {
      tasks.push(() => {
        return new Promise((resolve2) => {
          console.log(`Learning ${subject}`);
          resolve2();
        });
      });
    },
  };
}
```

## Array 相关

### 1. 实现拍平

```typescript
function flatArray(arr, cnt) {
  // 使用reduce
  arr.reduce(() => {});
}
```

### 2. 树转列表

### 3. 实现 lodash.get

```javascript
function get(source, path, defaultValue = undefined) {
  // a[3].b -> a.3.b
  const paths = path.replace(/\[(\d+)\]/g, ".$1").split(".");
  let result = source;
  for (const p of paths) {
    result = Object(result)[p];
    if (result === undefined) {
      return defaultValue;
    }
  }
  return result;
}
```

## 对象相关

### 1. 深拷贝（考虑循环引用、不可枚举属性和 Symbol 属性）

```javascript
/**
 * deepClone
 * @param obj 需要拷贝的对象
 * @param hash 标记每一个出现过的属性，避免循环引用
 */
function deepClone(obj, hash = new WeakMap()) {
  // 如果不为对象 直接返回自身
  if (typeof obj !== "object" || obj === null) {
    return obj;
  }
  // 函数 正则 日期 ES6新对象,执行构造题，返回新的对象
  const constructor = obj.constructor;
  if (/^(Function|RegExp|Date|Map|Set)$/i.test(constructor.name))
    return new constructor(obj);

  // 取weakMap中的 避免循环引用
  if (hash.get(obj)) {
    return hash.get(obj);
  }
  let res = Array.isArray(obj) ? [] : {};
  hash.set(obj, res);
  // 获取所有属性名，包括不可枚举的属性和symbol
  const keys = Reflect.ownKeys(obj);
  keys.forEach((key) => {
    if (typeof key === "symbol") {
      // 获取Symbol属性的描述符
      const descriptor = Object.getOwnPropertyDescriptor(obj, key);
      Object.defineProperty(res, key, {
        ...descriptor,
        value: deepClone(obj[key], hash),
      });
    } else {
      if (obj.hasOwnProperty(key)) {
        res[key] = deepClone(obj[key], hash);
      }
    }
  });
  return res;
}
```

### 2. 寄生组合式式继承

```javascript

```

## 组件设计题

### 轮播图

- 图片拼接，overflow 区域 hidden
- 左右箭头，收尾可循环播放
  - optional: 节流
- 自动播放
- 小圆点

**方法 1： 纯 CSS**

animation 分帧写关键帧

缺点：

- 根据图片张数写关键帧，不灵活
- 小圆点部分还是需要 js

**方法 2： Javascript**

left 的值控制拼接图移动

[https://codesandbox.io/p/sandbox/snowy-field-wcy5rg](https://codesandbox.io/p/sandbox/snowy-field-wcy5rg)

### 歌词滚动功能

### 遍历树组件

### 拖拽

### 选项卡
