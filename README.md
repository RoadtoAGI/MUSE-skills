# MUSE Skills

**简体中文** | [English](README.en.md)

MUSE 的四套创作技能：原创小说与剧本、文学作品拆解、连载创作和连载拆解。技能用麦基故事理论指导设计，从名著知识库中选取相关范例，辅助大纲、人物表演、场景写作和修订。

[论文](https://arxiv.org/abs/2609.15188) · [完整项目](https://github.com/RoadtoAGI/MUSE) · [Hugging Face 资源](https://huggingface.co/datasets/RoadtoASI/MUSE-skills) · [知识库说明](KNOWLEDGE_BASE.md)

## 最新版本：JEV 创作辅助

**2026-09-23 · muse-runtime v0.1.0**。原创写作包升级为 **v2.18.0**，名著检索包为 **v0.9.0**，连载写作包为 **v0.8.0**。

JEV 在召回后评价场景原文和灵感卡，帮助选择贴合当前任务的参考；在原创和连载 brainstorm 中比较具体剧情候选，辅助补强、重构和推荐。连载共创结合已有剧情、人物所知、固定设定和作者反馈讨论未来走向，作者采纳后将决定写回大纲和章节计划。

在选定的 Python 环境安装运行组件：

```bash
python3 -m pip install "git+https://github.com/RoadtoAGI/MUSE-skills.git@muse-runtime-v0.1.0"
python3 -m muse_runtime mode set jev --work-dir /absolute/path/to/work
python3 -m muse_runtime brainstorm guide
```

默认使用标准模式，安装插件不会自动启用 JEV。`MUSE_JEV_API_KEY` 由执行环境提供；`MUSE_JEV_TUZI_API_KEY` 可配置 Tuzi 备用通道。相关查询和候选内容会发送到配置的服务。连载 brainstorm 使用系列根目录，章节参考检索使用当次章目录，分别设置模式。详见[运行指南](src/muse_runtime/brainstorm-guide.md)和各包 README。

## 选择技能包

| 插件 | 用途 | 常用入口 |
|---|---|---|
| [muse-writing](plugins/muse-writing/README.md) | 原创短篇、中篇小说和剧本 | `short-story-writing`、`story-writing`、`screenplay-writing` |
| [muse-canon-distill](plugins/muse-canon-distill/README.md) | 文学作品拆解；结构、人物、对白和文风参考 | `novel-analysis`、`design-doc-reference`、`scene-reference` |
| [muse-serial-writing](plugins/muse-serial-writing/README.md) | 连载、同人、续写和番外 | `serial-outline`、`serial-chapter-writing` |
| [muse-serial-distill](plugins/muse-serial-distill/README.md) | 长篇连载分析和续写接管材料 | `serial-analysis`、`takeover-distill` |

原创写作建议安装前两个包。连载创作可安装后两个包，再加上 `muse-canon-distill` 获取文学参考；需要完整工具集时安装全部四个包。

```text
写作需求 → 构想与大纲 → 人物表演 → 场景正文 → 审阅修订
                ↑          ↑          ↑          ↑
             故事理论、名著范例、已经确定的创作决定

连载创作：卷章计划 → 当前章节 → 保存人物经历和未解伏笔 → 下一章
```

## Claude Code 安装

在 Claude Code 中添加一次插件源，然后安装所需插件：

```text
/plugin marketplace add RoadtoAGI/MUSE-skills
/plugin install muse-writing@muse-skills
/plugin install muse-canon-distill@muse-skills
/plugin install muse-serial-writing@muse-skills
/plugin install muse-serial-distill@muse-skills
```

按安装提示重新加载插件或开启新会话。例如：

```text
/muse-writing:short-story-writing 写一篇发生在深夜火车站的悬疑短篇。
```

插件名区分两套创作流程中的同名技能。例如 `muse-writing:writer` 和 `muse-serial-writing:writer` 分别属于原创和连载流程。使用完整入口名称，让主控继续调用对应包内的技能。

## Codex 安装

在终端运行：

```bash
codex plugin marketplace add RoadtoAGI/MUSE-skills
codex plugin add muse-writing@muse-skills
codex plugin add muse-canon-distill@muse-skills
codex plugin add muse-serial-writing@muse-skills
codex plugin add muse-serial-distill@muse-skills
```

开启新会话后，指定插件和创作任务，例如：“使用 muse-writing 的 short-story-writing，帮我写一篇深夜火车站的悬疑短篇。”

技能中的脚本从各自插件安装根目录执行。Claude Code 示例使用 `${CLAUDE_PLUGIN_ROOT}`；其他宿主应按已加载 `SKILL.md` 的实际位置定位包根，再执行对应命令。Claude hooks 按 Claude Code 的运行机制加载。

## 知识库配置

知识库随参考插件一起提供，包含作品分析、场景、人物材料和检索脚本。运行 Python 脚本前，按各包说明安装依赖。向量检索还需要配置兼容的嵌入服务并重建索引；变量和命令见[知识库配置](KNOWLEDGE_BASE.md#rebuild-retrieval-embeddings)。

麦基原理用于判断人物在压力下的选择、场景价值变化和情节因果。知识库提供具体作品中的写法：设计者参考结构分析，人物表演参考连续对白，正文作者参考场景原文和文风标注。选中的材料随设计决定传入后续写作。

## 更新

Claude Code 中先更新源，再更新已安装插件，例如：

```text
/plugin marketplace update muse-skills
/plugin update muse-writing@muse-skills
```

其余插件使用对应名称。Codex 通过插件管理界面检查更新；命令行选项以本机 `codex plugin --help` 为准。

从旧的独立仓库迁移时，卸载旧来源的插件，再从 `muse-skills` 安装对应包，避免同时启用两份相同插件。

## 项目来源

本仓库发布 [MUSE](https://github.com/RoadtoAGI/MUSE) 的技能包，保留脚本、参考资料、知识库和插件配置。[SOURCE.json](SOURCE.json) 记录对应的源代码版本。开发修改在主项目维护，发行版本同步到本仓库。

文学原作及译文的权利归各自作者和出版方所有；资源来源和联系处理方式见[知识库说明](KNOWLEDGE_BASE.md#resource-provenance)。

论文：**[MUSE: A Theory-Harnessed Story Engine for Vibe Narrativizing](https://arxiv.org/abs/2609.15188)**。
