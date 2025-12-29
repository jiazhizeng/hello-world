```mermaid
graph LR
    User((用户/浏览器))
    DB[(核心数据库)]

    subgraph "Web 服务器 (Python CGI)"
        direction TB
        Process[CGI 进程启动]
        SQL_Wait["同步执行大 SQL (Join 10+ 表)"]
        Render[HTML 模板渲染]
    end

    %% 流程连线
    User -->|1. 发起请求| Process
    Process --> SQL_Wait
    SQL_Wait -->|2. 建立连接| DB
    DB -.->|3. 漫长等待 8s+| SQL_Wait
    SQL_Wait -->|4. 获取数据| Render
    Render -->|5. 返回完整 HTML| User

    %% 样式
    style SQL_Wait fill:#ffcccc,stroke:#f00,stroke-width:2px
    style DB fill:#ffcccc,stroke:#f00,stroke-width:2px
