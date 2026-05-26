---
title: 编码规范
last_updated: 2026-05-19
---

# 编码规范

本文档按触发场景组织。编写对应语言/层次代码前查阅。

> 标记说明：【强制】= 必须遵守；【推荐】= 应尽量遵守

---

## Java 后端编码时适用

编写 Java 代码（Controller / Service / Mapper / Entity / DTO）时遵守：

- 【强制】构建管理：使用 Maven，多模块项目结构（如 `core`、`api`、`service`）
- 【强制】代码风格：遵循阿里巴巴 Java 开发手册，缩进 4 空格，单行长度 ≤ 120
- 【强制】类型安全：禁止使用 `Object` 作为泛型边界，优先使用具体类型；集合必须声明泛型
- 【强制】异常处理：不要吞掉异常，上层统一通过全局异常处理器返回标准错误响应
- 【强制】日志：使用 SLF4J + Logback，关键操作记录 `INFO`，异常记录 `ERROR` 并附上下文
- 【强制】数据库访问：使用 MyBatis-Plus 或 JPA，所有 SQL 参数化查询，禁止拼接字符串
- 【强制】事务管理：`@Transactional` 仅用于需要原子性的服务方法，注意事务传播行为
- 【强制】Bean 管理：优先使用构造器注入，避免字段 `@Autowired`
- 【强制】API 设计：RESTful 风格，路径使用复数名词，版本号为 `/api/v1/`
- 【强制】测试：单元测试使用 JUnit 5 + Mockito，覆盖率目标 ≥ 70%
- 【强制】所有 API 接口需定义清晰的请求/响应 DTO，避免直接暴露实体

---

## Vue 前端编码时适用

编写 Vue 组件 / Store / 路由 / 工具函数时遵守：

- 【强制】框架：Vue 3
- 【强制】语法：`<script setup lang="ts">`
- 【强制】语言：TypeScript (strict mode)
- 【强制】状态管理：Pinia (setup stores)
- 【强制】路由：Vue Router
- 【强制】工具库：VueUse
- 【强制】始终使用 Composition API
- 【强制】始终使用 TypeScript 定义 Props，例如 `defineProps<T>()`
- 【强制】避免使用 Options API 或 `any` 类型
- 【强制】组件命名采用 PascalCase，目录与文件名采用 kebab-case
- 【强制】状态提升：跨组件共享状态优先使用 Pinia，局部状态使用 `ref/reactive`
- 【强制】路由导航使用 `useRouter` 和 `useRoute`，不得直接操作 `window.location`
- 【强制】单元测试，覆盖率目标 ≥ 70%

---

## 数据库操作时适用

编写 SQL、MyBatis Mapper、Flyway migration、或者通过代码操作数据库时遵守：

- 【强制】所有表使用 InnoDB 引擎，字符集 `utf8mb4`，排序规则 `utf8mb4_bin`
- 【强制】主键统一使用自增 `BIGINT` 或雪花算法生成，命名为 `id`
- 【强制】字段命名使用下划线分隔小写（如 `created_at`），Java 实体使用驼峰映射
- 【强制】必备审计字段：`create_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP`，`update_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
- 【强制】索引命名：`idx_表名_字段名`，唯一索引 `uk_表名_字段名`
- 【强制】禁止外键，通过应用层保证数据完整性
- 【强制】多表查询避免 3 表以上 JOIN，必要时拆分或引入缓存
- 【强制】数据迁移脚本使用 Flyway 或 Liquibase，版本化存放于 `db/migration/`
- 【强制】抽象一层数据访问，屏蔽底层数据库实现
- 【强制】批量语句中使用绑定变量；批量删除后对索引进行 shrink compact 操作
- 【强制】连接数据库应该优先采用连接池方式
- 【强制】数据库连接池配置需合理，根据业务需要配置初始、最小、最大连接数；设置连接超时时间
