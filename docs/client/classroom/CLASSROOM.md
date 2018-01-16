# 班级API(v2)

**路由参数说明**

班级API的url中出现的参数

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| class_id | integer | 是 | 班级id | 1 |

**路由参数错误响应**

当参数的班级id非设备绑定的班级id时，返回如下响应：

```
Status: 401 Unauthorized
```

```json
{
    "error": "wrong_classroom",
    "error_code": 107,
    "message": "无权获取该班级信息(与绑定班级不符合)"
}
```

## class

指定班级详细信息

```
GET /classes/:class_id
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 班级唯一id |
| type | 班级类型: StaticClass(固定班)，DynamicClass(走班) |
| name | 班级名称 |
| honor | 是否表彰班级 |
| honor_image_url | 班级评比图片，没有时为`null` |
| config | 班级功能配置 |
| class_teacher | 班主任信息(expand)，[参考](#classteacher) |
| security | 安全负责人信息(expand)，[参考](#classteacher) |
| course_schedules | 课程表信息(expand)，expand时返回本周课程表，[参考](#courseschedules) |

**功能配置列表**

| 字段名 | 描述 |
| --- | --- |
| features | 班级简介 |
| albums | 班级故事 |
| praises | 表彰 |
| notifications | 通知 |
| feeds | 资讯(新版本客户端不应使用该字段，等待废弃) |
| feed_categories | 资讯栏目 |
| dynamic_classes | 走班 |
| course_schedules | 课程表 |
| sign_ins | 考勤 |
| duty_students | 值日生 |
| client_notis | 家长通知 |
| external_sections | 外链 |
| memos | 家长留言 |

**响应示例**

```
Status: 200 OK
```

```json
{
  "id": 1,
  "type": "StaticClass",
  "name": "初一(1)班",
  "honor": true,
  "honor_image_url": "https://cdn.adachina.net/honor_event_image",
  "class_teacher": "_",
  "course_schedules": "_",
  "extra": {
    "security": "_"
  },
  "config": {
    "features": true,
    "albums": false,
    "praises": true,
    "notifications": true,
    "duty_students": false,
    "client_notis": false,
    "memos": true,
    "feeds": true,
    "feed_categories": [
      {
        "feed_category_id": 1,
        "name": "资讯",
        "icon_url": "https://cdn.adachina.net/icon",
        "title_url": "https://cdn.adachina.net/title"
      },
      //....
    ],
    "external_sections": [
      {
        "external_section_id": 1,
        "title": "外部网页ABC",
        "link": "example.com",
        "image_url": "https://cdn.adachina.net/image.png",
        "icon_url": "https://cdn.adachina.net/icon.png",
      },
      //...
    ]
    // ...
  }
}
```

## class_teacher

班级对应班主任/安全负责人详细信息

```
GET /classes/:class_id/class_teacher
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 班级唯一id |
| name | 班级名称 |
| avatar | 教师头像url |

**响应示例**

```
Status: 200 OK
```

```json
{
  "id": 1,
  "name": "凌枫",
  "avatar": "http://cdn.brand.com/e8d573593a"
}
```

## duty_students

班级值日生当日信息

```
GET /classes/:class_id/duty_students
```

**响应参数**

| 字段名 | 描述 |
| --- | --- |
| id | 值日生id，非学生id |
| duty_date | 值日日期 |
| student | 值日学生信息(expand)，[参考](./baseinfo.md#student) |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "id": 1,
    "duty_date": "2016-11-30",
    "student": "https://{api_endpoint}/students/1"
  }
]
```

## course_schedules

班级每日课程表信息，默认返回本周课程数组，可能会出现课程信息为空的情况，但是节次不会为空

```
GET /classes/:class_id/course_schedules
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| from_date | integer | 否 | 开始日期，按照iso8601标准 | 1 |
| to_date | integer | 否 | 结束日期，按照iso8601标准 | 10 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| date | 课程表日期 |
| wday_text | 星期 |
| courses | 课程列表 |
| id | 课程id |
| name | 课程名称 |
| schedule_no | 课程节次 |
| type | 课程类型(固定班,走班,无课): ['static', 'dynamic', 'empty'] |
| course_time | 课程时间段 |
| sign_time | 签到时间段 |
| started_at | 时间段开始时间 |
| ended_at | 时间段结束时间 |
| teacher | 授课老师(expand)，[参考](#classteacher) |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "date": "2015-01-01",
    "wday_text": "星期四",
    "courses": [
      {
        "id": 1,
        "name": "数学",
        "schedule_no": "1",
        "type": "static",
        "course_time": {
          "started_at": "2016-10-25T10:15:17+08:00",
          "ended_at": "2016-10-25T11:15:17+08:00"
        },
        "sign_time": {
          "started_at": "2016-10-25T10:15:17+08:00",
          "ended_at": "2016-10-25T11:15:17+08:00"
        },
        "teacher": "https://{api_endpoint}/teachers/1"
      },
      {
        "id": null,
        "name": null,
        "schedule_no": "2",
        "type": null,
        "course_time": {
          "started_at": "2016-10-25T10:15:17+08:00",
          "ended_at": "2016-10-25T11:15:17+08:00"
        },
        "sign_time": {
          "started_at": "2016-10-25T10:15:17+08:00",
          "ended_at": "2016-10-25T11:15:17+08:00"
        },
        "teacher": null
      }
    ]
  }
]
```

## features

班级简介列表，按顺序返回

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

## selected_features

首页班级简介列表

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

## features/comments

某个班级简介的评论列表

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

## photo_albums

班级故事列表

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

## photo_albums/photos

某个班级故事的相片列表

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

## selected_photos

首页相片列表

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

## photos/comments

某张张片的评论

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

## praises

班级表彰列表

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

## selected_praises

首页班级表彰列表

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

## praises/comments

某个表彰的评论列表

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

## praises/students

某个表彰的学生列表

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

## client_notis

家长通知列表

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

## client_notis/:id

单个家长通知

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

## selected_client_notis

首页展示当天的家长通知

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

## memos (Demo功能)

家长留言

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

## memos/:student_id

学生家长留言列表

```
GET /api/classes/:id/memos/:student_id
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| student_id | string | 是 | 学生ID，路由参数 | 103 |
| last_id | integer | 否 | 返回该留言ID后的新留言 | 19 |

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

## memos/:student_id

创建留言
```
POST /api/classes/:id/memos/:student_id
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| student_id | string | 是 | 学生ID，路由参数 | 103 |
| video | string | 是 | 七牛视频文件Key | upload/abcd/dffa2 |

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
