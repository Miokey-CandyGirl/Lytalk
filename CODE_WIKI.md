# Lytalk 青聊 - Code Wiki

> 轻盈沟通，自在生活 —— 一款简约、现代、小清新的即时通讯 Web 应用

©2026 Lytalk 青聊 · 轻盈沟通，自在生活

---

## 目录

1. [项目概述](#项目概述)
2. [技术栈与依赖](#技术栈与依赖)
3. [项目架构](#项目架构)
4. [目录结构详解](#目录结构详解)
5. [设计系统与样式规范](#设计系统与样式规范)
6. [核心页面模块详解](#核心页面模块详解)
7. [关键功能与交互](#关键功能与交互)
8. [Vue组件架构规划](#vue组件架构规划)
9. [运行与开发指南](#运行与开发指南)
10. [数据模型设计](#数据模型设计)
11. [开发路线图](#开发路线图)

---

## 项目概述

### 项目简介

**Lytalk（青聊）** 是一款设计精美的即时通讯 Web 应用，采用小清新的设计风格，以青绿色(#14B8A6)为主色调，融合毛玻璃效果、圆角卡片、流畅动画等现代UI设计元素。项目目前处于高保真原型阶段，包含完整的UI设计和交互逻辑实现。

### 核心特性

- 💬 **即时通讯** - 单聊、群聊、消息收发、图片消息
- 👥 **社交系统** - 好友管理、分组、特别关心、动态朋友圈
- 🎮 **娱乐功能** - T听(音乐)、T视(视频)、T游(游戏)、T读(阅读)
- 🔧 **生活工具** - 天气、日历、时钟、便签、地图
- ⚙️ **系统设置** - 账号管理、隐私设置、主题切换(含暗色模式)
- 🎨 **精美UI** - 毛玻璃效果、流畅动画、响应式设计

### 项目状态

- **当前阶段**: 高保真HTML原型
- **Vue版本**: 框架结构已搭建，组件待实现
- **构建工具**: Vite
- **浏览器支持**: 现代浏览器 (Chrome, Firefox, Safari, Edge)

---

## 技术栈与依赖

### 前端技术栈

| 技术 | 用途 | 状态 |
|------|------|------|
| Vue 3 | 前端框架 | 计划中 |
| Vite | 构建工具 | 已配置 |
| HTML5 | 页面结构 | ✅ 已实现 |
| CSS3 | 样式 (CSS Variables, Flexbox, Grid, Backdrop Filter) | ✅ 已实现 |
| Vanilla JavaScript | 交互逻辑 | ✅ 已实现 |
| SVG Icons | 图标系统 | ✅ 已实现 |

### CSS 特性使用

- **CSS Variables**: 主题色、圆角、过渡动画统一管理
- **Backdrop Filter**: 毛玻璃效果 (`backdrop-filter: blur()`)
- **Flexbox/Grid**: 响应式布局
- **CSS Animations**: 涟漪效果、消息进入动画、背景渐变动画
- **Media Queries**: 移动端适配
- **prefers-reduced-motion**: 无障碍动画支持

### 设计Token (CSS变量)

```css
:root {
  --drawer-w: 300px;           /* 好友列表宽度 */
  --menu-w: 260px;             /* 侧边菜单宽度 */
  --primary: #14B8A6;          /* 主色调 */
  --primary-600: #0D9488;      /* 主色深色 */
  --primary-400: #2DD4BF;      /* 主色浅色 */
  --primary-300: #5EEAD4;      /* 主色更浅 */
  --primary-100: #CCFBF1;      /* 主色背景 */
  --primary-50: #F0FDFA;       /* 主色浅背景 */
  --danger: #EF4444;           /* 危险色 */
  --radius: 12px;              /* 标准圆角 */
  --radius-lg: 20px;           /* 大圆角 */
  --transition: 0.35s cubic-bezier(0.4, 0, 0.2, 1);  /* 标准过渡 */
}
```

---

## 项目架构

### 整体架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                        Lytalk Application                        │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    Side Menu (侧边菜单)                   │   │
│  │  消息 | 便签 | 动态 | T读 | T游 | T听 | T视 | 生活...      │   │
│  └─────────────────────────────────────────────────────────┘   │
│  ┌──────────────┐  ┌────────────────────────────────────────┐  │
│  │              │  │            Content Area                │  │
│  │   Friend     │  │  ┌──────────────────────────────────┐  │  │
│  │   Drawer     │  │  │        Chat Header               │  │  │
│  │   (好友列表)  │  │  ├──────────────────────────────────┤  │  │
│  │              │  │  │                                  │  │  │
│  │ - 搜索栏     │  │  │        Message Area              │  │  │
│  │ - 自己       │  │  │        (消息区域)                 │  │  │
│  │ - 特别关心   │  │  │                                  │  │  │
│  │ - 群聊       │  │  ├──────────────────────────────────┤  │  │
│  │ - 家人       │  │  │        Chat Footer               │  │  │
│  │ - 我的好友   │  │  │  (工具栏 + 输入框 + 发送按钮)      │  │  │
│  │              │  │  └──────────────────────────────────┘  │  │
│  └──────────────┘  └────────────────────────────────────────┘  │
│                                                                 │
│                      Gradient Background                        │
└─────────────────────────────────────────────────────────────────┘
```

### 三层布局结构

1. **侧边菜单层 (z-index: 25)** - 主导航菜单，从左侧滑出
2. **遮罩层 (z-index: 20)** - 菜单打开时的半透明遮罩
3. **好友列表层 (z-index: 10)** - 左侧好友/会话列表抽屉
4. **主内容层 (z-index: 5)** - 聊天区或各功能模块内容
5. **背景层 (z-index: 0)** - 渐变背景 + 径向光效动画

---

## 目录结构详解

```
/workspace/
├── consultation/              # 📱 HTML高保真原型 (18个页面)
│   ├── login.html            # 登录/注册页
│   ├── auth.html             # 认证页
│   ├── home.html             # 主页/聊天页
│   ├── friend-detail.html    # 好友详情页
│   ├── group-detail.html     # 群聊详情页
│   ├── add-friend.html       # 添加好友页
│   ├── my-profile.html       # 个人资料页
│   ├── settings.html         # 设置页
│   ├── moments.html          # 动态/朋友圈页
│   ├── notes.html            # 便签页
│   ├── read.html             # T读(阅读)页
│   ├── game.html             # T游(游戏)页
│   ├── music.html            # T听(音乐)页
│   ├── video.html            # T视(视频)页
│   ├── life.html             # 生活服务页
│   ├── weather.html          # 天气页
│   ├── calendar.html         # 日历页
│   └── clock.html            # 时钟页
│
├── src/                       # 📦 Vue源码目录
│   ├── assets/
│   │   └── images/
│   │       └── logo.png       # 应用Logo
│   ├── views/
│   │   ├── Login.vue          # 登录页面组件 (待实现)
│   │   ├── Friends.vue        # 好友/会话列表页 (待实现)
│   │   ├── modules/           # 功能模块组件
│   │   │   ├── Chat.vue       # 聊天模块
│   │   │   ├── Clock.vue      # 时钟模块
│   │   │   ├── TCalender.vue  # 日历模块
│   │   │   ├── TGame.vue      # 游戏模块
│   │   │   ├── TLife.vue      # 生活模块
│   │   │   ├── TMap.vue       # 地图模块
│   │   │   ├── TMoments.vue   # 动态模块
│   │   │   ├── TMusic.vue     # 音乐模块
│   │   │   ├── TNotes.vue     # 便签模块
│   │   │   ├── TRead.vue      # 阅读模块
│   │   │   ├── TVideo.vue     # 视频模块
│   │   │   └── TWeather.vue   # 天气模块
│   │   └── system/            # 系统页面组件
│   │       ├── AddFriends.vue # 添加好友
│   │       ├── AddGroup.vue   # 添加群组
│   │       ├── FriendDetail.vue  # 好友详情
│   │       ├── GroupDetail.vue   # 群组详情
│   │       ├── MyProfile.vue     # 个人资料
│   │       └── Settings.vue      # 设置
│   ├── App.vue                # 根组件 (待实现)
│   └── main.js                # 应用入口 (待实现)
│
├── index.html                 # 🚪 Vite入口HTML
├── vite.config.js             # ⚡ Vite配置 (空)
├── .env                       # 环境变量
├── .env.example               # 环境变量示例
└── README.md                  # 项目说明
```

---

## 设计系统与样式规范

### 色彩体系

#### 主色调
| 色值 | 用途 |
|------|------|
| `#14B8A6` | 主色、按钮、强调 |
| `#0D9488` | 深色主色、悬停状态 |
| `#2DD4BF` | 浅色主色、渐变 |
| `#5EEAD4` | 更浅、高亮 |
| `#CCFBF1` | 背景色 |
| `#F0FDFA` | 最浅背景 |

#### 状态色
| 色值 | 用途 |
|------|------|
| `#34D399` | 在线状态 (绿色) |
| `#FBBF24` | 离开状态 (黄色) |
| `#F87171` | 忙碌状态 (红色) |
| `#DBDBDB` | 离线状态 (灰色) |
| `#EF4444` | 未读徽章、危险操作 |

#### 头像渐变色系
为不同用户/群组分配预设渐变色：
- 小夏、老爸: `linear-gradient(135deg, #5EEAD4, #14B8A6)` (青绿)
- 阿杰、老妈: `linear-gradient(135deg, #FCD34D, #F59E0B)` (金黄)
- 婷婷、老弟、家庭群: `linear-gradient(135deg, #F472B6, #EC4899)` (粉红)
- 前端开发群、小王: `linear-gradient(135deg, #60A5FA, #3B82F6)` (蓝色)
- 产品讨论组、小张: `linear-gradient(135deg, #A78BFA, #8B5CF6)` (紫色)
- 小明: `linear-gradient(135deg, #6EE7B7, #10B981)` (翠绿)
- 小李: `linear-gradient(135deg, #FCA5A5, #EF4444)` (红色)
- 小赵: `linear-gradient(135deg, #FDE68A, #D97706)` (橙黄)

### 圆角规范

| 类名 | 尺寸 | 用途 |
|------|------|------|
| `--radius` | 12px | 按钮、头像、输入框 |
| `--radius-lg` | 20px | 卡片、面板、容器 |
| 圆形 | 50% | 发送按钮、用户头像(小) |
| 特殊 | 16px 16px 16px 4px | 对方消息气泡 |
| 特殊 | 16px 16px 4px 16px | 我的消息气泡 |

### 阴影规范

```css
/* 主按钮阴影 */
box-shadow: 0 3px 10px rgba(20,184,166,0.25);

/* 面板阴影 */
box-shadow: 0 4px 24px rgba(20,184,166,0.08);

/* 菜单阴影 */
box-shadow: 0 8px 32px rgba(20,184,166,0.15);

/* 悬停阴影增强 */
box-shadow: 0 6px 18px rgba(20,184,166,0.35);
```

### 动画效果

#### 页面级动画
- `bgShift` (20s): 背景渐变呼吸效果
- `gradientShift` (15s): 登录页渐变背景流动
- `auroraDrift` (20-25s): 极光效果漂移
- `bubbleFloat1~7` (8-14s): 气泡浮动动画

#### 组件级动画
- `msgIn` (0.35s): 消息进入动画 (淡入+上移+缩放)
- `pulse` (2s): 在线状态脉冲
- `scanLineMove` (2s): 扫一扫扫描线
- `rippleAnim` (0.5-0.6s): 按钮点击涟漪效果
- `successPop` (0.5s): 成功图标弹出

#### 过渡动画
- 侧边菜单滑入/滑出: `transform 0.35s cubic-bezier(0.4, 0, 0.2, 1)`
- 面板折叠/展开: `width/opacity/margin 0.35s`
- 工具栏展开: `max-height 0.3s cubic-bezier`

---

## 核心页面模块详解

### 1. 登录/注册页 ([login.html](file:///workspace/consultation/login.html))

**功能概要**: 用户身份认证入口

**主要UI组件**:
- 动画渐变背景 + 极光效果 + 浮动气泡 + 上升粒子
- 毛玻璃登录卡片
- TT号/密码输入框(带图标、密码显示切换)
- "记住我"复选框 + "忘记密码"链接
- 主登录按钮(shimmer效果)
- 5种第三方登录: 手机号、邮箱、QQ、微信、抖音
- 一键注册功能(自动生成TT号和密码弹窗)
- 注册成功弹窗(凭证展示、复制功能、安全提示)

**关键JavaScript函数**:

| 函数名 | 功能 |
|--------|------|
| `createRipple(e)` | 为按钮创建点击涟漪效果 |
| `randomTtid()` | 生成6位TT号(100000-999999) |
| `randomPassword(len)` | 生成包含字母/数字/符号的随机密码 |
| `showToast(msg)` | 显示顶部Toast提示 |
| `openModal()` | 打开注册成功弹窗 |
| `closeModal()` | 关闭弹窗 |

---

### 2. 主页/聊天页 ([home.html](file:///workspace/consultation/home.html))

**功能概要**: 核心聊天界面

**布局结构**:
1. **侧边菜单**: 应用主导航
2. **好友列表抽屉**: 
   - 顶部: Logo按钮(菜单) + 扫一扫 + 搜索栏
   - 列表: 自己 → 特别关心(2人) → 群聊(3个) → 家人(3人) → 我的好友(6人)
   - 每项: 头像(在线状态点) + 名字 + 时间 + 消息预览 + 未读徽章
3. **聊天区域**:
   - 头部: 收起按钮 + 居中的好友信息(头像+名字+在线状态) + 操作按钮
   - 消息区: 时间分隔、系统消息、对方/我的消息(文本/图片)
   - 底部: 展开式工具栏(表情/图片/文件等) + 输入框 + +号按钮 + 发送按钮

**交互功能**:
- 侧边菜单开合
- 好友列表折叠/展开
- 分组折叠(checkbox hack)
- 扫一扫弹窗
- 消息发送
- 工具栏展开/收起
- 移动端抽屉式侧滑

---

### 3. 设置页 ([settings.html](file:///workspace/consultation/settings.html))

**功能概要**: 应用设置中心

**设置分组**:
1. **账号信息卡片**: 头像 + 用户名 + TT号
2. **账号安全**: 修改密码、登录设备管理
3. **通知设置**: 消息通知、声音、震动
4. **隐私设置**: 好友验证、黑名单、在线状态
5. **通用设置**:
   - 深色模式切换(完整暗色主题已实现)
   - 语言设置
   - 字体大小
   - 聊天背景
6. **关于**: 版本信息、用户协议、隐私政策
7. **退出登录**: 红色边框危险按钮

**特色**: 完整的暗色模式CSS变量覆盖

---

### 4. 动态/朋友圈页 ([moments.html](file:///workspace/consultation/moments.html))

**功能概要**: 社交动态分享

**主要组件**:
- 封面图 + 个人信息头部
- 发布动态入口
- 动态列表:
  - 用户头像、昵称、时间
  - 文字内容、图片九宫格
  - 点赞、评论、分享按钮
- 评论区展开

---

### 5. 其他功能模块

| 页面 | 文件 | 核心功能 |
|------|------|----------|
| 好友详情 | [friend-detail.html](file:///workspace/consultation/friend-detail.html) | 资料查看、发消息、音视频通话、备注设置 |
| 群聊详情 | [group-detail.html](file:///workspace/consultation/group-detail.html) | 群资料、成员列表、群公告、群设置 |
| 添加好友 | [add-friend.html](file:///workspace/consultation/add-friend.html) | 搜索、扫一扫、好友推荐、好友请求列表 |
| 个人资料 | [my-profile.html](file:///workspace/consultation/my-profile.html) | 头像、昵称、个性签名、二维码、资料编辑 |
| 便签 | [notes.html](file:///workspace/consultation/notes.html) | 笔记列表、新建/编辑/删除、分类、搜索 |
| T读 | [read.html](file:///workspace/consultation/read.html) | 电子书阅读器、书架、阅读进度 |
| T游 | [game.html](file:///workspace/consultation/game.html) | 小游戏中心、游戏列表 |
| T听 | [music.html](file:///workspace/consultation/music.html) | 音乐播放器、歌单、播放控制 |
| T视 | [video.html](file:///workspace/consultation/video.html) | 视频播放、视频列表 |
| 生活 | [life.html](file:///workspace/consultation/life.html) | 生活服务聚合入口 |
| 天气 | [weather.html](file:///workspace/consultation/weather.html) | 当前天气、预报、城市切换 |
| 日历 | [calendar.html](file:///workspace/consultation/calendar.html) | 月历视图、日程管理、提醒 |
| 时钟 | [clock.html](file:///workspace/consultation/clock.html) | 世界时钟、闹钟、秒表、计时器 |

---

## 关键功能与交互

### 1. 涟漪效果 (Ripple)

**实现位置**: 所有按钮共用

```javascript
function createRipple(e) {
  const btn = e.currentTarget;
  const rect = btn.getBoundingClientRect();
  const size = Math.max(rect.width, rect.height);
  const x = e.clientX - rect.left - size / 2;
  const y = e.clientY - rect.top - size / 2;

  const ripple = document.createElement('span');
  ripple.className = 'ripple';
  ripple.style.width = ripple.style.height = size + 'px';
  ripple.style.left = x + 'px';
  ripple.style.top = y + 'px';
  btn.appendChild(ripple);

  setTimeout(() => ripple.remove(), 600);
}
```

### 2. 好友在线状态

**状态类型**:
- 在线 (`.on`): `#34D399` 绿色 + pulse动画
- 离开 (`.aw`): `#FBBF24` 黄色
- 忙碌 (`.by`): `#F87171` 红色
- 离线 (`.off`): `#DBDBDB` 灰色

### 3. 消息气泡设计

- **对方消息**: 左对齐，白色背景，圆角 `16px 16px 16px 4px`
- **我的消息**: 右对齐，青绿渐变背景，圆角 `16px 16px 4px 16px`
- **消息动画**: `msgIn` 淡入+上移+轻微缩放

### 4. 响应式断点

- **桌面端 (>768px)**: 三栏布局(菜单+抽屉+聊天)
- **移动端 (≤768px)**: 
  - 好友列表变为覆盖式抽屉
  - 侧边菜单适配全屏
  - 聊天区域边距调整

### 5. 无障碍支持

- `prefers-reduced-motion` 媒体查询: 禁用动画
- 按钮 `aria-label` 属性
- 语义化HTML结构

---

## Vue组件架构规划

### 组件层级

```
App.vue
├── AuthLayout
│   └── Login.vue
└── MainLayout
    ├── SideMenu.vue
    ├── FriendDrawer.vue
    │   ├── SearchBar.vue
    │   ├── FriendGroup.vue
    │   └── FriendItem.vue
    └── <router-view />
        ├── ChatView (Chat.vue)
        │   ├── ChatHeader.vue
        │   ├── MessageList.vue
        │   │   ├── MessageBubble.vue
        │   │   └── ImageMessage.vue
        │   └── ChatInput.vue
        │       ├── Toolbar.vue
        │       └── EmojiPicker.vue
        ├── MomentsView (TMoments.vue)
        ├── NotesView (TNotes.vue)
        ├── MusicView (TMusic.vue)
        ├── VideoView (TVideo.vue)
        ├── GameView (TGame.vue)
        ├── ReadView (TRead.vue)
        ├── WeatherView (TWeather.vue)
        ├── CalendarView (TCalender.vue)
        ├── ClockView (Clock.vue)
        ├── LifeView (TLife.vue)
        ├── MapView (TMap.vue)
        ├── FriendDetailView (FriendDetail.vue)
        ├── GroupDetailView (GroupDetail.vue)
        ├── AddFriendView (AddFriends.vue)
        ├── AddGroupView (AddGroup.vue)
        ├── ProfileView (MyProfile.vue)
        └── SettingsView (Settings.vue)
            ├── DarkModeToggle.vue
            └── SettingRow.vue
```

### 推荐新增依赖

```json
{
  "dependencies": {
    "vue": "^3.4.x",
    "vue-router": "^4.x",
    "pinia": "^2.x",
    "emoji-mart-vue-fast": "^15.x"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.x",
    "vite": "^5.x",
    "sass": "^1.x"
  }
}
```

### 建议的Vuex/Pinia状态模块

```
stores/
├── user.js          # 用户信息、登录状态
├── friends.js       # 好友列表、分组、在线状态
├── chats.js         # 会话列表、消息记录
├── settings.js      # 主题、语言、通知设置
└── ui.js            # 菜单开合、抽屉状态等UI状态
```

---

## 运行与开发指南

### 原型预览 (当前状态)

所有HTML原型可直接在浏览器中打开查看:

```bash
# 方式1: 直接打开
在浏览器中打开 /workspace/consultation/login.html

# 方式2: 使用本地HTTP服务器 (推荐)
cd /workspace
python3 -m http.server 8080
# 然后访问 http://localhost:8080/consultation/login.html

# 或使用npx serve
npx serve .
```

### Vite + Vue 开发 (未来)

```bash
# 1. 初始化package.json (需补充)
npm init -y

# 2. 安装依赖
npm install vue@3 vue-router@4 pinia
npm install -D vite @vitejs/plugin-vue

# 3. 配置vite.config.js
# 4. 实现src/main.js和src/App.vue
# 5. 启动开发服务器
npm run dev

# 6. 构建生产版本
npm run build
```

### 页面导航流

```
login.html
    ↓ (登录成功)
home.html ←→ friend-detail.html
    ↑↓           ↑↓
moments.html  group-detail.html
notes.html
read.html
game.html
music.html    ←→ add-friend.html
video.html
life.html     ←→ my-profile.html
weather.html
calendar.html ←→ settings.html
clock.html
```

---

## 数据模型设计

### 用户 (User)

```javascript
{
  ttid: "123456",           // TT号 (唯一标识)
  nickname: "小夏",          // 昵称
  avatar: "",               // 头像URL或渐变色
  signature: "",            // 个性签名
  status: "online",         // online | away | busy | offline
  gender: "female",         // male | female | unknown
  region: "",               // 地区
  birthday: "",             // 生日
  isStar: true,             // 是否特别关心
  groupId: "1",             // 所在分组ID
  remark: ""                // 好友备注
}
```

### 消息 (Message)

```javascript
{
  id: "msg_001",
  chatId: "user_123456",    // 会话ID (用户TT号或群ID)
  senderId: "123456",       // 发送者TT号
  type: "text",             // text | image | file | system | time
  content: "周末一起去看电影吗？",
  timestamp: 1718948400000, // 时间戳
  isRead: true,             // 是否已读
  status: "sent"            // sending | sent | failed
}
```

### 会话 (Chat)

```javascript
{
  id: "user_123456",        // user_xxx 或 group_xxx
  type: "private",          // private | group
  targetId: "123456",       // 对方TT号或群ID
  name: "小夏",             // 显示名称
  avatar: "",               // 头像
  lastMessage: "周末一起去看电影吗？",
  lastTime: "10:32",
  unreadCount: 2,
  isMuted: false,           // 是否免打扰
  isPinned: false           // 是否置顶
}
```

### 群组 (Group)

```javascript
{
  id: "group_dev",
  name: "前端开发群",
  avatar: "",
  memberCount: 28,
  members: [],              // 成员TT号列表
  ownerId: "123456",        // 群主TT号
  notice: "",               // 群公告
  isMuted: false
}
```

### 动态 (Moment)

```javascript
{
  id: "moment_001",
  userId: "123456",
  content: "今天天气真好~",
  images: [],               // 图片URL数组
  location: "",
  likes: [],                // 点赞用户ID列表
  comments: [
    { userId: "", content: "", timestamp: "" }
  ],
  timestamp: 1718948400000
}
```

---

## 开发路线图

### Phase 1: 基础框架搭建
- [ ] 完善 `package.json` 和依赖配置
- [ ] 实现 `vite.config.js`
- [ ] 实现 `src/main.js` (Vue初始化、路由、Pinia)
- [ ] 实现 `src/App.vue` (根布局)
- [ ] 提取公共CSS (reset、variables、animations)

### Phase 2: 核心功能实现
- [ ] 登录/注册页面 Vue 化
- [ ] 主布局框架 (侧边菜单 + 抽屉 + 内容区)
- [ ] 好友列表组件
- [ ] 聊天功能核心 (消息列表、发送消息)
- [ ] 路由配置和页面导航

### Phase 3: 功能模块完善
- [ ] 个人资料与设置页
- [ ] 添加好友/群组功能
- [ ] 好友/群聊详情页
- [ ] 动态/朋友圈模块
- [ ] 便签功能

### Phase 4: 娱乐与工具模块
- [ ] T听(音乐)播放器
- [ ] T视(视频)播放器
- [ ] T读(阅读)模块
- [ ] 天气模块
- [ ] 日历与时钟
- [ ] 生活服务聚合

### Phase 5: 优化与发布
- [ ] 暗色模式完整实现
- [ ] 响应式移动端适配
- [ ] 动画与交互优化
- [ ] WebSocket实时通讯对接
- [ ] 后端API对接
- [ ] PWA支持

---

## 设计资源与规范参考

### 图标系统
- 统一使用 Feather Icons 风格的SVG图标
- 线宽: 2px (默认), 2.5px (强调), 3px (小圆点)
- 尺寸: 18px (菜单项), 20px (Logo), 14-17px (按钮内)
- 颜色: 继承 `currentColor`

### 字体栈
```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", 
             "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
```

### 页面跳转
- 原型中使用 `onclick="window.location.href='xxx.html'"` 进行页面跳转
- Vue版本需使用 `<router-link>` 或 `router.push()`

---

## 版权信息

© 2026 Lytalk 青聊  
© 2025-2026 琳凯蒂亚星球

*本文档最后更新: 2026-07-16*
