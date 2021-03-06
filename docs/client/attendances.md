# 考勤API

## 学生签到

```
POST /attendance
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 | 备注 |
| --- | --- | --- | --- | --- | -- |
| card_number | string | 二选一 | 学生卡号 | 12345678 | 刷卡考勤时提供 |
| student_id | integer | 二选一 | 学生ID | 15 | 人脸考勤时提供 |

**响应字段**

| 字段名 | 描述 | 参数类型 |
| --- | --- | --- |
| id | 学生ID | integer |
| name | 学生姓名 |  string |
| avatar | 学生头像图片地址 | string |
| state | 签到状态类型 | string |
| message | 签到结果消息 | string |
| new_record | 是否存在新签到记录 | boolean |

**响应示例**

成功响应

```
Status: 200 OK
```

```json
{
  "student": {
    "id": 1,
    "name": "王小明",
    "avatar": "https://cdn.adachina.net/image"
  },
  "results": [{
    "state": "warning",
    "message": "上学迟到"
  },
  {
    "state": "success",
    "message": "按时上课"
  }],
  "new_record": false
}
```

失败响应

```
Status: 404 Forbidden
```

```json
{
  "error": "student_not_found",
  "error_code": 115,
  "message": "学生不存在"
}
```

## 上学考勤统计(班级)

```
GET /school_attendances
```

**响应字段**

| 字段名 | 描述 | 参数类型 |
| --- | --- | --- |
| total | 班级学生总数 | integer |
| on-time | 准时 | integer |
| late | 迟到 | integer |
| absent | 未签到 | integer |
| id | 学生ID | integer |
| name | 学生姓名 |  string |
| avatar | 学生头像图片地址 | string |

成功响应

```
Status: 200 OK
```

```json
{
  "total": 20,
  "ontime": 15,
  "late": 3,
  "absent": 2,
  "details": [
    {
      "status": "absent",
      "student": {
        "id": 1,
        "name": "王小明",
        "avatar": "https://cdn.adachina.net/image"
      }
    }
  ]
}
```

## 走班考勤统计

```
GET /course_attendances
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| date | string | 是 | 日期 | "2000-12-30" |
| schedule_no | integer | 是 | 节次 | 5 |

**响应字段**

| 字段名 | 描述 | 参数类型 |
| --- | --- | --- |
| total | 班级学生总数 | integer |
| ontime | 准时 | integer |
| late | 迟到 | integer |
| absent | 未签到 | integer |
| id | 学生ID | integer |
| name | 学生姓名 |  string |
| avatar | 学生头像图片地址 | string |

成功响应

```
Status: 200 OK
```

```json
{
  "total": 20,
  "ontime": 15,
  "late": 3,
  "absent": 2,
  "details": [
    {
      "status": "absent",
      "student": {
        "id": 1,
        "name": "王小明",
        "avatar": "https://cdn.adachina.net/image"
      }
    }
  ]
}
```

失败响应

```
Status: 404 Forbidden
```

```json
{
  "error": "course_schedule_not_found",
  "error_code": 118,
  "message": "走班课不存在"
}
```
