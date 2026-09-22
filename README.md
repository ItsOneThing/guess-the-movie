# 🎬 看图猜电影 · Indovina il film

一个为朋友聚会、活动和课堂小游戏设计的**双语互动电影猜谜游戏**。

选择电影主题后，系统会随机抽取电影剧照。玩家根据图片猜电影名称，再揭晓答案。

项目支持 **中文 🇨🇳 / Italiano 🇮🇹**，并内置一个基于 Firebase Realtime Database 的**多人实时抢答系统**，手机可以通过二维码加入游戏。

---

## ✨ Features

### 🎞️ 看图猜电影

- 选择 **2 个电影主题**
- 每局随机抽取 **5 部电影**
- 每部电影显示一张剧照
- 图片出现前有 **3 秒倒计时**
- 点击按钮揭晓电影名称和上映年份
- 游戏结束后显示本局电影回顾
- 已经抽取过的电影不会在同一局重复出现

游戏参数可以直接在 `index.html` 中修改：

```js
const ROUNDS = 5;     // 每局电影数量
const COUNTDOWN = 3;  // 图片出现前倒计时
const PICKS = 2;      // 每局选择的主题数量
```

---

## 🎡 随机主题转盘

如果不想自己选择主题，可以使用随机转盘。

转盘会从所有主题中随机选择两个主题，选择完成后可以直接开始游戏。

目前包含的主题包括：

| Emoji | 中文 | Italiano |
|---|---|---|
| 🦸 | 好贵的电影特效 | Effetti speciali da milioni |
| 🇭🇰 | 一开口就是经典台词 | Battute già leggendarie |
| 🌍 | 你不会真没看过吧 | Non dirmi che non l’hai visto! |
| 🌸 | 二次元DNA报警 | Allarme DNA otaku |
| 🇨🇳 | 中国人DNA吧？ | Cinema cinese: ce l’hai nel DNA? |
| 🏰 | 好像童年余额不足了 | Saldo infanzia in esaurimento |

电影和主题都直接定义在 `index.html` 中，因此不需要数据库就可以扩充题库。

---

## 🔔 多人实时抢答

项目还包含一个独立的 `buzz.html` 手机抢答页面。

主持人打开主页面后，会自动创建一个房间，并显示：

- 4 位房间码
- QR Code
- 当前玩家
- 本轮抢答顺序
- 玩家积分排行榜

玩家使用手机扫描 QR Code 后进入：

```text
buzz.html?room=XXXX
```

输入名字即可加入房间。

当主持人开始新一轮后，所有手机会实时同步状态。

### 主持人可以

- 🔓 开始新一轮抢答
- 🔒 锁定 / 解锁抢答
- ➕ 给玩家加分
- ➖ 给玩家扣分
- ❌ 移除玩家
- 🔄 创建新的房间

抢答顺序根据玩家提交抢答的时间戳确定。

---

## 📱 手机端抢答

`buzz.html` 是专门为手机设计的轻量页面。

玩家只需要：

1. 输入房间码
2. 输入自己的名字
3. 点击「加入」
4. 等待主持人开启抢答
5. 点击巨大的 **「抢答！」** 按钮

抢答后，页面会立即显示自己的排名。

例如：

```text
🥇
你是第一个！🎉
```

或者：

```text
#3
你是第 3 个
```

---

## 🌍 双语支持

整个游戏支持：

- 🇨🇳 中文
- 🇮🇹 Italiano

语言会根据浏览器语言自动选择，同时用户也可以手动切换。

语言选择会保存到浏览器的 `localStorage` 中，因此刷新页面后仍然会保持之前的语言。

主页面使用：

```js
localStorage.getItem('quizLang')
```

抢答页面使用：

```js
localStorage.getItem('buzzLang')
```

---

## 🎨 Design

项目采用比较轻松、可爱的电影院视觉风格。

主要设计元素包括：

- 🎟️ 电影票式主题卡片
- 🎬 电影院幕布
- 📽️ 16:9 银幕
- 🍿 电影票揭晓动画
- 🎡 随机转盘
- ✦ 揭晓时的彩蛋动画
- 🔔 抢答按钮
- 📱 Mobile-first 抢答页面

主页面使用：

- `ZCOOL KuaiLe`
- `Fredoka`
- `Noto Sans SC`

等字体，使中文和意大利语在视觉上保持一致的圆润风格。

---

## 🗂️ Project Structure

一个典型的项目结构如下：

```text
guess-the-movie/
│
├── index.html
├── buzz.html
├── README.md
│
└── images/
    ├── marvel/
    │   ├── 01.jpg
    │   ├── 02.jpg
    │   └── ...
    │
    ├── hk/
    ├── world/
    ├── anime/
    ├── china/
    └── disney/
```

---

## 🖼️ 添加电影图片

电影图片不需要写死 URL。

程序会自动从：

```text
images/<theme-id>/
```

寻找对应图片。

例如：

```text
images/marvel/01.jpg
```

对应 `marvel` 主题中第 1 部电影。

项目会依次尝试：

```text
.jpg
.jpeg
.png
.webp
```

因此下面这些格式都可以：

```text
images/marvel/01.jpg
images/marvel/02.webp
images/marvel/03.png
```

电影在主题数组中的顺序决定图片编号：

```js
{
  id: 'marvel',
  movies: [
    ['复仇者联盟4：终局之战', 2019, 'Avengers: Endgame'],
    ['复仇者联盟3：无限战争', 2018, 'Avengers: Infinity War'],
    // ...
  ]
}
```

所以第一部电影对应：

```text
images/marvel/01.jpg
```

第二部：

```text
images/marvel/02.jpg
```

---

## ➕ 添加新的主题

在 `THEMES` 数组中添加一个新的主题：

```js
{
  id: 'new-theme',
  emoji: '🎥',
  name: '新的主题',
  nameIt: 'Nuovo tema',
  color: '#AABBCC',
  movies: [
    ['电影中文名', 2024, 'Titolo italiano'],
    ['另一部电影', 2023, 'Altro titolo'],
  ]
}
```

然后创建对应的图片目录：

```text
images/new-theme/
```

并按照电影顺序添加：

```text
01.jpg
02.jpg
```

即可。

---

## 🔥 Firebase

多人抢答功能使用：

**Firebase Realtime Database**

主页面和 `buzz.html` 必须连接到同一个 Firebase 项目，才能实现实时同步。

Firebase 数据大致按照以下结构组织：

```text
rooms/
└── ABCD/
    ├── meta/
    │   ├── round
    │   └── locked
    │
    ├── players/
    │   ├── player-id/
    │   │   ├── name
    │   │   ├── score
    │   │   └── joinedAt
    │
    └── buzzes/
        └── round-number/
            └── player-id/
                ├── name
                └── ts
```

玩家加入房间后，会实时监听整个房间的数据变化。

抢答时则写入当前轮次：

```text
buzzes/<round>/<player-id>
```

因此不同手机之间可以实时看到抢答状态。

---

## ⚙️ Firebase 配置

在 `index.html` 和 `buzz.html` 中配置同一个 Firebase 项目：

```js
const FIREBASE_CONFIG = {
  apiKey: "...",
  authDomain: "...",
  databaseURL: "...",
  projectId: "..."
};
```

`buzz.html` 中的配置必须和主页面保持一致，否则手机端无法加入主持人的房间。

同时需要设置：

```js
const BUZZ_PAGE_URL = 'https://your-domain.com/buzz.html';
```

这个 URL 会被编码进 QR Code，玩家扫码后即可自动进入对应房间。

---

## 🚀 Run Locally

这是一个纯前端项目，不需要 Node.js 或后端服务器即可运行。

最简单的方法是使用静态服务器。

例如：

```bash
python3 -m http.server 8000
```

然后打开：

```text
http://localhost:8000
```

如果直接双击 `index.html`，部分浏览器功能可能因为 `file://` 协议受到限制，因此推荐使用本地 HTTP server。

---

## 🌐 Deployment

由于项目本身是静态 HTML / CSS / JavaScript，可以直接部署到静态网站托管服务，例如：

- GitHub Pages
- Cloudflare Pages
- Netlify
- Vercel

部署后只需要确保：

```text
index.html
buzz.html
images/
```

都位于正确的位置。

---

## 🎮 Typical Game Flow

```text
打开游戏
   ↓
选择两个主题
   ↓
或使用 🎡 随机转盘
   ↓
开始放映
   ↓
3 秒倒计时
   ↓
显示电影剧照
   ↓
玩家猜电影
   ↓
揭晓电影票
   ↓
下一张
   ↓
重复 5 次
   ↓
显示本局结果
```

多人模式：

```text
主持人打开游戏
       ↓
生成房间码 + QR Code
       ↓
玩家手机扫码
       ↓
输入名字加入
       ↓
主持人开始新一轮
       ↓
      🔔
   抢答！
       ↓
实时记录顺序
       ↓
主持人调整积分
       ↓
排行榜更新
```

---

## 🛠️ Tech Stack

项目没有使用大型前端框架，主要由以下技术组成：

- HTML5
- CSS3
- Vanilla JavaScript
- Firebase Realtime Database
- SVG
- Canvas
- QR Code generation
- Google Fonts
- LocalStorage

因此整个项目非常轻量，适合直接作为静态网站部署。

---

## ♿ Accessibility

项目包含一些基础的无障碍支持：

- `aria-label`
- `aria-live`
- `aria-pressed`
- `role="dialog"`
- `:focus-visible`
- 键盘 `Escape` 关闭弹窗
- `prefers-reduced-motion` 支持
- 图片 `alt` 文本

例如用户可以使用键盘关闭随机转盘或抢答控制台。

---

## 📌 Configuration

最常修改的几个配置位于 `index.html`：

```js
const IMAGE_DIR = 'images';

const IMAGE_EXTS = [
  'jpg',
  'jpeg',
  'png',
  'webp'
];

const ROUNDS = 5;
const COUNTDOWN = 3;
const PICKS = 2;
```

因此如果以后想做：

```text
每局 10 部电影
只选择 3 个主题
取消倒计时
```

只需要修改：

```js
const ROUNDS = 10;
const COUNTDOWN = 0;
const PICKS = 3;
```

---

## 📄 License

如果这个项目用于个人、朋友聚会或学校活动，可以根据自己的需求使用和修改。

电影名称、剧照及相关素材的版权归其各自的版权持有人所有。

本项目本身不提供商业电影内容，仅作为互动猜电影游戏的平台代码。

---

## 🎬 Credits

Made with ❤️ for movie lovers.

**看图猜电影 · Indovina il film**

🇨🇳 中文 · 🇮🇹 Italiano

🍿 Guess the movie.  
🎬 Have fun.  
🔔 Buzz first.
