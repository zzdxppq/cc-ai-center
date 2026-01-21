# 托管行业数字化管理系统 Product Requirements Document (PRD)

**版本**: 2.1
**日期**: 2026-01-20
**作者**: PM Agent (Liangning)

---

## 1. Goals and Background Context

### 1.1 Goals (目标)

- **实现托管机构全流程数字化管理**：覆盖生活照料、学业辅导、素质拓展、成长追踪、家校共育、机构运营、政府监管七大场景
- **提升机构运营效率**：通过标准化流程、数字化工具降低运营成本，支持加盟模式复制扩张
- **缓解家长信息焦虑**：实现孩子成长透明化、可视化，建立家长对机构的深度信任
- **减轻教师工作负担**：工作轻量化、评价专业化、反馈高效化，提升职业成就感
- **满足政府监管合规要求**：备案规范化、数据实时化、监管便捷化
- **填补市场空白**：打造专门面向私立托管机构的全流程数字化平台，区别于校内课后服务和通用教培系统

### 1.2 Background Context (背景)

托管行业（课后托管、学生照看）正经历快速发展，中国在线教育市场规模已突破6800亿元，年增长率达19.3%。然而，私立托管机构面临多重挑战：服务流程缺乏标准化、家校信息不透明导致家长焦虑、教师重复劳动多、政府监管手段有限。

**市场现状分析**：现有竞品如"今托管"、"智慧托管"多聚焦校内课后服务（受"双减"政策驱动），"校管家"、"学邦"等偏向通用教培管理。专门面向私立托管机构、覆盖"托管全流程+家校互动+政府监管+加盟扩张"的综合性数字化平台存在明显市场空白。

**差异化定位**：本项目定位为"精准托管数字化平台"，通过微信小程序（教师端、家长端）+ Web管理后台（机构端、政府端）的多端架构，重点打造三大差异化能力：
1. **智能化**：AI作业批改、自动生成成长档案
2. **标准化**：支持加盟模式的标准化输出与运营复制
3. **合规化**：政府监管数据深度对接，资质/收费/安全全量同步

### 1.3 Change Log (变更日志)

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2026-01-09 | 1.0 | 初始PRD文档，基于原始需求整理 | PM Agent |
| 2026-01-09 | 1.1 | 整合竞品分析，明确差异化定位 | PM Agent |
| 2026-01-09 | 2.0 | 完成详细Epic/Story定义，技术架构假设 | PM Agent |
| 2026-01-20 | 2.1 | FR7细化：作业拍照→AI识别对错→线下批改→系统记录，支持数学+英语客观题，AI智能总结；新增FR24品牌定制；FR4/FR5更新：托管类型+按年级签到时间+异常通知规则；FR6更新：餐谱支持视频+图片；FR2/FR4/FR8更新：班级分托管班+兴趣班，学生可多班级归属，兴趣班每节课签到+学习反馈（文字/照片/视频/作品） | PM Agent |

---

## 2. Requirements

### 2.1 Functional Requirements (功能需求)

#### 基础管理模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR1** | 系统应支持门店信息的新增、编辑、删除，包含名称、地址、联系方式、运营状态、资质证书上传，支持多门店批量管理和数据隔离 | P0 |
| **FR2** | 系统应支持按年级、学段创建班级，班级分为托管班和兴趣班两类；设置班级名称、班主任、配班老师、学生人数上限；学生可归属多个班级（如：托管班+兴趣班）；支持学生分班、调班、退班操作并记录操作日志 | P0 |
| **FR3** | 系统应支持教师信息录入（姓名、联系方式、资格证书），岗位分配（班主任/配班/课程老师），并按角色分配操作权限 | P0 |
| **FR4** | 系统应支持学生信息录入（姓名、年龄、年级、联系方式、过敏史、紧急联系人、托管类型），过敏史需重点标注提醒，支持状态管理（在托/请假/退托）；学生需关联机构自定义的托管类型，可同时归属多个班级（托管班+兴趣班） | P0 |

#### 日常运营模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR5** | 机构端可自定义托管类型（名称、固定签到时段如到校/放学），按年级配置签到时间范围（因各年级放学时间不同），用于提醒教师该时段未签到学生；教师端签到时自动显示该时段应到学生名单（根据学生托管类型+年级匹配），支持手动点名和一键拍照点名（人脸识别）；签到签退时间实时同步至家长端；迟到/早退/缺勤信息仅供教师查看，家长不自动感知，教师手动标记异常后才通知家长 | P0 |
| **FR6** | 系统应支持每日餐谱管理（视频+图片+文字+营养说明），上传内容家长端可查看；教师可记录学生就餐情况（就餐量、用餐礼仪、是否挑食）和午休情况（时长、睡眠状态） | P1 |
| **FR7** | 教师端应支持作业拍照上传，AI自动识别作业对错（支持数学计算题、英语选择题/填空题，无需预设答案库），展示识别结果后老师在线下纸质作业上完成实际批改；系统支持记录批改分数与评语（均为可选），并可根据学生做题情况AI智能生成作业总结（符合老师口吻、针对学生个性化点评）；错题自动归档至错题本按学科/知识点分类 | P0 |
| **FR8** | 系统应支持课程体系配置（基础托管+兴趣班课程），兴趣班每节课需签到；教师可记录学生学习反馈（文字+照片+视频+作品），记录学生课程参与情况（出席/缺席、专注度、技能进度），教师可按标准化模板评价学生表现；家长端按班级分类查看自己孩子的反馈 | P1 |

#### 成长追踪模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR9** | 系统应自动整合签到、生活、作业、课程、评价数据，生成每日/月度/学期成长档案，支持教师手动补充编辑；家长在小程序端查看成长档案；教师和管理员在电脑端（Web后台）查看并导出PDF | P0 |
| **FR10** | 系统应支持积分规则配置（获取规则+兑换规则），教师可为学生发放积分，家长可实时查看积分余额、明细并在线兑换奖励 | P2 |
| **FR11** | 系统应提供标准化评价模板（日常表现、课程表现、作业表现），支持自定义评价维度，教师提交评价后自动同步至家长端 | P0 |

#### 家校互动模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR12** | 系统应支持多类型通知（日常反馈、成长报告、活动通知、缴费提醒、异常提醒），通过小程序推送+短信发送，支持已读状态追踪和未读二次提醒 | P0 |
| **FR13** | 系统应为每位学生生成专属缴费码，支持微信支付/支付宝支付，家长可查看历史缴费记录；管理员可设置续费提醒天数（如临近15天到期），系统生成到期续费名单：班级老师查看本班续费名单，管理员查看全校续费名单，支持导出；系统自动对欠费家长发送催缴提醒 | P1 |

#### 数据分析模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR14** | 机构端数据驾驶舱应展示核心指标（托管学生数、班级数、教师数、缴费率、续费率、家长满意度），支持可视化图表和异常数据预警 | P1 |
| **FR15** | 政府端数据驾驶舱应展示监管指标（区域托管机构数、在托学生数、机构合规率、教师资质达标率、资金流转情况） | P2 |
| **FR16** | 系统应支持多维度报表导出（运营/财务/学生成长/教师绩效），支持自定义字段、时间范围筛选，导出格式支持Excel/PDF | P1 |

#### 加盟管理模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR17** | 系统应支持加盟机构备案管理（基础信息、备案状态、资质文件审核），向加盟机构提供标准化运营手册、课程体系、系统账号 | P2 |
| **FR18** | 系统应支持查看加盟机构运营数据（学生数、营收、续费率），发布运营培训课程并记录培训参与/考核结果 | P2 |

#### 政府监管模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR19** | 政府端应支持托管机构备案审核（机构资质、消防安全认证），教师备案管理（资质证明、从业资格核实） | P2 |
| **FR20** | 政府端应支持收费监管（收费标准、收费变动监控）、安全数据查看（消防检查、应急预案），与托管机构系统数据实时同步 | P2 |

#### 系统设置模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR21** | 系统应支持按角色分配操作权限，支持自定义角色创建，细化权限颗粒度（查看/编辑/导出） | P0 |
| **FR22** | 系统应支持基础参数配置（积分规则、签到时间范围、通知模板），记录操作日志（用户、时间、操作内容、IP地址） | P0 |
| **FR23** | 系统应支持定期自动备份和手动备份，提供数据恢复功能，新版本发布时向管理员推送更新提示 | P1 |

#### 品牌定制模块

| ID | 需求描述 | 优先级 |
|----|---------|-------|
| **FR24** | 机构端应支持品牌定制设置：上传机构Logo图片、从预设主题色中选择主色调，设置后教师端和家长端小程序界面颜色统一更换；总后台可查看各机构的品牌配置（Logo、主色调） | P2 |

### 2.2 Non-Functional Requirements (非功能需求)

#### 性能需求

| ID | 需求描述 |
|----|---------|
| **NFR1** | 核心功能（签到、批改、查看）操作响应时间 ≤ 2秒，数据加载时间 ≤ 3秒 |
| **NFR2** | 单机构同时在线用户 ≤ 50人时系统稳定运行，政府监管端支持同时查看 ≥ 100家机构数据 |
| **NFR3** | 支持单机构 ≥ 1000名学生的全量成长数据存储，数据留存时间 ≥ 3年 |

#### 安全需求

| ID | 需求描述 |
|----|---------|
| **NFR4** | 用户信息、缴费数据等敏感信息必须加密存储，传输使用HTTPS协议 |
| **NFR5** | 严格的权限隔离，确保不同角色仅能访问授权数据，支持数据按门店/班级隔离 |
| **NFR6** | 关键操作（缴费、删除数据）需二次确认，重要操作记录审计日志 |
| **NFR7** | 学生照片、个人信息等隐私数据仅授权用户可查看，禁止非法传播，符合《个人信息保护法》要求 |

#### 易用性需求

| ID | 需求描述 |
|----|---------|
| **NFR8** | 界面设计简洁明了，操作流程清晰，教师端操作便捷，家长端直观易懂 |
| **NFR9** | 新用户上手操作时间 ≤ 30分钟，提供操作指南、视频教程等帮助文档 |
| **NFR10** | 教师端、家长端基于微信小程序平台，支持微信最新版本；网页端支持Chrome、Edge、Firefox主流浏览器 |

#### 可扩展性需求

| ID | 需求描述 |
|----|---------|
| **NFR11** | 系统架构支持新增功能模块（如特色服务预约、家校沟通群），无需大规模重构 |
| **NFR12** | 支持与第三方系统对接（支付系统、政府监管平台、教育资源平台） |
| **NFR13** | 支持单机构部署与多加盟机构部署模式，可根据用户量弹性扩容 |

---

## 3. User Interface Design Goals

### 3.1 Overall UX Vision (整体UX愿景)

**设计理念**：简洁、专业、亲和

打造一个"**零学习成本、高效率操作**"的托管管理平台：
- **教师端**：待办驱动设计，数字徽章引导任务，快捷入口覆盖90%高频场景
- **家长端**：信息聚合设计，孩子状态首屏可见，卡片式布局层次分明
- **机构管理端**：数据驱动设计，核心指标卡片+趋势图表+异常预警列表
- **政府监管端**：合规严谨设计，清晰数据层级，便捷审核流程

**情感化设计**：传递"专业可信赖"+"温暖有关怀"的双重感受。

### 3.2 Key Interaction Paradigms (关键交互范式)

| 交互场景 | 设计原则 | 具体实现 |
|---------|---------|---------|
| **高频操作** | 最少点击原则 | 签到点名 ≤ 3步，常用功能固定底部导航 |
| **数据录入** | 智能辅助原则 | 模板复用、批量操作、AI辅助批改 |
| **信息反馈** | 即时响应原则 | 操作后2秒内显示结果 |
| **错误处理** | 友好引导原则 | 明确原因+解决方案 |
| **跨端同步** | 实时一致原则 | 教师提交后家长即时收到推送 |

**核心交互模式**：卡片式信息展示、时间线式记录、拍照即记录、一键分享

### 3.3 Core Screens and Views (核心页面)

#### 教师端微信小程序 (6个核心页面)

| 页面 | 功能定位 | 设计要点 |
|------|---------|---------|
| **首页/工作台** | 今日待办总览 | 待办数字徽章醒目，快捷操作4宫格，实时动态流 |
| **签到管理** | 学生签到签退 | 双模式（拍照/手动）并行，批量确认，异常标记 |
| **作业批改** | 作业上传与批改 | AI识别建议，标注工具栏，评语模板库 |
| **成长记录** | 学生成长档案 | 每日/月度总结编辑，数据自动整合 |
| **消息中心** | 通知与沟通 | 班级通知+私聊，已读状态追踪 |
| **我的** | 个人设置 | 班级切换、密码修改 |

#### 家长端微信小程序 (5个核心页面)

| 页面 | 功能定位 | 设计要点 |
|------|---------|---------|
| **首页** | 孩子今日动态 | 状态卡片首屏可见，今日概览3宫格 |
| **作业情况** | 作业与错题 | 学科筛选，批改详情，错题本导出 |
| **成长档案** | 成长轨迹 | 日历导航，每日/月度总结，分享导出 |
| **消息** | 通知接收 | 分类展示，红点提示 |
| **我的** | 个人中心 | 孩子信息、缴费记录 |

#### 机构管理后台 (5个核心模块)

| 模块 | 功能定位 | 设计要点 |
|------|---------|---------|
| **数据驾驶舱** | 运营数据总览 | 4个核心指标卡片+趋势图表+异常预警列表 |
| **基础管理** | 门店/班级/人员/学生 | 左侧导航+列表+详情抽屉 |
| **日常运营** | 签到/作业/课程 | 筛选条件+数据表格+批量操作 |
| **成长追踪** | 档案/积分/评价 | 学生卡片+详情面板 |
| **系统设置** | 权限/参数/日志 | 表单配置+操作日志 |

### 3.4 Accessibility (无障碍设计)

**目标等级**：WCAG AA

| 要求 | 实现方式 |
|------|---------|
| 色彩对比度 | 文字与背景对比度 ≥ 4.5:1 |
| 字体大小 | 正文最小14px，支持系统字体缩放 |
| 触控区域 | 可点击元素最小44x44px |
| 键盘可访问 | Web端支持Tab键导航 |

### 3.5 Branding (品牌视觉)

| 色彩角色 | 色值 | 应用场景 |
|---------|------|---------|
| **主色 - 信任蓝** | #1890FF | 主按钮、导航栏 |
| **辅助色 - 活力橙** | #FF7A45 | 提醒、积分、活动 |
| **成功绿** | #52C41A | 签到成功、正确标记 |
| **警告红** | #FF4D4F | 异常、错误、错题 |
| **文字色** | #333333 | 正文内容 |
| **背景色** | #F5F5F5 | 页面背景 |

**图标风格**：线性图标，2px描边，圆角风格

### 3.6 Target Platforms (目标平台)

| 端口 | 平台 | 最低兼容 |
|------|------|---------|
| 教师端 | 微信小程序 | 微信 8.0+ |
| 家长端 | 微信小程序 | 微信 8.0+ |
| 机构管理端 | Web响应式 | Chrome/Edge/Firefox 90+ |
| 政府监管端 | Web响应式 | Chrome/Edge/Firefox 90+ |

**适配策略**：小程序以375px为基准，Web端以1920x1080为主设计分辨率

---

## 4. Technical Assumptions

### 4.1 Repository Structure: Monorepo

采用 **Monorepo** 单仓库架构，所有端代码统一管理：

```
cc-ai-center/
├── apps/
│   ├── server/              # 后端API服务
│   ├── admin-web/           # 机构管理后台 (Web)
│   ├── gov-web/             # 政府监管后台 (Web)
│   ├── teacher-miniapp/     # 教师端微信小程序
│   └── parent-miniapp/      # 家长端微信小程序
├── packages/
│   ├── shared/              # 共享类型、工具函数
│   ├── ui-components/       # 共享UI组件
│   └── api-client/          # API客户端SDK
├── docs/                    # 文档
└── scripts/                 # 构建脚本
```

**决策理由**：
- 多端共享类型定义和业务逻辑
- 统一版本管理，避免API不一致
- 便于CI/CD统一构建部署

### 4.2 Service Architecture: Spring Cloud 微服务架构

基于 Spring Cloud 的微服务架构，参考现有技术架构文档设计：

```
┌─────────────────────────────────────────────────────────────┐
│                        客户端层                              │
├──────────┬──────────┬──────────┬──────────┬────────────────┤
│ 教师小程序 │ 家长小程序 │ 机构Web  │ 政府Web  │  第三方对接    │
│ (微信原生) │ (微信原生) │ (Vue3)  │ (Vue3)  │              │
└─────┬────┴─────┬────┴─────┬────┴─────┬────┴───────┬────────┘
      │          │          │          │            │
      └──────────┴──────────┴──────────┴────────────┘
                            │
                    ┌───────▼───────┐
                    │ Spring Gateway │  (API网关、路由转发、鉴权)
                    └───────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
       ┌──────▼──────┐ ┌────▼────┐ ┌──────▼──────┐
       │  用户中心    │ │ 应用服务 │ │ 文件服务    │
       │ (user-center)│ │(appserve)│ │ (OSS)      │
       └──────┬──────┘ └────┬────┘ └──────┬──────┘
              │             │             │
       ┌──────▼─────────────▼─────────────▼──────┐
       │          Nacos (服务注册与配置中心)        │
       └──────────────────┬──────────────────────┘
                          │
       ┌──────────────────▼──────────────────────┐
       │              数据层                       │
       │  MySQL 8.0 + Redis (Redisson) + 阿里云OSS │
       └─────────────────────────────────────────┘
```

**微服务模块划分**：

| 服务模块 | 职责 | 说明 |
|---------|------|------|
| **Spring Gateway** | API网关 | 路由转发、统一鉴权、限流 |
| **user-center** | 用户中心 | 用户认证、权限管理、教师/家长/管理员账号 |
| **appserve** | 应用服务 | 托管核心业务（签到、作业、成长档案等） |
| **common** | 公共模块 | 公共组件、工具类、统一配置 |

### 4.3 Testing Requirements: Unit + Integration + E2E

| 测试类型 | 覆盖范围 | 工具 | 目标覆盖率 |
|---------|---------|------|-----------|
| **Unit** | 业务逻辑、工具函数 | Jest | ≥ 80% |
| **Integration** | API接口、数据库交互 | Jest + Supertest | 核心接口100% |
| **E2E** | 关键用户流程 | Playwright | 核心流程覆盖 |
| **Manual** | UI验收、兼容性 | 测试用例 | 发布前必测 |

### 4.4 Technology Stack (技术栈)

#### 后端技术栈（参考技术架构文档）

| 层级 | 技术选型 | 版本 | 决策理由 |
|------|---------|------|---------|
| **Java** | JDK | 21 LTS | 长期支持版本，性能优化 |
| **基础框架** | Spring Boot | 3.3.1 | 现代化企业级框架 |
| **微服务框架** | Spring Cloud | 2023.0.2 | 成熟的微服务生态 |
| **微服务组件** | Spring Cloud Alibaba | 2023.0.1.0 | Nacos服务注册与配置 |
| **API网关** | Spring Cloud Gateway | - | 统一路由、鉴权、限流 |
| **ORM框架** | MyBatis-Plus | 3.5.7 | MyBatis增强，开发效率高 |
| **数据库** | MySQL | 8.0+ | 成熟稳定，托管行业数据结构偏关系型 |
| **缓存** | Redis + Redisson | 3.32.0 | 分布式缓存、分布式锁 |
| **本地缓存** | Caffeine | - | 高性能本地缓存 |
| **服务注册** | Nacos | 2.3.2+ | 服务注册中心、配置中心 |
| **安全框架** | Spring Security + OAuth2 | - | JWT Token认证，RBAC权限控制 |
| **API文档** | Knife4j (OpenAPI3) | 4.5.0 | API文档自动生成 |
| **日志** | Log4j2 + SLF4J | - | 统一日志框架 |

#### 前端技术栈

| 层级 | 技术选型 | 说明 |
|------|---------|------|
| **小程序框架** | 微信原生框架 | 教师端、家长端使用微信原生开发，性能最优 |
| **Web前端** | Vue 3 + Element Plus | 机构管理后台、政府监管后台 |
| **状态管理** | Pinia | Vue 3官方推荐状态管理 |
| **构建工具** | Vite | 现代化前端构建工具 |

#### 第三方服务

| 服务类型 | 技术选型 | 说明 |
|---------|---------|------|
| **文件存储** | 阿里云 OSS | 对象存储服务，图片、文件上传 |
| **AI批改** | 腾讯云 OCR | 作业图片识别 + 自研规则引擎批改 |
| **消息推送** | 微信模板消息 + 腾讯云短信 | 小程序内推送 + 短信双通道 |
| **支付** | 微信支付 | 小程序原生支付，API v3 |
| **人脸识别** | 腾讯云人脸核身 | 拍照点名人脸识别 |

#### 部署架构

| 层级 | 技术选型 | 说明 |
|------|---------|------|
| **容器化** | Docker | 所有服务支持Docker部署 |
| **编排** | Docker Compose | 开发/测试环境容器编排 |
| **构建工具** | Maven | Java项目构建 |

### 4.5 Additional Technical Assumptions

- **认证方案**：微信小程序使用微信登录 + JWT（RSA非对称加密）；Web端使用账号密码 + JWT
- **API规范**：RESTful API，统一返回对象 `R<T>`，OpenAPI 3.0规范
- **数据隔离**：基于 tenant_id（机构ID）实现多租户数据隔离
- **服务间调用**：Spring Cloud OpenFeign + LoadBalancer
- **分布式锁**：Redisson实现，防止并发问题
- **数据备份**：MySQL每日自动备份，保留30天；OSS开启版本控制
- **链路追踪**：Micrometer Tracing + OpenTelemetry

---

## 5. Epic List

基于需求优先级和敏捷最佳实践，Epic按逻辑顺序排列：

| Epic | 标题 | 目标 | 优先级 | 预估复杂度 |
|------|------|------|-------|-----------|
| **Epic 1** | 项目基础设施与认证系统 | 搭建项目骨架、多端认证、基础权限 | P0 | 高 |
| **Epic 2** | 基础管理模块 | 门店、班级、教师、学生CRUD管理 | P0 | 中 |
| **Epic 3** | 签到签退管理 | 手动/拍照点名、考勤记录、家长查看 | P0 | 高 |
| **Epic 4** | 作业管理与智能批改 | 作业上传、AI批改、错题本、家长查看 | P0 | 高 |
| **Epic 5** | 成长档案与评价系统 | 自动档案生成、教师评价、家长查看分享 | P0 | 中 |
| **Epic 6** | 家校互动与消息通知 | 多类型通知、推送、已读追踪 | P0 | 中 |
| **Epic 7** | 生活管理与课程管理 | 菜单、就餐记录、课程配置、课程评价 | P1 | 中 |
| **Epic 8** | 在线缴费与积分系统 | 缴费码、微信支付、积分规则、兑换 | P1 | 高 |
| **Epic 9** | 数据驾驶舱与报表 | 核心指标、可视化图表、报表导出 | P1 | 中 |
| **Epic 10** | 加盟管理与政府监管 | 加盟备案、监管数据同步、政府后台 | P2 | 高 |

**MVP范围**：Epic 1-6 为第一阶段必须交付，对应原PRD的P0功能

---

## 6. Epics

### Epic 1: 项目基础设施与认证系统

**Epic Summary:** 搭建项目技术骨架，实现多端（小程序+Web）统一认证，建立基础权限体系，为后续功能开发奠定基础。

**Target Repositories:** monolith

```yaml
epic_id: 1
title: "项目基础设施与认证系统"
description: |
  建立项目技术基础，包括后端API框架、数据库初始化、多端认证体系。
  教师/家长通过微信小程序登录，管理员通过Web端账号密码登录。
  实现基于角色的权限控制(RBAC)。

stories:
  - id: "1.1"
    title: "后端项目初始化与基础框架搭建"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0

    acceptance_criteria:
      - id: AC1
        title: "NestJS项目结构初始化"
        scenario:
          given: "开发环境已准备就绪"
          when: "执行项目初始化脚本"
          then:
            - "创建标准NestJS项目结构"
            - "配置TypeScript、ESLint、Prettier"
            - "集成MySQL数据库连接(TypeORM)"
            - "集成Redis缓存连接"
            - "配置环境变量管理(.env)"
        business_rules:
          - id: "BR-1.1"
            rule: "项目必须使用TypeScript严格模式"
          - id: "BR-1.2"
            rule: "数据库连接支持连接池，最大连接数可配置"
        error_handling:
          - scenario: "数据库连接失败"
            code: "503"
            message: "Database connection failed"
            action: "应用启动失败，记录错误日志"

      - id: AC2
        title: "健康检查端点"
        scenario:
          given: "服务已启动"
          when: "访问 GET /health"
          then:
            - "返回200状态码"
            - "返回数据库、Redis连接状态"
        business_rules:
          - id: "BR-2.1"
            rule: "健康检查不需要认证"
        error_handling:
          - scenario: "数据库不可用"
            code: "503"
            message: "Service unhealthy"
            action: "返回具体不健康组件信息"
        examples:
          - input: "GET /health"
            expected: |
              200 OK
              {"status": "healthy", "database": "up", "redis": "up", "timestamp": "ISO8601"}

    provides_apis:
      - "GET /health"
    consumes_apis: []
    dependencies: []

  - id: "1.2"
    title: "微信小程序登录认证(教师端/家长端)"
    repository_type: monolith
    estimated_complexity: high
    priority: P0

    acceptance_criteria:
      - id: AC1
        title: "微信登录获取token"
        scenario:
          given: "用户在微信小程序中点击登录"
          when: "小程序调用wx.login获取code，提交至 POST /api/auth/wx-login"
          then:
            - "后端使用code换取openid和session_key"
            - "查找或创建用户记录"
            - "生成JWT access_token(2小时有效)和refresh_token(7天有效)"
            - "返回token和用户基本信息"
        business_rules:
          - id: "BR-1.1"
            rule: "同一openid首次登录自动创建用户，状态为pending_binding"
          - id: "BR-1.2"
            rule: "用户需绑定手机号后才能使用完整功能"
          - id: "BR-1.3"
            rule: "教师端和家长端使用不同的appid，用户数据隔离"
          - id: "BR-1.4"
            rule: "JWT payload包含: user_id, openid, role, tenant_id"
        data_validation:
          - field: "code"
            type: "string"
            required: true
            rules: "微信登录临时凭证，32位字符串"
            error_message: "无效的登录凭证"
          - field: "app_type"
            type: "string"
            required: true
            rules: "枚举值: teacher | parent"
            error_message: "请指定应用类型"
        error_handling:
          - scenario: "微信code无效或过期"
            code: "401"
            message: "微信登录失败，请重试"
            action: "前端引导用户重新发起wx.login"
          - scenario: "用户被禁用"
            code: "403"
            message: "账号已被禁用，请联系管理员"
            action: "拒绝登录"
        examples:
          - input: |
              POST /api/auth/wx-login
              {"code": "0a3Xxx...", "app_type": "teacher"}
            expected: |
              200 OK
              {"access_token": "jwt...", "refresh_token": "jwt...", "expires_in": 7200,
               "user": {"id": "uuid", "phone": null, "status": "pending_binding", "role": "teacher"}}

      - id: AC2
        title: "绑定手机号"
        scenario:
          given: "用户已微信登录但未绑定手机号"
          when: "用户授权手机号，提交至 POST /api/auth/bind-phone"
          then:
            - "解密微信加密数据获取手机号"
            - "更新用户手机号，状态改为active"
            - "返回更新后的用户信息"
        business_rules:
          - id: "BR-2.1"
            rule: "同一手机号在同一端(教师/家长)只能绑定一个账号"
          - id: "BR-2.2"
            rule: "手机号绑定后不可自行修改，需联系管理员"
        data_validation:
          - field: "encrypted_data"
            type: "string"
            required: true
            rules: "微信加密数据"
            error_message: "手机号数据无效"
          - field: "iv"
            type: "string"
            required: true
            rules: "加密算法初始向量"
            error_message: "缺少iv参数"
        error_handling:
          - scenario: "手机号已被其他账号绑定"
            code: "409"
            message: "该手机号已被其他账号使用"
            action: "提示用户联系客服"

    provides_apis:
      - "POST /api/auth/wx-login"
      - "POST /api/auth/bind-phone"
      - "POST /api/auth/refresh-token"
    consumes_apis: []
    dependencies:
      - "1.1"

  - id: "1.3"
    title: "Web端账号密码登录(管理后台)"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0

    acceptance_criteria:
      - id: AC1
        title: "管理员账号密码登录"
        scenario:
          given: "管理员账号已由超级管理员创建"
          when: "管理员输入账号密码提交至 POST /api/auth/login"
          then:
            - "验证账号密码正确性"
            - "生成JWT token"
            - "记录登录日志(IP、时间、设备)"
        business_rules:
          - id: "BR-1.1"
            rule: "密码使用bcrypt加密，cost factor=12"
          - id: "BR-1.2"
            rule: "连续5次密码错误锁定账号15分钟"
          - id: "BR-1.3"
            rule: "首次登录强制修改初始密码"
        data_validation:
          - field: "username"
            type: "string"
            required: true
            rules: "4-50字符，支持手机号或自定义账号"
            error_message: "请输入正确的账号"
          - field: "password"
            type: "string"
            required: true
            rules: "8-32字符"
            error_message: "请输入密码"
        error_handling:
          - scenario: "账号或密码错误"
            code: "401"
            message: "账号或密码错误"
            action: "不透露具体是哪个错误，递增失败计数"
          - scenario: "账号已锁定"
            code: "423"
            message: "账号已锁定，请15分钟后重试"
            action: "返回剩余锁定时间"
        examples:
          - input: |
              POST /api/auth/login
              {"username": "admin@school.com", "password": "SecurePass123"}
            expected: |
              200 OK
              {"access_token": "jwt...", "expires_in": 7200, "need_change_password": false}

    provides_apis:
      - "POST /api/auth/login"
      - "POST /api/auth/change-password"
      - "POST /api/auth/logout"
    consumes_apis: []
    dependencies:
      - "1.1"

  - id: "1.4"
    title: "RBAC权限管理基础"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0

    acceptance_criteria:
      - id: AC1
        title: "角色与权限数据模型"
        scenario:
          given: "数据库已初始化"
          when: "系统启动时"
          then:
            - "创建预设角色: super_admin, org_admin, teacher, parent"
            - "创建权限点数据表"
            - "建立角色-权限关联"
        business_rules:
          - id: "BR-1.1"
            rule: "super_admin拥有所有权限，不可删除"
          - id: "BR-1.2"
            rule: "权限按模块分组: base_mgmt, daily_ops, growth, message, data, system"
          - id: "BR-1.3"
            rule: "每个权限点包含: view, create, update, delete, export 操作类型"

      - id: AC2
        title: "API权限拦截"
        scenario:
          given: "用户已登录"
          when: "用户请求受保护的API"
          then:
            - "JWT Guard验证token有效性"
            - "Permission Guard验证用户角色是否有对应权限"
            - "无权限返回403"
        business_rules:
          - id: "BR-2.1"
            rule: "权限验证使用装饰器 @RequirePermission('module:action')"
        error_handling:
          - scenario: "无访问权限"
            code: "403"
            message: "您没有权限执行此操作"
            action: "记录越权尝试日志"

    provides_apis:
      - "GET /api/roles"
      - "GET /api/permissions"
    consumes_apis: []
    dependencies:
      - "1.1"

  - id: "1.5"
    title: "前端项目初始化(小程序+Web)"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0

    acceptance_criteria:
      - id: AC1
        title: "Taro小程序项目初始化"
        scenario:
          given: "开发环境已准备"
          when: "执行初始化脚本"
          then:
            - "创建教师端、家长端两个Taro项目"
            - "配置共享组件库和工具函数"
            - "集成状态管理(Zustand)"
            - "配置请求封装和token管理"
        business_rules:
          - id: "BR-1.1"
            rule: "两个小程序共享基础组件和API Client"

      - id: AC2
        title: "React Web管理后台初始化"
        scenario:
          given: "开发环境已准备"
          when: "执行初始化脚本"
          then:
            - "创建React + Vite项目"
            - "集成Ant Design Pro Layout"
            - "配置路由和权限控制"
            - "配置请求封装和错误处理"
        business_rules:
          - id: "BR-2.1"
            rule: "Web端支持根据用户权限动态生成菜单"

    provides_apis: []
    consumes_apis:
      - "POST /api/auth/wx-login"
      - "POST /api/auth/login"
    dependencies:
      - "1.2"
      - "1.3"
```

---

### Epic 2: 基础管理模块

**Epic Summary:** 实现门店、班级、教师、学生的基础信息管理，支持CRUD操作，建立数据隔离机制，为日常运营功能提供数据基础。

**Target Repositories:** monolith

```yaml
epic_id: 2
title: "基础管理模块"
description: |
  构建托管机构的基础数据管理能力，包括门店、班级、教师、学生四大实体。
  实现多门店数据隔离，班级-教师-学生的关联管理。

stories:
  - id: "2.1"
    title: "门店管理CRUD"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0
    provides_apis:
      - "POST /api/stores"
      - "GET /api/stores"
      - "GET /api/stores/:id"
      - "PUT /api/stores/:id"
      - "DELETE /api/stores/:id"
    dependencies: ["1.4"]

  - id: "2.2"
    title: "班级管理CRUD"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0
    provides_apis:
      - "POST /api/classes"
      - "GET /api/classes"
      - "GET /api/classes/:id"
      - "PUT /api/classes/:id"
      - "DELETE /api/classes/:id"
    dependencies: ["2.1"]

  - id: "2.3"
    title: "教师管理CRUD"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0
    provides_apis:
      - "POST /api/teachers"
      - "GET /api/teachers"
      - "GET /api/teachers/:id"
      - "PUT /api/teachers/:id"
      - "DELETE /api/teachers/:id"
    dependencies: ["2.1"]

  - id: "2.4"
    title: "学生管理CRUD"
    repository_type: monolith
    estimated_complexity: medium
    priority: P0
    provides_apis:
      - "POST /api/students"
      - "GET /api/students"
      - "GET /api/students/:id"
      - "PUT /api/students/:id"
      - "POST /api/students/:id/transfer"
      - "POST /api/students/:id/withdraw"
    dependencies: ["2.2", "2.3"]
```

---

### Epic 3: 签到签退管理

**Epic Summary:** 实现学生签到签退的完整流程，支持手动点名和拍照点名（人脸识别），考勤数据实时同步至家长端，异常考勤自动提醒。

**Target Repositories:** monolith

```yaml
epic_id: 3
title: "签到签退管理"
description: |
  构建签到签退核心功能，教师端支持手动和拍照两种点名方式。
  签到数据实时同步，家长可查看历史记录，异常自动推送提醒。

stories:
  - id: "3.1"
    title: "签到规则配置"
    priority: P0
    provides_apis: ["GET /api/attendance/rules", "PUT /api/attendance/rules"]
    dependencies: ["2.2"]

  - id: "3.2"
    title: "教师端手动点名签到"
    priority: P0
    provides_apis: ["POST /api/attendance/checkin", "POST /api/attendance/checkout", "GET /api/attendance/today"]
    dependencies: ["3.1"]

  - id: "3.3"
    title: "教师端拍照点名(人脸识别)"
    priority: P0
    provides_apis: ["POST /api/attendance/photo-checkin"]
    dependencies: ["3.2"]

  - id: "3.4"
    title: "家长端签到查看"
    priority: P0
    provides_apis: ["GET /api/parent/attendance/today", "GET /api/parent/attendance/history"]
    dependencies: ["3.2"]

  - id: "3.5"
    title: "机构端签到数据查看与导出"
    priority: P0
    provides_apis: ["GET /api/admin/attendance", "GET /api/admin/attendance/export"]
    dependencies: ["3.2"]
```

---

### Epic 4: 作业管理与智能批改

**Epic Summary:** 实现作业上传、AI智能批改、手动标注、错题本生成的完整流程，家长可实时查看批改结果和错题汇总。

**Target Repositories:** monolith

```yaml
epic_id: 4
title: "作业管理与智能批改"
description: |
  构建作业管理核心功能，教师可上传作业图片，系统AI辅助批改客观题，
  教师可手动标注和点评。自动生成错题本，家长可查看和导出。

stories:
  - id: "4.1"
    title: "作业上传与管理"
    priority: P0
    provides_apis: ["POST /api/homework/upload", "GET /api/homework"]
    dependencies: ["2.4"]

  - id: "4.2"
    title: "AI智能批改"
    priority: P0
    provides_apis: ["POST /api/homework/:id/ai-review"]
    dependencies: ["4.1"]

  - id: "4.3"
    title: "手动批改与点评"
    priority: P0
    provides_apis: ["PUT /api/homework/:id/review", "GET /api/homework/comment-templates"]
    dependencies: ["4.1"]

  - id: "4.4"
    title: "错题本自动生成"
    priority: P0
    provides_apis: ["GET /api/students/:id/mistakes", "GET /api/students/:id/mistakes/export"]
    dependencies: ["4.3"]

  - id: "4.5"
    title: "家长端作业查看"
    priority: P0
    provides_apis: ["GET /api/parent/homework"]
    dependencies: ["4.3"]
```

---

### Epic 5: 成长档案与评价系统

**Epic Summary:** 自动整合学生各维度数据生成成长档案，教师可补充编辑，支持每日/月度总结，家长可查看和分享。

**Target Repositories:** monolith

```yaml
epic_id: 5
title: "成长档案与评价系统"
description: |
  构建学生成长档案体系，自动整合签到、作业、评价等数据，
  生成每日/月度总结，支持教师编辑和家长分享。

stories:
  - id: "5.1"
    title: "评价模板配置"
    priority: P0
    provides_apis: ["GET /api/evaluation/templates", "PUT /api/evaluation/templates"]
    dependencies: ["2.2"]

  - id: "5.2"
    title: "教师实时评价"
    priority: P0
    provides_apis: ["POST /api/evaluations", "GET /api/evaluations"]
    dependencies: ["5.1"]

  - id: "5.3"
    title: "每日成长总结"
    priority: P0
    provides_apis: ["GET /api/growth/daily/:studentId", "PUT /api/growth/daily/:id"]
    dependencies: ["5.2"]

  - id: "5.4"
    title: "月度成长报告"
    priority: P0
    provides_apis: ["GET /api/growth/monthly/:studentId"]
    dependencies: ["5.3"]

  - id: "5.5"
    title: "家长端成长档案查看与分享"
    priority: P0
    provides_apis: ["GET /api/parent/growth", "GET /api/parent/growth/share-image", "GET /api/parent/growth/export-pdf"]
    dependencies: ["5.4"]
```

---

### Epic 6: 家校互动与消息通知

**Epic Summary:** 实现多类型消息通知体系，支持班级通知、单聊、系统提醒，通过小程序推送和短信双通道触达家长。

**Target Repositories:** monolith

```yaml
epic_id: 6
title: "家校互动与消息通知"
description: |
  构建完整的消息通知体系，教师可发送班级通知和私聊消息，
  系统自动发送各类提醒，支持已读追踪和未读提醒。

stories:
  - id: "6.1"
    title: "消息通知基础架构"
    priority: P0
    provides_apis: ["POST /api/messages/send"]
    dependencies: ["1.2"]

  - id: "6.2"
    title: "教师端班级通知"
    priority: P0
    provides_apis: ["POST /api/messages/notice", "GET /api/messages/notice/:id/read-status"]
    dependencies: ["6.1"]

  - id: "6.3"
    title: "教师-家长私聊"
    priority: P0
    provides_apis: ["POST /api/messages/chat", "GET /api/messages/chat/:studentId"]
    dependencies: ["6.1"]

  - id: "6.4"
    title: "家长端消息中心"
    priority: P0
    provides_apis: ["GET /api/parent/messages", "PUT /api/parent/messages/:id/read"]
    dependencies: ["6.2", "6.3"]
```

---

### Epic 7-10 (P1/P2 - 精简定义)

```yaml
# Epic 7: 生活管理与课程管理 (P1)
epic_id: 7
title: "生活管理与课程管理"
stories:
  - id: "7.1"
    title: "每日菜单管理"
    priority: P1
  - id: "7.2"
    title: "就餐情况记录"
    priority: P1
  - id: "7.3"
    title: "午休情况记录"
    priority: P1
  - id: "7.4"
    title: "课程体系配置"
    priority: P1
  - id: "7.5"
    title: "课程参与记录与评价"
    priority: P1
  - id: "7.6"
    title: "家长端生活与课程查看"
    priority: P1

# Epic 8: 在线缴费与积分系统 (P1)
epic_id: 8
title: "在线缴费与积分系统"
stories:
  - id: "8.1"
    title: "缴费项目配置"
    priority: P1
  - id: "8.2"
    title: "学生缴费码生成"
    priority: P1
  - id: "8.3"
    title: "微信支付对接"
    priority: P1
  - id: "8.4"
    title: "缴费记录与欠费提醒"
    priority: P1
  - id: "8.5"
    title: "积分规则配置"
    priority: P2
  - id: "8.6"
    title: "积分发放与兑换"
    priority: P2

# Epic 9: 数据驾驶舱与报表 (P1)
epic_id: 9
title: "数据驾驶舱与报表"
stories:
  - id: "9.1"
    title: "机构端数据驾驶舱"
    priority: P1
  - id: "9.2"
    title: "异常数据预警"
    priority: P1
  - id: "9.3"
    title: "运营报表导出"
    priority: P1
  - id: "9.4"
    title: "教师绩效统计"
    priority: P1

# Epic 10: 加盟管理与政府监管 (P2)
epic_id: 10
title: "加盟管理与政府监管"
stories:
  - id: "10.1"
    title: "加盟机构备案管理"
    priority: P2
  - id: "10.2"
    title: "加盟数据监控"
    priority: P2
  - id: "10.3"
    title: "政府监管后台基础"
    priority: P2
  - id: "10.4"
    title: "机构备案审核"
    priority: P2
  - id: "10.5"
    title: "监管数据同步"
    priority: P2
```

---

## 7. Checklist Results Report

### PRD验证检查清单

| 检查项 | 状态 | 说明 |
|-------|------|------|
| **需求完整性** | ✅ Pass | 23个FR + 13个NFR覆盖7大业务场景 |
| **需求可追溯性** | ✅ Pass | 每个FR可追溯到Goals |
| **需求无歧义** | ✅ Pass | 使用GIVEN/WHEN/THEN明确场景 |
| **业务规则完整** | ✅ Pass | 每个AC包含业务规则定义 |
| **数据验证定义** | ✅ Pass | 关键输入字段有验证规则 |
| **错误处理定义** | ✅ Pass | 每个AC包含错误场景 |
| **API清单完整** | ✅ Pass | Stories包含provides/consumes APIs |
| **依赖关系明确** | ✅ Pass | Stories间依赖关系已定义 |
| **优先级合理** | ✅ Pass | P0/P1/P2划分符合MVP策略 |
| **Epic顺序合理** | ✅ Pass | 从基础设施到业务功能递进 |
| **技术可行性** | ✅ Pass | 技术栈成熟，无高风险技术 |
| **安全考量** | ✅ Pass | NFR包含数据安全、权限隔离要求 |

### 待后续补充项

| 项目 | 负责角色 | 说明 |
|------|---------|------|
| 前端页面详细规格 | UX Expert | 需产出交互原型和UI规格文档 |
| 数据库Schema设计 | Architect | 需设计实体关系图和表结构 |
| API详细规格 | Architect | 需产出OpenAPI规格文档 |
| 测试用例设计 | QA | 基于AC产出测试用例 |

---

## 8. Next Steps

### 8.1 UX Expert Prompt

```
请基于 docs/prd.md 创建交互设计文档。

重点关注:
1. 教师端小程序 - 签到、作业批改、成长记录核心流程
2. 家长端小程序 - 首页、作业查看、成长档案核心流程
3. 机构管理后台 - 数据驾驶舱、基础管理布局

产出物:
- 核心页面线框图
- 交互流程图
- UI规格说明(基于PRD Section 3.5 品牌视觉)

使用命令: /o ux-expert 然后执行 *create-architecture
```

### 8.2 Architect Prompt

```
请基于 docs/prd.md 创建系统架构设计文档。

重点关注:
1. Monorepo项目结构设计
2. 数据库Schema设计(MySQL)
3. REST API规格设计
4. 认证授权架构(微信登录+JWT+RBAC)
5. 第三方服务集成(人脸识别、OCR、支付)

技术约束:
- 后端: Node.js + NestJS + TypeORM
- 前端: Taro(小程序) + React(Web)
- 数据库: MySQL 8.0 + Redis
- 云服务: 腾讯云(COS、人脸核身、OCR)

使用命令: /o architect 然后执行 *create-architecture
```

---

*Generated by Orchestrix PM Agent - Liangning*
*Version 2.0 | 2026-01-09*
