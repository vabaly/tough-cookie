根据 README 意译

# API

## tough

`tough-cookie` 暴露一个 `tough` 对象，所有的 API 都在这个对象上。

`tough` 对象里面的方法都可以作为单独的函数使用，可以不需要通过类似 `tough.someFunction` 的形式调用，因为这些函数都是纯函数。

### `parseDate(string)`

日期字符串转 `Date` 对象

### `formatDate(date)`

`Date` 对象转日期字符串

**疑问：JS 的日期字符串和 RFC6265 标准中定义的不一样？**

### `canonicalDomain(str)`

域名转成符合 `RFC6265` 规范的域名。

符合规范的域名满足以下条件：

1. 域名字符串两头没有空格
1. 全是小写
1. 省略域名开头的 `.`
1. （可选）进行[国际化域名编码](https://zh.wikipedia.org/wiki/%E5%9B%BD%E9%99%85%E5%8C%96%E5%9F%9F%E5%90%8D%E7%BC%96%E7%A0%81)

### `domainMatch(str,domStr[,canonicalize=true])`

计算域名 `str` 和 Cookie 中的 `Domain` 是否匹配。`canonicalize` 为 `true` 就会对两个域名进行规范化操作。

### `defaultPath(path)`

给定一个 URL 的 `path` 部分，返回最适合（或者是默认的？）存储在 Cookie 中 `Path` 的值。

### `pathMatch(reqPath,cookiePath)`

判断请求地址的 `path` 部分是否与 `cookie` 中 `Path` 的值相匹配

### `getPublicSuffix(hostname)`

返回 `hostname` 主机名的公共后缀？只有一个主机名怎么定义公共呢？

### `cookieCompare(a,b)`

此方法是 `.sort` 方法的比较函数，与 `.sort` 一起使用的。目的是将一些 cookie 进行排序，且按规范的建议顺序进行排序。

规范的顺序为：

1. 最长的 `Path`
1. 时间戳更早的
1. 时间戳一样（精度问题），但创建序号更小的

【Todo】这样能保证较复杂的（？？）、较旧的 cookie 优先被解析。

不过好像这样对分布式系统不够友好？？这就不管了，知道有这个点就行。

### `permuteDomain(domain)`

返回某个域名的所有可能域名

### `permutePath(path)`

返回某个 Path 的所有可能 Path

### `parse(cookieString[, options])` = `tough.Cookie.parse`
### `fromJSON(string)` = `tough.Cookie.fromJSON`

### `Cookie`

#### `Cookie.parse(cookieString[, options])`

将单个 Cookie（？）或 HTTP `Set-Cookie` 头解析成一个 `Cookie` 对象。

`options` 如下：

  - loose，默认 false，true 代表会解析类似 `=abc` 或 `=` 这样的不被规范允许的、没有键的 `Cookie`

一个 `Cookie` 对象大概是这样：

```js
{
  // ======= 下述是 HTTP 规范承认的内容
  key: 'string',
  value: 'string',
  expires: 'Date',
  maxAge: 'seconds',
  domain: 'string',
  path: 'string',
  secure: 'boolean',
  httpOnly: 'boolean',
  sameSite: 'string',
  // 上述是 HTTP 规范承认的内容 =======

  // 任何不符合规范的内容
  extensions: 'Array<string>'

  // tough-cookie 内部的一些属性
  creation: 'Date',
  creationIndex: 'number'
}
```

