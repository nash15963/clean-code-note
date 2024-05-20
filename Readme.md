# Clean code


## 目錄
  1. [介绍](#介绍)
  2. [變量](#變量)
  3. [函数](#函數)
  4. [對象和數據結構](#objects-and-data-structures)
  5. [類](#類)
  6. [測試](#測試)
  7. [開發](#開發)
  8. [錯誤處理](#錯誤處理)
  9. [格式化](#格式化)
  10. [註解](#註解)
  11. [參考資料](#參考資料)

## 介绍

## 變量
### 可讀性的變量名稱

**反例**:
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**正例**:
```javascript
const yearMonthDay = moment().format('YYYY/MM/DD');
```
**[回到目錄](#目錄)**

### 使用 ES6 的 const 定義常數
使用 const 宣告不可變量，如果為可變量使用 let 宣告。

ES6 之後的版本已經不推薦使用 var（會需要考慮 hoisting 和作用域）。

**反例**:
```javascript
let FIRST_US_PRESIDENT = "George Washington";
```

**正例**:
```javascript
const FIRST_US_PRESIDENT = "George Washington";
```
**[回到目錄](#目錄)**

### 對功能類似的變量名稱採用統一的命名風格

**反例**:
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**正例**:
```javascript
getUser();
```
**[回到目錄](#目錄)**

### 使用易於檢索的變量名稱
以例子來說，其他開發者難以理解526000的意義，宣告有意義的變數名稱並加上註解，有助於其他開發者理解 magic number 的意義。

**反例**:
```javascript
// what is 525600 ?
for (let i = 0; i < 525600; i++) {
  runCronJob();
}
```

**正例**:
```javascript
// Declare them as capitalized `const` globals.
const MINUTES_IN_A_YEAR = 525600;
for (const i = 0; i < MINUTES_IN_A_YEAR; i++) {
  runCronJob();
}
```
**[回到目錄](#目錄)**

### 使用说明变量(即有意义的变量名)
**反例**:
```javascript
const cityStateRegex = /^(.+)[,\\s]+(.+?)\s*(\d{5})?$/;
saveCityState(cityStateRegex.match(cityStateRegex)[1], cityStateRegex.match(cityStateRegex)[2]);
```

**正例**:
```javascript
const ADDRESS = 'One Infinite Loop, Cupertino 95014';
const cityStateRegex = /^(.+)[,\\s]+(.+?)\s*(\d{5})?$/;
const match = ADDRESS.match(cityStateRegex)
const city = match[1];
const state = match[2];
saveCityState(city, state);
```
**[回到目錄](#目錄)**

### 直白的描述參數

**反例**:
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  // What is l？
  dispatch(l);
});
```

**正例**:
```javascript
var locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  dispatch(location);
});
```
**[回到目錄](#目錄)**

### 避免重复的描述
當 Class 已經有意義時，對其變量進行命名不需要再次重複。

**反例**:
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**正例**:
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
**[回到目錄](#目錄)**

### 避免重複的條件判斷

**反例**:
```javascript
function createMicrobrewery(name) {
  let breweryName;
  if (name) {
    breweryName = name;
  } else {
    breweryName = 'Hipster Brew Co.';
  }
}
```

**正例**:
```javascript
function createMicrobrewery(name) {
  let breweryName = name || 'Hipster Brew Co.'
}
```
**[回到目錄](#目錄)**

## 函數

## 對象和數據結構

## 類

## 參考資料
- [clean-code.md]("https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29")

- [clean-code-js]("https://github.com/ryanmcdermott/clean-code-javascript")

- [clean-code-js 簡體中文]("https://github.com/alivebao/clean-code-js?tab=readme-ov-file#%E5%B9%B6%E5%8F%91")

- [Clean Code 無瑕的程式碼 讀書心得]("https://www.mropengate.com/2022/10/clean-code.html")