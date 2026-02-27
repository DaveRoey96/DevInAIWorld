# AI时代的调试与排错

> AI 不是万能的，但不会用 AI 调试是万万不能的。

## AI 调试的优势

### 传统调试 vs AI 调试

| 方面 | 传统方式 | AI 辅助 |
|-----|---------|--------|
| 错误理解 | 需要经验 | 快速解释 |
| 根因分析 | 靠积累 | 引导分析 |
| 解决方案 | 靠搜索 | 多种方案 |
| 效率 | 低 | 高 |

---

## 常见使用场景

### 1. 解释错误信息

**场景**：遇到看不懂的错误

**Prompt**：
```
这个错误是什么意思？
Error: Cannot read properties of undefined (reading 'map')
at UserList.render (UserList.js:45:23)
React 版本 18.2.0
```

**AI 能做的**：
- 解释错误含义
- 指出可能原因
- 给出修复建议

---

### 2. 定位 Bug

**场景**：程序运行结果不对

**Prompt**：
```
帮我找找这段代码的问题：
```javascript
function calculateTotal(items) {
  return items.reduce((sum, item) => {
    return sum + item.price * item.quantity;
  }, 0);
}

// 测试
console.log(calculateTotal([
  { price: 10, quantity: 2 },
  { price: 5, quantity: 3 }
]));
// 期望: 35，实际: 35...但有时候结果不对
```

**AI 能做的**：
- 分析代码逻辑
- 指出潜在问题
- 建议测试用例

---

### 3. 性能问题

**场景**：程序慢

**Prompt**：
```
这段 Python 代码很慢，处理 10 万条数据需要 30 秒：
```python
def process_data(data):
    result = []
    for item in data:
        new_item = {}
        for key, value in item.items():
            new_item[key] = transform(value)
        result.append(new_item)
    return result
```
如何优化？

data 是字典列表，每个字典约 20 个键值对
```

**AI 能做的**：
- 分析性能瓶颈
- 给出优化方案
- 提供代码示例

---

### 4. 异常处理

**场景**：程序崩溃

**Prompt**：
```
这个 Python 程序偶尔会崩溃：
```
Traceback (most recent call last):
  File "app.py", line 45, in get_user
    return users[user_id]
KeyError: '12345'
```
users 是从数据库查出来的，可能不存在
如何优雅地处理？
```

**AI 能做的**：
- 分析异常原因
- 给出处理方案
- 推荐最佳实践

---

## 调试 Prompt 技巧

### 1. 提供完整上下文

**❌ 信息不足**：
```
代码报错，帮看看
```

**✅ 信息完整**：
```
Python 3.11 + FastAPI 环境下，这段代码报错：
```
Error: pydantic.error_wrappers.ValidationError
```
代码在 user_service.py 第 23 行：
```python
class User(BaseModel):
    name: str
    age: int
    email: str

user = User(name="Tom", age="25")  # age 传了字符串
```
预期 age 应该是 int，传了字符串会报错
如何修复？
```

---

### 2. 描述期望 vs 实际

```
期望：用户登录成功后跳转到 /dashboard
实际：页面卡住不动，控制台没有错误
```

---

### 3. 提供环境信息

- 编程语言和版本
- 框架和版本
- 操作系统
- 依赖版本

---

### 4. 让 AI 推理而非直接给答案

**Prompt**：
```
帮我分析这个问题，不要直接给答案，引导我自己找到解决方案：
1. 先解释这个问题是什么
2. 可能的原因有哪些
3. 如何验证是哪个原因
```

---

## AI 调试的局限

### ⚠️ AI 不擅长的

1. **复现特定环境问题**
   - 生产环境特有的 bug
   - 并发问题
   - 内存泄漏

2. **需要实验验证**
   - 需要实际运行测试
   - 需要查看监控数据
   - 需要 profiling

3. **复杂分布式系统**
   - 多服务交互问题
   - 网络问题
   - 硬件问题

---

### 正确使用方式

| 适合 AI | 不适合 AI |
|--------|----------|
| 解释错误 | 复现生产 bug |
| 分析代码逻辑 | 调试并发问题 |
| 给出优化思路 | 硬件/网络问题 |
| 推荐最佳实践 | 复杂系统交互 |

---

## 调试工作流

### 1. 遇到问题

```
先尝试自己理解 + 搜索引擎
```

### 2. 使用 AI

```
描述问题：
- 错误信息
- 期望 vs 实际
- 复现步骤
- 环境信息
```

### 3. 验证方案

```
不要盲目相信 AI 的方案
本地测试验证
```

### 4. 如果 AI 解决不了

```
- 提供更多细节
- 搜索相关案例
- 请教同事
- 线下排查
```

---

## 最佳实践

1. **先自己理解**：AI 是助手，不是替代
2. **给足信息**：上下文越多，答案越准
3. **验证一切**：AI 也会错
4. **学会提问**：好问题才有好答案
5. **记录经验**：形成自己的调试笔记

---

*AI 能帮你调试，但不能帮你理解。*
*最终的判断，还是在你。*
