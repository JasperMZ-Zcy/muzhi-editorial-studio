# 牧之远见 · 杂志插画制作

![杂志插画制作工作流示意图](docs/assets/hero.svg)

一个面向 Codex 的中文杂志插画视频工作流插件。它把内容生产中最容易被“看起来做完了”掩盖的问题，变成可追溯的项目合同、阶段检查、媒体实测、版本锁和本地成本台账。

这里的“牧之远见”是插件的公开来源品牌与作者署名，而不是对使用者声音、口号、选题或视觉风格的强制要求。任何团队都应当用自己拥有授权的品牌规范、素材和发布渠道来运行它。

> 图中的卡片和流程均为工作流示意，不是产品界面或功能截图。

## 它解决什么问题

- **37 项质量否决不被简化入口遗漏。** 原始否决条目、行号、检查路径与内容哈希都被收在公开目录中；目录校验会确认 37 条规则仍完整可追溯。
- **把“能播”与“做好了”分开。** 从设计锁、故事板、样片、动态素材回收、低清粗剪到最终媒体审计，每一阶段都有真实输入和相应检查入口；视觉与听感仍必须由人实际判断。
- **局部返工不带崩整片。** 封面、音乐、单镜等修改遵循最小影响范围，已通过的录音、字幕、时间线和输出不会因为一个局部请求被默认重做。
- **生成前先做可见的成本约束。** 本地台账能留存预算、预估成本、任务指纹、外部 job ID、输出哈希和实际成本；相同已完成输出复用，失败重试需要单独的真实授权记录。
- **供应商可换，后期输入不换。** WAN、MiniMax、Google Flow 和项目内新登记的提供方最终都回收到同一种本地镜头交付合同，避免让渠道差异污染剪辑、字幕和混音。

## 重要边界

这不是云视频平台，也不包含模型、额度、账号、密钥或自动发布能力。插件不会替你连接 WAN、MiniMax、Google Flow 或任何新平台；它编排你已经可用、并且本次已获授权的能力。供应商仍可能失败，费用仍以你的账户和回执为准，质量检查也不能保证任何内容“永不出错”。

## 安装

在 Codex CLI 中添加公开 Marketplace，然后安装插件：

```bash
codex plugin marketplace add JasperMZ-Zcy/muzhi-editorial-studio
codex plugin add muzhi-editorial-studio@muzhi-editorial
```

安装后请**新开一个任务**，再用自然语言从当前项目继续，例如：

```text
按杂志插画工作流继续这个项目。先读取现有状态和已经通过的内容；
本轮只把封面调得更温暖，不重做录音、字幕、镜头或定时发布状态。
```

更多从零启动、继续旧项目、成本留额和排错说明见 [快速开始](docs/GETTING_STARTED.md)。安装后应在新任务中用代表性请求验证工作流。[OpenAI官方插件说明](https://learn.chatgpt.com/docs/plugins)

## 制作时各工具负责什么

| 环节 | 可以使用的能力 | 本插件负责 |
| --- | --- | --- |
| 内容、导演与分镜 | Codex及用户提供的真实资料 | 目标、语义、阶段、批准和版本不漂移 |
| 插画、首帧与封面 | 当前可用的内置生图或用户授权图像工具 | 统一设计、人物连续性、文字可读与参考图检查 |
| 图生视频 | WAN / MiniMax已配置API，Google Flow手动，或项目中新接入的平台 | 渠道选择、动作提示词、单层包、回收合同 |
| 原创背景音乐 | 例如已连接的ChatCut音乐工具，或其他获得许可的音乐来源 | 新曲身份、费用、正确导出、配合口播与复用边界 |
| 剪辑与合成 | 已有FFmpeg、Remotion或HyperFrames等适合本条内容的工具 | 母钟、字幕、音轨、素材时长、最终QA和交付 |

这些平台是可对接的工具示例，不是随插件赠送的服务，也不代表官方合作。使用者需自行配置账号和获得相应权限。

## 选择视频渠道

| 渠道 | 插件的角色 | 你仍需负责的部分 |
| --- | --- | --- |
| WAN | 记录已配置能力的选择、任务回执和统一交付合同 | 自己的配置、模型可用性、授权与费用 |
| MiniMax | 同上，保留模型／首帧／时长等实际回执 | 自己的配置、模型可用性、授权与费用 |
| Google Flow | 生成首帧与提示词的单层手动包，回收生成结果 | 在 Flow 中手动提交与下载 |
| 新提供方 | 在**当前项目**登记为已配置 API 或手动包，并使用标准输出合同 | 连接方式、实际验证、费用与失败恢复 |

详情见 [渠道与交付合同](docs/PLATFORMS.md)。

## 验证基线

这个公开包保留四组独立的标准库测试与三组上游负向校验，当前发布基线为 **149 个测试**：

```bash
cd plugins/muzhi-editorial-studio
python -X utf8 scripts/test_project_state.py
python -X utf8 scripts/test_provider_router.py
python -X utf8 scripts/test_production_runner.py
python -X utf8 scripts/test_generation_ledger.py
python -X utf8 vendor/editorial-magazine-explainer-producer/scripts/test_director_storyboard.py
python -X utf8 vendor/editorial-magazine-explainer-producer/scripts/test_dynamic_asset_duration.py
python -X utf8 vendor/editorial-magazine-explainer-producer/scripts/test_production_enforcement.py
python -X utf8 scripts/verify_rule_coverage.py
```

测试证明脚本在合成夹具中的边界行为，不能替代真实素材的观看、听感、版权与平台检查。质量规则的适用方式见 [质量与验收](docs/QUALITY.md)，安全与隐私边界见 [安全说明](docs/SECURITY.md)。

## 文档导航

- [快速开始](docs/GETTING_STARTED.md)
- [渠道与交付合同](docs/PLATFORMS.md)
- [质量与验收](docs/QUALITY.md)
- [安全说明](docs/SECURITY.md)

---

作者：牧之远见 · 公开站点：[muzhiedu.top](https://muzhiedu.top)
