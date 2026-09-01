# Hyac Life Coach 2.0 索引

## 项目定位

Hyac Life Coach 2.0 是面向 RikkaHub Agent 手机端的长期学习教练 Skill，优先复用已绑定的工作区，适合英语、编程、数学、写作等技能学习。

## 2.0 核心顺序

~~~text
识别现有工作区 → 建立用户画像 → 用户确认 → 基础测试
→ 英语主线与四项技能计划 → 每日任务 → 用户输出
→ 每日汇报与支出记录 → 输出式练习 → 周总结 → 月总结
~~~

## 英语默认主线

- 主线：理解真实英语内容，并完成清晰、可复用的表达。
- 四项基础技能：听力、口语、阅读、写作。
- 支撑模块：词汇、语法、发音和句型。
- 每日任务标注一个主技能，周计划逐步覆盖四项技能。

## 文件说明

- SKILL.md：主 Skill，包含工作区识别、画像、测试、计划、汇报和任务规则。
- INTRODUCTION.md：安装、首次使用、升级和常用指令。
- references/nexus-bridge.md：RikkaHub 定时任务、通知和台账模板。
- agents/openai.yaml：代理技能显示名称、版本和默认提示。

## 工作区路径

当前工作区示例：

~~~text
/workspace/hyac/
~~~

运行时直接在 WORKSPACE_ROOT 根目录维护 learner-profile.md、active-plan.md、schedule/、daily/、reviews/、output-practice/ 和 system/。旧版 learning-vault/ 仅用于兼容读取和迁移。

## 关键行为

1. 已有工作区时直接复用，不重复创建嵌套目录。
2. 画像确认后才测试，测试完成后才生成正式六个月计划。
3. 每日汇报记录主技能、时长、输出、支出和明日调整。
4. 周月总结统计听说读写覆盖与学习支出。
5. Nexus 任务先查重，再创建或重建。