# 架构师提示词 (Architect)

## 角色定位
**架构师** - 负责全链路扫描、架构设计、技术布道等工作

---

## 场景 1: 全链路扫描 (Deep Scan)

### 目标
扫描依赖、环境与技术栈

### 提示词

```
请对当前项目/模块进行深度扫描：
1. **依赖分析**：扫描 [pom.xml / package.json]，列出所有第三方库及其版本，标记出过时或高风险的依赖。
2. **环境依赖**：识别代码中对特定 OS、JDK 版本或环境变量的隐式依赖。
3. **类依赖图**：分析类 [OrderService] 的耦合度，列出它依赖了哪些内部模块（如 `UserModule`, `PaymentModule`）。
4. **技术栈快照**：总结当前使用的核心技术栈（如 Spring Boot 2.7, React 18, MyBatis-Plus）。
```

### 预计输出
- 依赖健康度报告
- 环境约束清单
- 模块耦合度拓扑图

### 效果
✅ 快速掌握项目全貌，识别架构腐化风险。

---

## 场景 2: C4 架构图 (Architecture)

### 目标
生成清晰的 Mermaid C4 系统上下文图

### 提示词

```
请为 [电商订单系统] 生成 Mermaid C4 Context 图。
1. **系统边界**：核心是 OrderSystem。
2. **外部依赖**：包含 PaymentGateway (第三方支付), UserCenter (内部 RPC), InventoryDB (数据库)。
3. **用户**：MobileUser, AdminUser。
4. **关系**：描述清楚 User -> OrderSystem -> PaymentGateway 的调用流向。
5. **代码要求**：输出标准 Mermaid 代码，确保图表布局合理。
```

### 预计输出
- Mermaid C4 代码块
- 清晰的系统边界定义
- 外部依赖关系梳理

### 效果
✅ 一键生成架构图，快速对齐系统上下文。

---

## 场景 3: 复杂时序图 (Sequence)

### 目标
梳理复杂的异步业务流程

### 提示词

```
请生成 Mermaid 时序图，描述"订单支付成功后的回调处理"流程：
1. **参与方**：PaymentService, OrderService, MQ, InventoryService, NotificationService。
2. **流程**：
   - PaymentService 收到回调 -> 更新本地流水 -> 发送 MQ 消息。
   - OrderService 消费 MQ -> 幂等检查 -> 更新订单状态。
   - OrderService -> 调用 InventoryService 扣减库存 (RPC)。
   - 并行：调用 NotificationService 发送短信。
3. **异常**：如果库存扣减失败，需体现补偿逻辑 (Compensating Transaction)。
```

### 预计输出
- 包含异步/并行逻辑的时序图
- 异常路径的可视化

### 效果
✅ 将晦涩的文字流程转化为直观的时序图，减少沟通歧义。

---

## 场景 4: 原理可视化 (Dynamic Canvas)

### 目标
用动态图形解释抽象算法

### 提示词

```
我需要给团队讲解 [Raft 共识算法的 Leader 选举] 过程。
请编写一个单文件 HTML：
1. **画布**：使用 HTML5 Canvas 或 SVG。
2. **逻辑**：用 JavaScript 模拟 5 个节点的状态（Follower/Candidate/Leader）。
3. **动画**：
   - 显示"心跳包"从 Leader 发出的动态效果。
   - 模拟 Leader 宕机，节点变色并发起投票的过程。
4. **交互**：点击节点可模拟宕机/恢复。
```

### 预计输出
- 可交互的 HTML 演示文件
- 动态的算法执行过程

### 效果
✅ 让抽象的技术原理"活"起来，极大地降低理解门槛。

---

## 使用建议

💡 **适用场景：**
- 系统架构评审
- 技术方案设计
- 团队技术培训
- 遗留系统分析

**注意事项：**
- 生成的架构图应定期更新，保持与实际架构同步
- 使用 C4 模型时注意区分不同层次（Context、Container、Component、Code）
- 时序图应重点关注关键路径和异常处理流程
