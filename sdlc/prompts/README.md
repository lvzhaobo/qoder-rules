# SDLC 角色提示词文档索引

本目录包含了软件开发全生命周期（SDLC）各个角色的提示词文档，方便根据角色进行代码审查和工作指导。

## 📋 文档列表

### 个人效能 (Personal Productivity)
- **[personal-productivity.md](./personal-productivity.md)** - 全员通用
  - 遗留代码理解
  - 微任务自动化
  - 报错排查

### 深度分析 (Deep Analysis)
- **[architect.md](./architect.md)** - 架构师
  - 全链路扫描
  - C4 架构图
  - 复杂时序图
  - 原理可视化

- **[security-expert.md](./security-expert.md)** - 安全专家
  - 性能与安全分析

- **[reflector.md](./reflector.md)** - 反思者
  - 红队挑战

### 可视化与设计 (Visualization & Design)
- **[designer.md](./designer.md)** - UI/UX 设计师
  - UI 原型设计

### 项目管理 (Project Management)
- **[project-manager.md](./project-manager.md)** - 项目经理
  - WBS 任务拆解
  - 进度与风险分析

### 运维与部署 (DevOps)
- **[devops-engineer.md](./devops-engineer.md)** - 运维工程师/DevOps
  - 阿里云 ECS 部署
  - CI/CD 流水线

### 管理视窗 (Management)
- **[rd-director.md](./rd-director.md)** - 研发总监
  - 稳定性审计
  - 效能扫描

### 产品 (Product)
- **[product-manager.md](./product-manager.md)** - 产品经理
  - 复杂需求拆解
  - 用户故事

- **[product-director.md](./product-director.md)** - 产品总监
  - PRD 评审
  - 路线图规划

### 后端开发 (Backend)
- **[backend-developer.md](./backend-developer.md)** - 后端开发
  - 企业级 API 设计

- **[backend-lead.md](./backend-lead.md)** - 后端组长
  - 代码审查
  - 技术债务评估

### 前端开发 (Frontend)
- **[frontend-developer.md](./frontend-developer.md)** - 前端开发
  - Design System 实现

- **[frontend-lead.md](./frontend-lead.md)** - 前端组长
  - 性能审计
  - 跨团队联调

### 测试与运维 (QA/Ops)
- **[qa-engineer.md](./qa-engineer.md)** - 测试工程师
  - 自动化用例生成

- **[ops-lead.md](./ops-lead.md)** - 运维组长
  - 基础设施审计

---

## 🚀 使用方式

### 1. 代码审查场景
根据你的角色选择对应的文档，使用其中的提示词来审查代码：

```
示例：作为后端组长审查代码
1. 打开 backend-lead.md
2. 复制"代码审查"场景的提示词
3. 替换占位符（如 [阿里巴巴 Java 开发手册]）
4. 将提示词和待审查代码一起提交给 AI
```

### 2. 配置 IDE System Prompt
将通用规则配置到 IDE 的 System Prompt 中：

```
"你是一位就职于 [公司名称] 的高级工程师。我们的技术栈是 [Spring Boot 2.7 / React 18]，内部封装库为 [MyCorp-Common-Lib]。请遵循 [阿里巴巴Java开发手册] 规范。回答请保持简洁专业。"
```

### 3. 自定义占位符
所有提示词中的 `[...]` 标记都是占位符，需要根据实际情况替换：

- `[公司名称]` → 你的公司名称
- `[Spring Boot 2.7]` → 实际使用的技术栈版本
- `[阿里巴巴Java开发手册]` → 团队遵循的规范文档

---

## 📌 注意事项

1. **占位符替换**：使用前务必替换所有 `[...]` 占位符
2. **上下文注入**：为获得最佳效果，请提前注入企业上下文
3. **场景选择**：根据实际需求选择合适的场景提示词
4. **持续更新**：建议根据团队实践定期更新和完善提示词

---

## 🔗 相关资源

- **原始文档**：[SDLC_Prompts_Table.html](../SDLC_Prompts_Table.html)
- **适用团队**：50+ 人的大型研发团队
- **优化方向**：区分个人效能、团队交付与管理审核视角

---

**更新时间**：2026-01-07  
**文档版本**：v1.0  
**维护团队**：研发效能团队
