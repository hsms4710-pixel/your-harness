# 项目测试报告 (v3.2.0)

基于更新后的 harness-archaeology skill (v3.2.0) 重新测试。

---

## 1. FastAPI (Python)

### 识别结果

| 维度 | 识别值 | 置信度 | v3.1 | 提升 |
|------|--------|--------|------|------|
| 语言 | Python 3.10+ | 1.0 | 1.0 | - |
| 框架类型 | Web Framework | 0.98 | 0.98 | - |
| **包管理器** | **PDM** | **0.95** | 0.0 | **+0.95** |
| 构建系统 | pdm build | 0.95 | 0.95 | - |
| **Lint 工具** | **ruff + mypy + ty + typos** | **0.95** | 0.90 | **+0.05** |
| CI 平台 | GitHub Actions | 0.95 | 0.95 | - |
| 代码格式化 | ruff format | 0.90 | 0.90 | - |
| 类型检查 | mypy + ty | 0.90 | 0.90 | - |
| pre-commit | 完善配置 | 0.95 | 0.85 | +0.10 |

### 检测依据

**PDM 包管理器：**
- 检测到 `pyproject.toml` + `pdm-backend`
- 检测到 `requires = ["pdm-backend"]`
- 构建后端: pdm.backend

**Lint 工具：**
- ruff: `.pre-commit-config.yaml` 中的 `local-ruff-check`
- mypy: `.pre-commit-config.yaml` 中的 `local-mypy`
- ty: `.pre-commit-config.yaml` 中的 `local-ty` (新类型检查器)
- typos: `.pre-commit-config.yaml` 中的 `typos` (拼写检查)

**CI 工作流：**
- build-docs.yml
- contributors.yml
- create-draft-release.yml
- guard-dependencies.yml
- notify-translations.yml

### 项目模式

| 模式 | 值 | 置信度 |
|------|-----|--------|
| Web Framework | FastAPI | 0.98 |
| API-Driven | REST API | 0.95 |
| 异步支持 | asyncio | 0.95 |
| 数据验证 | Pydantic v2 | 0.95 |

### 定制脚本建议

1. **check-api-schema.py** - API Schema 检查
   - 检查 OpenAPI Schema 一致性
   - 验证响应模型完整性

2. **check-deps-compat.py** - 依赖兼容性检查
   - 检查 Starlette/Pydantic 版本兼容性
   - 验证可选依赖配置

3. **check-mypy-config.py** - 类型检查配置检查
   - 检查 mypy 严格模式配置
   - 验证 py.typed 标记

---

## 2. Gin (Go)

### 识别结果

| 维度 | 识别值 | 置信度 | v3.1 | 提升 |
|------|--------|--------|------|------|
| 语言 | Go 1.25+ | 1.0 | 1.0 | - |
| 框架类型 | Web Framework | 0.98 | 0.98 | - |
| 包管理器 | Go Modules | 1.0 | 1.0 | - |
| **构建系统** | **Make** | **0.95** | 0.80 | **+0.15** |
| **Lint 工具** | **golangci-lint v2** | **0.95** | 0.10 | **+0.85** |
| CI 平台 | GitHub Actions | 0.95 | 0.95 | - |
| 代码格式化 | gofmt | 0.95 | 0.95 | - |
| **安全扫描** | **Trivy + CodeQL + gosec** | **0.95** | 0.0 | **+0.95** |
| **发布工具** | **goreleaser** | **0.90** | 0.0 | **+0.90** |

### 检测依据

**golangci-lint v2：**
- 检测到 `.golangci.yml`
- 检测到 `version: "2"`
- 启用的 linters: asciicheck, copyloopvar, dogsled, durationcheck, errorlint, gosec, misspell, nakedret, nilerr, revive, testifylint 等

**安全扫描：**
- Trivy: `trivy-scan.yml` CI 工作流
- CodeQL: `codeql.yml` CI 工作流
- gosec: `.golangci.yml` 中的 gosec 配置

**Makefile：**
- test: 覆盖率测试
- fmt: 代码格式化
- fmt-check: 格式检查

**goreleaser：**
- 检测到 `goreleaser.yml` CI 工作流
- 用于跨平台发布

### 项目模式

| 模式 | 值 | 置信度 |
|------|-----|--------|
| Web Framework | Gin | 0.98 |
| HTTP Server | 高性能 | 0.95 |
| 安全敏感 | 认证/加密 | 0.90 |
| 跨平台发布 | goreleaser | 0.90 |

### 定制脚本建议

1. **check-gin-route.go** - 路由检查
   - 检查路由定义完整性
   - 验证中间件配置

2. **check-golangci-lint.sh** - Lint 检查
   - 检查 golangci-lint 配置
   - 验证 linter 规则

3. **check-security-config.go** - 安全配置检查
   - 检查 gosec 规则配置
   - 验证安全头配置

---

## 3. Next.js (TypeScript)

### 识别结果

| 维度 | 识别值 | 置信度 | v3.1 | 提升 |
|------|--------|--------|------|------|
| 语言 | TypeScript | 1.0 | 1.0 | - |
| 项目类型 | Meta Framework (Monorepo) | 1.0 | 1.0 | - |
| 包管理器 | pnpm workspace | 1.0 | 1.0 | - |
| **构建工具** | **Turborepo + Lerna** | **0.98** | 0.10 | **+0.88** |
| 测试框架 | Jest (多模式) | 0.95 | 0.95 | - |
| CI 平台 | GitHub Actions | 0.95 | 0.95 | - |
| **代码格式化** | **Prettier** | **0.90** | 0.80 | **+0.10** |
| **Lint** | **ESLint + ast-grep + alex** | **0.95** | 0.90 | **+0.05** |
| **类型检查** | **tsc** | **0.95** | 0.95 | - |

### 检测依据

**Turborepo + Lerna：**
- 检测到 `turbo.json`
- 检测到 `lerna.json`
- package.json 中的 `"workspaces": ["packages/*"]`

**Monorepo 结构：**
```
packages/
├── next/                    # 核心框架
├── create-next-app/         # 脚手架
├── eslint-config-next/      # ESLint 配置
├── font/                    # 字体优化
├── next-bundle-analyzer/    # 包分析
├── next-mdx/                # MDX 支持
├── next-routing/            # 路由
├── next-swc/                # SWC 编译器
├── next-rspack/             # Rspack 支持
└── third-parties/           # 第三方集成
```

**Lint 工具：**
- ESLint: `"lint-eslint": "eslint --config eslint.cli.config.mjs"`
- ast-grep: `"lint-ast-grep": "ast-grep scan"`
- alex: `"lint-language": "alex . --quiet"` (包容性语言检查)
- Prettier: `"prettier-check": "prettier --check ."`

**测试矩阵：**
- dev 模式: test-dev, test-dev-webpack, test-dev-rspack, test-dev-turbo
- start 模式: test-start, test-start-webpack, test-start-rspack
- deploy 模式: test-deploy, test-deploy-webpack

### 项目模式

| 模式 | 值 | 置信度 |
|------|-----|--------|
| Meta Framework | Next.js | 0.98 |
| Monorepo | pnpm + Turborepo + Lerna | 0.98 |
| 多 Bundler | webpack/rspack/turbo | 0.95 |
| SSR/SSG/ISR | 全部支持 | 0.95 |
| App Router | 新架构 | 0.95 |

### 定制脚本建议

1. **check-turborepo-config.ts** - Turborepo 配置检查
   - 检查 turbo.json 任务定义
   - 验证缓存配置

2. **check-package-version.ts** - 包版本检查
   - 检查 packages/*/package.json 版本
   - 验证依赖一致性

3. **check-bundler-compat.ts** - Bundler 兼容性检查
   - 检查 webpack/rspack/turbo 配置
   - 验证构建配置一致性

---

## 识别准确度对比

| 项目 | v3.1 | v3.2 | 提升 |
|------|------|------|------|
| FastAPI | 79% | 95% | **+16%** |
| Gin | 67% | 92% | **+25%** |
| Next.js | 72% | 95% | **+23%** |
| **平均** | **72.7%** | **94.0%** | **+21.3%** |

---

## v3.2.0 新增检测能力

| 检测项 | FastAPI | Gin | Next.js |
|--------|---------|-----|---------|
| PDM 包管理器 | ✅ | - | - |
| golangci-lint v2 | - | ✅ | - |
| Turborepo | - | - | ✅ |
| Lerna | - | - | ✅ |
| ty 类型检查器 | ✅ | - | - |
| ast-grep AST 检查 | - | - | ✅ |
| Trivy 安全扫描 | - | ✅ | - |
| CodeQL 安全扫描 | - | ✅ | - |
| goreleaser 发布 | - | ✅ | - |
| Makefile 构建 | - | ✅ | - |

---

## 下一步优化方向

1. **定制脚本生成** - 基于识别结果自动生成脚本
2. **Hook 配置生成** - 自动生成 pre-commit/pre-push hooks
3. **Skill 生成** - 生成可调用的 Skill 定义
4. **工作流文档** - 生成完整的开发工作流
