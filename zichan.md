```mermaid
graph TD
    %% 触发层：解决“为何交接”
    subgraph Trigger_Layer [1. 多维触发层]
        T1[离职检测: 企微回调服务] 
        T2[线索盘活: 业务侧沉淀线索激活]
        T1 & T2 --> Dispatcher{继承任务分发}
    end

    %% 执行层：解决“如何交接”，体现原子化操作
    subgraph Execution_Layer [2. 资产继承执行层]
        Dispatcher -->|离职路径| LC[离职客户继承]
        LC -->|API原子操作| LG[离职客户群继承]
        
        Dispatcher -->|在职路径| ZC[在职客户继承]
        ZC -->|API原子操作| ZG[在职群继承]
    end

    %% 反馈与校验层：体现异步处理能力
    subgraph Feedback_Layer [3. 状态回执与闭环层]
        LG & ZG --> Async_Check[异步查询接替状态]
        Async_Check -->|校验不通过| Retry[自动重试机制/限频控制]
        Async_Check -->|校验通过| Sync_CRM[同步更新 CRM 负责人状态]
    end

    %% 审计层：体现 T10 级别的安全兜底思维
    subgraph Audit_Layer [4. 稳定性保障层]
        Sync_CRM --> Alert[异常提醒: 企微/邮件告警]
        Sync_CRM --> Recon[离线对账: 定期全量扫描校验]
    end

    %% 数据产出
    Recon --> Outcome{0% 资产流失率}
