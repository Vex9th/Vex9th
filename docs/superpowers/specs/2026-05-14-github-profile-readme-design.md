# GitHub Profile README 设计规格

> 日期：2026-05-14
> 用户：Vex9th
> 目标仓库：`Vex9th/Vex9th`（GitHub 特殊机制：与用户名同名的 public 仓库根目录下的 README.md 会自动显示在 `https://github.com/Vex9th` 主页）

## 目标
为 GitHub 用户 `Vex9th` 创建 Profile README，将当前空白的主页变成有格调的极简自我陈述。

## 上下文
- 用户名：`Vex9th`
- 当前主页状态：bio "🌴 Lucky"，地点 Singapore，2 个公开仓库，活动设为 private
- 当前不存在 `Vex9th/Vex9th` 仓库（404）
- 用户特点：大部分代码在私有仓库，**没有公开作品流可供 showcase**——传统的 stats / 贡献图 / 置顶仓库套路在此处会显得寒酸，所以反其道而行

## 设计定位
**极简存在感 + 神秘型 + 老派 hacker ASCII 美学。**

整个 README 只有两个元素：
1. 大号 ASCII 银发风格的名字（figlet ANSI Shadow 字体）
2. 一行 inline-code 风格的 tagline，形如代码注释

不展示数据、不堆徽章、不放贡献图。气场来自留白和克制。

## README 完整源码

````markdown
```
 __     __         ___  _   _
 \ \   / /__ __  _/ _ \| |_| |__
  \ \ / / _ \\ \/ / (_) | __| '_ \
   \ V /  __/>  < \__, | |_| | | |
    \_/ \___/_/\_\  /_/ \__|_| |_|
```

`// signal lost in the tropics.`
````

ASCII 名字是 figlet `ANSI Shadow` 字体输出 `Vex9th` 的结果。**实现阶段应实际运行 figlet 重新生成一次**，以保证字符精确（不依赖本规格中的手抄版本）；最终视觉须与本规格预览匹配（5 行、整体宽度约 35 字符）。

## 设计原则
- **YAGNI 严格执行**：任何"再加一点会更好看"的诱惑都拒绝
- **代码块对齐**：ASCII 包在 ` ``` ` fenced code block 里，强制等宽对齐
- **注释语义**：tagline 用反引号包成 inline code，加强"它是一条代码注释"的暗示，呼应 `//`
- **Dark mode 优先**：暗背景 + 绿字（GitHub 代码块默认配色）效果最强；light mode 下保持可读但视觉张力较弱，接受这个权衡

## 显式排除清单（YAGNI 红线）
以下元素**禁止**出现在 README 中：
- ❌ GitHub Stats 卡片（`github-readme-stats`）
- ❌ Top Languages 卡片
- ❌ Contribution graph / streak / 热力图
- ❌ Shields.io 徽章（technology / social / version 任何一种）
- ❌ Profile views 访客计数器
- ❌ "👋 Hi, I'm..." / "About me" / "Skills" / "Tech stack" 段落
- ❌ Emoji 列表（`- 🔭 Currently working on...`）
- ❌ 外部链接（X / Twitter / blog / email / Discord）
- ❌ 动画 SVG / typing animation / wave banner

排除链接是有意识的选择：与"活动私密 + signal lost"的人设一致——找得到的人自然找得到。

## 仓库设置
- 仓库名：`Vex9th/Vex9th`
- 可见性：**public**（私有仓库的 README 不会显示在主页）
- 仓库描述：留空，或 `Vex9th`
- 仓库标签 / topics：无
- License：无
- 唯一文件：`README.md`（根目录）

## 成功标准
- ✅ 访问 `https://github.com/Vex9th` 时，主页 bio 下方显示 ASCII 名字 + tagline
- ✅ ASCII 5 行在 GitHub markdown 渲染下等宽对齐、不错行（在 desktop 全宽窗口）
- ✅ Dark mode 下 ASCII 显示为 GitHub 代码块默认配色（在 dark 主题下偏白偏绿）
- ✅ 整个 README 渲染高度 < 1 屏（约 240px 内）
- ✅ Markdown 源码 ≤ 10 行（含空行和 fence）

## 不在本规格范围
- 头像更换：保持现有头像不变
- bio 修改：保持 "🌴 Lucky" 不变（与 tagline 的 tropics 呼应）
- 置顶仓库 pin 选择：另议
- 现有 2 个公开仓库的整理：另议
