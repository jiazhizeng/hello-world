graph LR
    User((用户)) -->|请求| CGI[Python CGI 脚本]
    subgraph Server Side
    CGI -->|1. 解析请求| Logic
    Logic -->|2. 执行超大SQL (Join 10表)| DB[(数据库)]
    DB -->|3. 返回大量数据 (慢)| Logic
    Logic -->|4. 渲染 HTML 模板| Template
    end
    Template -->|5. 返回完整 HTML (10s)| User
    style CGI fill:#f9f,stroke:#333
    style DB fill:#ff9,stroke:#333
