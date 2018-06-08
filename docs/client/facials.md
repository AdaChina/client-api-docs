# 人脸识别API

## 获取学生数据

按条件获取学生数据

| 提供的请求参数 | 返回结果范围 |
| -- | -- |
| 学校ID | 全校学生数据 |
| 学校ID + 年级 | 学校当前年级学生数据 |
| 不提供 | 设备当前班级学生数据 |

```
GET /face_set
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| school_id | integer | 否 | 学校ID | 12345678 |
| grade | integer | 否 | 年级 | 12 |

**响应字段**

| 字段名 | 描述 | 参数类型 |
| --- | --- | --- |
| id | 学生ID | integer |
| name | 学生姓名 |  string |
| avatar | 学生头像图片地址 | string |
| card_number | 学生卡号 | string |

**响应示例**

成功响应

```
Status: 200 OK
```

```json
[
  {
    "id": 1,
    "name": "王小明",
    "avatar": "https://cdn.adachina.net/image_1.jpg",
    "card_number": "123123"
  },
  {
    "id": 2,
    "name": "张文怡",
    "avatar": "https://cdn.adachina.net/image_2.jpg",
    "card_number": "123124"
  }
]
```
