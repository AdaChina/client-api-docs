## 服务器推送

应用与后台建立[socket.io](http://socket.io/)长连接。

## 推送响应

为确保消息送达以及实现客户端状态实时查询，客户端接收到**所有**事件时都应该作出响应\(身份验证除外\)。响应的body规范为`JSON字符串`。

推送事件目前分为两类：

1. 无需客户端返回数据
2. 客户端需要返回指定数据

对于第一类事件，客户端应返回统一的响应表示消息已送达，JSON格式如下:

```json
{
  "ack": true
}
```

对于第二类事件，参考具体事件说明。

具体实现方式参考实例代码。

## 地址

测试服务器  
`https://bpstaging.adachina.net`

生产服务器  
`https://adacampus.com`

## 客户端JS实例代码

```javascript
var socket = io('https://bpstaging.adachina.net', {path: "/socket/socket.io"});

    socket.on('connect', function () {
      socket.emit('authentication', { signature: 'abcdefg123456' });
    });
    socket.on('auth:ok', function () {
      console.log("authentication ok");
    });
    socket.on('auth:fail', function (data) {
      console.log(data.error_msg);
    });
    socket.on('class:update', function (data, callback) {
      console.log(data);
      callback({"ack": true});
      document.getElementById('message').innerHTML = data;
    })
```

## 身份验证

客户端监听链接建立事件`connect`，链接建立成功后向服务器发送事件`authentication`，携带签名信息。客户端需要对serial\_no和device\_key进行base64编码生成签名：

```
echo -n "sn12345678:abcdef" | base64
```

示例：

```json
// emit: authentication

{
  "signature": "kajsldfjnsewjrklwe"
}
```

身份认证结果由事件返回  
认证成功返回`auth:ok`，该事件不会携带返回数据。

```json
// event: auth:ok
```

认证失败返回`auth:fail`:

```json
// emit: auth:fail

{
  "error_msg": "signature not valid."
}
```

身份认证成功后，会接受到服务器推送的事件和数据。

## 实时推送

### 全屏信息发布

事件: `media_notifications`

终端收到信息发布推送时，应该把当前轮播的信息替换成推送中的信息

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| show_type | string | 通知类型: normal(常规), high_priority(高优先级) | normal
| ended\_at | string | 消息停止时间 | "2016-10-25T18:15:17+08:00" |
| type | string | 通知类型: image\(图片\), webview\(网页\) | image |
| url | string | 素材/网页url | "[https://cdn.adachina.net/a.jpg](https://cdn.adachina.net/a.jpg)" |
| duration | integer | 素材展示时长,单位: 秒 | 7200 |

推送内容示例:

```json
{
  "show_type": "high_priority",
  "ended_at": "2016-10-25T18:15:17+08:00",
  "notifications": [
    {
      "type": "image",
      "url": "https://cdn.adachina.net/b.jpg",
      "duration": 3600
    },
    {
      "type": "webview",
      "url": "https://bp.adachina.net/webviews/1",
      "duration": 3600
    }
  ]  
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 滚动通知

事件: `text_notifications`

终端收到信息发布推送时，应该把当前轮播的信息替换成推送中的信息

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| content | string | 素材url | "滚动通知" |
| ended\_at | string | 消息停止时间 | "2016-10-25T18:15:17+08:00" |

推送内容示例:

```json
{
  "notifications": [
    {
      "content": "滚动通知1"
    },
    {
      "content": "滚动通知2"
    },
    {
      "content": "滚动通知3"
    }
  ],
  "ended_at": "2016-10-25T18:15:17+08:00"
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 全屏信息停止播放

事件: `media_stop`

终端收到该事件时，停止全屏信息播放

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| stop | boolean | 该字段只会为true | true |

推送内容示例:

```json
{
  "stop": true
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 滚动通知停止播放

事件: `text_stop`

终端收到该事件时，停止全屏信息播放

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| stop | boolean | 该字段只会为true | true |

推送内容示例:

```json
{
  "stop": true
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 设备解除绑定班级

事件: `unbind_class`

后台管理员解除设备的班级绑定时，终端会接收该事件

推送内容示例:

```json
{
  "unbind": true
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 设备绑定班级

事件: `bind_class`

后台管理员把设备绑定到班级时，终端会接收该事件

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| id | integer | 班级id | 1 |

推送内容示例:

```json
{
  "id": 1
  "name": "初三1班"
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 设备解除绑定学校

事件: `unbind_school`

后台管理员解除设备的学校绑定时，终端会接收该事件

推送内容示例:

```json
{
  "unbind": true
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 设备绑定学校

事件: `bind_school`

后台管理员把设备绑定到学校时，终端会接收该事件

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| id | integer | 学校id | 1 |
| name | string | 学校名称 | xx中学 |

推送内容示例:

```json
{
  "id": 1,
  "name": "xx中学"
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 更新开关机时间

事件: `update_power_schedule`

设备开关机时间更新时，终端会收到该事件

推送内容示例:

```json
{
  "power_schedule": [
    {
      "shutdown_wday": 5,
      "startup_wday": 6,
      "shutdown_at": "2017-02-24T23:52:00+08:00",
      "startup_at": "2017-02-25T23:57:00+08:00"
    },
    {
      "shutdown_wday": 6,
      "startup_wday": 0,
      "shutdown_at": "2017-02-25T23:52:00+08:00",
      "startup_at": "2017-02-26T23:57:00+08:00"
    },
    {
      "shutdown_wday": 0,
      "startup_wday": 1,
      "shutdown_at": "2017-02-26T23:52:00+08:00",
      "startup_at": "2017-02-27T23:57:00+08:00"
    },
    {
      "shutdown_wday": 1,
      "startup_wday": 2,
      "shutdown_at": "2017-02-28T23:52:00+08:00",
      "startup_at": "2017-03-01T23:57:00+08:00"
    }
  ]
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 重启设备

事件: `restart_device`

后台管理员要求设备重启时，终端会接收该事件

| 参数名 | 类型 | 说明 | 例子 |
| --- | --- | --- | --- |
| restart | boolean | 该字段只会为true | true |

推送内容示例:

```json
{
  "restart": true
}
```

客户端响应示例:

```json
{
  "ack": true
}
```

### 查询设备状态

事件: `device_status`

终端收到事件后返回设备信息

推送内容示例:

```json
// 空
```

客户端响应示例:

```json
{
  "ack": true
  "status": "on" //开机状态: "on", 关屏状态: "sleep"
}
```

### 守护进程升级

事件: `upgrade_watcher`

推送内容示例:

```json
{
  "upgrade_url": "http://....",
  "version": "1.0.0.1"
}
```

客户端响应示例:

```json
{
  "ack": true
}
```
