# 应用升级

## 检查更新

```
GET /upgrades
```

#### 请求参数说明

| 名称 | 类型 | 说明 | 必填 | 例子 |
| -- | -- | -- | -- | -- |
| version | string | APP当前版本号 | 是 | 1.1 |


#### 返回值说明

| 名称 | 类型 | 说明 | 例子 |
| -- | -- | -- | -- |
| need_upgrade | boolean | 当前版本是否需要更新 | true |
| version | string | 最新版本版本号 | "1.2" |
| url | string | 最新版本下载地址 | true |
| task_id | integer | 本次更新id | 1 |

#### 请求例子

```
GET /upgrades?version=1.1
```

#### 响应例子

```json
{
    "need_upgrade": true,
    "version": "1.2",
    "url": "https://www.brand.com/update.zip",
    "task_id": 1
}
```

## 升级结果提交

```
POST /upgrade_records
```

#### 请求参数说明

请求body应为json字符串

| 名称 | 类型 | 说明 | 必填 | 例子 |
| -- | -- | -- | -- | -- |
| result | integer | 升级结果。0表示成功，1表示失败 | 是 | 0 |
| desc | string | 详细信息，提供用于debug的信息 | 是 | 'success' |
| from | string | 升级前版本 | 否 | '1.1' |
| to | string | 升级后版本 | 否 | '1.2' |
| task_id | integer | 检查更新接口处获取的task_id | 是 | 1 |

#### 请求例子

```json
POST /upgrade_records

{
    "result": 0,
    "desc": "success",
    "from": "1.1",
    "to": "1.2",
    "task_id": 1
}
```

#### 响应例子

```json
{
    "msg": "ok"
}
```
