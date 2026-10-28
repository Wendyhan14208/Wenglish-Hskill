# oralenglish-skill

专四（TEM-4）英语口语思辨陪练 Skill：一轮一话题，AI 出题给正反参考句型，用户口头作答，AI 转写原句、输出 1 版口语优化句（重点修正中式英语）并简短点评，以【继续】（深度追问）和【重新回答】（同一话题再答一次）循环训练。

## 项目解决什么问题

英语专业学生在备考专四口语时普遍存在三个痛点：

1. **不知道说什么**：思辨话题缺少抓手，开口前没有立场表达框架；
2. **中式英语顽固**：中文直译语序、中式搭配（如 "learn knowledge"）、缺主语缺连接词等母语负迁移问题反复出现，且没有针对性纠正；
3. **练了没反馈**：自己开口说没人指出问题，练再多也无法形成"开口 → 被纠正 → 再开口"的闭环。

本项目把"出题 + 参考句型 + 逐句优化 + 成因点评 + 循环深挖"打包成一个可复用的 Skill，让任何支持文件夹式 Skill 的 AI Agent 都能充当专四口语思辨陪练教练。

## 主要功能

- **逐轮思辨训练**：每轮给出 1 个英文思辨话题（贴近专四口语常考范围：校园、教育、科技、社会、生活方式、环境、职业、健康等），并配套 2 句纯英文参考句型——【肯定立场】【否定立场】各 1 句，作为可套用的句型框架。
- **原句转写**：【我的原句】原样转写用户口述英文，保留原始表达、语序与语法错误，不做隐性纠正。
- **单版口语优化**：【优化句】仅生成 1 版，优先修正四类中式英语——中文直译语序、中式搭配、逻辑断层、母语负迁移；拒绝浅层同义词替换；核心改动加粗；口语化表达、不过度书面化。
- **简短点评**：点明本次最典型的中式英语问题，并分别评价逻辑、句式、用词地道度。
- **双分支循环**：【继续】基于当前话题深挖追问，开启新一轮作答；【重新回答】不换话题再答一次，循环反馈直到输入【继续】。
- **边界处理**：作答过短时先引导展开再反馈；中文作答时提醒改用英文并重发参考句型；可随时换话题或指定话题方向。

## 安装方法

本项目是纯文件夹式 Skill，无安装脚本、无 Python 依赖（`SKILL.md` 为文本指令，`agents/openai.yaml` 为元数据）。

**方式一：复制到本机 Skill 目录**（豆包等桌面 Agent）

```
1. 将 oralenglish-skill/ 整个文件夹复制到 Agent 的 Skill 根目录，例如：
   <工作区>/workspace/.user_skills/oralenglish-skill/
2. 重启或刷新 Agent 会话，使 Skill 被识别加载。
```

**方式二：按所用平台的 Skill 规则导入**（支持开源 Skill 的 Agent 平台）

- 若平台支持从文件夹/仓库导入 Skill，将 `oralenglish-skill/` 文件夹直接导入即可；
- 目录必须保持如下结构，不可拆散：

```
oralenglish-skill/
├── SKILL.md              # 技能完整指令（训练流程、反馈格式、优化原则）
└── agents/
    └── openai.yaml       # 技能元数据（name / description / version / license）
```

**安装后校验（可选）**：使用官方 skill-creator-for-work 工具集的校验脚本：

```bash
python scripts/quick_validate.py <skill目录>/oralenglish-skill
```

输出 `Skill is valid!` 即通过。

## 使用方法

1. 在对话中发起训练：说启用口令"小砖，陪我练口语"，或使用"开始专四口语思辨训练"、"陪我练练口语辩论"、"练一个关于科技的思辨话题"等说法。
2. AI 给出 Topic 与 2 句正反参考句型后，用英文口头作答（语音或打字均可，3-5 句）。
3. 收到【我的原句】【优化句】【简短点评】后二选一：
   - 输入【继续】→ 基于当前话题深度追问，开启新一轮作答；
   - 输入【重新回答】→ 不换话题再答一次，循环反馈直到输入【继续】。
4. 随时说"换个话题"或指定方向（如"来个科技类话题"），即重新开场。

## 输入输出示例

**用户输入**：开始专四口语思辨训练

**AI 输出**：

```
Topic: Should college students take part-time jobs?

参考句型（供套用，不必照抄）：
【肯定立场】I firmly believe that college students should take part-time jobs, because it helps them ...
【否定立场】While part-time jobs may bring some benefits, I would argue that they ... , since ...

请参考以上句型，用英文口头作答（3-5 句），完成后发送给我。
```

**用户作答**：I think students should do part-time job because can earn money and learn many things in society.

**AI 反馈**：

```
【我的原句】I think students should do part-time job because can earn money and learn many things in society.
【优化句】I think students **should take** part-time **jobs**, because they **can** earn money **and gain experience of the real world**.
【简短点评】
- 中式英语：do part-time job 是"做兼职"的直译搭配，英文惯用 take/have a part-time job；because 后缺主语，属中文"因为可以…"的省略式直译。
- 逻辑：因果方向正确，但"赚钱 + 学东西"两点未展开，说服力偏弱。
- 句式：只有一个 because 长句，缺少口语节奏。
- 用词地道度：learn many things in society 偏中式，gain experience of the real world 更自然。

两个选项：
【继续】基于本话题深挖追问
【重新回答】本话题再答一次
```

**用户输入【继续】后，AI 深挖追问（新一轮 Topic + 新参考句型）**：

```
Topic: Some argue that the time spent on part-time jobs could be better used for study and campus activities. Do you still support working part-time during the semester?

参考句型（供套用，不必照抄）：
【肯定立场】I still support working part-time during the semester, because the skills you gain on the job outweigh the study time you lose, such as ...
【否定立场】Although part-time work builds independence, I would admit that ... may suffer when ...

请参考以上句型，用英文口头作答（3-5 句），完成后发送给我。
```

## 边界与依赖

- 不支持：网页抓取、PDF 处理、图形界面操作。
- 本技能运行时不读取任何文件、不调用外部工具，仅依赖 Agent 的对话能力；安装校验用的 `quick_validate.py` 来自 skill-creator-for-work 官方工具集，不属于本技能运行依赖。

## 开源许可

MIT License。
