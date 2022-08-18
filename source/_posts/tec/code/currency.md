---
title: JavaScript 中的货币计算
avatar: /images/avatar.jpeg
comments: true
author: Hesperus｜Venus
cover: 'https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/img/cover/post/currency.jpg'
photos: 'https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/img/banner/coding.jpg'
date: 2021-09-17 19:24:01
authorLink: hehuadong.cn
authorAbout: javascript
authorDesc: 
tags: 
  - /
  - code
keywords: javascript
description: 当你用金钱进行计算时，每一分钱都需要计算在内。不幸的是，JS 的 Number 类型不能胜任这项任务。在本文中，Julio Sampaio 向我们展示了原因并教我们如何在 JavaScript 中以正确的方式进行货币计算。

---
### 1.The Intl API

The [ECMAScript Internationalization API](https://norbertlindenberg.com/2012/12/ecmascript-internationalization-api/index.html) is a collective effort to provide standardized formatting for international purposes. It allows applications to decide which functionalities they need and how they will be approached.

=>  ECMAScript国际化API是为国际目的提供标准化格式的集体努力。它允许应用程序决定它们需要哪些功能，以及如何实现这些功能。

example:

```javascript
var formatterUSD = new Intl.NumberFormat('en-US'); // 美国货币格式化
var formatterBRL = new Intl.NumberFormat('pt-BR'); // 巴西货币格式化
var formatterJPY = new Intl.NumberFormat('ja-JP'); // 日本货币格式化

console.log(formatterUSD.format(0.2233 + 0.1)); // logs "0.323"
console.log(formatterBRL.format(0.2233 + 0.1)); // logs "0,323"
console.log(formatterJPY.format(0.2233 + 0.1)); // logs "0.323"
```

Note how the decimal system changes from one country to another and how the Intl API properly calculated the result of our monetary sum for all the different currencies.

=>  请注意十进制系统如何从一个国家到另一个国家的变化，以及Intl API如何正确地计算所有不同货币的货币总数。

<b>如果您想设置最大有效位数，只需将代码更改为</b>

```javascript
var formatterUSD = new Intl.NumberFormat('en-US', {
  maximumSignificantDigits: 2
});

console.log(formatterUSD.format(0.2233 + 0.1)); // logs "0.32"
```

<i>有趣的实例场景： 加油站付费</i>

<b>API甚至允许格式化货币值，包括特定国家的货币符号</b>

```javascript
var formatterJPY = new Intl.NumberFormat('ja-JP', {
  maximumSignificantDigits: 2,
  style: 'currency',
  currency: 'JPY'
});

console.log(formatterJPY.format(0.2233 + 0.1)); // logs "￥0.32"
```

Furthermore, it allows the conversion of various formats, such as speed (e.g., *kilometer-per-hour) and volume (e.g., _liters*). At this [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat), you may find all the available options for the Intl `NumberFormat`.

=>  此外，它允许各种格式的转换，如速度(如公里/小时)和体积(如升)。在这个链接上，您可以找到所有可用的Intl NumberFormat选项。

Ps: 注意这个特性的浏览器兼容性限制是很重要的。由于它是一个标准，一些浏览器版本不支持它的部分选项，如Internet Explorer和Safari。对于这些情况，如果您愿意在这些web浏览器上支持您的应用程序，那么欢迎使用一种备用方法

### 2.轻量级的库

#### Dinero.js

Dinero.js是一个轻量级的、不可变的、可链接的JavaScript库，开发它的目的是使用货币值，并支持全局设置、扩展格式/四舍五入选项、轻松的货币转换和对Intl的本机支持

安装：

```bash
npm install dinero.js
```

One of the greatest benefits of using this library is that it completely embraces Fowler's definition of money, which means it supports both amount and currency values:

=>  使用这个库的最大好处之一是它完全接受了Fowler对货币的定义，这意味着它支持数量和货币价值

```javascript
const tax = Dinero({ amount: 10, currency: 'USD' })
const result = money.subtract(tax) // returns new Dinero object

console.log(result.getAmount()) // logs 90
```

重要的是，Dinero.js不单独处理美分。金额以较小的货币单位指定，这取决于您使用的货币。如果你使用的是美元，那么货币就用美分表示。

为了帮助处理格式化部分，它提供了toFormat()方法，该方法接收带有要格式化的货币模式的字符串。

```javascript
Dinero({ amount: 100 }).toFormat('$0,0') // logs "$1"
Dinero({ amount: 100000 }).toFormat('$0,0.00') // logs "$1,000.00"
```

您可以控制库如何处理格式。例如，如果您正在处理具有不同指数(即小数点后两位以上)的货币，则可以显式定义精度，如下所示

```javascript
Dinero({ amount: 100000, precision: 3 }).toFormat('$0,0.000') // logs "$100.000"
Dinero({ amount: 100, currency: 'JPY', precision: 0 }).toFormat() // logs "¥100.00"
```

Perhaps one of its greatest features is the chainable support for its methods, which leads to better readability and code maintenance:

=>  也许它最伟大的特性之一是对其方法的可链接支持，这将带来更好的可读性和代码维护

```javascript
Dinero({ amount: 10000, currency: 'USD' })
.add(Dinero({ amount: 20000, currency: 'USD' }))
    .divide(2)
    .percentage(50)
    .toFormat() // logs "$75.00"
```

Ps: Dinero.js also provides a way to set up a local or remote exchange conversion API via its [convert](https://dinerojs.com/module-dinero#~convert) method. You can either fetch the exchange data from an external REST API or configure a local database with a JSON file that Dinero.js can use to perform conversions.

=>  Dinero.js还提供了一种通过其convert方法来设置本地或远程交换转换API的方法。您可以从外部REST API获取交换数据，也可以使用JSON文件配置本地数据库，Dinero.js可以使用该文件执行转换。

#### Currency.js

currency.js是一个非常小(只有1.14 kB)的JavaScript库，用于处理货币值。为了解决我们所讨论的浮点数问题，currency.js在幕后处理整数，并确保十进制精度始终正确。

安装：

```bash
npm install currency.js
```

The library can be even less verbose than Dinero.js, by encapsulating the monetary value (whether it is a string, a decimal, a number, or a currency) into its `currency()` object

=>   通过将货币值(无论它是字符串、小数、数字还是货币)封装到其currency()对象中，库甚至可以比Dinero.js更简洁

```javascript
currency(100).value // logs 100

currency(100)
.add(currency("$200"))
.divide(2)
.multiply(0.5) // simulates percentage
.format() // logs "$75.00"
```

It also accepts string parameters, such as a monetary value, with the sign, as seen above. The format() method, in turn, returns a human-friendly currency format.

=>  它还接受字符串参数，如带符号的货币值，如上所示。format()方法则返回一种人类友好的货币格式

```javascript
const USD = value => currency(value);
const BRL = value => currency(value, {
  symbol: 'R$',
  decimal: ',',
  separator: '.'
});
const JPY = value => currency(value, {
  precision: 0,
  symbol: '¥'
});

console.log(USD(110.223).format()); // logs "$110.22"
console.log(BRL(110.223).format()); // logs "R$110,22"
console.log(JPY(110.223).format()); // logs "¥110"
```

Ps: Currency.js is cleaner than Dinero.js in terms of verbosity, which is great. However, it doesn't have a built-in way to perform exchange conversions, so be aware of this limitation if your application needs to do so.

=>  在冗长方面，Currency.js比Dinero.js更简洁，这很好。但是，它没有执行交换转换的内置方法，所以如果您的应用程序需要这样做，请注意这个限制。

#### Numeral.js

Numeral .js更多的是一个通用库，在JavaScript中处理格式化和操作数字。

安装：

```bash
npm install numeral
```

 当将货币值封装到其numeric()对象时，它的语法非常类似于currency.js

```javascript
numeral(100).value() // logs 100
```

在格式化这些值时，它更接近Dinero.js的语法

```javascript
numeral(100).format('$0,0.00') // logs "$100.00"
```

该库具有有限的内置国际化特性，您必须设置自己的库，以备需要新的货币系统时使用

```javascript
numeral.register('locale', 'es', {
  delimiters: {
    thousands: ' ',
    decimal: ','
  },
  currency: {
    symbol: '€'
  }
})

numeral.locale('es')

console.log(numeral(10000).format('$0,0.00')) // logs "€10 000,00"

//可链式调用
const money = numeral(100)
  .add(200)
  .divide(2)
  .multiply(0.5) // simulates percentage
  .format('$0,0.00') // logs "$75.00"
```

Ps: Numeral.js is, by far, the most flexible library to deal with numbers on our list. Its flexibility includes the capacity to create locales and formats as you wish. However, be careful when using it since it doesn't provide a default way to calculate with zero precision for decimal numbers, for example.

=> Numeral.js是处理列表中数字的最灵活的库。它的灵活性包括按照您的意愿创建地区和格式的能力。但是，在使用它时要小心，因为它没有提供一种默认的方法来为小数提供零精度计算。

---