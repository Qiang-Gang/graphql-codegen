# GraphQL Codegen

## 简介

这是一个学习 GraphQL Code Generator 的全栈示例项目。通过 GraphQL Code Generator 实现前后端类型安全的数据交互，展示了从 Schema 设计、类型生成到前后端集成的完整开发流程。项目采用 Monorepo 架构，融合了 Vue 和 NestJS 生态系统的最佳实践。

## 项目结构

```
├── apps/            # 应用目录
│   ├── web/        # Nuxt.js 3 前端应用
│   │   ├── pages/  # 页面组件
│   │   ├── components/# Vue 组件
│   │   ├── composables/# 组合式函数
│   │   ├── layouts/# 布局组件
│   │   ├── plugins/# Nuxt 插件
│   │   └── graphql/# GraphQL 查询和类型
│   └── server/     # NestJS 后端应用
│       ├── src/    # 源代码
│       │   ├── modules/# 业务模块
│       │   ├── common/# 公共模块
│       │   └── config/# 应用配置
│       └── graphql/# GraphQL schema 和解析器
├── libs/           # 共享库目录
│   ├── graphql/    # 共享 GraphQL schema 和类型
│   │   ├── schema/ # 基础 schema 定义
│   │   └── types/  # 共享 GraphQL 类型
│   ├── models/     # 共享数据模型
│   ├── config/     # 共享配置
│   │   ├── eslint/ # ESLint 配置
│   │   └── tsconfig/# TypeScript 配置
│   └── utils/      # 共享工具函数
│       ├── database/# 数据库相关
│       └── helpers/# 通用辅助函数
├── docker/         # Docker 相关配置
│   ├── mysql/      # MySQL 容器配置
│   └── nginx/      # Nginx 容器配置
└── scripts/        # 项目脚本
    ├── setup/      # 环境设置脚本
    ├── build/      # 构建脚本
    └── deploy/     # 部署脚本
```

## 技术栈

### 核心技术
- 语言：TypeScript 5.0+
- GraphQL 相关：
  - GraphQL 16.x
  - GraphQL Code Generator 5.x
  - Apollo Server 4.x (后端)
  - Apollo Client 3.x (前端)
  - GraphQL Tools 9.x

### 后端
- 框架：NestJS 10.x
- 数据库相关：
  - MySQL 8.0+
  - TypeORM 0.3.x / Prisma 5.x
- 特性：
  - 模块化架构
  - 依赖注入系统
  - GraphQL 装饰器支持
  - 数据库迁移工具
  - 认证与授权
  - API 文档自动生成

### 前端
- 框架：Nuxt.js 3.7+
- 核心：Vue 3.3+
- UI 框架：PrimeVue 3.x
- 状态管理：Pinia 2.x
- 工具集：
  - VueUse 10.x
  - Vue Apollo 4.x
  - Vue Router 4.x (Nuxt 内置)
  - Nuxt DevTools

### 工程化工具
- 包管理器：pnpm 8.x
- 运行环境：Node.js 18.x LTS
- 代码规范：
  - ESLint 8.x
  - Prettier 3.x
  - TypeScript strict 模式
  - Husky (Git Hooks)
  - Commitlint
- 构建工具：
  - Vite 4.x (Nuxt 内置)
  - esbuild (NestJS)
- 测试工具：
  - Vitest
  - Cypress
  - Supertest

## 主要特性

1. 完整的 GraphQL API 开发流程演示
2. 自动生成 TypeScript 类型定义
3. 前端 GraphQL 查询代码生成
4. 后端 GraphQL 解析器代码生成
5. Schema First 开发方式实践
6. Vue 组合式 API 与 GraphQL 的最佳实践集成
7. NestJS 与 Vue 生态系统的无缝协作

## 开发环境设置

1. 安装 pnpm (如果未安装)
```bash
npm install -g pnpm
```

2. 安装项目依赖
```bash
pnpm install
```

3. 数据库配置
```bash
# 确保 MySQL 服务已启动，并创建数据库
mysql -u root -p
CREATE DATABASE graphql_codegen;
```

4. 启动开发服务器
```bash
# 启动后端服务
pnpm --filter server dev

# 启动前端服务
pnpm --filter web dev
```

## GraphQL Code Generator 配置

项目使用 GraphQL Code Generator 来自动生成类型定义和相关代码。配置文件位于各个应用的根目录下：

- `apps/web/codegen.ts` - 前端代码生成配置
- `apps/server/codegen.ts` - 后端代码生成配置

## GraphQL 安全最佳实践

### 查询复杂度控制
- 使用 Query Complexity 限制查询深度
- 实现 Rate Limiting
- 设置合理的超时时间

### 身份认证和授权
- 使用 JWT 进行身份验证
- 实现细粒度的字段级权限控制
- 使用 GraphQL Shield 处理权限逻辑

### 数据验证
- 输入验证使用 class-validator
- 自定义标量类型验证
- 实现请求参数净化

## GraphQL Code Generator 使用示例

### 基础配置示例
```yaml
# codegen.yml
schema: './libs/graphql/schema/*.graphql'
generates:
  ./libs/graphql/types/index.ts:
    plugins:
      - typescript
      - typescript-resolvers
    config:
      contextType: './context#Context'
      enumsAsTypes: true
```

### 前端查询示例
```graphql
# 在 apps/web/graphql/queries/user.graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
    email
  }
}
```

### 生成的类型示例
```typescript
// 自动生成的类型定义
export type User = {
  id: string;
  name: string;
  email: string;
};

export type GetUserQuery = {
  user: User;
};
```

## 本地开发环境要求

### 必需软件
- Node.js 18.x LTS
- pnpm 8.x
- Docker Desktop
- MySQL 8.x（如不使用 Docker）
- Git 2.x+

### 可选但推荐的工具
- TablePlus/DBeaver（数据库管理工具）
- Postman/Insomnia（API 测试工具）
- GraphQL Playground（GraphQL API 测试工具）
- Docker Desktop Dashboard（容器管理）

### 关键文件说明

```bash
├── apps/web/
│   ├── nuxt.config.ts          # Nuxt.js 配置文件
│   ├── codegen.yml            # 前端 GraphQL 代码生成配置
│   └── tsconfig.json          # 前端 TypeScript 配置
├── apps/server/
│   ├── src/app.module.ts      # NestJS 根模块
│   ├── codegen.yml           # 后端 GraphQL 代码生成配置
│   └── ormconfig.js          # TypeORM 配置
├── libs/graphql/
│   ├── schema/
│   │   ├── user.graphql      # 用户相关 Schema
│   │   └── common.graphql    # 共享类型定义
│   └── types/                # 生成的 TypeScript 类型
└── docker-compose.yml        # Docker 编排配置
```

### 推荐的 IDE 配置
- VS Code 扩展：
  - Vue Language Features
  - GraphQL
  - ESLint
  - Prettier
  - TypeScript Vue Plugin
  - Docker
  - GitLens

### 环境变量配置
```bash
# .env.example
# 后端配置
SERVER_PORT=3000
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=root
MYSQL_DATABASE=graphql_codegen

# 前端配置
NUXT_PUBLIC_API_URL=http://localhost:3000/graphql
```

### 推荐的 VS Code 设置
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib"
}
```

## GraphQL Code Generator 详细配置

### 后端配置示例
```yaml
# apps/server/codegen.yml
schema: '../../libs/graphql/schema/*.graphql'
generates:
  src/generated/graphql.ts:
    plugins:
      - typescript
      - typescript-resolvers
    config:
      contextType: '../types#Context'
      mappers:
        User: '../entities#UserEntity'
      scalars:
        DateTime: Date
      enumsAsTypes: true
```

### 前端配置示例
```yaml
# apps/web/codegen.yml
schema: '../../libs/graphql/schema/*.graphql'
documents: './graphql/**/*.graphql'
generates:
  ./graphql/generated/index.ts:
    plugins:
      - typescript
      - typescript-operations
      - typescript-vue-apollo
    config:
      vueCompositionApiImportFrom: vue
      composition: true
      withCompositionFunctions: true
```

## 学习要点

1. GraphQL Schema 设计
2. 类型定义自动生成
3. 前端查询代码生成
4. 后端解析器类型生成
5. Fragment 使用和代码复用
6. 自定义标量类型处理
7. GraphQL 指令使用

## 注意事项

- 确保 Node.js 版本 >= 18.x LTS
- 使用 pnpm 作为包管理器
- 遵循 Schema First 的开发方式
- 保持生成的代码与 Schema 同步

## 技术选型说明

1. **前端技术栈**
   - Nuxt.js 3 提供完整的 Vue 3 + TypeScript 开发体验
   - Vue Apollo 完美集成 Vue 的响应式系统
   - PrimeVue 提供丰富的 UI 组件，对 Vue 3 支持良好

2. **后端技术栈**
   - NestJS 提供模块化架构，完美支持 GraphQL
   - 支持多种 GraphQL 实现（Apollo、Mercurius）
   - 与 TypeORM/Prisma 完美集成，提供强大的数据库操作能力

3. **全栈协作**
   - 前后端使用 TypeScript，提供一致的开发体验
   - GraphQL Code Generator 自动生成类型，确保前后端类型安全
   - Monorepo 结构便于共享代码和类型定义

## 开发规范

### Git 提交规范
- 使用 Conventional Commits
- 格式：`<type>(<scope>): <description>`
- 主要类型：
  - feat: 新功能
  - fix: 修复
  - docs: 文档更新
  - style: 代码格式
  - refactor: 重构
  - test: 测试相关
  - chore: 构建/工具链相关

### 数据库规范
- 使用 TypeORM Migration 进行数据库版本控制
- 表名使用小写下划线命名
- 字段名使用小写下划线命名
- 必须包含 created_at, updated_at 字段
- 外键命名规则：fk_表名_关联表名

### 开发流程
1. 功能分支开发
   - 从 develop 分支创建特性分支
   - 分支命名：feature/功能名称
   - Bug 修复分支：fix/问题描述

2. 代码审查
   - 提交 Pull Request
   - 至少一个审查者批准
   - CI 检查通过

3. 测试要求
   - 单元测试覆盖率 > 80%
   - E2E 测试覆盖关键流程
   - 提供测试文档

### GraphQL 开发流程
1. Schema 设计
   - 在 libs/graphql/schema 定义基础 schema
   - 遵循 GraphQL 命名规范
   - 使用 GraphQL 注释文档化

2. 类型生成
   - 运行 codegen 生成类型
   - 验证生成的类型
   - 更新相关测试

3. 后端实现
   - 实现 Resolver
   - 添加单元测试
   - 文档化 API

4. 前端集成
   - 实现查询/变更
   - 集成生成的类型
   - 添加组件测试

## 部署说明

### 开发环境
```bash
docker-compose -f docker-compose.dev.yml up -d
```

### 生产环境
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### 数据库迁移
```bash
# 创建迁移
pnpm --filter server migration:generate

# 运行迁移
pnpm --filter server migration:run
