```mermaid
graph TD
    ClientA[业务服务 A] -->|1. 请求 Token (带 AppID)| TokenSvc[Token 管理服务]
    ClientB[业务服务 B] -->|1. 请求 Token (带 AppID)| TokenSvc
    
    subgraph "Token 管理服务内部"
        MemCache[内存缓存]
        Logic[刷新逻辑]
        SecretMgr[Secret 管理/配置]
    end
    
    TokenSvc -->|2. 读缓存| Redis[(Redis)]
    Logic -->|3. 定时刷新/加锁| WeCom[企业微信 API]
    Logic -->|4. 获取 Secret| Vault[HashiCorp Vault / KMS / 加密DB]
