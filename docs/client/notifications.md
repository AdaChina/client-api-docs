# 通知API

## 历史通知

```
GET /history_notifications
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| -- | -- | -- | --| -- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| -- | -- |
| title | 标题 |
| type | 通知类型，分为image(图片)和webview(网页) |
| thumb_url | 缩略图url，当type为webview时为`null` |
| image_url | 图片/网页url |
| date | 通知日期 |
| date_id | 日期年月格式 |

```json
[
    {
        "title": "通知1",
        "type": "image",
        "thumb_url": "https://cdn.host.com/thumb_image",
        "image_url": "https://cdn.host.com/image",
        "date": "2017-08-01",
        "date_id": 201708
    },
    {
        "title": "通知2",
        "type": "webview",
        "thumb_url": null,
        "image_url": "https://cdn.host.com/webview",
        "date": "2017-08-01",
        "date_id": 201708
    }
]
```
