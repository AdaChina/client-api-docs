# 应用功能API

## 请求错误情况

### 设备认证header有误

认证header有误或未被添加到后台

```
Status: 401 Unauthorized
```

```json
{
    "error": "not_authorize",
    "error_code": 102,
    "message": "该设备密钥信息有误"
}
```

### 未绑定学校

除基础功能API中的设备API和其他功能API外，其他接口都需要设备绑定学校才能访问。未绑定的情况下返回如下响应：

```
Status: 403 Forbidden
```

```json
{
    "error": "school_not_bind",
    "error_code": 104,
    "message": "该设备未被添加到学校"
}
```

### 未绑定班级

设备访问指定班级的相关接口时，未绑定的情况下返回如下响应：

```
Status: 403 Forbidden
```

```json
{
    "error": "class_not_bind",
    "error_code": 103,
    "message": "该设备没绑定班级"
}
```

### 学校未开启功能

设备访问需要可配置的功能API时(如考勤API)，若学校未开启该功能，则返回如下响应：

```
Status: 403 Forbidden
```

```json
{
    "error": "ability_disable",
    "error_code": 114,
    "message": "学校未开启此功能"
}
```
