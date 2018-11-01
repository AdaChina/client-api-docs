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
