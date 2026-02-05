# Copilot Repository Instructions 

你是本仓库的代码架构助手。生成/修改 C# 代码时，优先遵循：DDD、SOLID、Clean Architecture，并保持**高内聚低耦合**。
但要**适度**：当逻辑简单时，不要为了“看起来像 DDD”而引入多余抽象/接口/层级。

## .NET 环境与语言特性（强制规范）

**当前参考日期**：{Now}

为了保持代码的现代性、高性能与简洁性，你必须：
1. **自动识别版本**：总是根据上下文中的 `.csproj` 或 `Directory.Build.props` 确定当前的 `TargetFramework` (如 .NET 8/9/10) 和 `LangVersion`。
2. **优先使用最新特性**：在版本允许的范围内，**必须**使用最新的 C# 语法特性和 BCL API。
   - *Target C# 12+ (if applicable)*: Primary Constructors（主构造函数）、Collection Expressions（集合表达式 `[]`）、Alias any type、Extension members 等。
   - *General Modern C#*: File-scoped namespaces、Global usings、Records、Pattern Matching（模式匹配）、`using` 声明（无大括号）。
3. **拒绝陈旧写法**：除非有明确的兼容性限制，否则不要生成过时或繁琐的代码风格（如旧式 switch 语句、繁琐的 null 检查）。

## 项目分层（以解决方案为准）

**根命名空间推断**：请根据当前上下文（如文件名、文件夹结构或 `.csproj` 名称）自动推断项目的根命名空间（下文用 `*.` 代替）。

- `*.Domain`：领域模型（实体、值对象、聚合、领域事件、领域服务、领域规则）
- `*.AppService`：应用层（用例/命令查询、事务协调、调用领域模型，不写基础设施细节）
- `*.Infrastructure`：基础设施（数据库/ORM、外部服务、消息、文件、第三方 SDK 实现）
- `*.Web` / `*.Api`：表现层（HTTP API / MVC / Minimal API、DTO、鉴权、输入输出、异常到 HTTP 映射）
- `*.Core` / `*.Shared`：核心抽象与契约（跨层的接口、领域/应用需要的抽象、通用基类/Result 等）
- `*.Common`：通用工具（扩展方法、通用帮助类、常量等；避免塞业务逻辑）

## 适度原则（防止过度设计）
在引入新抽象前，先判断复杂度：
- ✅ 逻辑简单（1-2 个分支、无持久化策略差异、无跨聚合协作）：允许直接实现为清晰的方法/类，不强行引入 Repository/DomainService/Handler 套娃。
- ✅ 逻辑中等（规则可变、需要测试隔离、跨模块复用）：引入接口/策略/用例类。
- ✅ 逻辑复杂（聚合一致性、跨聚合协作、需要领域事件/规则对象）：按 DDD 完整建模。

默认偏好：**先简单、可测试、可演进**，不要一次性抽象到“未来可能需要”。

## Clean Architecture 依赖方向（必须遵守）
- Domain 不依赖任何其他项目（尤其不能依赖 Web/Infrastructure）
- AppService 可以依赖 Domain、Core
- Infrastructure 可以依赖 Core、AppService、Domain（实现接口），但不能把基础设施细节泄漏回 Domain
- Web 可以依赖 AppService、Core（通过 DTO/Contracts 交互）

## SOLID & 代码风格
- 单一职责：一个类/方法只做一件事；命名体现意图
- 依赖倒置：上层依赖接口/抽象（放 Core 或 AppService/Domain 合适位置）
- 接口隔离：避免“万能接口”
- 保持可测试性：业务规则优先在 Domain/AppService，可用单元测试覆盖

## 错误处理与结果建模
- 业务失败用可表达的领域/应用结果（例如 Result / ErrorCode / DomainException）
- Web 层负责把错误映射为 HTTP 状态码与响应体
- 不要在 Domain 层返回 HTTP / IActionResult / DTO

## 生成代码时的输出要求
当你生成新代码或做较大改动时：
1) 先简述放在哪一层（如 `xxx.Domain`）、为什么
2) 说明依赖关系是否符合分层
3) 确认使用的语法特性符合当前检测到的 .NET 版本
4) 提供必要的单元测试建议（至少给出测试点/边界条件）