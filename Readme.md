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

### 使用说明變量
避免在複雜邏輯中直接使用數字或字符串，使用說明變量來解釋數字或字符串的意義。
在下面的例子中，saveCityState 這個 function 的參數可讀性因過長的正則表達式而變差。

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
const locations = ['Austin', 'New York', 'San Francisco'];
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

### 避免重複的描述
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

### 避免重複或沒有意義的條件判斷

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
### 函数参数 (理想情况下不應該超過兩個參數)，並且盡可能短小
限制函數參數的數量有助於測試，並且使得函數更容易理解。通常情況下，超過兩個參數意味著函數功能過於複雜，這時需要重新優化你的函數。當確實需要多個參數時，大多情況下可以考慮這些參數封裝成一個對象。

JS 定義對象非常方便，當需要多個參數時，可以使用一個對象進行替代。

兩個參數並非唯一參考的唯一指標，書中傳遞的大意在於函數應該儘可能短小，在第二個例子中，過長的程式碼使得這支程式碼的功能難以理解。

**反例**:
```javascript
function createMenu(title, body, buttonText, cancellable) {
  ...
}
```

```java
public static String testableHtml(
  PageData pageData,
  boolean includeSuiteSetup
) throws Exception {
  WikiPage wikiPage = pageData.getWikiPage();
  StringBuffer buffer = new StringBuffer();
  if (pageData.hasAttribute("Test")) {
    if (includeSuiteSetup) {
      WikiPage suiteSetup =
      PageCrawlerImpl.getInheritedPage(
              SuiteResponder.SUITE_SETUP_NAME, wikiPage
      );
      if (suiteSetup != null) {
      WikiPagePath pagePath =
        suiteSetup.getPageCrawler().getFullPath(suiteSetup);
      String pagePathName = PathParser.render(pagePath);
      buffer.append("!include -setup .")
            .append(pagePathName)
            .append("\n");
      }
    }
    WikiPage setup =
      PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
    if (setup != null) {
      WikiPagePath setupPath =
        wikiPage.getPageCrawler().getFullPath(setup);
      String setupPathName = PathParser.render(setupPath);
      buffer.append("!include -setup .")
            .append(setupPathName)
            .append("\n");
    }
  }
  buffer.append(pageData.getContent());
  if (pageData.hasAttribute("Test")) {
    WikiPage teardown =
      PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
    if (teardown != null) {
      WikiPagePath tearDownPath =
        wikiPage.getPageCrawler().getFullPath(teardown);
      String tearDownPathName = PathParser.render(tearDownPath);
      buffer.append("\n")
            .append("!include -teardown .")
            .append(tearDownPathName)
            .append("\n");
    }
    if (includeSuiteSetup) {
      WikiPage suiteTeardown =
        PageCrawlerImpl.getInheritedPage(
                SuiteResponder.SUITE_TEARDOWN_NAME,
                wikiPage
        );
      if (suiteTeardown != null) {
        WikiPagePath pagePath =
          suiteTeardown.getPageCrawler().getFullPath (suiteTeardown);
        String pagePathName = PathParser.render(pagePath);
        buffer.append("!include -teardown .")
              .append(pagePathName)
              .append("\n");
      }
    }
  }
  pageData.setContent(buffer.toString());
  return pageData.getHtml();
}

```

**正例**:
```javascript
const menuConfig = {
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
}

function createMenu(menuConfig) {
  ...
}

```

```javascript
function createDom(){}
function getTransactionHistory(){}
function setTxnHistoryDatatable(){}
// 重新組織架構
function handleClick() {
    createDom()
    getTransactionHistory()
    setTxnHistoryDatatable()
}

```
**[回到目錄](#目錄)**


### 函數功能單一性，DO ONE THING 只做一件事
功能不單一的函數難以重構和測試。

**反例**:
```javascript
function emailClients(clients) {
  clients.forEach(client => {
    let clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**正例**:
```javascript
function emailClients(clients) {
  clients.forEach(client => {
    emailClientIfNeeded(client);
  });
}

function emailClientIfNeeded(client) {
  if (isClientActive(client)) {
    email(client);
  }
}

function isClientActive(client) {
  let clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[回到目錄](#目錄)**

### 函數名稱應該清楚表達其用途

**反例**:
```javascript
function dateAdd(date, month) {
  // ...
}

let date = new Date();

dateAdd(date, 1);
```

**正例**:
```javascript
function dateAddMonth(date, month) {
  // ...
}

let date = new Date();
dateAddMonth(date, 1);
```
**[回到目錄](#目錄)**

### 函数的抽象層級
當函數的抽象層級過多時，通常意味著函數功能過於複雜，這時需要重新優化你的函數。

**反例**:
```javascript
function parseBetterJSAlternative(code) {
  let REGEXES = [
    // ...
  ];

  let statements = code.split(' ');
  let tokens;
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    })
  });

  let ast;
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  })
}
```

**正例**:
```javascript
function tokenize(code) {
  let REGEXES = [
    // ...
  ];

  let statements = code.split(' ');
  let tokens;
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    })
  });

  return tokens;
}

function lexer(tokens) {
  let ast;
  tokens.forEach((token) => {
    // ...
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  let tokens = tokenize(code);
  let ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  })
}
```
**[回到目錄](#目錄)**

### 避免大量重複的程式碼
減少大量重複的程式碼有助於修改與維護

**反例**:
```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary: expectedSalary,
      experience: experience,
      githubLink: githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary: expectedSalary,
      experience: experience,
      portfolio: portfolio
    };

    render(data);
  });
}
```

**正例**:
```javascript
function showList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    let portfolio;

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    } else {
      portfolio = employee.getGithubLink();
    }

    var data = {
      expectedSalary: expectedSalary,
      experience: experience,
      portfolio: portfolio
    };

    render(data);
  });
}
```
**[回到目錄](#目錄)**

### 採用默認參數
**反例**:
```javascript
function writeForumComment(subject, body) {
  subject = subject || 'No Subject';
  body = body || 'No text';
}

```

**正例**:
```javascript
function writeForumComment(subject = 'No subject', body = 'No text') {
  ...
}

```
**[回到目錄](#目錄)**

### 使用 Object.assign 設定默認物件

**反例**:
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
}

function createMenu(config) {
  config.title = config.title || 'Foo'
  config.body = config.body || 'Bar'
  config.buttonText = config.buttonText || 'Baz'
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;

}

createMenu(menuConfig);
```

**正例**:
```javascript
const menuConfig = {
  title: 'Order',
  // User did not include 'body' key
  buttonText: 'Send',
  cancellable: true
}

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[回到目錄](#目錄)**


### 減少使用Flag作为函数参数
這意味單一函數具有多個以上的功能性，可以考慮再次劃分功能

**反例**:
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create('./temp/' + name);
  } else {
    fs.create(name);
  }
}
```

**正例**:
```javascript
function createTempFile(name) {
  fs.create('./temp/' + name);
}

----------


function createFile(name) {
  fs.create(name);
}
```
**[回到目錄](#目錄)**

### 避免副作用的寫法
帶有副作用寫法的函數可能會因為不可預期的行為而導致程式碼的狀態難以預測。

**反例**:
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**正例**:
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott'
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[回到目錄](#目錄)**

### 不要在前端中撰寫全局變數
在 JS 中污染全局是一個非常不好的作法，這麼做可能和其他引用衝突，且調用你的 API 的用戶在實際環境中得到一個 exception 前對這一情況是一無所知的。

想像以下例子：如果你想擴展 JS 中的 Array，為其添加一個 `diff` 函數顯示兩個數組間的差異，此時應如何去做？你可以將 diff 寫入 `Array.prototype`，但這麼做會和其他有類似需求的庫造成衝突。如果另一個庫對 diff 的需求為比較一個數組中首尾元素間的差異呢？

使用 ES6 中的 class 對全局的 Array 做簡單的擴展顯然是一個更棒的選擇。


**反例**:
```javascript
Array.prototype.diff = function(comparisonArray) {
  var values = [];
  var hash = {};

  for (var i of comparisonArray) {
    hash[i] = true;
  }

  for (var i of this) {
    if (!hash[i]) {
      values.push(i);
    }
  }

  return values;
}
```

**正例**:
```javascript
class SuperArray extends Array {
  constructor(...args) {
    super(...args);
  }

  diff(comparisonArray) {
    var values = [];
    var hash = {};

    for (var i of comparisonArray) {
      hash[i] = true;
    }

    for (var i of this) {
      if (!hash[i]) {
        values.push(i);
      }
    }

    return values;
  }
}
```
**[回到目錄](#目錄)**

### 封装必要的判斷條件

**反例**:
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  /// ...
}
```

**正例**:
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[回到目錄](#目錄)**

### 盡量避免“否定情况”的使用

**反例**:
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**正例**:
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[回到目錄](#目錄)**

### 避免大量的條件判斷

大多數人聽到這的第一反應是："怎麼可能不用 if 完成其他功能呢？"許多情況下通過使用多態(polymorphism)可以達到同樣的目的。

第二個問題在於採用這種方式的原因是什麼。答案是我們之前提到過的：保持函數功能的單一性。

**反例**:
```javascript
class Airplane {
  //...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return getMaxAltitude() - getPassengerCount();
      case 'Air Force One':
        return getMaxAltitude();
      case 'Cessna':
        return getMaxAltitude() - getFuelExpenditure();
    }
  }
}
```

**正例**:
```javascript
class Airplane {
  //...
}

class Boeing777 extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude() - getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude();
  }
}

class Cessna extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude() - getFuelExpenditure();
  }
}
```
**[回到目錄](#目錄)**

### 避免類型判斷（第一部分）
JavaScript 是一種弱型別語言，這意味著函數可以接受任何類型的參數。

盡量避免以下的寫法，錯綜復雜的判斷式會導致程式碼的可讀性降低。

**反例**:
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.peddle(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**正例**:
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[回到目錄](#目錄)**

### 避免類型判斷（第二部分）
如果需要處理的資料為字串、整數、陣列等類型，無法使用多型並仍有必要對其進行類型檢測時，可以考慮使用 TypeScript 或是 JS Doc 來輔助型別的判定。

**反例**:
```javascript
function combine(val1, val2) {
  if (typeof val1 == "number" && typeof val2 == "number" ||
      typeof val1 == "string" && typeof val2 == "string") {
    return val1 + val2;
  } else {
    throw new Error('Must be of type String or Number');
  }
}
```

**正例**:
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[回到目錄](#目錄)**

### 避免過度優化
現代的瀏覽器在運行時會對程式碼自動進行優化。有時人為對程式碼進行優化可能是在浪費時間。

[這裡可以找到許多真正需要優化的地方](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)

**反例**:
```javascript

// 這裡使用變數len是因為在舊式瀏覽器中，
// 直接使用正例中的方式會導致每次迴圈均重複計算list.length的值，
// 而在現代瀏覽器中會自動完成優化，這一行為是沒有必要的
for (var i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**正例**:
```javascript
for (var i = 0; i < list.length; i++) {
  // ...
}
```
**[回到目錄](#目錄)**

### 刪除無效的程式碼
不再被呼叫的程式碼應該及時刪除，需要調閱時去 Git 上查看歷史記錄。

**反例**:
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

var req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**正例**:
```javascript
function newRequestModule(url) {
  // ...
}

var req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[回到目錄](#目錄)**

### 自頂向下閱讀程式碼 Reading Code from Top to Bottom: The Stepdown Rule
逐步下降規則建議你應該以一種自上而下的方式來組織你的代碼。這意味著高層次的函數應該在文件的頂部，並且這些高層次的函數應該調用位於更低層次的詳細函數。這樣，閱讀代碼的人可以從文件的頂部開始，逐漸深入了解代碼的具體實現細節。

**反例**:
```javascript
// 低層次的實現細節散亂在文件的各處
function handleSubmit() {
  // 處理提交邏輯
}

function fetchData() {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => {
      console.log(data);
    });
}

function setupEventListeners() {
  document.getElementById('submitButton').addEventListener('click', handleSubmit);
}

function initializeUI() {
  document.getElementById('mainContainer').style.display = 'block';
}

// 高層次的函數位於文件的底部
function initializeApp() {
  setupEventListeners();
  initializeUI();
  fetchData();
}

```

**正例**:
```javascript
// 高層次的函數在文件的頂部
function initializeApp() {
  setupEventListeners();
  initializeUI();
  fetchData();
}

// 逐步下降到具體的實現細節
function setupEventListeners() {
  document.getElementById('submitButton').addEventListener('click', handleSubmit);
}

function initializeUI() {
  document.getElementById('mainContainer').style.display = 'block';
}

function fetchData() {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => {
      console.log(data);
    });
}

function handleSubmit() {
  // 處理提交邏輯
}

```


## 對象和數據結構

## 類

## 參考資料
- [clean-code.md](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29)

- [clean-code-js](https://github.com/ryanmcdermott/clean-code-javascript)

- [ChatGPT 4o](https://chatgpt.com)
