# 托管行业数字化管理系统 Frontend Architecture Document

**版本**: 1.0
**日期**: 2026-01-14
**作者**: UX Expert Agent (Jingwen)
**项目**: cc-ai-center

---

## 目录

1. [模板与框架选择](#1-模板与框架选择)
2. [前端技术栈](#2-前端技术栈)
3. [项目结构](#3-项目结构)
4. [组件规范](#4-组件规范)
5. [状态管理](#5-状态管理)
6. [API 集成](#6-api-集成)
7. [路由配置](#7-路由配置)
8. [样式指南](#8-样式指南)
9. [测试策略](#9-测试策略)
10. [环境配置](#10-环境配置)
11. [编码规范](#11-编码规范)
12. [快速参考](#12-快速参考)

---

## 1. 模板与框架选择

### 1.1 框架决策

| 端口 | 框架 | 版本 | 说明 |
|------|------|------|------|
| **教师端小程序** | 微信原生框架 | 基础库 3.3+ | WeChat Mini Program Native |
| **家长端小程序** | 微信原生框架 | 基础库 3.3+ | WeChat Mini Program Native |
| **机构管理后台** | Vue 3 + Element Plus | Vue 3.4+ | Web 响应式 |
| **政府监管后台** | Vue 3 + Element Plus | Vue 3.4+ | Web 响应式 |

### 1.2 决策理由

**微信原生框架** vs Taro/UniApp：
- ✅ 性能最优：原生框架无跨端编译开销
- ✅ API兼容性：直接使用微信最新特性（人脸识别、模板消息等）
- ✅ 维护简单：无第三方框架依赖升级风险
- ⚠️ Trade-off：无法复用代码到其他平台，但项目需求仅限微信生态

**Vue 3** vs React：
- ✅ 团队技术栈统一：后端 Spring Cloud 团队更熟悉 Vue 生态
- ✅ Element Plus 成熟：企业管理后台组件丰富
- ✅ Composition API：逻辑复用更灵活

---

## 2. 前端技术栈

### 2.1 Web 前端技术栈 (机构管理后台 / 政府监管后台)

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| **Framework** | Vue | 3.4+ | 核心前端框架 | Composition API、性能优化、团队熟悉 |
| **UI Library** | Element Plus | 2.6+ | UI组件库 | 企业级组件丰富、Vue 3原生支持 |
| **State Management** | Pinia | 2.1+ | 全局状态管理 | Vue 3官方推荐、TypeScript支持好 |
| **Routing** | Vue Router | 4.3+ | 路由管理 | Vue 3配套、支持路由守卫 |
| **Build Tool** | Vite | 5.0+ | 构建打包 | 冷启动快、HMR即时、ESM原生支持 |
| **HTTP Client** | Axios | 1.6+ | API请求 | 拦截器、取消请求、成熟稳定 |
| **Styling** | SCSS + CSS Variables | - | 样式预处理 | 变量复用、主题切换支持 |
| **Charts** | ECharts | 5.5+ | 数据可视化 | 数据驾驶舱图表、国产化支持好 |
| **Form Handling** | Element Plus Form | - | 表单验证 | 内置验证、与UI库统一 |
| **Date Handling** | Day.js | 1.11+ | 日期处理 | 轻量、API兼容Moment.js |
| **Utils** | VueUse | 10+ | 组合式工具 | 常用Hooks封装、减少样板代码 |
| **TypeScript** | TypeScript | 5.3+ | 类型系统 | 类型安全、IDE智能提示 |
| **Dev Tools** | Vue DevTools | - | 调试工具 | 状态调试、性能分析 |

### 2.2 微信小程序技术栈 (教师端 / 家长端)

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|-----------|
| **Framework** | 微信原生框架 | 基础库 3.3+ | 小程序核心 | 性能最优、API完整支持 |
| **UI Library** | WeUI | 2.5+ | 微信风格UI | 官方组件、体验一致 |
| **State Management** | MobX-miniprogram | 4.13+ | 状态管理 | 响应式、跨页面共享 |
| **HTTP Client** | wx.request封装 | - | API请求 | 拦截器、Token管理 |
| **Styling** | WXSS + CSS Variables | - | 样式 | 原生支持、rpx适配 |
| **Storage** | wx.setStorageSync | - | 本地存储 | Token、用户信息缓存 |
| **TypeScript** | TypeScript | 5.0+ | 类型系统 | 小程序原生支持TS |
| **Dev Tools** | 微信开发者工具 | - | 开发调试 | 真机预览、性能分析 |

### 2.3 共享依赖

| Category | Technology | Purpose |
|----------|------------|---------|
| **类型定义** | @types/wechat-miniprogram | 小程序类型定义 |
| **API Schema** | 共享 TypeScript 接口 | 前后端类型同步 |
| **工具函数** | packages/shared | 日期格式化、验证规则等 |
| **常量配置** | packages/shared/constants | 错误码、枚举值、配置项 |

---

## 3. 项目结构

### 3.1 整体 Monorepo 结构

```plaintext
cc-ai-center/
├── apps/
│   ├── admin-web/                    # 机构管理后台 (Vue 3)
│   ├── gov-web/                      # 政府监管后台 (Vue 3)
│   ├── teacher-miniapp/              # 教师端微信小程序
│   └── parent-miniapp/               # 家长端微信小程序
│
├── packages/
│   ├── shared/                       # 共享类型、工具函数、常量
│   │   ├── src/
│   │   │   ├── types/               # TypeScript 类型定义
│   │   │   ├── constants/           # 常量、枚举
│   │   │   ├── utils/               # 工具函数
│   │   │   └── validators/          # 验证规则
│   │   └── package.json
│   │
│   ├── api-client/                   # API 客户端 SDK
│   │   ├── src/
│   │   │   ├── modules/             # 按模块划分的 API
│   │   │   ├── interceptors/        # 请求/响应拦截器
│   │   │   └── types/               # API 类型定义
│   │   └── package.json
│   │
│   └── ui-components/                # 共享 UI 组件 (Web端复用)
│       ├── src/
│       │   ├── components/          # 通用组件
│       │   └── styles/              # 共享样式变量
│       └── package.json
│
├── docs/                             # 项目文档
├── scripts/                          # 构建、部署脚本
├── pnpm-workspace.yaml               # pnpm 工作区配置
├── package.json                      # 根 package.json
└── tsconfig.base.json                # 基础 TypeScript 配置
```

### 3.2 Vue 3 Web 项目结构 (admin-web / gov-web)

```plaintext
apps/admin-web/
├── public/
│   └── favicon.ico
├── src/
│   ├── api/                          # API 模块
│   │   ├── index.ts
│   │   └── modules/
│   │       ├── auth.ts
│   │       ├── store.ts
│   │       ├── class.ts
│   │       ├── student.ts
│   │       ├── attendance.ts
│   │       ├── homework.ts
│   │       └── growth.ts
│   │
│   ├── assets/                       # 静态资源
│   │   ├── images/
│   │   ├── icons/
│   │   └── styles/
│   │       ├── variables.scss
│   │       ├── mixins.scss
│   │       └── global.scss
│   │
│   ├── components/                   # 通用组件
│   │   ├── common/
│   │   ├── business/
│   │   └── charts/
│   │
│   ├── composables/                  # 组合式函数
│   │   ├── useAuth.ts
│   │   ├── usePermission.ts
│   │   ├── useTable.ts
│   │   ├── useForm.ts
│   │   └── useMessage.ts
│   │
│   ├── layouts/                      # 布局组件
│   │   ├── DefaultLayout.vue
│   │   ├── BlankLayout.vue
│   │   └── components/
│   │
│   ├── router/                       # 路由配置
│   │   ├── index.ts
│   │   ├── routes/
│   │   └── guards.ts
│   │
│   ├── stores/                       # Pinia 状态管理
│   │   ├── index.ts
│   │   ├── modules/
│   │   └── types.ts
│   │
│   ├── views/                        # 页面视图
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── base/
│   │   ├── daily/
│   │   ├── growth/
│   │   └── system/
│   │
│   ├── utils/                        # 工具函数
│   │   ├── request.ts
│   │   ├── storage.ts
│   │   ├── format.ts
│   │   └── validate.ts
│   │
│   ├── App.vue
│   ├── main.ts
│   └── env.d.ts
│
├── .env
├── .env.development
├── .env.production
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

### 3.3 微信小程序项目结构 (teacher-miniapp / parent-miniapp)

```plaintext
apps/teacher-miniapp/
├── miniprogram/
│   ├── api/                          # API 模块
│   │   ├── index.ts
│   │   ├── request.ts
│   │   └── modules/
│   │
│   ├── components/                   # 自定义组件
│   │   ├── student-card/
│   │   ├── attendance-item/
│   │   ├── homework-item/
│   │   └── empty-state/
│   │
│   ├── pages/                        # 页面
│   │   ├── index/
│   │   ├── attendance/
│   │   ├── homework/
│   │   ├── growth/
│   │   ├── message/
│   │   └── mine/
│   │
│   ├── store/                        # MobX 状态管理
│   │   ├── index.ts
│   │   ├── user.ts
│   │   ├── class.ts
│   │   └── message.ts
│   │
│   ├── utils/
│   ├── styles/
│   ├── app.ts
│   ├── app.json
│   └── app.wxss
│
├── typings/
├── project.config.json
├── tsconfig.json
└── package.json
```

---

## 4. 组件规范

### 4.1 Vue 3 组件模板

```typescript
<!-- src/components/business/StudentCard.vue -->
<script setup lang="ts">
/**
 * @description 学生信息卡片组件
 */
import { computed } from 'vue'
import type { Student } from '@/types/student'

// ============ Props 定义 ============
interface Props {
  /** 学生数据 */
  student: Student
  /** 是否显示操作按钮 */
  showActions?: boolean
  /** 卡片尺寸 */
  size?: 'small' | 'medium' | 'large'
}

const props = withDefaults(defineProps<Props>(), {
  showActions: true,
  size: 'medium'
})

// ============ Emits 定义 ============
interface Emits {
  (e: 'click', student: Student): void
  (e: 'edit', student: Student): void
  (e: 'delete', id: string): void
}

const emit = defineEmits<Emits>()

// ============ Computed ============
const statusClass = computed(() => {
  const statusMap: Record<string, string> = {
    active: 'status--active',
    leave: 'status--leave',
    withdrawn: 'status--withdrawn'
  }
  return statusMap[props.student.status] || ''
})

// ============ Methods ============
const handleClick = () => {
  emit('click', props.student)
}
</script>

<template>
  <div
    class="student-card"
    :class="[`student-card--${size}`, statusClass]"
    @click="handleClick"
  >
    <div class="student-card__avatar">
      <el-avatar :size="size === 'small' ? 40 : 60" :src="student.avatar">
        {{ student.name.charAt(0) }}
      </el-avatar>
    </div>
    <div class="student-card__info">
      <h4 class="name">{{ student.name }}</h4>
      <p class="meta">{{ student.grade }} · {{ student.className }}</p>
    </div>
    <div v-if="showActions" class="student-card__actions">
      <slot name="actions" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.student-card {
  display: flex;
  align-items: center;
  padding: var(--spacing-md);
  background: var(--bg-color-container);
  border-radius: var(--border-radius-md);
  cursor: pointer;
  transition: box-shadow 0.2s;

  &:hover {
    box-shadow: var(--shadow-sm);
  }
}
</style>
```

### 4.2 微信小程序组件模板

```typescript
// miniprogram/components/student-card/index.ts
Component({
  options: {
    multipleSlots: true,
    addGlobalClass: true,
    styleIsolation: 'apply-shared'
  },

  properties: {
    student: {
      type: Object,
      value: {}
    },
    showActions: {
      type: Boolean,
      value: true
    },
    size: {
      type: String,
      value: 'medium'
    }
  },

  methods: {
    onTap() {
      this.triggerEvent('click', { student: this.data.student })
    }
  }
})
```

### 4.3 命名规范

| 类别 | Vue 3 Web | 微信小程序 | 示例 |
|------|-----------|------------|------|
| **组件文件** | PascalCase.vue | kebab-case/ | `StudentCard.vue` / `student-card/` |
| **组件名** | PascalCase | kebab-case | `<StudentCard>` / `<student-card>` |
| **Props** | camelCase | camelCase | `showActions` |
| **Events** | camelCase | camelCase | `@click` / `bind:click` |
| **CSS Class** | BEM (kebab-case) | BEM (kebab-case) | `.student-card__avatar` |
| **Composables** | use + PascalCase | - | `useAuth.ts` |
| **Store Modules** | camelCase | camelCase | `user.ts` |
| **Types/Interfaces** | PascalCase | PascalCase | `interface Student {}` |
| **Constants** | UPPER_SNAKE_CASE | UPPER_SNAKE_CASE | `MAX_UPLOAD_SIZE` |

---

## 5. 状态管理

### 5.1 Pinia Store 结构 (Vue 3 Web)

```plaintext
src/stores/
├── index.ts                 # Store 统一导出
├── types.ts                 # 类型定义
└── modules/
    ├── user.ts              # 用户状态
    ├── permission.ts        # 权限状态
    ├── app.ts               # 应用状态
    └── tabs.ts              # 标签页状态
```

### 5.2 User Store 模板

```typescript
// src/stores/modules/user.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { UserInfo, LoginParams } from '@/types/user'
import { login, logout, getUserInfo } from '@/api/modules/auth'
import { storage } from '@/utils/storage'

export const useUserStore = defineStore('user', () => {
  // ============ State ============
  const token = ref<string>(storage.get('token') || '')
  const userInfo = ref<UserInfo | null>(null)
  const roles = ref<string[]>([])

  // ============ Getters ============
  const isLoggedIn = computed(() => !!token.value)
  const userName = computed(() => userInfo.value?.name || '')
  const isAdmin = computed(() => roles.value.includes('admin'))

  // ============ Actions ============
  async function loginAction(params: LoginParams) {
    try {
      const res = await login(params)
      token.value = res.access_token
      storage.set('token', res.access_token)
      await fetchUserInfo()
      return res
    } catch (error) {
      throw error
    }
  }

  async function fetchUserInfo() {
    try {
      const res = await getUserInfo()
      userInfo.value = res
      roles.value = res.roles || []
      return res
    } catch (error) {
      throw error
    }
  }

  async function logoutAction() {
    try {
      await logout()
    } finally {
      resetState()
    }
  }

  function resetState() {
    token.value = ''
    userInfo.value = null
    roles.value = []
    storage.remove('token')
  }

  return {
    // State
    token,
    userInfo,
    roles,
    // Getters
    isLoggedIn,
    userName,
    isAdmin,
    // Actions
    loginAction,
    fetchUserInfo,
    logoutAction,
    resetState
  }
})
```

### 5.3 MobX Store 结构 (微信小程序)

```plaintext
miniprogram/store/
├── index.ts                 # Store 实例
├── user.ts                  # 用户状态
├── class.ts                 # 班级状态
└── message.ts               # 消息状态
```

### 5.4 小程序 User Store 模板

```typescript
// miniprogram/store/user.ts
import { observable, action } from 'mobx-miniprogram'

export const userStore = observable({
  // ============ State ============
  token: '' as string,
  userInfo: null as WxUserInfo | null,
  currentClass: null as ClassInfo | null,

  // ============ Computed ============
  get isLoggedIn() {
    return !!this.token
  },

  get userName() {
    return this.userInfo?.name || ''
  },

  // ============ Actions ============
  setToken: action(function(this: typeof userStore, token: string) {
    this.token = token
    wx.setStorageSync('token', token)
  }),

  setUserInfo: action(function(this: typeof userStore, info: WxUserInfo) {
    this.userInfo = info
  }),

  setCurrentClass: action(function(this: typeof userStore, classInfo: ClassInfo) {
    this.currentClass = classInfo
  }),

  logout: action(function(this: typeof userStore) {
    this.token = ''
    this.userInfo = null
    this.currentClass = null
    wx.removeStorageSync('token')
  })
})
```

---

## 6. API 集成

### 6.1 Axios 封装 (Vue 3 Web)

```typescript
// src/utils/request.ts
import axios, { type AxiosInstance, type AxiosRequestConfig } from 'axios'
import { ElMessage, ElMessageBox } from 'element-plus'
import { useUserStore } from '@/stores/modules/user'
import router from '@/router'

// 统一响应格式
interface ApiResponse<T = any> {
  code: number
  message: string
  data: T
}

// 创建实例
const service: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// 请求拦截器
service.interceptors.request.use(
  (config) => {
    const userStore = useUserStore()
    if (userStore.token) {
      config.headers.Authorization = `Bearer ${userStore.token}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)

// 响应拦截器
service.interceptors.response.use(
  (response) => {
    const res = response.data as ApiResponse

    // 业务错误
    if (res.code !== 0 && res.code !== 200) {
      ElMessage.error(res.message || '请求失败')

      // Token 过期
      if (res.code === 401) {
        ElMessageBox.confirm('登录已过期，请重新登录', '提示', {
          confirmButtonText: '重新登录',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          const userStore = useUserStore()
          userStore.resetState()
          router.push('/login')
        })
      }

      return Promise.reject(new Error(res.message))
    }

    return res.data
  },
  (error) => {
    const message = error.response?.data?.message || error.message || '网络错误'
    ElMessage.error(message)
    return Promise.reject(error)
  }
)

// 封装请求方法
export function request<T>(config: AxiosRequestConfig): Promise<T> {
  return service(config)
}

export function get<T>(url: string, params?: object): Promise<T> {
  return request({ method: 'GET', url, params })
}

export function post<T>(url: string, data?: object): Promise<T> {
  return request({ method: 'POST', url, data })
}

export function put<T>(url: string, data?: object): Promise<T> {
  return request({ method: 'PUT', url, data })
}

export function del<T>(url: string, params?: object): Promise<T> {
  return request({ method: 'DELETE', url, params })
}

export default service
```

### 6.2 API 模块示例

```typescript
// src/api/modules/student.ts
import { get, post, put, del } from '@/utils/request'
import type { Student, StudentListParams, StudentListResult } from '@/types/student'

const PREFIX = '/api/students'

/**
 * 获取学生列表
 */
export function getStudentList(params: StudentListParams): Promise<StudentListResult> {
  return get(PREFIX, params)
}

/**
 * 获取学生详情
 */
export function getStudentDetail(id: string): Promise<Student> {
  return get(`${PREFIX}/${id}`)
}

/**
 * 新增学生
 */
export function createStudent(data: Partial<Student>): Promise<Student> {
  return post(PREFIX, data)
}

/**
 * 更新学生
 */
export function updateStudent(id: string, data: Partial<Student>): Promise<Student> {
  return put(`${PREFIX}/${id}`, data)
}

/**
 * 删除学生
 */
export function deleteStudent(id: string): Promise<void> {
  return del(`${PREFIX}/${id}`)
}

/**
 * 学生调班
 */
export function transferStudent(id: string, classId: string): Promise<void> {
  return post(`${PREFIX}/${id}/transfer`, { classId })
}
```

### 6.3 微信小程序请求封装

```typescript
// miniprogram/utils/request.ts
import { userStore } from '../store/user'

interface RequestConfig {
  url: string
  method?: 'GET' | 'POST' | 'PUT' | 'DELETE'
  data?: object
  header?: object
  showLoading?: boolean
  showError?: boolean
}

interface ApiResponse<T = any> {
  code: number
  message: string
  data: T
}

const BASE_URL = 'https://api.example.com'

export function request<T>(config: RequestConfig): Promise<T> {
  const { url, method = 'GET', data, header = {}, showLoading = true, showError = true } = config

  return new Promise((resolve, reject) => {
    if (showLoading) {
      wx.showLoading({ title: '加载中...', mask: true })
    }

    wx.request({
      url: `${BASE_URL}${url}`,
      method,
      data,
      header: {
        'Content-Type': 'application/json',
        'Authorization': userStore.token ? `Bearer ${userStore.token}` : '',
        ...header
      },
      success: (res) => {
        const response = res.data as ApiResponse<T>

        if (response.code === 0 || response.code === 200) {
          resolve(response.data)
        } else {
          if (response.code === 401) {
            // Token 过期
            userStore.logout()
            wx.redirectTo({ url: '/pages/login/index' })
          }

          if (showError) {
            wx.showToast({ title: response.message || '请求失败', icon: 'none' })
          }
          reject(new Error(response.message))
        }
      },
      fail: (error) => {
        if (showError) {
          wx.showToast({ title: '网络错误', icon: 'none' })
        }
        reject(error)
      },
      complete: () => {
        if (showLoading) {
          wx.hideLoading()
        }
      }
    })
  })
}

export const get = <T>(url: string, data?: object) => request<T>({ url, method: 'GET', data })
export const post = <T>(url: string, data?: object) => request<T>({ url, method: 'POST', data })
export const put = <T>(url: string, data?: object) => request<T>({ url, method: 'PUT', data })
export const del = <T>(url: string, data?: object) => request<T>({ url, method: 'DELETE', data })
```

---

## 7. 路由配置

### 7.1 Vue Router 配置

```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'
import { setupRouterGuards } from './guards'

// 静态路由
const constantRoutes: RouteRecordRaw[] = [
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/login/index.vue'),
    meta: { title: '登录', hidden: true }
  },
  {
    path: '/404',
    name: 'NotFound',
    component: () => import('@/views/error/404.vue'),
    meta: { title: '页面不存在', hidden: true }
  }
]

// 动态路由 (根据权限动态添加)
export const asyncRoutes: RouteRecordRaw[] = [
  {
    path: '/',
    component: () => import('@/layouts/DefaultLayout.vue'),
    redirect: '/dashboard',
    children: [
      {
        path: 'dashboard',
        name: 'Dashboard',
        component: () => import('@/views/dashboard/index.vue'),
        meta: { title: '数据驾驶舱', icon: 'dashboard' }
      }
    ]
  },
  {
    path: '/base',
    component: () => import('@/layouts/DefaultLayout.vue'),
    redirect: '/base/store',
    meta: { title: '基础管理', icon: 'setting' },
    children: [
      {
        path: 'store',
        name: 'StoreManage',
        component: () => import('@/views/base/store/index.vue'),
        meta: { title: '门店管理', permission: 'base:store:view' }
      },
      {
        path: 'class',
        name: 'ClassManage',
        component: () => import('@/views/base/class/index.vue'),
        meta: { title: '班级管理', permission: 'base:class:view' }
      },
      {
        path: 'teacher',
        name: 'TeacherManage',
        component: () => import('@/views/base/teacher/index.vue'),
        meta: { title: '教师管理', permission: 'base:teacher:view' }
      },
      {
        path: 'student',
        name: 'StudentManage',
        component: () => import('@/views/base/student/index.vue'),
        meta: { title: '学生管理', permission: 'base:student:view' }
      }
    ]
  },
  {
    path: '/daily',
    component: () => import('@/layouts/DefaultLayout.vue'),
    redirect: '/daily/attendance',
    meta: { title: '日常运营', icon: 'calendar' },
    children: [
      {
        path: 'attendance',
        name: 'AttendanceManage',
        component: () => import('@/views/daily/attendance/index.vue'),
        meta: { title: '签到管理', permission: 'daily:attendance:view' }
      },
      {
        path: 'homework',
        name: 'HomeworkManage',
        component: () => import('@/views/daily/homework/index.vue'),
        meta: { title: '作业管理', permission: 'daily:homework:view' }
      }
    ]
  },
  {
    path: '/growth',
    component: () => import('@/layouts/DefaultLayout.vue'),
    redirect: '/growth/archive',
    meta: { title: '成长追踪', icon: 'star' },
    children: [
      {
        path: 'archive',
        name: 'GrowthArchive',
        component: () => import('@/views/growth/archive/index.vue'),
        meta: { title: '成长档案', permission: 'growth:archive:view' }
      },
      {
        path: 'evaluation',
        name: 'EvaluationManage',
        component: () => import('@/views/growth/evaluation/index.vue'),
        meta: { title: '评价管理', permission: 'growth:evaluation:view' }
      }
    ]
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes: constantRoutes
})

// 设置路由守卫
setupRouterGuards(router)

export default router
```

### 7.2 路由守卫

```typescript
// src/router/guards.ts
import type { Router } from 'vue-router'
import { useUserStore } from '@/stores/modules/user'
import { usePermissionStore } from '@/stores/modules/permission'
import NProgress from 'nprogress'

const whiteList = ['/login', '/404']

export function setupRouterGuards(router: Router) {
  router.beforeEach(async (to, from, next) => {
    NProgress.start()

    const userStore = useUserStore()
    const permissionStore = usePermissionStore()

    // 白名单直接放行
    if (whiteList.includes(to.path)) {
      next()
      return
    }

    // 未登录跳转登录页
    if (!userStore.isLoggedIn) {
      next(`/login?redirect=${to.path}`)
      return
    }

    // 已登录但没有用户信息，获取用户信息
    if (!userStore.userInfo) {
      try {
        await userStore.fetchUserInfo()
        // 动态添加路由
        const accessRoutes = await permissionStore.generateRoutes(userStore.roles)
        accessRoutes.forEach(route => router.addRoute(route))
        next({ ...to, replace: true })
      } catch (error) {
        userStore.resetState()
        next(`/login?redirect=${to.path}`)
      }
      return
    }

    // 权限检查
    if (to.meta.permission && !permissionStore.hasPermission(to.meta.permission as string)) {
      next('/403')
      return
    }

    next()
  })

  router.afterEach(() => {
    NProgress.done()
  })
}
```

### 7.3 小程序路由配置

```json
// miniprogram/app.json (教师端)
{
  "pages": [
    "pages/index/index",
    "pages/attendance/list/index",
    "pages/attendance/detail/index",
    "pages/homework/list/index",
    "pages/homework/upload/index",
    "pages/homework/review/index",
    "pages/growth/daily/index",
    "pages/growth/edit/index",
    "pages/message/list/index",
    "pages/message/chat/index",
    "pages/mine/index/index",
    "pages/mine/settings/index"
  ],
  "tabBar": {
    "color": "#999999",
    "selectedColor": "#1890FF",
    "backgroundColor": "#ffffff",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "工作台",
        "iconPath": "images/tab/home.png",
        "selectedIconPath": "images/tab/home-active.png"
      },
      {
        "pagePath": "pages/attendance/list/index",
        "text": "签到",
        "iconPath": "images/tab/attendance.png",
        "selectedIconPath": "images/tab/attendance-active.png"
      },
      {
        "pagePath": "pages/homework/list/index",
        "text": "作业",
        "iconPath": "images/tab/homework.png",
        "selectedIconPath": "images/tab/homework-active.png"
      },
      {
        "pagePath": "pages/message/list/index",
        "text": "消息",
        "iconPath": "images/tab/message.png",
        "selectedIconPath": "images/tab/message-active.png"
      },
      {
        "pagePath": "pages/mine/index/index",
        "text": "我的",
        "iconPath": "images/tab/mine.png",
        "selectedIconPath": "images/tab/mine-active.png"
      }
    ]
  },
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#1890FF",
    "navigationBarTitleText": "精准托管",
    "navigationBarTextStyle": "white"
  }
}
```

---

## 8. 样式指南

### 8.1 CSS 变量主题系统

```scss
// src/assets/styles/variables.scss (Vue 3 Web)
:root {
  // ============ 品牌色彩 (来自 PRD Section 3.5) ============
  --color-primary: #1890FF;           // 信任蓝 - 主按钮、导航栏
  --color-primary-light: #40A9FF;
  --color-primary-dark: #096DD9;

  --color-secondary: #FF7A45;         // 活力橙 - 提醒、积分、活动
  --color-secondary-light: #FFA07A;

  --color-success: #52C41A;           // 成功绿 - 签到成功、正确标记
  --color-warning: #FAAD14;           // 警告黄
  --color-danger: #FF4D4F;            // 警告红 - 异常、错误、错题
  --color-info: #1890FF;

  // ============ 文字色彩 ============
  --text-color-primary: #333333;      // 正文内容
  --text-color-secondary: #666666;
  --text-color-placeholder: #999999;
  --text-color-disabled: #C0C4CC;

  // ============ 背景色彩 ============
  --bg-color-page: #F5F5F5;           // 页面背景
  --bg-color-container: #FFFFFF;
  --bg-color-overlay: rgba(0, 0, 0, 0.5);

  // ============ 边框 ============
  --border-color: #E4E7ED;
  --border-color-light: #EBEEF5;
  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-radius-lg: 12px;
  --border-radius-round: 999px;

  // ============ 间距 ============
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  // ============ 字体 ============
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  --font-size-xs: 12px;
  --font-size-sm: 13px;
  --font-size-md: 14px;
  --font-size-lg: 16px;
  --font-size-xl: 18px;
  --font-size-xxl: 20px;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 600;

  // ============ 阴影 ============
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);

  // ============ 层级 ============
  --z-index-dropdown: 1000;
  --z-index-modal: 2000;
  --z-index-toast: 3000;

  // ============ 动画 ============
  --transition-fast: 0.15s ease;
  --transition-normal: 0.3s ease;
}

// 暗色主题 (可选)
[data-theme='dark'] {
  --bg-color-page: #1A1A1A;
  --bg-color-container: #2D2D2D;
  --text-color-primary: #E5E5E5;
  --text-color-secondary: #A0A0A0;
  --border-color: #404040;
}
```

### 8.2 小程序样式变量

```css
/* miniprogram/styles/variables.wxss */
page {
  /* 品牌色彩 */
  --color-primary: #1890FF;
  --color-secondary: #FF7A45;
  --color-success: #52C41A;
  --color-danger: #FF4D4F;

  /* 文字色彩 */
  --text-color-primary: #333333;
  --text-color-secondary: #666666;
  --text-color-placeholder: #999999;

  /* 背景色彩 */
  --bg-color-page: #F5F5F5;
  --bg-color-container: #FFFFFF;

  /* 间距 (rpx) */
  --spacing-xs: 8rpx;
  --spacing-sm: 16rpx;
  --spacing-md: 32rpx;
  --spacing-lg: 48rpx;

  /* 字体 */
  --font-size-sm: 24rpx;
  --font-size-md: 28rpx;
  --font-size-lg: 32rpx;

  /* 边框圆角 */
  --border-radius-sm: 8rpx;
  --border-radius-md: 16rpx;
  --border-radius-lg: 24rpx;
}
```

### 8.3 响应式断点

```scss
// src/assets/styles/mixins.scss
// 响应式断点 (移动优先)
$breakpoints: (
  'sm': 576px,    // 手机
  'md': 768px,    // 平板
  'lg': 992px,    // 小屏电脑
  'xl': 1200px,   // 桌面
  'xxl': 1600px   // 大屏
);

@mixin respond-to($breakpoint) {
  @if map-has-key($breakpoints, $breakpoint) {
    @media (min-width: map-get($breakpoints, $breakpoint)) {
      @content;
    }
  }
}

// 使用示例
// .container {
//   padding: var(--spacing-sm);
//   @include respond-to('md') {
//     padding: var(--spacing-lg);
//   }
// }
```

---

## 9. 测试策略

### 9.1 测试类型与覆盖目标

| 测试类型 | 覆盖范围 | 工具 | 目标覆盖率 |
|---------|---------|------|-----------|
| **Unit** | 组件、工具函数、Store | Vitest + Vue Test Utils | ≥ 80% |
| **Integration** | 页面交互、API调用 | Vitest + MSW | 核心流程100% |
| **E2E** | 关键用户流程 | Playwright | 核心流程覆盖 |
| **小程序测试** | 组件、页面 | miniprogram-simulate | 核心组件覆盖 |

### 9.2 组件测试模板

```typescript
// src/components/business/__tests__/StudentCard.spec.ts
import { describe, it, expect, vi } from 'vitest'
import { mount } from '@vue/test-utils'
import StudentCard from '../StudentCard.vue'
import type { Student } from '@/types/student'

const mockStudent: Student = {
  id: '1',
  name: '张小明',
  avatar: '',
  grade: '三年级',
  className: '1班',
  status: 'active',
  allergies: []
}

describe('StudentCard', () => {
  it('renders student name correctly', () => {
    const wrapper = mount(StudentCard, {
      props: { student: mockStudent }
    })
    expect(wrapper.text()).toContain('张小明')
  })

  it('displays grade and class info', () => {
    const wrapper = mount(StudentCard, {
      props: { student: mockStudent }
    })
    expect(wrapper.text()).toContain('三年级')
    expect(wrapper.text()).toContain('1班')
  })

  it('emits click event when clicked', async () => {
    const wrapper = mount(StudentCard, {
      props: { student: mockStudent }
    })
    await wrapper.trigger('click')
    expect(wrapper.emitted('click')).toBeTruthy()
    expect(wrapper.emitted('click')![0]).toEqual([mockStudent])
  })

  it('hides actions when showActions is false', () => {
    const wrapper = mount(StudentCard, {
      props: { student: mockStudent, showActions: false }
    })
    expect(wrapper.find('.student-card__actions').exists()).toBe(false)
  })

  it('shows allergy warning when student has allergies', () => {
    const studentWithAllergy = { ...mockStudent, allergies: ['花生'] }
    const wrapper = mount(StudentCard, {
      props: { student: studentWithAllergy }
    })
    expect(wrapper.find('.allergy-tag').exists()).toBe(true)
  })
})
```

### 9.3 测试最佳实践

1. **Unit Tests**: 测试组件独立功能，mock 外部依赖
2. **Integration Tests**: 测试组件交互，使用 MSW mock API
3. **E2E Tests**: 测试关键用户流程（登录、签到、作业批改）
4. **Coverage Goals**: 核心业务代码 80% 覆盖率
5. **Test Structure**: Arrange-Act-Assert 模式
6. **Mock External Dependencies**: API调用、路由、状态管理

---

## 10. 环境配置

### 10.1 Vue 3 Web 环境变量

```bash
# .env.development
VITE_APP_TITLE=托管管理系统(开发)
VITE_API_BASE_URL=http://localhost:8080
VITE_UPLOAD_URL=http://localhost:8080/api/upload
VITE_OSS_DOMAIN=https://dev-oss.example.com

# .env.production
VITE_APP_TITLE=托管管理系统
VITE_API_BASE_URL=https://api.example.com
VITE_UPLOAD_URL=https://api.example.com/api/upload
VITE_OSS_DOMAIN=https://oss.example.com
```

### 10.2 环境变量类型定义

```typescript
// src/env.d.ts
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_APP_TITLE: string
  readonly VITE_API_BASE_URL: string
  readonly VITE_UPLOAD_URL: string
  readonly VITE_OSS_DOMAIN: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

### 10.3 小程序环境配置

```typescript
// miniprogram/config/env.ts
type EnvType = 'development' | 'trial' | 'release'

interface EnvConfig {
  API_BASE_URL: string
  OSS_DOMAIN: string
  WX_APP_ID: string
}

const envConfigs: Record<EnvType, EnvConfig> = {
  development: {
    API_BASE_URL: 'https://dev-api.example.com',
    OSS_DOMAIN: 'https://dev-oss.example.com',
    WX_APP_ID: 'wx_dev_appid'
  },
  trial: {
    API_BASE_URL: 'https://test-api.example.com',
    OSS_DOMAIN: 'https://test-oss.example.com',
    WX_APP_ID: 'wx_test_appid'
  },
  release: {
    API_BASE_URL: 'https://api.example.com',
    OSS_DOMAIN: 'https://oss.example.com',
    WX_APP_ID: 'wx_prod_appid'
  }
}

const { envVersion = 'release' } = wx.getAccountInfoSync().miniProgram
export const config = envConfigs[envVersion as EnvType] || envConfigs.release
```

---

## 11. 编码规范

### 11.1 Critical Coding Rules

#### 通用规则

| 规则 | 说明 |
|------|------|
| **TypeScript 严格模式** | `strict: true`，禁止 `any` 类型（除非必要） |
| **禁止硬编码** | API路径、常量、配置项抽取到配置文件 |
| **统一错误处理** | 使用统一的错误处理机制，不吞掉异常 |
| **禁止 console.log** | 生产环境移除所有 console，使用 logger |
| **异步错误处理** | async/await 必须 try-catch 或 .catch() |
| **Props 类型定义** | 所有组件 Props 必须有 TypeScript 类型定义 |

#### Vue 3 特定规则

| 规则 | 说明 |
|------|------|
| **使用 Composition API** | 新组件统一使用 `<script setup>` 语法 |
| **响应式声明** | 优先使用 `ref`，对象用 `reactive`，避免解构丢失响应性 |
| **避免 v-if 和 v-for 同时使用** | v-if 优先级更高，需要时用 template 包裹 |
| **Key 绑定** | v-for 循环必须绑定唯一 key |
| **组件命名** | 多词组件名，避免与 HTML 标签冲突 |
| **Props 不可变** | 禁止直接修改 Props，使用 emit 通知父组件 |

#### 小程序特定规则

| 规则 | 说明 |
|------|------|
| **setData 优化** | 避免频繁 setData，合并多次更新 |
| **图片懒加载** | 列表图片使用 lazy-load 属性 |
| **分包加载** | 非核心页面放入分包，减少主包体积 |
| **避免频繁 wx.request** | 相同数据缓存，避免重复请求 |
| **页面栈管理** | 使用 navigateTo 不超过10层，适时使用 redirectTo |

### 11.2 ESLint + Prettier 配置

```javascript
// .eslintrc.cjs
module.exports = {
  root: true,
  env: { browser: true, node: true, es2022: true },
  extends: [
    'eslint:recommended',
    'plugin:vue/vue3-recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier'
  ],
  parser: 'vue-eslint-parser',
  parserOptions: {
    parser: '@typescript-eslint/parser',
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  rules: {
    'vue/multi-word-component-names': 'error',
    'vue/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'warn'
  }
}
```

---

## 12. 快速参考

### 12.1 常用命令

```bash
# Vue 3 Web 项目
pnpm dev                    # 启动开发服务器
pnpm build                  # 生产构建
pnpm preview                # 预览生产构建
pnpm lint                   # ESLint 检查
pnpm lint:fix               # ESLint 自动修复
pnpm test                   # 运行测试
pnpm test:coverage          # 测试覆盖率

# 小程序项目
pnpm dev:mp                 # 编译小程序 (watch 模式)
pnpm build:mp               # 构建小程序
npm run compile             # TypeScript 编译 (小程序原生)
```

### 12.2 关键导入模式

```typescript
// Vue 3 组件导入
import { ref, computed, watch, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { storeToRefs } from 'pinia'
import { ElMessage, ElMessageBox } from 'element-plus'

// Store 使用
import { useUserStore } from '@/stores/modules/user'
const userStore = useUserStore()
const { userInfo, isLoggedIn } = storeToRefs(userStore)

// API 调用
import { getStudentList, createStudent } from '@/api/modules/student'

// 工具函数
import { formatDate, formatMoney } from '@/utils/format'
import { storage } from '@/utils/storage'

// 类型导入
import type { Student, StudentListParams } from '@/types/student'
```

### 12.3 项目特定模式

```typescript
// 表格页面标准模式
import { useTable } from '@/composables/useTable'

const {
  loading,
  tableData,
  pagination,
  handleSearch,
  handleReset,
  handlePageChange
} = useTable(getStudentList)

// 表单页面标准模式
import { useForm } from '@/composables/useForm'

const {
  formRef,
  formData,
  formRules,
  handleSubmit,
  handleReset
} = useForm<StudentForm>({
  initialData: { name: '', grade: '' },
  onSubmit: createStudent
})

// 权限检查
import { usePermission } from '@/composables/usePermission'

const { hasPermission, hasRole } = usePermission()
const canEdit = hasPermission('student:edit')
```

### 12.4 文件命名速查

| 类型 | 命名规范 | 示例 |
|------|---------|------|
| Vue 组件 | PascalCase.vue | `StudentCard.vue` |
| 页面视图 | index.vue (目录同名) | `views/student/index.vue` |
| Composable | use + PascalCase.ts | `useAuth.ts` |
| Store | camelCase.ts | `user.ts` |
| API | camelCase.ts | `student.ts` |
| 类型 | camelCase.ts / PascalCase | `student.ts` / `interface Student` |
| 工具函数 | camelCase.ts | `format.ts` |
| 样式 | kebab-case.scss | `variables.scss` |
| 小程序页面 | kebab-case/index | `attendance-list/index` |
| 小程序组件 | kebab-case/ | `student-card/` |

---

## Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2026-01-14 | 1.0 | 初始版本，完整前端架构文档 | UX Expert (Jingwen) |

---

*Generated by Orchestrix UX Expert Agent - Jingwen*
*Version 1.0 | 2026-01-14*
