# 泛型

## 接口泛型

```typescript
interface GenericIdentityFn<T> {
  (arg: T): T;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

## 函数泛型（多态）

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("myString"); // 类型为 "string"
let output2 = identity<number>(100); // 类型为 "number"


// T的作用域在单个签名中，每次调用filter将为T绑定独立的类型
type Filter = {
    <T>(array: T[], f: (item: T) => boolean): T[]
}
// 或者写成
type Filter = <T>(array: T[], f: (item: T) => boolean): T[]
let filter: Filter = //... 自动绑定的

// T的作用域涵盖全部签名，在声明Filter类型的函数时绑定T
type Filter<T> = {
    (array: T[], f: (item: T) => boolean): T[]
}
// 或者写成
type Filter<T> = (array: T[], f: (item: T) => boolean): T[]
let filter: Filter<number> = //...
```

## 类泛型

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```
