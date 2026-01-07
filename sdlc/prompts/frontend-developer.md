# 前端开发提示词 (Frontend Developer)

## 角色定位
**前端开发** - 负责基于 Design System 的组件开发

---

## 场景: Design System 实现 (Component)

### 目标
基于公司设计系统开发组件

### 提示词

```
请基于 [Ant Design 5 / Material UI]，封装一个业务组件"用户选择器 `UserSelector`"。
需求：
1. **样式**：使用 [Tailwind CSS / Styled Components]，遵循公司品牌色 `#0052CC`。
2. **功能**：支持远程搜索（防抖）、多选、回显。
3. **接口**：数据源调用 `UserService.search(keyword)`。
4. **类型**：提供完整的 TypeScript Interface。
```

### 预计输出
- 类型安全的组件代码
- 符合设计规范的样式实现
- 集成了业务逻辑的复用组件

### 效果
✅ 确保 UI 一致性，减少重复造轮子。

---

## 使用建议

💡 **适用场景：**
- 业务组件封装
- Design System 落地
- 组件库建设
- 跨项目复用组件

**注意事项：**
- 组件应遵循单一职责原则，不要做太多事情
- Props 设计要考虑灵活性和易用性的平衡
- 样式要支持自定义，但有合理的默认值
- TypeScript 类型定义要完整，方便使用者理解
- 组件应具备良好的可访问性（A11y）
