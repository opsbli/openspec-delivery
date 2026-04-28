# 示例 Change

当你需要一个具体的 `OpenSpec + Superpowers` change 起点时，使用本参考。

下面示例使用 feature `add-user-auth`。

## 推荐 Change Layout

```text
openspec/
└── changes/
    └── add-user-auth/
        ├── proposal.md
        ├── design.md
        ├── tasks.md
        └── specs/
            └── auth/
                └── spec.md
```

## 示例 `proposal.md`

```md
# Proposal: 新增 User Authentication

## Why

当前产品没有 authenticated user flow。敏感页面和用户专属操作需要基于 account 的 access control。

## Goals

- 支持 email 与 password registration
- 支持 email 与 password login
- 签发 authenticated sessions 或 tokens
- 保护 authenticated routes

## Non-Goals

- Social login
- Two-factor authentication
- Password recovery

## Impact

- Backend auth module
- User persistence model
- Frontend login 与 registration pages
- Route guards 或 session middleware
```

## 示例 `design.md`

```md
# Design: 新增 User Authentication

## Overview

引入基础 email-password authentication system，包含 route protection 和 session persistence。

## Decisions

### Credential handling

- persistence 前 hash passwords
- 永不向 clients 返回 password hashes

### Session model

- 使用带 expiration 的 signed tokens
- 如果后续需要 refresh mechanism，再单独设计

### Frontend flow

- 增加独立 login 与 registration screens
- authenticated users 访问 auth pages 时重定向
- 使用 guard 保护 authenticated routes

## Risks

- Token expiration edge cases
- Registration 与 login error-state consistency
- Frontend/backend validation rules 不一致
```

## 示例 `specs/auth/spec.md`

```md
# Auth Specification：User Authentication

## ADDED Requirements

### Requirement: Register with email and password

系统 SHALL 允许新用户使用有效 email address 和 password 注册。

#### Scenario: Valid registration

- GIVEN 用户提供有效 email 和 password
- WHEN registration request 被提交
- THEN account 被创建
- AND response 表示 success

#### Scenario: Duplicate email

- GIVEN 该 email 已存在 account
- WHEN registration request 被提交
- THEN request 被拒绝
- AND response 表示该 email 已被使用

### Requirement: Login with valid credentials

系统 SHALL 使用有效 credentials 认证用户。

#### Scenario: Valid login

- GIVEN registered user 提供有效 credentials
- WHEN login request 被提交
- THEN authentication 成功
- AND 系统返回 authenticated session 或 token

#### Scenario: Invalid password

- GIVEN registered user 提供 invalid password
- WHEN login request 被提交
- THEN authentication 失败
- AND response 不暴露 sensitive internals

### Requirement: Protect authenticated routes

系统 SHALL 阻止 unauthenticated access 访问 protected routes。

#### Scenario: Unauthenticated request

- GIVEN request 指向 protected route 且没有 valid authentication
- WHEN request 被处理
- THEN access 被拒绝
```

## 示例 `tasks.md`

```md
# Tasks: 新增 User Authentication

## Guardrails

### Non-Goals

- 不做 social login
- 不做 2FA
- 不做 password reset flow

## Phase 1: Backend foundation

- [ ] 1.1 添加 user auth persistence model
  DoD: model 可以存储 email 和 password hash，invalid inputs 被 tests 拒绝
- [ ] 1.2 添加 password hashing behavior
  DoD: plaintext 永不持久化，并覆盖一个 boundary case
- [ ] 1.3 添加 registration endpoint
  DoD: valid registration 成功，duplicate email 失败，tests 覆盖两者
- [ ] 1.4 添加 login endpoint
  DoD: valid login 返回 authenticated state，invalid password 安全失败
- [ ] 1.5 添加 route protection middleware 或 guard
  DoD: protected route 拒绝 unauthenticated requests

## Phase 2: Frontend flow

- [ ] 2.1 使用 `web-design-engineer` 定义 auth UI behavior
  DoD: login/registration flow、states、route behavior 已反映在 design 或 spec artifacts 中
- [ ] 2.2 实现 login page
  DoD: success、loading、error states 存在
- [ ] 2.3 实现 registration page
  DoD: validation 与 duplicate-email behavior 清晰呈现
- [ ] 2.4 将 auth state 接入 protected navigation
  DoD: unauthenticated users 被正确重定向

## Phase 3: Verification

- [ ] 3.1 为 validation 和 auth logic 添加 unit tests
  DoD: 每个 behavior slice 先写一个 failing test
- [ ] 3.2 为 registration 和 login 添加 integration coverage
  DoD: happy path 和一个 failure path 被覆盖
- [ ] 3.3 运行 verify 并协调差异
  DoD: implementation 匹配 proposal、design、specs 和 tasks
```

## 使用方式

1. 将 change id 和 capability names 替换为真实 feature。
2. 替换示例 goals 与 non-goals。
3. 围绕 externally observable behavior 收紧 `spec.md`。
4. 如果 `tasks.md` 中某项过宽，继续拆到可执行的 test-first cycle。
5. frontend changes 必须把 `web-design-engineer` 保留为明确任务或 prerequisite。
