# 工具API

## 获取七牛上传凭证

```
GET /upload_token
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| type | string | 是 | 上传文件类型 | video |

| type 参考值 | 描述 |
| :--- | :--- |
| video | 上传视频 |
| audio | 上传音频 |
| image | 上传图片 |
| file  | 上传日志 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| key | 七牛上传凭证Key |
| token | 七牛上传凭证Token |
| ttl | 凭证自获取有效期限(秒) |

**响应示例**

验证成功响应

```
Status: 200 OK
```

```json
{
  "key": "uploads/simple_video/be123123123123123123123",
  "token": "123123123:qic7yL23123123X-h1l123Dk-s=:eypbmUiOjE1MT0=",
  "ttl": 1800
}
```

## 上传视频到七牛CDN(外部API)

参考：[https://developer.qiniu.com/kodo/manual/1272/form-upload](https://developer.qiniu.com/kodo/manual/1272/form-upload)

```
POST http://up-z2.qiniu.com
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| token | string | 是 | 七牛上传凭证Token | 123123...T0= |
| key | string | 是 | 七牛上传凭证Key | upload......123 |
| file | file | 是 | 视频/音频文件 | abc.mp4 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| hash | 文件Hash值 |
| key | 文件CDN Key |

**响应示例**

验证成功响应

```
Status: 200 OK
```

```json
{
  "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
  "key": "1sdffdsfsa"
}
```
