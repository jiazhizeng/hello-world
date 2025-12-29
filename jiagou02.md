```mermaid
graph LR
    User((用户/浏览器))
    CDN[静态资源 CDN\n(HTML/JS/CSS)]
    
    subgraph "Golang 服务端 (高并发)"
        API[API 网关]
        subgraph "Goroutines 并行处理"
            G1[协程 1: 查主表]
            G2[协程 2: 查关联表]
            G3[协程 3: 查缓存]
        end
    end
    
    DB[(数据库/Redis)]

    %% 阶段 1：静态资源秒开
    User -->|1. 请求页面框架| CDN
    CDN -.->|2. 秒级渲染首屏| User

    %% 阶段 2：异步数据拉取
    User -->|3. Ajax 请求数据| API
    API --> G1 & G2 & G3
    G1 & G2 & G3 --> DB
    DB -.->|4. 快速返回| API
    API -->|5. 返回 JSON| User

    %% 样式修饰：用绿色代表高效
    style CDN fill:#ccffcc,stroke:#090
    style API fill:#ccffcc,stroke:#090
    style G1 fill:#e6ffcc,stroke:#333
    style G2 fill:#e6ffcc,stroke:#333
    style G3 fill:#e6ffcc,stroke:#333
