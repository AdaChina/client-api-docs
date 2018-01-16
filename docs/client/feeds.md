# 资讯API

## 当前日期展示的资讯

若不带资讯栏目id返回全部资讯（仅用于支持旧版客户端，以后可能，新版客户端按必填处理）

```
GET /today_feeds
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| -- | -- | -- | --| -- |
| feed_category_id | integer | 否 | 资讯栏目id | 1 |

**响应字段**

| 字段名 | 描述 |
| -- | -- |
| id | id |
| total | 总数 |
| title | 资讯标题 |
| image_url | 资讯图片url |
| feed_category_id | 资讯栏目id |

**响应示例**

```
Status: 200 OK
```

```json
[
    {
        "id": 1,
        "title": "资讯标题",
        "image_url": "https://cdn.host.com/image"
    }
]
```

## 历史资讯

若不带资讯栏目id返回全部资讯（仅用于支持旧版客户端，以后可能，新版客户端按必填处理）

```
GET /history_feeds
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| -- | -- | -- | --| -- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |
| feed_category_id | integer | 否 | 资讯栏目id | 1 |

**响应字段**

| 字段名 | 描述 |
| -- | -- |
| id | id |
| total | 总数 |
| title | 资讯标题 |
| thumb_url | 缩略图url |
| image_url | 资讯图片url |
| date | 资讯日期 |
| date_id | 日期年月格式 |

**响应示例**

```
Status: 200 OK
```

```json
{
    "total": 1,
    "items": [
        {
            "id": 1,
            "title": "资讯标题",
            "thumb_url": "https://cdn.host.com/thumb_image",
            "image_url": "https://cdn.host.com/image",
            "date": "2017-08-01",
            "date_id": 201708
        }
    ]
}
```
