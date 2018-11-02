# 个人中心

## 个人中心面板

```
GET /api/personal/dashboard
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| card_number | string | 否 | 校园卡号 | 0123456789 |
| student_id | integer | 否 | 学生ID | 12 |

必须提供`card_number`或`student_id`其中之一

**响应示例**

成功响应

```
Status: 200 OK
```

```json
{
  "student": {
    "id": 1,
    "name": "姓名",
    "classroom_name": "三年级一班",
    "avatar": "https://cdn.com/1.png",
    "number": "123456789"
  },
  "personal_cards": [
    {
      "type": "student_schedule",
      "icon": "https://cdn.com/icon.png",
      "title": "我的课表",
      "subtitle": "计算机",
      "data": ["计算机一室", "下一节"],
      "ext_url": null
    },
    {
      "type": "memo",
      "icon": "https://cdn.com/icon.png",
      "title": "家长留言",
      "subtitle": "10",
      "data": ["未读条数"],
      "ext_url": null
    },
    {
      "type": "praise",
      "icon": "https://cdn.com/icon.png",
      "title": "我的表彰",
      "subtitle": "3",
      "data": ["本周表彰次数"],
      "ext_url": null
    },
    {
      "type": "external",
      "icon": "https://cdn.com/icon.png",
      "title": " 投放卡片",
      "subtitle": "3",
      "data": ["份作业"],
      "ext_url": "http://example.com?student_id=123school_id=123&class_id=123"
    }
  ]
}
```

## 学生表彰列表

```
GET /api/personal/:student_id/praises
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

## 学生每日课程表信息

默认返回本周课程数组，可能会出现课程信息为空的情况，但是节次不会为空

```
GET /personal/:student_id/course_schedules
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
        "teacher": {
          "id": 250,
          "name": "方红健",
          "avatar": "https://cdn.com/avatar.png"
        },
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
        "teacher": {
          "id": 251,
          "name": "方志刚",
          "avatar": "https://cdn.com/avatar.png"
        },
      }
    ]
  }
]
```
