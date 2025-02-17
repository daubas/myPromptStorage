# Coze触发器(Triggers)功能说明

## 概述

触发器允许代理在特定时间或事件发生时自动执行任务。目前有以下重要限制：

- 仅在Discord渠道生效
- 每个代理最多支持10个触发器
- 触发的工作流或插件需在1分钟内完成
- 工作流需禁用流式输出功能

### 支持的触发器类型

1. **定时触发器(Scheduled trigger)**
   - 在指定时间触发执行
   
2. **事件触发器(Event trigger)**
   - 当服务器向webhook URL发送HTTPS请求时触发执行

## 定时触发器配置

### 配置步骤

1. 登录Coze
2. 从左侧"My Workspace"面板选择团队空间
3. 在agents页面选择或创建代理
4. 在开发页面找到Triggers功能并点击"+"图标

### 配置项说明

| 配置项 | 说明 |
|--------|------|
| Name | 触发器名称 |
| Trigger type | 选择"Scheduled trigger" |
| Trigger time | 设置触发时间 |
| Task execution | 选择执行方式 |

### 任务执行方式

1. **代理提示(agent prompt)**
   - 使用自然语言描述任务
   - 示例：每天早上9点发送最新AI新闻

2. **插件(Plugin)**
   - 点击Task execution字段上方的"+"图标选择插件
   - 根据需要输入插件参数

3. **工作流(Workflow)**
   - 点击Task execution字段上方的"+"图标选择工作流

## 事件触发器配置

### 步骤1：配置事件触发器

1. 进入触发器配置页面（同定时触发器步骤1-4）
2. 在创建触发器对话框中完成以下配置：

| 配置项 | 说明 |
|--------|------|
| Name | 触发器名称 |
| Trigger type | 选择"Event trigger" |
| Mode | 仅支持Webhook模式，当向webhook URL发送HTTPS请求时触发 |
| Bearer token | 用于请求认证的令牌，可使用默认值或自定义 |
| Parameters | POST请求的请求体参数定义 |
| Task execution | 任务执行方式选择 |

#### Task execution配置选项：

1. **代理提示(Agent prompt)**
   - 使用自然语言描述任务
   - 支持使用{{variable}}引用参数值
   
2. **插件或工作流(Plugin/Workflow)**
   - 点击Task execution上方的"+"图标选择
   - 参数值支持引用webhook请求参数

### 步骤2：激活事件触发器

发送HTTPS请求到webhook URL来触发任务执行：

```bash
curl --location --request POST '<Trigger Webhook URL>' \
--header 'Authorization: Bearer <Trigger Bearer Token>' \
--header 'Content-Type: application/json' \
--data '<Trigger Parameters>'
```

参数说明：
- `<Trigger Webhook URL>`: webhook URL地址，可在配置触发器时复制
- `<Trigger Bearer Token>`: 用于认证的令牌，可在配置触发器时复制
- `<Trigger Parameters>`: JSON格式的请求数据

请求示例：
```bash
curl --location 'https://api.xxxx/api/xxxx' \
--header 'Authorization: Bearer ABCxxxxx' \
--header 'Content-Type: application/json' \
--data '{"url": "www.example.com"}'
```

#### curl命令说明
- `curl`: 支持通过HTTP、HTTPS、FTP等协议发送和接收数据的命令行工具
- `--request POST`: 定义请求方法为POST
- `--header`: 设置请求头信息
- `--data`: 设置请求体数据

#### 响应处理
- 当`BaseResp`中的`StatusCode`为`0`时，表示请求成功
- 当`StatusCode`不为`0`时，需根据`HttpCallBackRespDatas`中的错误信息调整请求
- 请求成功后，触发器中配置的任务将被执行

![触发器执行示意图](https://p16-arcosite-va.ibyteimg.com/tos-maliva-i-10qhjjqwgv-us/d82205c9748b47fa93d1f29347055427~tplv-10qhjjqwgv-image.image)

### 允许用户配置定时任务

Coze支持用户在与代理对话时创建定时任务：

1. 在代理的Develop页面找到Triggers功能
2. 勾选"允许用户在对话中创建定时任务"选项
3. 点击"在开场白中添加引导问题"链接
4. 系统会自动在Opening Dialog面板中添加一个开场白
5. 您可以修改开场白或添加更多引导用户设置定时任务的问题


// Webhook配置
Mode：Webhook
  ├─ Webhook URL：触发器专属调用地址
  ├─ Bearer token：身份验证令牌
  └─ Parameters：定义请求参数结构

// 参数引用示例
在插件/工作流参数值处：
选择 Reference > 选择预定义的webhook参数
