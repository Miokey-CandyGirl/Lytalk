# Lytalk

> 轻盈沟通，自在生活 —— 一款简约、现代、小清新的即时通讯 Web 应用

---

## 一、产品概述

Lytalk 是一款以「轻盈沟通」为核心理念的即时通讯 Web 应用，集成聊天、便签、动态、阅读、游戏、音乐、短视频、工具等多种功能于一体。整体视觉采用 Mint Green（薄荷绿）为主色调，搭配毛玻璃质感与流畅动效，包含水滴、星球、星星等动态效果，营造清新、自然、动感、无负担的使用体验。

- **通用产品名**：Lytalk
- **中文产品名**：青聊
- **Slogan**：轻盈沟通，自在生活
- **设计风格**：简约 · 现代 · 动感 · 小清新（Minimal · Modern · Dynamic · Fresh）
- **主色调**：Teal-Mint 薄荷绿
- **设计年份**：©2026 Lytalk 青聊

---

## 二、技术栈

| 类别 | 技术选型 | 版本 |
|------|---------|------|
| **构建工具** | [Vite](https://vitejs.dev/) | ^5.x |
| **前端框架** | [Vue 3](https://vuejs.org/) (Composition API + `<script setup>`) | ^3.4 |
| **路由** | [Vue Router](https://router.vuejs.org/) | ^4.x |
| **状态管理** | [Pinia](https://pinia.vuejs.org/) | ^2.x |
| **后端即服务（BaaS）** | [Supabase](https://supabase.com/) | ^2.x |
| **数据库** | PostgreSQL（Supabase 托管） | — |
| **实时通信** | Supabase Realtime（WebSocket） | — |
| **认证** | Supabase Auth | — |
| **文件存储** | Supabase Storage | — |
| **语言** | TypeScript | ^5.x |
| **CSS 方案** | 原生 CSS Variables + Scoped Styles | — |
| **图标** | 内联 SVG（Lucide 风格线性图标） | — |
| **PWA** | vite-plugin-pwa | ^0.x |
| **代码规范** | ESLint + Prettier | — |
| **包管理器** | pnpm / npm | — |
| **Node 要求** | >= 18.x | — |

### 2.1 为什么选择 Vite + Vue 3

- **Vite**：极快的冷启动（ESM 原生加载）、毫秒级 HMR、按需编译、开箱支持 TS/CSS 预处理器
- **Vue 3 Composition API**：更好的逻辑复用（Composables）、更优的 Tree-shaking、`<script setup>` 语法糖简洁高效
- **Pinia**：Vue 官方推荐状态管理，TS 友好，模块化设计，DevTools 支持
- **TypeScript**：类型安全，提升大型项目可维护性
- **无 UI 组件库依赖**：基于设计系统自定义组件，保持视觉统一和包体积极致轻量

### 2.2 为什么选择 Supabase

Lytalk 所有后端数据、账户、注册逻辑、实时消息等均由 Supabase 提供，无需自建后端服务：

- **PostgreSQL 数据库**：用户资料、好友关系、消息记录、便签、动态、日程、闹钟等全部数据存储于 Supabase Postgres，通过 RLS（Row Level Security）行级安全策略保证数据隔离
- **Supabase Auth**：TT号+密码一键注册/登录、第三方 OAuth（手机号/邮箱/QQ/微信/抖音），JWT 会话管理
- **Supabase Realtime**：聊天消息、好友在线状态、动态点赞等通过 PostgreSQL 的 Realtime Channels 做 WebSocket 实时推送，无需自建 WebSocket 服务
- **Supabase Storage**：头像、聊天图片/文件、动态图片、便签附件等存储，支持 CDN 加速和签名 URL 访问
- **Edge Functions**：敏感逻辑（添加好友验证、自动生成 TT 号、敏感词过滤等）部署为 Deno Edge Functions
- **优势**：零运维、开箱即用的 TypeScript 客户端 SDK（`@supabase/supabase-js`）、免费额度足够早期使用、可平滑扩展

---

## 三、快速开始

### 3.1 环境准备

- Node.js >= 18.x
- pnpm（推荐）或 npm
- 一个 Supabase 项目（已有账号）

### 3.2 环境变量配置

复制 `.env.example` 为 `.env`，填入 Supabase 项目凭证：

```env
VITE_SUPABASE_URL=https://xxxxxxxx.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOi...（Supabase Project Settings → API → anon public key）
```

### 3.3 安装与启动

```bash
# 安装依赖
pnpm install

# 启动开发服务器（默认 http://localhost:5173）
pnpm dev

# 类型检查
pnpm type-check

# 构建生产版本（输出到 dist/）
pnpm build

# 预览生产构建
pnpm preview
```

### 3.4 环境要求

- Node.js >= 18.0.0
- pnpm >= 8.x（推荐）或 npm >= 9.x

### 3.5 浏览器支持

- Chrome/Edge >= 100
- Firefox >= 100
- Safari >= 15
- 移动端 Safari/Chrome >= 15
- 其他主流浏览器（如小米、oppo、vivo等）

---

## 四、项目架构

### 4.1 目录结构

```
lytalk/
├── public/
│   ├── favicon.ico
│   ├── manifest.json           # PWA 配置
│   └── icons/                  # PWA 图标 (192/512)
├── src/
│   ├── assets/
│   │   └── logo.svg
│   ├── styles/
│   │   ├── variables.css       # 设计 Tokens（颜色/字体/圆角/阴影/动画）
│   │   ├── reset.css           # 全局 Reset
│   │   ├── global.css          # 全局样式/工具类
│   │   └── animations.css      # 关键帧动画（bgShift/ripple/pulse/spin）
│   ├── composables/
│   │   ├── useRipple.ts        # 按钮水波纹指令/composable
│   │   ├── useDrawer.ts        # 抽屉开合状态管理
│   │   ├── useMenu.ts          # 侧边菜单状态管理
│   │   ├── useTheme.ts         # 主题切换（明/暗色）
│   │   └── useClock.ts         # 实时时钟（世界时钟/秒表/倒计时）
│   ├── stores/                 # Pinia Stores
│   │   ├── user.ts             # 用户信息/登录状态
│   │   ├── chat.ts             # 聊天会话/消息
│   │   ├── friends.ts          # 好友列表/分组
│   │   └── settings.ts         # 应用设置
│   ├── router/
│   │   └── index.ts            # Vue Router 路由配置
│   ├── components/
│   │   ├── common/             # 通用组件
│   │   │   ├── AppLogo.vue     # Logo 按钮
│   │   │   ├── AppButton.vue   # 按钮（主/次/幽灵/危险/FAB）
│   │   │   ├── AppToggle.vue   # Toggle 开关
│   │   │   ├── AppAvatar.vue   # 头像（6档尺寸+状态点）
│   │   │   ├── AppCard.vue     # 卡片容器
│   │   │   ├── AppInput.vue    # 输入框
│   │   │   ├── AppTab.vue      # Tab 标签
│   │   │   ├── AppToast.vue    # Toast 提示
│   │   │   └── Ripple.vue      # 水波纹效果
│   │   ├── layout/
│   │   │   ├── AppShell.vue    # 应用外壳（背景+光晕）
│   │   │   ├── FriendDrawer.vue # 好友列表抽屉（300px 可折叠）
│   │   │   ├── SideMenu.vue    # 侧边功能菜单（260px）
│   │   │   ├── PageHeader.vue  # 页面统一 Header（60px）
│   │   │   └── Overlay.vue     # 遮罩层
│   │   └── chat/
│   │       ├── MessageBubble.vue # 消息气泡
│   │       ├── ChatInput.vue     # 输入框+工具栏
│   │       └── FriendItem.vue    # 好友列表项
│   ├── views/                  # 页面视图（对应路由）
│   │   ├── Login.vue           # 登录页（TT号+密码，一键注册/登录）
│   │   ├── Friends.vue         # 好友列表（主页，登录后立刻展示,含logo导航、搜索框、扫一扫功能）
│   │   ├── system/
│   │   │   ├── AddFriends.vue      # 添加好友
│   │   │   ├── AddGroup.vue        # 添加群聊
│   │   │   ├── MyProfile.vue       # 个人主页
│   │   │   ├── Settings.vue        # 系统设置页
│   │   │   ├── FriendDetail.vue    # 好友详情页
│   │   │   └── GroupDetail.vue     # 群聊详情页
│   │   ├── modules/
│   │   │   ├── TNotes.vue          # 便签模块
│   │   │   ├── TMoments.vue        # 动态模块
│   │   │   ├── TRead.vue           # 阅读模块
│   │   │   ├── TGame.vue           # 游戏模块
│   │   │   ├── TMusic.vue          # 音乐模块
│   │   │   ├── TWeather.vue        # 天气模块
│   │   │   ├── TCalendar.vue       # 日程模块
│   │   │   ├── Clock.vue           # 闹钟模块
│   │   │   ├── Chat.vue            # 聊天模块
│   │   │   ├── TLife.vue           # 生活模块
│   │   │   ├── TMap.vue            # 地图模块
│   │   │   └── TVideo.vue          # 视频模块
│   ├── types/                  # TypeScript 类型定义
│   │   ├── user.ts
│   │   ├── message.ts
│   │   ├── friend.ts
│   │   └── index.ts
│   ├── utils/
│   │   ├── format.ts           # 时间/数字格式化
│   │   └── localstorage.ts     # localStorage 封装
│   ├── lib/                    # 第三方库初始化
│   │   └── supabase.ts         # Supabase 客户端实例（createClient）
│   ├── api/                    # 数据访问层（封装 Supabase 查询）
│   │   ├── auth.ts             # 登录/注册/一键注册/登出
│   │   ├── users.ts            # 用户资料CRUD
│   │   ├── friends.ts          # 好友关系/分组
│   │   ├── messages.ts         # 聊天消息/实时订阅
│   │   ├── notes.ts            # 便签CRUD
│   │   ├── moments.ts          # 动态/点赞/评论
│   │   ├── calendar.ts         # 日程CRUD
│   │   └── clocks.ts           # 闹钟/世界时钟配置
│   ├── App.vue                 # 根组件（AppShell + RouterView）
│   └── main.ts                 # 入口（createApp + Pinia + Router + Supabase）
├── supabase/                   # Supabase 项目配置
│   ├── migrations/             # 数据库迁移 SQL
│   ├── functions/              # Edge Functions（Deno）
│   └── config.toml             # Supabase CLI 配置
├── index.html                  # Vite 入口 HTML
├── vite.config.ts              # Vite 配置（别名/PWA/代理）
├── tsconfig.json
├── .env                        # 环境变量（SUPABASE_URL / SUPABASE_ANON_KEY）
├── .env.example                # 环境变量示例
├── package.json
├── pnpm-lock.yaml
└── README.md
```

### 4.2 路由设计

```ts
// src/router/index.ts
const routes = [
  { path: '/', redirect: '/chat' },
  { path: '/login', component: LoginPage },
  {
    path: '/',
    component: AppShell,       // 带抽屉+菜单的主布局
    children: [
      { path: 'chat',              component: HomePage },           // 主页（聊天）
      { path: 'friend/:id',        component: FriendDetailPage },   // 好友详情
      { path: 'group/:id',         component: GroupDetailPage },    // 群聊详情
      { path: 'add-friend',        component: AddFriendPage },      // 添加好友
      { path: 'profile',           component: MyProfilePage },      // 个人主页
      { path: 'settings',          component: SettingsPage },       // 系统设置
      { path: 'notes',             component: NotesPage },          // 便签
      { path: 'moments',           component: MomentsPage },        // 动态
      { path: 'read',              component: ReadPage },           // T读
      { path: 'game',              component: GamePage },           // T游
      { path: 'music',             component: MusicPage },          // T听
      { path: 'video',             component: VideoPage },          // T视
      { path: 'weather',           component: WeatherPage },        // 天气
      { path: 'calendar',          component: CalendarPage },       // 日历
      { path: 'clock',             component: ClockPage },          // 时钟
    ]
  }
]
```

共 **1 个独立登录页 + 1个主应用好友列表页内视图 + 18 个主应用（system + modules）内视图 = 20 个路由视图**。

所有主应用子路由共享 `<AppShell>` 布局（mint 绿渐变背景 + `<FriendDrawer>` + `<SideMenu>` + `<RouterView>`），抽屉/菜单折叠状态通过 Pinia `useAppStore` 全局管理，路由切换时仅替换 `<RouterView>` 内容区，好友列表抽屉（Friends.vue）和它的左上角点击logo后弹出的侧边菜单状态保持不重置。

### 4.3 外部链接（非路由）

侧边菜单「探索」分组下的三个入口为**外部产品网页链接**，不在 Lytalk 路由体系内，点击后在新标签页打开：

| 名称 | 链接方式 | 说明 |
|------|---------|------|
| 琳凯蒂亚星球 | `<a href="https://rincatian.top" target="_blank" rel="noopener">` | 独立外部产品，新标签页打开 |
| iDrome | `<a href="https://rincatian.top/public/idrome/dromai.html" target="_blank" rel="noopener">` | 独立外部产品，新标签页打开 |
| 田语 | `<a href="https://rincatian.top/dictionary/dictionary.html" target="_blank" rel="noopener">` | 独立外部产品，新标签页打开 |

在 `SideMenu.vue` 中通过 `external: true` 配置项区分内部路由（`<router-link>`）与外部链接（`<a target="_blank">`）。

### 4.4 状态管理（Pinia Stores）

| Store | 职责 | 后端 |
|-------|------|------|
| `useUserStore` | 用户登录状态、个人资料、Token 管理 | Supabase Auth + `profiles` 表 |
| `useChatStore` | 当前会话、消息列表、发送消息、未读数、Realtime 订阅 | Supabase Realtime（`messages` 表） |
| `useFriendsStore` | 好友列表、分组、群聊、在线状态 | Supabase Realtime（`friends`/`groups` 表） |
| `useSettingsStore` | 深色模式、通知设置、外观偏好（持久化到 localStorage） | `user_settings` 表 |
| `useAppStore` | 抽屉开合、菜单开合、当前激活页面、全局 Toast | 前端本地状态 |

### 4.5 组件通信

- 父子组件：`props` + `emit`
- 跨层级/全局状态：Pinia Store
- 组合式逻辑复用：Composables（`useXxx`）
- 事件总线（极简场景）：Vue 3 `mitt`

---

## 五、设计系统（Design Tokens）

所有设计 Token 以 CSS Variables 形式定义在 `src/styles/variables.css` 的 `:root` 中，暗色模式通过 `html.dark` 类覆盖。

### 5.1 主色阶（Primary Color Scale - Teal Mint）

| Token | HEX | 用途 |
|-------|-----|------|
| `--color-primary-50` | `#F0FDFA` | 页面背景、卡片底色 |
| `--color-primary-100` | `#CCFBF1` | 选中背景、hover态、浅色填充 |
| `--color-primary-200` | `#99F6E4` | 装饰线、浅色强调 |
| `--color-primary-300` | `#5EEAD4` | 图标色、徽章色 |
| `--color-primary-400` | `#2DD4BF` | 渐变起点、按钮hover |
| `--color-primary-500` | `#14B8A6` | **主色 Primary** |
| `--color-primary-600` | `#0D9488` | 渐变终点、按钮默认态 |
| `--color-primary-700` | `#0F766E` | 深色文字强调 |
| `--color-primary-800` | `#115E59` | 深色模式 |
| `--color-primary-900` | `#134E4A` | 最深强调 |

### 5.2 渐变色（Gradient）

```css
--gradient-primary: linear-gradient(135deg, #2DD4BF, #0D9488);  /* 主按钮 */
--gradient-bg:      linear-gradient(135deg, #F0FDFA 0%, #CCFBF1 50%, #F0FDFA 100%);
--gradient-cover:   linear-gradient(135deg, #5EEAD4, #0D9488);  /* 封面大卡 */
--gradient-temp:    linear-gradient(to right, #BAE6FD, #5EEAD4, #FCD34D);
```

### 5.3 中性色（Neutral Scale）

| Token | HEX | 用途 |
|-------|-----|------|
| `--color-neutral-0` | `#FFFFFF` | 纯白 |
| `--color-neutral-50` | `#FAFAFA` | 页面底色 |
| `--color-neutral-100` | `#F5F5F5` | 分隔、mute背景 |
| `--color-neutral-200` | `#EBEBEB` | 边框 |
| `--color-neutral-400` | `#B8B8B8` | placeholder |
| `--color-neutral-500` | `#8C8C8C` | 次级文字 |
| `--color-neutral-600` | `#636363` | 辅助文字 |
| `--color-neutral-800` | `#262626` | 主文字 |

### 5.4 语义色（State Colors）

| Token | HEX | 用途 |
|-------|-----|------|
| `--state-success` | `#10B981` | 成功、在线 |
| `--state-warning` | `#F59E0B` | 警告、早时差 |
| `--state-error` | `#EF4444` | 错误、删除、周末 |
| `--color-online` | `#34D399` | 在线状态点 |
| `--color-away` | `#FBBF24` | 离开 |
| `--color-busy` | `#F87171` | 忙碌 |

### 5.5 毛玻璃（Glassmorphism）

```css
--glass-panel:  rgba(255, 255, 255, 0.65);  /* 主面板 */
--glass-drawer: rgba(255, 255, 255, 0.82);  /* 抽屉 */
--glass-menu:   rgba(255, 255, 255, 0.92);  /* 菜单 */
--glass-blur:   blur(20px);
--glass-blur-lg: blur(24px);
```

### 5.6 字体（Typography）

- **字体族**：`-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`
- **等宽字体**（时钟/计时器）：`'Courier New', Consolas, monospace`

| 级别 | 大小 | 行高 | 用途 |
|------|------|------|------|
| xs | 12px | 1.4 | 标签、时间、辅助信息 |
| sm | 13px | 1.4 | 次级正文 |
| base | 14px | 1.5 | 正文 |
| lg | 16px | 1.4 | 小标题、按钮 |
| xl | 18px | 1.3 | 标题 |
| 2xl | 22px | 1.25 | 大标题 |
| 3xl | 28px | 1.2 | 昵称、数据数字 |
| 4xl+ | 36-80px | 1 | 天气温度、时钟数字 |

### 5.7 圆角（Radius）

| Token | 值 | 用途 |
|-------|-----|------|
| `--radius-sm` | 4px | 小圆点、小标签 |
| `--radius-md` | 8px | 小按钮、徽章 |
| `--radius-lg` | 12px | 输入框、小卡片、头像 |
| `--radius-xl` | 16px | 卡片、菜单项 |
| `--radius-2xl` | 20px | **主面板、抽屉、页面级卡片** |
| `--radius-full` | 9999px | 圆形按钮、头像、胶囊 |

### 5.8 阴影（Shadow）

```css
--shadow-sm:    0 1px 2px rgba(0,0,0,0.03);
--shadow-md:    0 2px 8px rgba(0,0,0,0.04);
--shadow-lg:    0 4px 16px rgba(0,0,0,0.05);
--shadow-float: 0 8px 40px rgba(0,0,0,0.12);  /* FAB浮动按钮 */
--shadow-btn:   0 3px 10px rgba(20,184,166,0.25); /* 主按钮 */
```

### 5.9 间距（Spacing）

基于 4px 栅格：`4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48 / 64px`

### 5.10 动画（Transition & Animation）

| 名称 | 时长 | 曲线 | 用途 |
|------|------|------|------|
| `--transition-fast` | 0.15s | ease | 按钮hover |
| `--transition-normal` | 0.25s | ease | 常规切换 |
| `--transition-drawer` | 0.3s | cubic-bezier(0.4,0,0.2,1) | 抽屉、菜单滑入 |
| `bgShift` | 20s | ease-in-out infinite alternate | 背景光晕呼吸 |
| `rippleAnim` | 0.6s | ease-out | 按钮水波纹 |
| `pulse` | 2s | ease-in-out infinite | 在线状态脉冲 |
| `spin` | 1s | linear infinite | 刷新旋转 |

---

## 六、布局架构

### 6.1 两栏抽屉式布局（Two-Panel Drawer）（具体参考consultation/home.html）

```
┌──────────────────────────────────────────────────────┐
│  <AppShell> (mint渐变背景, 100vh, bgShift动画)        │
│  ┌────────────┐  ┌─────────────────────────────────┐ │
│  │<FriendDraw>│  │  <RouterView>                   │ │
│  │   组件固定  │  │       组件替换                   │ │
│  │  好友列表   │  │  ┌─────────────────────────────┐│ │
│  │  300px宽   │  │  │  <PageHeader> (60px)        ││ │
│  │  毛玻璃    │  │  │  ←  标题/副标题  功能按钮     ││ │
│  │  v-model   │  │  ├─────────────────────────────┤│ │
│  │  可折叠    │  │  │                             ││ │
│  │            │  │  │  <Page Content> (flex:1)    ││ │
│  │  Logo/搜索 │  │  │                             ││ │
│  │  我自己    │  │  │                             ││ │
│  │  特别关心   │  │  ├─────────────────────────────┤│ │
│  │  群聊      │  │  │  底部功能区（视页面而定）      ││ │
│  │  我的好友  │  │  └─────────────────────────────┘│ │
│  └────────────┘  └─────────────────────────────────┘ │
│  ┌────────────┐                                       │
│  │<SideMenu>  │  260px宽, z-index:25, translateX动画  │
│  └────────────┘                                       │
│  ┌────────────┐                                       │
│  │ <Overlay>  │  z-index:20, 半透明遮罩               │
│  └────────────┘                                       │
└──────────────────────────────────────────────────────┘
```

### 6.2 响应式断点

Lytalk 采用 **Mobile-First** 响应式策略，通过 CSS 媒体查询 + `useBreakpoint` composable 联动 JS 逻辑，一套代码自适应手机、平板、桌面、超宽屏。

| 断点 | 宽度 | 典型设备 | 布局行为 |
|------|------|---------|---------|
| **Mobile S** | ≤375px | iPhone SE / 小屏安卓 | 抽屉全屏覆盖（300px→0 translateX）；内容区内边距12px；聊天气泡最大宽80%；功能栏图标缩小到20px；顶栏标题字号14px；好友头像36px |
| **Mobile M/L** | 376px–767px | 主流手机（iPhone 14/15、安卓旗舰） | 抽屉覆盖式滑入（translateX 0 ↔ -100%）；内容区全宽单列；网格列数从3→2；顶栏左侧仅Logo按钮；底部输入栏按钮44px触控尺寸；菜单从左侧滑入全宽 |
| **Tablet** | 768px–1023px | iPad mini / 平板 | 抽屉默认展开（300px）；内容区自适应；便签/书籍/视频网格2列；好友详情/设置为居中卡片（max-width 720px） |
| **Desktop** | 1024px–1439px | 笔记本/小桌面 | 抽屉宽度动画收起/展开（width: 0 ↔ 300px）；内容区flex-1填充；网格3列；顶栏三按钮布局（折叠/居中好友信息/右侧操作）；侧边菜单260px滑入 |
| **Desktop L** | 1440px–1919px | 标准桌面显示器 | 聊天区最大宽度不限制，内容区内容卡片max-width 1200px居中；网格3–4列；便签瀑布流3列 |
| **4K / UltraWide** | ≥1920px | 2K/4K显示器/超宽屏 | 聊天区居中（max-width 1600px）；侧边留白；好友抽屉320px；便签瀑布流4列；T读/视频网格4–5列；字体基准15px |

#### 关键响应式策略

1. **抽屉行为切换**：
   - `>768px`：桌面模式，抽屉 `width` 过渡动画，收起时宽度为0，内容区 `flex:1` 自然扩展
   - `≤768px`：移动端，抽屉使用 `transform: translateX` 覆盖式滑入，内容区不动，遮罩层 `<Overlay>` 必现
2. **网格布局**：通过 CSS Grid 的 `auto-fill / minmax()` 实现自适应列数，无需 JS 介入
3. **触控适配**：移动端所有可点击元素最小尺寸 44×44px（符合 Apple HIG），间距增大
4. **字体缩放**：根 `font-size` 在大屏上不放大，通过组件内 `clamp()` 保证标题在各尺寸下的可读性
5. **侧边菜单**：
   - `>768px`：260px 从上而下滑入展开动画
   - `≤768px`：从上而下滑入展开动画
6. **聊天区**：
   - 移动端：消息气泡最大宽度 80%，输入栏固定在底部（`position: sticky` 不使用fixed避免键盘遮挡）
   - 桌面端：消息气泡最大宽度 60%，输入栏在聊天区内正常流布局
7. **`useBreakpoint` composable**：返回 `{ isMobile, isTablet, isDesktop }` 响应式布尔值，用于 JS 逻辑（如移动端默认折叠抽屉、关闭hover提示等）

---

## 七、组件规范

### 7.1 通用组件 Props 约定

所有通用组件使用 `v-bind="$attrs"` 透传属性，支持 `class`/`style` 合并。

**AppButton 变体：**

```vue
<AppButton variant="primary">发消息</AppButton>     <!-- mint绿渐变，默认 -->
<AppButton variant="secondary">取消</AppButton>    <!-- 白底绿边 -->
<AppButton variant="ghost" icon>🎤</AppButton>     <!-- 透明图标 -->
<AppButton variant="danger">删除好友</AppButton>   <!-- 红色 -->
<AppButton variant="fab" icon>+</AppButton>        <!-- 圆形浮动大按钮 -->
<AppButton variant="icon" size="sm">🔍</AppButton> <!-- 38px圆形图标 -->
```

**AppToggle 双向绑定：**

```vue
<AppToggle v-model="enabled" />
```

**AppAvatar 尺寸：**

```vue
<AppAvatar size="xs|sm|md|lg|xl|2xl" :status="'online'" :gradient="['#2DD4BF','#0D9488']">夏</AppAvatar>
```

### 7.2 Logo 按钮

- 尺寸：38×38px，圆角 12px
- 背景：`var(--gradient-primary)`
- 图标：logo.png
- 含扫一扫图标（扫描二维码）
  - 点击：打开浏览器扫描二维码
- 点击：触发 `toggleMenu()`（Pinia `useAppStore().toggleMenu()`）

### 7.3 侧边菜单（SideMenu）

- 宽度 260px，圆角 20px，毛玻璃
- 菜单项高 42px，padding 11px 14px，圆角 12px
- **菜单分组**：
  1. **畅聊**（内部路由）：消息、动态
  2. **生活**（内部路由）：便签、T读、T游、T听、T视、地图、生活
  2. **探索**（外部链接，新标签页打开）：琳凯蒂亚星球、iDrome、田语
  3. **工具箱**（内部路由）：天气、日历、时钟
  4. **操作**（内部路由）：添加好友（绿色高亮）、添加群聊（蓝色高亮）
  5. **底部**（内部路由，固定在底部）：设置、我的
- 激活项：通过 `useRoute().path` 与 router-link 的 `active-class` 自动匹配，点击菜单项后更新<RouterView>
- 分隔线：1px 薄荷绿浅色

### 7.4 好友列表项（FriendItem）

- 高度：64px，padding 0 15px
- 头像 48px 圆形 + 名称加粗 + 预览灰色小字 + 时间/未读徽章
- 选中态：薄荷绿浅背景 + 左侧 3px 绿色指示条
- 未读徽章：红色圆形白色数字（最多显示 99+）
- 分组可折叠（chevron 图标旋转）

### 7.5 页面 Header（PageHeader）

- 高度 60px，flex 布局
- 左侧：折叠箭头按钮（`toggleDrawer()`）
- 中间：标题（16px 加粗）+ 副标题（11px），绝对居中
- 右侧：作用域插槽 `#actions`，各页面自定义功能按钮

### 7.6 动态水波纹（Ripple Directive）

通过自定义指令 `v-ripple` 应用到任意可点击元素：

```vue
<AppButton v-ripple variant="primary">发送</AppButton>
```

点击时在元素内动态创建 span，从点击点 scale(0) → scale(3)，opacity 1 → 0，0.6s ease-out。

---

## 八、页面清单（Views）

项目共 **20 个路由视图**（1 个独立登录页 + 1个主应用好友列表页内视图 + 18 个主应用扩展功能内视图），位于 `src/views/` 目录。侧边菜单「探索」分组中的**琳凯蒂亚星球、iDrome、田语**为外部链接，不属于内部路由。

### 8.1 认证（参考consultation/login.html）
| # | 页面 | 路由 | 说明 |
|---|------|------|------|
| 1 | 登录页 | `/login` | Mint绿渐变背景，无白色面板，TT号+密码登录，5种第三方登录（手机/邮箱/QQ/微信/抖音），浮动气泡动画，@2026 Lytalk 青聊 页脚 |

### 8.2 核心聊天
| # | 页面 | 路由 | 说明 |
|---|------|------|------|
| 2 | 好友聊天（主页） | `/chat` | 两栏抽屉式，抽屉可折叠，底部输入框+纸飞机发送+圆形+号工具栏展开，消息气泡（对方白底/我方mint绿渐变），WebSocket消息 |
| 3 | 好友详情 | `/friend/:id` | 好友主页，240px薄荷绿封面，大头像+在线点，发消息/语音/视频按钮，个人资料，好友设置7项toggle，拉黑+删除好友 |
| 4 | 群聊详情 | `/group/:id` | 群信息+群公告，群成员6列网格（群主带👑），添加/移除，群设置9项，群主管理（公告/成员/转让/解散），退出群聊 |
| 5 | 个人主页 | `/profile` | 个人封面+大头像，数据统计（动态/关注/粉丝/获赞），资料5项，个人设置7项，复制Lytalk号，退出登录 |
| 6 | 添加好友 | `/add-friend` | 大搜索框+4个快捷入口，"可能认识的人"横向卡片，"新的朋友"请求列表（接受/已添加/等待验证），发现更多 |

### 8.3 内容功能
| # | 页面 | 路由 | 说明 |
|---|------|------|------|
| 7 | 便签 | `/notes` | 3列瀑布流彩色便签卡片（6种柔和色调），分类标签筛选，右上角新建+右下角FAB |
| 8 | 动态 | `/moments` | 居中信息流（朋友圈风格），点赞变红心弹跳动画，发布按钮 |
| 9 | T读 | `/read` | 电子书/新闻阅读，本周推荐Banner，继续阅读横向进度条，分类Tab，彩色封面+星级评分 |
| 10 | T游 | `/game` | 网页游戏中心，热门推荐Banner，最近在玩，游戏网格（emoji图标），hover绿色边框+"开始"按钮 |
| 11 | T听 | `/music` | 音乐/有声书，sticky迷你播放器（旋转专辑封面+进度条），彩色歌单，热门歌曲（EQ动效），有声书 |
| 12 | T视 | `/video` | 短视频发现页，2列瀑布流（错落比例），渐变封面+播放按钮，点击播放/暂停 |

### 8.4 工具箱
| # | 页面 | 路由 | 说明 |
|---|------|------|------|
| 13 | 天气 | `/weather` | 大薄荷绿渐变天气卡+装饰云朵SVG，24小时预报（当前高亮），7日预报（渐变温度条），生活指数网格 |
| 14 | 日历 | `/calendar` | 左右双栏，左侧月份日历（今天mint绿/周末红字/日程圆点），右侧日程面板（彩色左边框），快速添加 |
| 15 | 时钟 | `/clock` | 4个Tab：世界时钟（实时更新+昼夜判断）、闹钟（toggle）、秒表（10ms精度+计次）、倒计时（圆形SVG进度环） |

### 8.5 设置
| # | 页面 | 路由 | 说明 |
|---|------|------|------|
| 16 | 系统设置 | `/settings` | 6组设置：账号/通用（深色模式实时切换）/通知/聊天/关于/退出，toggle开关，@2026 Lytalk 青聊 页脚 |

---

## 九、全局交互规范

### 9.1 菜单交互
- 点击 Logo 按钮 → SideMenu 滑入（translateX 0.3s）+ Overlay 淡入
- 关闭方式：点击 Overlay / 点击关闭 × / 按 ESC 键 / 点击菜单项（路由跳转后自动关闭）
- 菜单项使用 `<router-link>`，active-class 自动高亮

### 9.2 抽屉交互
- 桌面端（>768px）：点击折叠箭头 → 抽屉宽度从 300px 平滑收起到 0，主面板扩展
- 移动端（≤768px）：抽屉 absolute 覆盖，从左侧 -110% 滑入到 0
- 箭头图标旋转 180° 反馈折叠状态

### 9.3 消息发送
- Enter 键或点击纸飞机按钮发送
- 我方消息：右侧，mint绿渐变气泡，入场 slideUp+fadeIn（Vue `<Transition>`）
- 输入框 `autoResize`（1-5行，通过 input 事件动态设置 height）
- 点击+号：工具栏展开/收起（max-height+opacity transition），+号旋转45°变×

### 9.4 时钟实时更新
- 使用 `useClock` composable，基于 `requestAnimationFrame` 或 `setInterval`
- 秒表 10ms 精度，支持计次（lap）
- 倒计时 SVG 圆环通过 `stroke-dasharray` + `stroke-dashoffset` 实现进度动画
- 世界时钟根据当地小时判断 ☀️(6-18点) / 🌙 图标

---

## 十、视觉语言（Visual Language）

### 10.1 动态视觉
- **背景呼吸**：页面背景 20s 循环光晕渐变位移（CSS `@keyframes bgShift`）
- **毛玻璃质感**：所有面板/抽屉/菜单使用 `backdrop-filter: blur()`，轻盈通透
- **渐变强调**：主按钮、封面、选中态使用 mint 绿渐变，避免纯色沉闷
- **微交互**：hover 上浮、v-ripple 涟漪、toggle 滑动、tab 下划线、卡片右移
- **柔和阴影**：阴影透明度控制在 0.03-0.12，避免厚重感
- **在线脉冲**：在线状态点 2s 呼吸动画（`@keyframes pulse`）

### 10.2 色彩搭配原则
- 主色（薄荷绿）仅用于：主按钮、激活态、强调元素、渐变
- 白色/浅灰承担：卡片背景、输入框、未选中态
- 彩色渐变色块用于：封面、歌单/书籍/游戏封面、头像背景（装饰性，不抢主色）
- 红色仅用于：危险操作（删除/退出/拉黑）、错误提示、周末日历色
- 黄色/橙色仅用于：警告、早时差、温度高温端

### 10.3 图标风格
- 全部使用 SVG 线性图标（stroke-width 2，stroke-linecap round，stroke-linejoin round）
- 图标尺寸统一 18-20px，与文字居中对齐
- 推荐图标库：[Lucide Vue Next](https://lucide.dev/)（按需引入）

---

## 十一、PWA 支持

- 通过 `vite-plugin-pwa` 自动生成 Service Worker
- `manifest.json` 配置：
  - `name`: "Lytalk"
  - `short_name`: "Lytalk"
  - `theme_color`: `#14B8A6`（mint 绿）
  - `background_color`: `#F0FDFA`
  - `display`: `standalone`
  - 图标尺寸：192×192、512×512
- 支持安装到桌面、离线缓存（`generateSW` 策略）
- 待完善：后台消息推送（Push API）

---

## 十二、部署方案

### 12.1 架构：静态托管 + BaaS

Lytalk 采用**前后端完全分离**部署：

| 层 | 服务 | 说明 |
|----|------|------|
| **前端静态资源** | GitHub Pages | `pnpm build` 产物 `dist/` 目录托管，免费、自带 CDN、自动 HTTPS、支持自定义域名 |
| **后端 / 数据库 / 实时 / 鉴权 / 文件** | Supabase Cloud | 托管于 Supabase 云（AWS 基础设施），API 通过 `https://<project-ref>.supabase.co` 访问，自带 SSL |
| **自定义域名** | GitHub Pages + CNAME | 通过 `CNAME` 文件绑定到 `lytalk.rincatian.top` 域名，DNS CNAME 指向 GitHub Pages |

前端所有数据请求均为 HTTPS 直连 Supabase，无需自建 Node 服务器，真正实现零运维。

### 12.2 GitHub Pages 部署

#### 方式一：GitHub Actions 自动部署（推荐）

在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v3
        with: { version: 9 }
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - run: pnpm build
        env:
          VITE_SUPABASE_URL: ${{ secrets.VITE_SUPABASE_URL }}
          VITE_SUPABASE_ANON_KEY: ${{ secrets.VITE_SUPABASE_ANON_KEY }}
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with: { path: ./dist }

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

配置步骤：
1. 在 GitHub 仓库 `Settings → Pages → Build and deployment → Source` 选择 **GitHub Actions**
2. 在 `Settings → Secrets and variables → Actions` 添加两个 Repository Secret：
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
3. 推送 `main` 分支自动触发构建与部署，产物地址：`https://<username>.github.io/<repo>/`

#### 方式二：手动构建部署

```bash
pnpm build
# 将 dist/ 目录内容推送到 gh-pages 分支
pnpm add -D gh-pages
# package.json 添加脚本："deploy": "gh-pages -d dist"
pnpm deploy
```

### 12.3 Vite 配置要点（GitHub Pages 子路径）

若部署在 `https://<username>.github.io/<repo>/` 子路径下，需在 `vite.config.ts` 配置 `base`：

```ts
// vite.config.ts
export default defineConfig({
  base: process.env.GITHUB_REPOSITORY
    ? '/' + process.env.GITHUB_REPOSITORY.split('/')[1] + '/'
    : '/',
  // ...其他配置
})
```

Vue Router 使用 `createWebHistory` 时需配合 404 回退：在 `public/` 下放置与 `index.html` 内容相同的 `404.html`，避免刷新子路由 404。

### 12.4 Supabase 生产环境配置

- **Authentication → URL Configuration**：
  - Site URL：`https://<username>.github.io/<repo>/`
  - Redirect URLs：添加 GitHub Pages 地址，用于第三方 OAuth 回调
- **Storage → CORS 配置**：允许 GitHub Pages 域名访问上传资源
- **RLS 策略**：生产环境必须启用 Row Level Security，禁止匿名写入
- **anon key 可公开**：`VITE_SUPABASE_ANON_KEY` 用于前端直连，本身是公开的，安全性由 RLS 策略保证
- **service_role key 严禁前端暴露**：仅用于 Edge Functions 和 CI 脚本

### 12.5 PWA 与 GitHub Pages 注意事项

- `vite-plugin-pwa` 的 `registerType: 'autoUpdate'` 在静态托管下开箱即用
- Service Worker 作用域需与 `base` 路径一致，插件自动处理
- GitHub Pages 默认启用强缓存，建议在 `vite.config.ts` 中对 `sw.js` 和 `workbox-*.js` 设置 `cacheControl: 'no-cache'`（通过插件的 `manifestTransforms` 或文件名哈希解决）

### 12.6 环境划分

| 环境 | 前端地址 | Supabase 项目 | 用途 |
|------|---------|--------------|------|
| 本地开发 | `http://localhost:5173` | Supabase 开发项目 | 功能开发 |
| Preview PR | Vercel/Cloudflare Pages Preview | 开发项目 | PR 预览 |
| 生产 | `https://<username>.github.io/lytalk/` | Supabase 生产项目 | 用户访问 |

---

## 十三、开发规范

### 13.1 组件命名
- 页面视图：`Xxx.vue`（PascalCase）
- 通用组件：`AppXxx.vue`（App 前缀，避免与 HTML 元素冲突）
- 业务组件：按模块目录组织（如 `chat/MessageBubble.vue`）
- Composables：`useXxx.ts`（use 前缀）
- Stores：`xxx.ts`（按功能命名，useXxxStore 导出）

### 13.2 样式规范
- 优先使用 CSS Variables（来自 `variables.css`），禁止硬编码颜色值
- 组件样式使用 `<style scoped>`
- 全局样式（reset/工具类/动画）放 `src/styles/`
- BEM 命名约定：`.block__element--modifier`（或 scoped + 语义 class）

### 13.3 Git 提交规范（Conventional Commits）

```
feat: 新功能
fix: 修复bug
docs: 文档更新
style: 样式调整（不影响逻辑）
refactor: 重构
perf: 性能优化
test: 测试
chore: 构建/工具链
```

---

## 十四、后续迭代方向

- [ ] 深色模式全局适配（settings页toggle框架已就绪）
- [ ] WebSocket 实时消息接入
- [ ] WebRTC 音视频通话
- [ ] 文件传输/图片预览组件
- [ ] Emoji 选择面板
- [ ] Service Worker 离线缓存完善
- [ ] 移动端手势操作（Swipe 打开抽屉）
- [ ] 全局消息搜索
- [ ] 国际化（i18n）
- [ ] 单元测试（Vitest）+ E2E（Playwright）

---

## 十五、数据安全与隐私保护

Lytalk 采用「前端静态托管 + Supabase 后端即服务」的架构：前端 `dist/` 构建产物部署在 GitHub Pages（公开仓库或私有仓库 + Pages），所有用户数据、账户、消息均存储在 Supabase（PostgreSQL + Auth + Storage + Realtime）。本章明确数据安全与隐私保护策略，确保前端代码不含任何敏感信息，用户数据得到充分保护。

### 15.1 敏感信息分级

| 级别 | 内容 | 存放位置 | 是否可进入前端代码 |
|------|------|---------|-------------------|
| **公开（Public）** | Supabase URL、Supabase `anon` public key、PWA 配置、静态资源 | `.env` → `VITE_SUPABASE_URL` / `VITE_SUPABASE_ANON_KEY` → 打入前端 bundle | ✅ 可以（设计为公开） |
| **机密（Secret）** | Supabase `service_role` key、数据库连接串、Edge Function 密钥、第三方 OAuth 客户端 Secret | **仅** 存于 Supabase Dashboard / 后端环境变量 / GitHub Actions Secrets | ❌ 绝对禁止进入前端代码与 Git 仓库 |
| **用户隐私（Private User Data）** | 聊天记录、好友关系、个人资料、便签、钱包余额、位置信息、支付记录 | Supabase PostgreSQL（受 RLS 保护），用户客户端经 JWT 鉴权访问 | ❌ 前端只在用户本人会话内按需请求，不持久化敏感明文 |

### 15.2 前端代码安全规范

**严格禁止以下内容出现在前端代码（包括 `src/`、`public/`、HTML 设计稿、`.env` 提交到 Git 的文件）中：**

1. **Supabase Service Role Key**（`service_role`）：该密钥绕过所有 RLS 策略，拥有数据库完全读写权限，**永远不得出现在前端代码、浏览器环境、Git 历史中**
2. **数据库连接字符串**（`postgresql://postgres:***@db.xxx.supabase.co:5432/postgres`）：仅用于后端/CI 迁移
3. **第三方 API Secret**：微信/QQ/抖音 OAuth AppSecret、支付商户密钥、短信/邮件服务 API Key
4. **JWT Secret** 或任何自定义签名密钥
5. **硬编码的用户隐私数据**：测试账号密码、真实手机号、身份证号、银行卡号
6. **内网地址、调试后门、未关闭的 mock 管理员账号**

**代码级防护措施：**

- 所有环境变量通过 `import.meta.env.VITE_XXX` 注入，仅以 `VITE_` 开头的变量才会被 Vite 暴露给客户端
- `.env`（含真实 key）加入 `.gitignore`，仅提交 `.env.example`（含占位符）
- CI/CD（GitHub Actions）通过 `secrets.VITE_SUPABASE_URL` / `secrets.VITE_SUPABASE_ANON_KEY` 在构建时注入，构建日志自动屏蔽
- ESLint 规则 `no-secrets` / 自定义检测脚本，阻止类似密钥格式的字符串提交
- Code Review Checklist 必须包含「是否引入了新的 Secret」一项

### 15.3 Supabase 安全配置

**行级安全策略（RLS - Row Level Security）**：

所有用户数据表必须启用 RLS，禁止无策略访问（默认拒绝）：

```sql
-- 示例：用户只能读写自己的消息
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;

CREATE POLICY "用户只能读自己参与的会话消息"
  ON messages FOR SELECT
  USING (auth.uid() = sender_id OR auth.uid() = receiver_id);

CREATE POLICY "用户只能发送自己作为sender的消息"
  ON messages FOR INSERT
  WITH CHECK (auth.uid() = sender_id);
```

**认证安全**：

- JWT 过期时间默认 1 小时，配合 Refresh Token 自动续期
- 密码强度策略（最少 8 位、包含数字+字母）
- 第三方 OAuth 仅请求必要 scope（头像、昵称）
- 邮箱/手机号验证后才可使用全部功能
- 登录速率限制（Supabase Auth 默认启用）

**Storage 安全**：

- 用户上传文件（头像、聊天图片）存放在独立 bucket，开启 RLS
- 文件路径包含 `user_id` 前缀，仅本人可写
- 通过签名 URL（signed URL）访问私密文件，默认不公开
- 限制上传文件类型与大小（图片 ≤ 10MB，视频 ≤ 100MB）

**Realtime 安全**：

- Realtime Channels 受 RLS 约束，用户只能订阅自己有权限读取的表变更
- 禁止广播敏感系统事件到公开频道

### 15.4 GitHub Pages 部署安全

- 前端构建产物为纯静态文件（HTML/CSS/JS），不包含任何服务端逻辑
- GitHub Pages 仅承载静态资源，所有数据请求直连 Supabase（HTTPS + WSS）
- 启用 GitHub Dependabot 自动更新依赖，修复 npm 包安全漏洞
- 生产环境 `vite.config.ts` 中关闭 sourcemap（避免源码泄露），或仅上传 hidden sourcemap 到错误监控平台
- CSP（Content-Security-Policy）响应头限制脚本/样式/图片来源：
  ```
  Content-Security-Policy: default-src 'self';
    connect-src 'self' https://*.supabase.co wss://*.supabase.co;
    img-src 'self' data: https://*.supabase.co blob:;
    style-src 'self' 'unsafe-inline';
  ```

### 15.5 数据传输与存储加密

| 环节 | 加密方式 |
|------|---------|
| 浏览器 ↔ Supabase API | TLS 1.3（HTTPS） |
| 浏览器 ↔ Supabase Realtime | WSS（WebSocket over TLS） |
| 数据库静态存储 | Supabase 托管自动加密（At-Rest Encryption, AES-256） |
| 客户端敏感状态 | 仅内存保存，不写入 localStorage（除用户显式勾选"记住我"的 Refresh Token） |
| 密码存储 | Supabase Auth 使用 bcrypt/Argon2 单向哈希，前端永不传输明文密码以外的内容 |

### 15.6 用户隐私控制

- **数据最小化**：注册仅收集必要信息（TT号+密码），昵称、头像等为可选
- **本地数据控制**：
  - localStorage 仅存储主题偏好、登录会话标识，不存聊天记录明文
  - 提供「清除本地缓存」按钮（设置页）
- **用户权利**（符合 GDPR/个人信息保护法）：
  - 支持导出个人数据（Supabase Edge Function 打包用户相关记录为 JSON）
  - 支持注销账号并请求删除个人数据
- **日志策略**：不记录敏感消息内容到任何前端日志系统，错误监控脱敏（Sentry `beforeSend` 过滤 PII）

### 15.7 安全审计 Checklist（发布前必查）

- [ ] `.gitignore` 包含 `.env`、`.env.local`、`.env.production`
- [ ] 仓库代码中 grep 搜索 `service_role`、`postgres:`、`password` 等敏感关键字，无命中
- [ ] 所有 Supabase 表已启用 RLS 并配置最小权限策略
- [ ] Supabase Auth 未启用匿名注册（如需使用需明确策略）
- [ ] Storage bucket 为私有访问，仅通过签名 URL 分发
- [ ] 生产构建 bundle 中不包含 sourcemap（或已上传到私有错误监控）
- [ ] GitHub Actions 日志审查，无 Secret 泄露
- [ ] 第三方依赖已运行 `pnpm audit`，无 critical/high 级别漏洞
- [ ] CSP 头已配置并在生产环境生效
- [ ] 测试账号已从数据库清除，无默认 admin 账号

---

©2026 Lytalk 青聊 · 轻盈沟通，自在生活
