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
