# 验证API

## 学生刷卡验证身份(Demo环境)

```
GET /api/student_card_auth
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| student_id | string | 是 | 学生ID | 1 |
| card_number | string | 是 | 学生卡号 | 12345678 |

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 学生ID |
| name | 学生姓名 |
| avatar | 学生头像图片地址 |

**响应示例**

验证成功响应

```
Status: 200 OK
```

```json
{
  "id": 1,
  "name": "王小明",
  "avatar": "https://cdn.adachina.net/image"
}
```

验证失败响应

```
Status: 403 Forbidden
```

```json
{
  "error": "forbidden",
  "error_code": 303,
  "message": "验证失败"
}
```
