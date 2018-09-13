## 指定班级详细信息

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
| honor_events | 班级评比 |
| exam_events | 考场安排 |
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
  "honor_events": [
    {
      "image_url": 'https://cdn.adachina.net/img1',
      "image_url": 'https://cdn.adachina.net/img2'
    }
  ],
  "exam_events": true,
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
        "partner": false
      },
      //...
    ]
    // ...
  }
}
```

## 班级对应班主任/安全负责人详细信息

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

## 班级值日生当日信息

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

## 班级每日课程表信息

默认返回本周课程数组，可能会出现课程信息为空的情况，但是节次不会为空

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

## 班级荣誉

返回所有班级荣誉，按学期归类，默认按学期日期倒序排列

```
GET /classes/:class_id/term_honor_events
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| term_name | 学期名称 |
| honor_events | 班级荣誉列表 |
| title | 班级荣誉名称 |
| date | 班级荣誉时间 |

**响应示例**

```
Status: 200 OK
```

```json
[
  {
    "term_name": "三年级下学期",
    "honor_events": [
      {
        "title": "班级荣誉",
        "date": "2018-02-01"
      }
    ]
  },
  {
    "term_name": "三年级上学期",
    "honor_events": [
      {
        "title": "班级荣誉",
        "date": "2017-09-01"
      }
    ]
  }
]
```
