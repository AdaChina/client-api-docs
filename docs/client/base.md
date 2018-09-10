# 基础功能

## 当前班牌的设备信息和绑定信息

```
GET /device
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| serial_no | 设备序列号 |
| iot: id | 阿里物联网 ID |
| iot: secret | 阿里物联网设备 Secret |
| iot: product_key |  阿里物联网 Product Keys |
| iot: face_license | 是否授权注册人脸识别 |
| school | 设备绑定学校信息，若无绑定学校时为`null` |
| badge_url | 校徽图片url，没有时为`null` |
| assistant_image_url | 小艾图片 |
| class | 设备绑定班级信息(内部字段可以expand，[参考](./classroom.md#class)), 若无绑定班级时为`null` |
| power_schedule | 设备定时开关机日程表,默认返回从请求当天往后7天的数据 |
| config | 设备功能配置 |

**响应示例**

```
Status: 200 OK
```

```json
{
    "serial_no": "sn12345678",
    "iot": {
      "id": "JIO#GJJ$SA",
      "secret": "KKEEOO#$KA",
      "product_key": "ABCDKEY",
      "face_license": "false"
    },
    "school": {
        "id": 1,
        "name": "XX中学",
        "badge_url": "https://cdn.adachina.net/badge",
        "assistant_image_url": "https://cdn.adachina.net/assistant_image"
    },
    "class": {
        "id": 1,
        "type": "StaticClass",
        "name": "初一(1)班",
        "watchword": "我成功因为我志在成功！",
        "security": "https://{api_endpoint}/teacers/1",
        "class_teacher": "https://{api_endpoint}/classes/1/class_teacher",
        "duty_students": "https://{api_endpoint}/classes/1/duty_students",
        "course_schedules": "https://{api_endpoint}/classes/1/course_schedules",
        "config": {
            "features": false,
            "photo_albums": false,
            "praises": false,
            "feeds": false,
            "feed_categories": [],
            "dynamic_classes": true,
            "notifications": false,
            "course_schedules": false,
            "client_notis": false,
            "external_sections": [],
            "authorization": {
                "card": false,
                "facial_recog": true,
                "facial_recog_extensions": ["human_detection"],
                "facial_recog_exts": {"human_detection": true, "detection_threshold": "60.5"}
            },
            "school_feature": {
                "memo": true,
                "attendance": false
            }
        }
    },
    "power_schedule": [
        {
            "shutdown_wday": 5,
            "startup_wday": 6,
            "shutdown_at": "2017-02-24T23:52:00+08:00",
            "startup_at": "2017-02-25T23:57:00+08:00"
        },
        {
            "shutdown_wday": 6,
            "startup_wday": 0,
            "shutdown_at": "2017-02-25T23:52:00+08:00",
            "startup_at": "2017-02-26T23:57:00+08:00"
        },
        {
            "shutdown_wday": 0,
            "startup_wday": 1,
            "shutdown_at": "2017-02-26T23:52:00+08:00",
            "startup_at": "2017-02-27T23:57:00+08:00"
        },
        {
            "shutdown_wday": 1,
            "startup_wday": 2,
            "shutdown_at": "2017-02-28T23:52:00+08:00",
            "startup_at": "2017-03-01T23:57:00+08:00"
        }
    ],
}
```

## 设备定时开关机时间（第二版）

```
GET /device/power_schedule
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| startup_at | 开机时间(24小时制) |
| shutdown_at | 关机时间(24小时制) |
| effective_days | 有效日(星期一: 1, 星期日: 7) |

**响应示例**

```
Status: 200 OK
```

```json
{
    "startup_at": "09:30",
    "shutdown_at": "22:30",
    "effective_days": [1,2,3,4,5,6,7]
}
```

## 设备可绑定的班级列表

班级信息只提供id,类型和班级名

```
GET /avaliable_classes
```

**响应字段**

| 字段名 | 描述 |
| --- | --- |
| id | 班级id |
| type | 班级类型: StaticClass(固定班)，DynamicClass(走班) |
| name | 班级名 |

**响应示例**

```
Status: 200 OK
```

```json
[
    {
        "id": 1,
        "type": "StaticClass",
        "name": "初一(1)班"
    }
]
```

## 绑定班级

```
POST /bind_class
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| class_id | integer | 是 | 要绑定的班级id | 1 |

**响应**

```
Status: 201 created
```

响应数据参考设备基础信息API的响应

**失败响应**

已绑定其他班级

```
Status: 403 Forbidden
```

```json
{
    "error": "already_bind_class",
    "error_code": 106,
    "message": "已绑定班级"
}
```

绑定过程失败

```
Status: 403 Forbidden
```

```json
{
    "error": "bind_fail",
    "error_code": 111,
    "message": "绑定班级失败"
}
```

## 解除设备当前绑定班级

```
POST /unbind_class
```

**响应示例**

```
Status: 200 OK
```

```json
{
    "msg": "ok"
}
```

**失败响应**

解除绑定失败

```
Status: 403 Forbidden
```

```json
{
    "error": "unbind_fail",
    "error_code": 112,
    "message": "解除绑定班级失败"
}
```

## 人脸库同步状态反馈

```
POST /report_face_status
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| result | string | 是 | 同步结果 | success |
| success_ids | string | 否 | 成功学生ID列表 | "1,2,3" |
| failure_ids | string | 否 | 失败学生ID列表 | "4,5,6" |

| result 参考值| 描述 |
| -- | -- |
| success | 同步成功 |
| error | 同步出错 |

**响应示例**

成功响应:

```
Status: 201 created
```

```json
{
  "result": "received"
}
```

失败响应:

```
Status: 400 Bad Request
```

```json
{
    "error": "face_status_invalid",
    "error_code": 119,
    "message": "无法识别结果参数"
}
```

## 定时开关机状态反馈

```
POST /report_power_schedule_status
```

**请求参数**

| 参数名 | 参数类型 | 必填 | 描述 | 示例 |
| --- | --- | --- | --- | --- |
| result | string | 是 | 设置结果 | success |
| info | string | 否 | 调试参考信息 | time: 20181215, startup: 0700, shutdown: 1800 |

| result 参考值| 描述 |
| -- | -- |
| success | 设置成功 |
| failure | 设置失败 |

**响应示例**

成功响应:

```
Status: 201 created
```

```json
{
  "result": "received",
  "status": "power_schedule_success",
  "info": "time: 20181215, startup: 0700, shutdown: 1800"
}
```
