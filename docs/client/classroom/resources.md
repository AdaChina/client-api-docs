# 班级资源

## 班级简介列表

按顺序返回

```
GET /classes/:class_id/features
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 简介id |
| title | 标题 |
| description | 描述 |
| image_url | 图片url |
| thumb_url | 缩略图url |
| order | 顺序 |
| fullscreen | 图片是否屏展示 |
| style | 简介类型 |
| likes_count | 点赞数 |
| comments_count | 评论数 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#featurescomments) |

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
      "title": "班主任寄语",
      "description": "奔跑是我们的方向......",
      "image_url": "https://cdn.adachina.net/1",
      "thumb_url": "https://cdn.adachina.net/1_thumb",
      "order": 1,
      "fullscreen": true,
      "style": "fullscreen",
      "likes_count": 10,
      "comments_count": 5,
      "comments": "_"
    }
  ]
}
```

## 首页班级简介列表

```
GET /classes/:class_id/selected_features
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 简介id |
| title | 标题 |
| description | 描述 |
| image_url | 图片url |
| thumb_url | 缩略图url |
| order | 顺序 |
| fullscreen | 图片是否屏展示 |
| style | 简介类型 |
| likes_count | 点赞数 |
| comments_count | 评论数 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#featurescomments) |

```json
[
  {
    "id": 1,
    "title": "班主任寄语",
    "description": "奔跑是我们的方向......",
    "image_url": "https://cdn.adachina.net/1",
    "thumb_url": "https://cdn.adachina.net/1_thumb",
    "order": 1,
    "fullscreen": true,
    "style": "fullscreen",
    "likes_count": 10,
    "comments_count": 5,
    "comments": "_"
  }
]
```

## 班级简介的评论列表

```
GET /classes/:class_id/features/:feature_id/comments
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| feature_id | integer | 是 | 班级简介id，路由参数 | 1 |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 评论id |
| content | 评论内容 |
| avatar_url | 评论者头像url |

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
      "content": "评论.....",
      "avatar_url": "https://cdn.adachina.net/avatar"
    }
  ]
}
```

## 班级故事列表

```
GET /classes/:class_id/photo_albums
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 班级故事id |
| title | 标题 |
| date | 日期 |
| date_id | 日期年月格式 |
| photos_count | 照片数量 |
| photos(expand) | 照片列表，展开后默认10条，[参考](#photoalbumsphotos) |

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
      "title": "标题.....",
      "date": "2017-01-01",
      "date_id": 201701,
      "photos_count": 30,
      "photos": "_"
    }
  ]
}
```

## 班级故事的相片列表

```
GET /classes/:class_id/photo_albums/:photo_album_id/photos
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| photo_album_id | integer | 是 | 班级故事id，路由参数 | 1 |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 相片id |
| image_url | 相片url |
| thumb_url | 缩略图url |
| album_title | 对应相册标题 |
| album_date | 对应相册日期 |
| description | 相片描述 |
| likes_count | 点赞数量 |
| comments_count | 照片数量 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#photoscomments) |

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
      "image_url": "https://cdn.adachina.net/image",
      "thumb_url": "https://cdn.adachina.net/image_thumb",
      "album_name": "xxx",
      "album_date": "2017-01-01",
      "description": "xxx合影...",
      "likes_count": 10,
      "comments_count": 30,
      "comments": "_"
    }
  ]
}
```

## 首页相片列表

```
GET /classes/:class_id/selected_photos
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 相片id |
| image_url | 相片url |
| thumb_url | 缩略图url |
| album_title | 对应相册标题 |
| album_date | 对应相册日期 |
| description | 相片描述 |
| likes_count | 点赞数量 |
| comments_count | 照片数量 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#photoscomments) |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "id": 1,
    "image_url": "https://cdn.adachina.net/image",
    "thumb_url": "https://cdn.adachina.net/image_thumb",
    "album_name": "xxx",
    "album_date": "2017-01-01",
    "description": "xxx合影...",
    "likes_count": 10,
    "comments_count": 30,
    "comments": "_"
  }
]
```

## 照片的评论

```
GET /classes/:class_id/photos/:photo_id/comments
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| photo_id | integer | 是 | 相片id，路由参数 | 1 |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 评论id |
| content | 内容 |
| avatar_url | 评论者头像url |

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
      "content": "评论.....",
      "avatar_url": "https://cdn.adachina.net/avatar"
    }
  ]
}
```

## 班级表彰列表

```
GET /classes/:class_id/praises
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 表彰id |
| title | 表彰标题 |
| description | 表彰描述 |
| style | 表彰类型 |
| date | 日期 |
| date_id | 日期年月格式 |
| image_url | 图片url |
| thumb_url | 缩略图url |
| likes_count | 点赞数量 |
| students_count | 学生数量 |
| studnets(expand) | 学生列表 [参考](#praisesstudents) |
| comments_count | 照片数量 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#praisescomments) |

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
      "title": "优秀学习标兵",
      "description": "好好学习天天向上......",
      "style": "image",
      "image_url": "https://cdn.adachina.net/image",
      "thumb_url": "https://cdn.adachina.net/image_thumb",
      "date": "2017-01-01",
      "date_id": 201701,
      "likes_count": 10,
      "students_count": 5,
      "students": "_",
      "comments_count": 10,
      "comments": "_"
    }
  ]
}
```

## 首页班级表彰列表

```
GET /classes/:class_id/selected_praises
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 表彰id |
| title | 表彰标题 |
| description | 表彰描述 |
| style | 表彰类型 |
| image_url | 图片url |
| thumb_url | 缩略图url |
| likes_count | 点赞数量 |
| students_count | 学生数量 |
| studnets(expand) | 学生列表 [参考](#praisesstudents) |
| comments_count | 照片数量 |
| comments(expand) | 评论列表，展开后默认10条，[参考](#praisescomments) |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "id": 1,
    "title": "优秀学习标兵",
    "description": "好好学习天天向上......",
    "style": "fullscreen",
    "image_url": "https://cdn.adachina.net/image",
    "thumb_url": "https://cdn.adachina.net/image_thumb",
    "likes_count": 10,
    "students_count": 5,
    "students": "_",
    "comments_count": 10,
    "comments": "_"
  }
]
```

## 表彰的评论列表

```
GET /classes/:class_id/praises/:praise_id/comments
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| praise_id | integer | 是 | 表彰id，路由参数 | 1 |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 评论id |
| content | 内容 |
| avatar_url | 评论者头像url |

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
      "content": "评论.....",
      "avatar_url": "https://cdn.adachina.net/avatar"
    }
  ]
}
```

## 表彰的学生列表

```
GET /classes/:class_id/praises/:praise_id/students
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| praise_id | integer | 是 | 表彰id，路由参数 | 1 |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 学生id |
| name | 学生姓名 |
| avatar | 学生头像url |
| card_number | 学生卡号 |

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
      "name": "王小明",
      "avatar": "https://cdn.adachina.net/avatar",
      "card_number": 1234678
    }
  ]
}
```

## 家长通知列表

```
GET /classes/:class_id/client_notis
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| page | integer | 否 | 页数 | 1 |
| per_page | integer | 否 | 每页对象数量 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 总数 |
| id | 家长通知id |
| type | 家长通知类别 |
| content | 通知内容 |
| image_url | 图片地址 |
| thumb_url | 缩略图url |
| created_at | 创建时间 |
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
      "type": "image",
      "content": "通知内容",
      "image_url": "https://cdn.adachina.net/image",
      "thumb_url": "https://cdn.adachina.net/image_thumb",
      "created_at": "2017-01-01 18:30",
      "date_id": 201701
    }
  ]
}
```

## 单个家长通知

```
GET /api/classes/:id/client_notis/:client_noti_id
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| client_noti_id | integer | 是 | 家长通知id，路由参数 | 1 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 家长通知id |
| content | 通知内容 |
| image_url | 图片地址 |
| thumb_url | 缩略图url |
| created_at | 创建时间 |
| date_id | 日期年月格式 |

**响应示例**

```
Status: 200 OK
```

```json
{
  "id": 1,
  "type": "text",
  "content": "通知内容",
  "image_url": "",
  "thumb_url": "https://cdn.adachina.net/image_thumb",
  "created_at": "2017-01-01 18:30",
  "date_id": 201701,
}
```

## 首页展示当天的家长通知

```
GET /classes/:class_id/selected_client_notis
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 家长通知id |
| content | 通知内容 |
| image_url | 图片地址 |
| thumb_url | 缩略图url |
| created_at | 创建时间 |
| date_id | 日期年月格式 |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "id": 1,
    "type": "text",
    "content": "通知内容",
    "image_url": "",
    "thumb_url": "https://cdn.adachina.net/image_thumb",
    "created_at": "2017-01-01 18:30",
    "date_id": 201701,
  }
]
```

## 家长留言

```
GET /classes/:class_id/memos
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| has_unread | 班级是否有未读留言 |
| unread_count | 班级未读留言总数 |
| id | 学生ID |
| card_number | 学生卡号 |
| name | 学生姓名 |
| avatar | 学生头像图片地址 |
| has_unread | 学生是否有未读留言 |
| unread_count | 未读留言数量 |

**响应示例**

```
Status: 200 OK
```

```json
{
  "has_unread": true,
  "unread_count": 3,
  "items": [
    {
      "id": 1,
      "card_number": 129418301,
      "name": "王小明",
      "avatar": "https://cdn.adachina.net/image",
      "has_unread": false,
      "unread_count": 0
    }
  ]
}
```

## 学生家长留言列表

```
GET /api/classes/:id/memos/:student_id
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| student_id | string | 是 | 学生ID，路由参数 | 103 |
| start_id | integer | 否 | 起始留言(从新到旧)的id，为空时按最新留言返回 | 15 |
| end_id | integer | 否 | 结束留言的id | 5 |
| limit | integer | 是 | 返回留言最大数目 | 10 |
| limit_from | string | 是 | (start/end)，从start_id或end_id开始计算最大数目 | start |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| total | 留言数量 |
| id | 留言ID |
| content | 留言内容 |
| content_type | 留言媒体类型 |
| video_thumb_url | 视频缩略图 |
| media_duration | 媒体时长 |
| has_read | 是否已读 |
| created_at | 创建时间 |
| sent_at | 发送时间 |
| sent_by_student | 是否当前学生留言 |
| sender_name | 发布人姓名/呢称 |
| sender_avatar | 发布人头像图片地址 |

**响应示例**

请求成功响应

```
Status: 200 OK
```

```json
{
  "total": 3,
  "items": [
    {
      "id": 1,
      "content": "文字文字文字",
      "content_type": "text",
      "video_thumb_url": null,
      "media_duration": null,
      "has_read": false,
      "sent_at": "2017-12-19T12:28:43+08:00",
      "created_at": "2018-01-01 18:30",
      "sent_by_student": false,
      "sender_name": "王大明",
      "sender_avatar": "https://cdn.adachina.net/image"
    },
    {
      "id": 1,
      "content": "http://cdn.com/3aa",
      "content_type": "video",
      "video_thumb_url": "http://cdn.com/3aa?vframe/jpg/offset/3",
      "media_duration": "02:30",
      "has_read": true,
      "sent_at": "2017-12-19T12:28:43+08:00",
      "created_at": "2018-01-01 18:30",
      "sent_by_student": true,
      "sender_name": "王小明",
      "sender_avatar": "https://cdn.adachina.net/image"
    }
  ]
}
```

请求失败响应

```
Status: 404 Not Found
```

```json
{
  "error": "student_not_found",
  "error_code": 115,
  "message": "学生不存在"
}
```

### 参数使用说明

以下按设备端的三种加载情况举例说明参数的使用方法

现在假设已存在50条消息，id为1~50

#### 1. 初始化加载

目标: 获取最新的10条消息

参数: `?limit=10&limit_from=start`

当end_id和start_id为空时，消息的范围为`1~50`，limit_from为`start`，因此返回的消息从id`50`开始往上数10条。结果返回的消息为`41~50`。

#### 2. 下拉加载历史消息

目标: 消息列表顶部的消息id为`30`，需要获取`30`以前的10条消息

参数: `?limit=10&limit_from=start&start_id=30`

start_id为30，end_id为空，因此消息范围为`1~29`，limit_from为`start`，因此返回的消息从id`29`开始往上数10条。结果返回的消息为`20~29`。

#### 3. 上滑加载新消息

目标: 消息列表底部的消息id为`35`，需要获取`35`之后的10条消息

参数: `?limit=10&limit_from=end&end_id=35`

start_id为空，end_id为35，因此消息范围为`36~50`，limit_from为`end`，因此返回的消息从id`36`开始往下数10条。结果返回的消息为`36~45`。

## 创建留言

```
POST /api/classes/:id/memos/:student_id
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| student_id | string | 是 | 学生ID，路由参数 | 103 |
| video | string | 否 | 七牛视频文件Key | upload/abcd/dffa2 |
| audio | string | 否 | 七牛音频文件Key | upload/abcd/au3c |
| text | string | 否 | 文字留言内容 | HelloWorld |

***`video`, `audio`, `text` 必须提供其中一个参数***

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 留言ID |
| content | 留言内容 |
| content_type | 留言媒体类型 |
| video_thumb_url | 视频缩略图 |
| media_duration | 媒体时长 |
| has_read | 是否已读 |
| created_at | 创建时间 |
| sent_at | 发送时间 |
| sent_by_student | 是否当前学生留言 |
| sender_name | 发布人姓名/呢称 |
| sender_avatar | 发布人头像图片地址 |

**响应示例**

请求成功响应

```
Status: 200 OK
```

```json
{
  "id": 1,
  "content": "http://cdn.com/3aa",
  "content_type": "video",
  "video_thumb_url": "http://cdn.com/3aa?vframe/jpg/offset/3",
  "media_duration": "02:30",
  "has_read": true,
  "sent_at": "2017-12-19T12:28:43+08:00",
  "created_at": "2018-01-01 18:30",
  "sent_by_student": false,
  "sender_name": "王小明",
  "sender_avatar": "https://cdn.adachina.net/image"
}
```

请求失败响应

```
Status: 404 Not Found
```

```json
{
  "error": "student_not_found",
  "error_code": 115,
  "message": "学生不存在"
}
```
```
Status: 422 Unprocessable Entity
```

```json
{
  "error": "media_invalid",
  "error_code": 116,
  "message": "媒体文件无效或损坏"
}
```
```
Status: 422 Unprocessable Entity
```

```json
{
  "error": "memo_invalid",
  "error_code": 117,
  "message": "留言参数不正确"
}
```
