# Coze Plugin 創建指南

## 目錄
- [概述](#概述)
- [創建步驟](#創建步驟)
- [授權配置](#授權配置)
- [調試與發布](#調試與發布)
- [API 文件格式](#api-文件格式)

## 概述

Coze Plugin 允許通過導入 JSON 或 YAML 文件來定義 API 並創建插件。主要特點：

- 個人工作區的插件僅對個人可見
- 團隊工作區的插件對所有團隊成員可見
- 插件更新後，所有使用該插件的代理和工作流會自動切換到最新版本
- 支持 OpenAPI、Swagger 或 Postman Collection 協議

## 創建步驟

### 1. 登入與工作區選擇
1. 登入 Coze 平台
2. 在左側導航欄選擇工作區
3. 從頂部工作區列表中選擇個人或團隊工作區

### 2. 創建插件
1. 點擊資源庫頁面右上角的 "+ Resource"
2. 選擇 "Plugin" 選項

### 3. 導入 API
點擊右上角的 "Import"，選擇以下任一方式：
- **本地文件**：上傳本地 JSON 或 YAML 文件
- **URL 導入**：輸入 API 文件的 URL 地址
- **原始數據**：直接輸入 JSON 或 YAML 格式的 API 數據

### 4. 配置基本信息
填寫以下必要信息：
- **插件圖標**：上傳自定義圖標
- **插件名稱**：便於 LLM 搜索和使用的清晰名稱
- **插件描述**：說明插件用途
- **插件 URL**：API 訪問地址（必須使用域名格式）
- **Header 列表**：HTTP 請求頭參數

## 授權配置

支持以下授權方式：

### 1. 無需授權
適用於公開 API

### 2. Service 認證
#### a) Service token / API key
- **位置**：Header 或 Query
- **參數名稱**：密鑰或令牌的參數名
- **值**：實際的密鑰或令牌值

#### b) OAuth 2.0 & OIDC
- **授權類型**：
  - TokenExchange
  - ClientCredential
- **配置項**：
  - endpoint_url
  - audience
  - scope
  - client_id

### 3. OAuth 標準
- client_id
- client_secret
- client_url
- scope
- authorization_url
- authorization_content_type

## 調試與發布

### 調試流程
1. 在插件詳情頁啟用工具
2. 點擊操作欄中的 "Debug" 按鈕
3. 完成工具調試
4. 確認調試通過

### 發布流程
1. 點擊右上角 "Publish"
2. 確認是否需要填寫個人信息
3. 同意相關協議
4. 可選擇發布到 Coze 商店

## API 文件格式

支持以下格式：
1. OpenAPI 3.0
2. Swagger (OpenAPI 2.0)
3. Postman Collection

### 注意事項
- 插件 URL 必須使用域名格式，不支持 IP 格式
- 多個 API 同時導入時需共享相同的 base URL
- 需遵守相關隱私法規和 Coze 用戶協議
- 發布到商店可增加插件的可見性和使用範圍

## 最佳實踐
1. 使用清晰易懂的插件名稱
2. 提供詳細的插件描述
3. 確保 API 文檔格式正確
4. 完整測試所有功能點
5. 注意安全性配置

## API 範例

### OpenAPI 3.0 範例

```yaml
info:
    description: httpbin apis
    title: Httpbin APIs
    version: v1
openapi: 3.0.1
paths:
    /get:
        get:
            operationId: get
            parameters:
                - description: name
                  in: query
                  name: name
                  schema:
                    type: string
            requestBody:
                content:
                    application/json:
                        schema:
                            type: object
            responses:
                "200":
                    content:
                        application/json:
                            schema:
                                properties:
                                    args:
                                        properties:
                                            name:
                                                type: string
                                        type: object
                                type: object
                    description: new desc
                default:
                    description: ""
            summary: API to get
    /post:
        post:
            operationId: post
            requestBody:
                content:
                    application/json:
                        schema:
                            properties:
                                A:
                                    description: A
                                    type: string
                                B:
                                    description: B
                                    type: integer
                            type: object
            responses:
                "200":
                    content:
                        application/json:
                            schema:
                                properties:
                                    json:
                                        properties:
                                            A:
                                                type: string
                                            B:
                                                type: number
                                        type: object
                                type: object
                    description: new desc
                default:
                    description: ""
            summary: API to get
servers:
    - url: https://httpbin.org
```

### Swagger (OpenAPI 2.0) 範例

```yaml
swagger: "2.0"
info:
  description: "這是一個示例 Petstore 服務器。更多信息可以在 Swagger.io 找到。"
  version: "1.0.0"
  title: "Swagger_Petstore_Sample"
  termsOfService: "http://swagger.io/terms/"
host: "petstore.swagger.io"
basePath: "/v2"
tags:
- name: "pet"
  description: "關於寵物的所有操作"
schemes:
- "https"
- "http"
paths:
  /pet:
    post:
      tags:
      - "pet"
      summary: "添加新寵物到商店"
      description: ""
      operationId: "addPet"
      consumes:
      - "application/json"
      produces:
      - "application/json"
      parameters:
      - in: "body"
        name: "body"
        description: "需要添加的寵物對象"
        required: true
        schema:
          $ref: "#/definitions/Pet"
      responses:
        "400":
          description: "無效輸入"
```

### Postman Collection 範例

```json
{
    "info": {
        "name": "REST API CRUD",
        "description": "# 🚀 CRUD 操作示例 (GET, POST, PUT, DELETE)",
        "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
    },
    "item": [
        {
            "name": "Get data",
            "request": {
                "method": "GET",
                "header": [
                    {
                        "key": "Connection",
                        "value": "Keep-Alive"
                    }
                ],
                "url": {
                    "raw": "{{base_url}}/get?city=beijing&type=park",
                    "host": ["{{base_url}}"],
                    "path": ["get"],
                    "query": [
                        {
                            "key": "city",
                            "value": "beijing"
                        },
                        {
                            "key": "type",
                            "value": "park"
                        }
                    ]
                },
                "description": "這是一個 GET 請求示例，用於從端點獲取數據。"
            }
        }
    ],
    "variable": [
        {
            "key": "base_url",
            "value": "https://httpbin.org/"
        }
    ]
}
```

> 注意：Postman Collection 格式的請求體和響應體僅支持 application/json 格式。