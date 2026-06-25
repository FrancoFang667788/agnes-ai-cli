# Agnes AI CLI - AI 图片/视频生成命令行工具

一套基于 [Agnes AI](https://agnes-ai.com) 的命令行工具，支持文字生成图片、图片风格转换、图片内容识别、视频生成等功能。

无需安装任何第三方依赖，只要有 Python 3 就能运行。

---

## 目录

- [功能一览](#功能一览)
- [准备工作](#准备工作)
- [功能详解](#功能详解)
  - [文字生成图片](#1-文字生成图片text-to-image)
  - [图片风格转换](#2-图片风格转换image-to-image)
  - [图片内容识别](#3-图片内容识别)
  - [文字生成视频](#4-文字生成视频text-to-video)
  - [图片生成视频](#5-图片生成视频image-to-video)
  - [多图关键帧视频](#6-多图关键帧视频)
- [可用模型](#可用模型)
- [常见问题](#常见问题)
- [Claude Code 集成](#claude-code-集成)
  - [安装 Claude Code](#安装-claude-code)
  - [安装本 Skill](#安装本-skill)
  - [使用方式](#使用方式)
  - [Skill 工作原理](#skill-工作原理)
- [开源协议](#开源协议)

---

## 功能一览

| 功能 | 说明 | 脚本 |
|------|------|------|
| 文字生成图片 | 输入一段文字描述，AI 生成对应图片 | `agnes-image.py` |
| 图片风格转换 | 给一张图片 + 文字指令，AI 生成新风格的图片 | `agnes-image.py` |
| 图片内容识别 | 给一张图片，AI 分析并描述图片内容 | `agnes-image.py` |
| 文字生成视频 | 输入一段文字描述，AI 生成对应视频 | `agnes-video.py` |
| 图片生成视频 | 给一张静态图片，AI 让它动起来 | `agnes-video.py` |
| 多图关键帧视频 | 给多张图片作为关键帧，AI 生成过渡视频 | `agnes-video.py` |

---

## 准备工作

### 第一步：下载项目

打开终端（Mac 按 `Command + 空格`，输入"终端"回车），输入以下命令：

```bash
git clone https://github.com/FrancoFang667788/agnes-ai-cli.git
```

然后进入项目目录：

```bash
cd agnes-ai-cli
```

### 第二步：确认 Python 3 已安装

```bash
python3 --version
```

如果能看到类似 `Python 3.x.x` 的输出，说明已安装。

如果没有安装，请前往 [Python 官网](https://www.python.org/downloads/) 下载安装。

### 第三步：获取 Agnes AI API Key

1. 打开 [Agnes AI 官网](https://agnes-ai.com)
2. 注册账号并登录
3. 在个人中心或 API 页面找到你的 API Key（类似 `sk-xxxx` 格式的字符串）

### 第四步：配置 API Key

有两种方式，选一种即可：

**方式一：设置环境变量（推荐）**

Mac / Linux 用户在终端输入：

```bash
export AGNES_API_KEY="你的API Key"
```

> 💡 如果希望每次打开终端都自动生效，可以把这行加到 `~/.zshrc` 或 `~/.bashrc` 文件中：
> ```bash
> echo 'export AGNES_API_KEY="你的API Key"' >> ~/.zshrc
> source ~/.zshrc
> ```

**方式二：写入文件**

```bash
echo "你的API Key" > ~/.agnes-ai-key
```

### 完成！

现在你可以开始使用了。

---

## 功能详解

### 1. 文字生成图片（Text-to-Image）

输入一段文字描述，AI 会根据描述生成一张图片。

**基本用法：**

```bash
python3 scripts/agnes-image.py --prompt "一只可爱的橘猫坐在窗台上看日落"
```

运行后会输出图片的 URL 链接，在浏览器中打开即可查看。

**保存图片到本地：**

```bash
python3 scripts/agnes-image.py --prompt "一只可爱的橘猫坐在窗台上看日落" --output cat.png
```

图片会保存为当前目录下的 `cat.png` 文件。

**指定图片尺寸：**

```bash
python3 scripts/agnes-image.py --prompt "一座未来城市" --size 1024x768 --output city.png
```

常用尺寸：`512x512`、`1024x1024`（默认）、`1024x768`、`768x1024`

**生成多张图片：**

```bash
python3 scripts/agnes-image.py --prompt "水墨画风格的山水" --n 3 --output painting.png
```

会生成 `painting_0.png`、`painting_1.png`、`painting_2.png` 三张图。

**固定随机种子（可复现结果）：**

```bash
python3 scripts/agnes-image.py --prompt "赛博朋克风格的东京街头" --seed 42 --output tokyo.png
```

使用相同的 seed 和 prompt，每次生成的图片会一样。

---

### 2. 图片风格转换（Image-to-Image）

给一张已有的图片，加上文字指令，AI 会生成新风格的图片。

**使用本地图片：**

```bash
python3 scripts/agnes-image.py --prompt "转换为水彩画风格" --image 你的图片.png
```

**使用网络图片：**

```bash
python3 scripts/agnes-image.py --prompt "转换为动漫风格" --image "https://example.com/photo.png"
```

**保存结果：**

```bash
python3 scripts/agnes-image.py --prompt "变成油画风格" --image photo.jpg --output oil_painting.png
```

---

### 3. 图片内容识别

给一张图片，AI 会分析并描述图片中的内容。

**识别本地图片：**

```bash
python3 scripts/agnes-image.py recognize --image photo.jpg
```

**识别网络图片：**

```bash
python3 scripts/agnes-image.py recognize --image "https://example.com/photo.png"
```

**自定义提问：**

```bash
python3 scripts/agnes-image.py recognize --image dog.jpg --prompt "这是什么品种的狗？"
```

```bash
python3 scripts/agnes-image.py recognize --image receipt.jpg --prompt "帮我识别这张发票上的金额"
```

---

### 4. 文字生成视频（Text-to-Video）

输入文字描述，AI 生成一段视频。

> ⚠️ 视频生成是**异步**的：提交任务后需要等待几分钟才能完成。

**提交任务并等待完成：**

```bash
python3 scripts/agnes-video.py --prompt "一只猫在沙滩上慢步行走，夕阳西下" --wait
```

加上 `--wait` 参数会自动轮询直到视频生成完毕，并输出视频下载链接。

**保存视频到本地：**

```bash
python3 scripts/agnes-video.py --prompt "樱花飘落的街道" --wait --output sakura.mp4
```

**指定视频尺寸：**

```bash
python3 scripts/agnes-video.py --prompt "星空延时摄影" --width 1152 --height 768 --wait
```

**只提交任务（不等待）：**

```bash
python3 scripts/agnes-video.py --prompt "海浪拍打礁石"
```

会输出一个 Task ID，之后手动查询状态：

```bash
python3 scripts/agnes-video.py status 你的TaskID
```

加 `--wait` 可以等待完成：

```bash
python3 scripts/agnes-video.py status 你的TaskID --wait --output wave.mp4
```

---

### 5. 图片生成视频（Image-to-Video）

给一张静态图片，AI 会让图片中的内容动起来。

```bash
python3 scripts/agnes-video.py --prompt "女孩转过身微笑" --image girl.png --wait --output girl.mp4
```

---

### 6. 多图关键帧视频

提供多张图片作为关键帧，AI 生成平滑的过渡视频。

```bash
python3 scripts/agnes-video.py --prompt "场景平滑过渡" --images img1.png img2.png --mode keyframes --wait --output transition.mp4
```

---

## 可用模型

| 模型名称 | 用途 | 说明 |
|----------|------|------|
| `agnes-image-2.0-flash` | 图片生成 | 默认模型，速度快 |
| `agnes-image-2.1-flash` | 图片生成 | 最新模型，效果更好 |
| `agnes-2.0-flash` | 图片识别 | 视觉理解模型 |
| `agnes-video-v2.0` | 视频生成 | 支持文生视频和图生视频 |

使用 `--model` 参数切换模型，例如：

```bash
python3 scripts/agnes-image.py --prompt "一朵玫瑰" --model agnes-image-2.1-flash
```

---

## 常见问题

### Q: 报错 `Error: Set AGNES_API_KEY env var or create ~/.agnes-ai-key`

**原因：** 没有配置 API Key。

**解决：** 参考上面的 [第四步：配置 API Key](#第四步配置-api-key)。

### Q: 报错 `API Error 401`

**原因：** API Key 不正确或已过期。

**解决：** 去 Agnes AI 官网重新获取 API Key。

### Q: 图片生成很慢

**正常情况。** 图片生成通常需要 10-30 秒，视频生成需要 2-5 分钟。

### Q: 视频生成后看不到链接

**解决：** 确保加了 `--wait` 参数，否则只会返回 Task ID，需要手动查询状态。

### Q: 本地图片无法识别

**解决：** 确保图片路径正确，支持 PNG、JPG、JPEG、GIF 等格式。

---

## 参数速查表

### agnes-image.py 参数

| 参数 | 简写 | 说明 | 默认值 |
|------|------|------|--------|
| `--prompt` | `-p` | 文字描述（必填） | - |
| `--model` | `-m` | 模型名称 | agnes-image-2.0-flash |
| `--size` | `-s` | 图片尺寸 | 1024x1024 |
| `--n` | - | 生成数量 | 1 |
| `--seed` | - | 随机种子 | 随机 |
| `--image` | `-i` | 输入图片（图生图用） | - |
| `--output` | `-o` | 保存到本地路径 | - |

### agnes-video.py 参数

| 参数 | 简写 | 说明 | 默认值 |
|------|------|------|--------|
| `--prompt` | `-p` | 文字描述（必填） | - |
| `--model` | `-m` | 模型名称 | agnes-video-v2.0 |
| `--width` | `-W` | 视频宽度 | 1152 |
| `--height` | `-H` | 视频高度 | 768 |
| `--num-frames` | - | 帧数（121帧≈5秒） | 121 |
| `--frame-rate` | - | 帧率 | 24 |
| `--image` | `-i` | 单张输入图片 | - |
| `--images` | - | 多张输入图片 | - |
| `--mode` | - | 模式（如 keyframes） | - |
| `--output` | `-o` | 保存到本地路径 | - |
| `--wait` | `-w` | 等待任务完成 | 否 |

---

## Claude Code 集成

本项目可以作为 [Claude Code](https://claude.com/claude-code) 的 **Skill**（技能插件）使用。安装后，你可以直接用自然语言让 Claude 帮你生成图片和视频，不需要记任何命令。

### 什么是 Claude Code？

[Claude Code](https://claude.com/claude-code) 是 Anthropic 推出的命令行 AI 助手，可以在终端里和 Claude 对话，让它帮你写代码、操作文件、执行命令等。Skill 是 Claude Code 的插件系统，安装 Skill 后 Claude 就能获得新的能力。

### 安装 Claude Code

如果你还没装 Claude Code，先安装：

```bash
npm install -g @anthropic-ai/claude-code
```

安装完成后，在终端输入 `claude` 就能启动。

### 安装本 Skill

**第一步：** 创建 Skill 目录

```bash
mkdir -p ~/.claude/skills/images-agnes-ai
```

**第二步：** 把项目文件复制到 Skill 目录

```bash
# 先 clone 项目（如果还没 clone）
git clone https://github.com/FrancoFang667788/agnes-ai-cli.git

# 复制文件到 Skill 目录
cp -r agnes-ai-cli/* ~/.claude/skills/images-agnes-ai/
```

**第三步：** 配置 API Key

```bash
# 方式一：环境变量（推荐写入 ~/.zshrc 永久生效）
echo 'export AGNES_API_KEY="你的API Key"' >> ~/.zshrc
source ~/.zshrc

# 方式二：写入文件
echo "你的API Key" > ~/.agnes-ai-key
```

**第四步：** 验证安装

启动 Claude Code：

```bash
claude
```

然后输入：

```
帮我画一张可爱的猫咪图片
```

如果 Claude 能成功调用 Agnes AI 生成图片，说明安装成功！

### 使用方式

安装完成后，你只需要用自然语言和 Claude 对话，它会自动识别并调用对应的功能：

**生成图片：**

```
> 画一张赛博朋克风格的东京夜景
> 生成一张水墨画风格的山水图，保存到桌面
> 画3张不同风格的猫咪头像
```

**图片风格转换：**

```
> 把 ~/Desktop/photo.jpg 转成油画风格
> 把这张图片变成动漫风格
```

**图片识别：**

```
> 识别一下 ~/Desktop/photo.jpg 里面有什么
> 这张图片里的植物是什么品种？
```

**生成视频：**

```
> 生成一段樱花飘落的视频
> 把 ~/Desktop/girl.png 这张图片做成视频，让她转过身
```

### Skill 工作原理

当你在 Claude Code 中提到图片生成、识别、视频等关键词时，Claude 会自动加载 `SKILL.md` 中的配置，然后调用 `scripts/` 目录下的 Python 脚本与 Agnes AI API 交互。你不需要手动输入任何命令，Claude 会帮你处理一切。

### 目录结构

安装完成后，Skill 目录结构如下：

```
~/.claude/skills/images-agnes-ai/
├── SKILL.md              # Skill 配置文件（Claude 读取这个来了解能力）
├── README.md             # 本说明文档
├── LICENSE               # 开源协议
├── .gitignore            # Git 忽略规则
└── scripts/
    ├── agnes-image.py    # 图片生成/识别脚本
    └── agnes-video.py    # 视频生成脚本
```

---

## 开源协议

[MIT License](LICENSE) — 可自由使用、修改和分发。
