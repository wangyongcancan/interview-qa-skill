# Interview QA — AI/NLP 技术面试问答教练

> 将面试视频转化成文本后，使用该 skill，输入 `/interview-qa` 即可激活。

## 这是什么

一个 Claude Code 自定义 Skill，把一场真实的 AI/NLP 技术面试（20 分钟）转化成了**交互式面试教练**。

它包含面试官提出的技术问题的深度解析，每个问题都用**通俗的生活故事**来讲解
## 快速开始

### 1. 准备面试转录

首先将你的面试视频/音频转换为文本。推荐方式：

```bash
# 安装依赖
pip install faster-whisper
winget install Gyan.FFmpeg

# 提取音频
ffmpeg -i "面试视频.mp4" -vn -acodec pcm_s16le -ar 16000 -ac 1 面试音频.wav

# 转录为文本
python -c "
from faster_whisper import WhisperModel
model = WhisperModel('small', device='cpu', compute_type='int8')
segments, _ = model.transcribe('面试音频.wav', language='zh', vad_filter=True)
with open('面试转录.txt', 'w') as f:
    for s in segments:
        f.write(f'[{s.start:.1f}s -> {s.end:.1f}s] {s.text}\n')
"
```

> 如果 HuggingFace 连接超时，设置 `os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'`

### 2. 安装 Skill

将本仓库克隆到你的 Claude Code 项目目录中，或直接复制 `.claude/commands/interview-qa.md` 和 `面试/` 文件夹到你的项目下：

```
你的项目/
├── .claude/
│   └── commands/
│       └── interview-qa.md    ← Skill 入口
├── 面试/
│   ├── 面试QA扩展版.md         ← 21 个 QA 知识库
│   └── 面试_transcript.txt    ← 原始面试转录（示例）
└── .gitignore
```

### 3. 激活

在 Claude Code 中输入：

```
/interview-qa
```



## 覆盖的面试问题
例如：
### 职业背景类
- 工作后回读研的决策逻辑
- 行业前沿关注度
- AI 编程工具的使用深度

### 模型选型类（核心战场）
- 为什么选 32B 而不是 7B 或 72B？
- 效果-成本-延迟三维权衡框架
- 量化选型：INT4 vs FP8 vs BF16

### 评估指标类（决定胜负）
- "口语化能力"如何定义和量化？
- 口语化 vs 理解能力的本质区别
- 训练指标 → 评测指标 → 业务指标的指标体系

### 部署与性能类
- 推理部署架构设计
- QPS 估算与容量规划
- 线上质量监控的三层体系

### 微调与业务指标类
- 难负样本（Hard Negative）的挖掘与利用
- 微调迭代节奏
- T0 转换率等业务指标的解读

### 工具生态类
- LandGauge / LandTrain / IG 的关系与原理

## Skill 特点

- **故事优先**：每个技术概念用一个通俗类比讲解（三兄弟搬砖、压缩照片、驾校错题本、奶茶店部署...）
- **面试视角**：始终解释"面试官为什么问这个"和"怎么回答能加分"
- **分层回答**：一句话结论 → 通俗故事 → 技术细节
- **交互式教学**：苏格拉底式引导，帮助建立自己的思考框架

## 示例对话

```
用户: /interview-qa 选型

Claude: # 模型选型三兄弟

面试官问"为什么用 32B"时，他不是在考你数字，是在考你的决策框架。

## 一句话结论
32B 是满足你业务三个底线（效果、成本、延迟）的唯一选择——
不是它最好，是只有它及格。

## 通俗故事：三兄弟搬砖
...（展开故事）

## 你该怎么回答
采用"三底线淘汰法"的结构：先确定底线 → 逐个淘汰 → 验证剩余最优
```

## 文件说明

| 文件 | 说明 |
|------|------|
| `.claude/commands/interview-qa.md` | Skill 定义文件，Claude Code 自动加载 |
| `面试/面试QA扩展版.md` | 完整知识库，21 个 QA 的详细解析 |
| `面试/面试_transcript.txt` | 原始面试转录（带时间戳） |
| `面试/面试总结.md` | 面试内容的结构化总结 |

## License

MIT
