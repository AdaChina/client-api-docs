# 班牌API文档

## 当前版本

例如当前api版本为**v1**，需要在request header中都带上api版本信息

```
Accept: application/vnd.class-brand-v1+json
```

## API Endpoint

测试服务器：
```
https://bpstaging.adachina.net/api
```

## 请求规范

* 所有api请求都采用https。所有表单参数使用`json`格式发送。

* 传递表单参数时，header需设置：

  ```
  Content-Type:application/json
  ```

  班牌应用请求时必须在header带上当前APP的版本号和硬件版本号

  ```
  AdaCampus-App-Version:1.1.0
  AdaCampus-HW-Version:M7-V1
  ```

  守护进程请求时必须在header带上当前守护进程版本号

  ```
  AdaCampus-Watcher-Version:1.1.0
  ```

## 响应规范

* 所有返回数据使用`json`格式。

* 涉及业务逻辑的字段均使用CST时间，使用ISO8601进行格式化，如：

  ```
  "created_at": "2016-10-25T18:15:17+08:00"
  ```
* 信息展示使用24小时制CST日期和时间，如：

  ```
  "created_at": "2017-01-01 18:30"
  ```

* 空字段返回`null`，而不是直接删去该字段

* 空数组返回`[]`。

* 所有header带有[Etag](https://robots.thoughtbot.com/introduction-to-conditional-http-caching-with-rails#etags)支持客户端缓存，在请求header中设置`If-None-Match`，若资源没更新，将返回：

  ```
  304 Not Modified
  ```

## 身份认证

身份认证采用[HTTP Basic Authentication](http://en.wikipedia.org/wiki/Basic_access_authentication)，客户端需要对`serial_no`和`device_key`进行base64编码：

```
echo -n "sn12345678:abcdef" | base64
```

然后设置请求header：

```
Authorization: Basic c24xMjM0NTY3ODphYmNkZWY=
```

## 响应状态码
返回状态码列表参考[http status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

### 2XX Success

成功的请求响应以`2`开头，将按照文档的格式返回数据。

#### 200 OK

请求成功，返回数据。

#### 201 Created

资源创建成功，部分api可能会返回数据。

### 4XX Client Error

客户端错误请求的响应将以`4`开头。

#### 401 Unauthorized

身份认证失败。

#### 404 Not Found

资源不存在

### 5XX Server Error

客户端错误的响应将以`5`开头。

#### 500 Internal Server Error

内部服务器错误

## 错误信息

对于`4XX`和`5XX`的错误，将返回错误error和用户能读懂的错误信息，如：

```
HTTP/1.1 401 Unauthorized
```

```json
{
  "error": "auth_error",
  "error_code": 100,
  "message": "认证失败..."
}
```

## 分页

分页参数为URL参数

```
curl https://www.brand.com/api/students?page=2&per_page=10
```

分页参数如下：

**可选参数**

| 名称       | 类型      | 描述     | 默认值   |
| :------- | ------- | ------ | ---- |
| page     | integer | 分页页码   | 1    |
| per_page | integer | 每页资源数量 | 10   |

**注意**:
`page`参数起始页为**`1`**

## 资源嵌套

api中，默认对每个资源的请求都只会返回该资源的属性，嵌套资源返回对应url（通过GET请求访问url可获取信息），如：

```
curl "https://www.brand.com/api/classes/1"
```

```json
{
  "id": 1,
  "name": "x年x班",
  "students": "https://www.brand.com/api/classes/1/students"
}
```

可以使用`expand`参数展开资源，如：

```
curl "https://www.brand.com/api/classes/1?expand[]=students"
```

```json
{
  "id": 1,
  "name": "x年x班",
  "students": [
    {
      "id": 101,
      "name": "小明",
      "homework": "https://www.brand.com/api/students/101/homework"
    },
    {
      "id": 102,
      "name": "小红",
      "homework": "https://www.brand.com/api/students/101/homework"
    }
  ]
}
```

**在v2版本中，未展开的资源字段值由`url字符串`改成`"_"`**。

要展开多级资源:

```
curl "https://www.brand.com/api/classes/1?expand[]=students.homework"
```

要展开多个属性:

```
curl "https://www.brand.com/api/classes/1?expand[]=students&expand[]=teachers"
```

嵌套分页:

不支持嵌套分页，若嵌套资源数量过多，默认返回**10**个
