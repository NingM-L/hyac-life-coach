# Nexus Bridge：Hyac Life Coach 2.0 工作区与任务接入

## 工作区路径

把 RikkaHub 当前绑定的工作区根目录记为 WORKSPACE_ROOT。当前实际使用示例为：

~~~text
/workspace/hyac/
~~~

任务提示中的所有文件都相对于 WORKSPACE_ROOT。直接使用：

~~~text
WORKSPACE_ROOT/learner-profile.md
WORKSPACE_ROOT/active-plan.md
WORKSPACE_ROOT/schedule/current-week.md
WORKSPACE_ROOT/daily/YYYY-MM-DD.md
WORKSPACE_ROOT/system/tasks.md
WORKSPACE_ROOT/system/reminders.md
~~~

旧版 learning-vault/ 只作为兼容数据源。已有工作区时不创建新的 learning-vault/。

## 固定任务

任务名保持稳定：

1. 开始学习提醒
2. 提交学习汇报
3. 每周总结
4. 每月总结

创建任务前用 list_jobs 查找现有任务。同名且时间、周期、提示内容一致时保留原任务；配置变化时删除旧任务并重建。创建或重建后将任务 ID 写入 WORKSPACE_ROOT/system/reminders.md。

## 四个任务的提示模板

### 开始学习提醒

~~~text
你是 Hyac Life Coach 2.0 的每日学习提醒任务。
1. 读取当前绑定工作区根目录 WORKSPACE_ROOT 下的 learner-profile.md、active-plan.md、schedule/current-week.md 和 system/tasks.md。
2. 获取设备本地日期。
3. 如果当前状态是 paused，记录跳过原因并结束。
4. 找出今天唯一最重要的任务，读取今日主技能。
5. 使用 post_notification 发送技能、任务、预计时长和需要提交的输出。
6. 把执行时间和结果写入 WORKSPACE_ROOT/system/reminders.md。
7. 读取失败时记录失败原因和下一次重试动作。
~~~

### 提交学习汇报

~~~text
你是 Hyac Life Coach 2.0 的每日汇报提醒任务。
1. 读取当前工作区 WORKSPACE_ROOT 下的 daily/YYYY-MM-DD.md、system/tasks.md 和 learner-profile.md。
2. 判断当天是否已经有完整的【今日学习汇报】。
3. 已汇报时记录 skipped_already_reported，不重复提醒。
4. 未汇报时提醒用户提交包含主技能、学习时长、输出成果和今天花了多少钱的固定模板。
5. 到次日仍未汇报时，将当天状态写为 missed，并写入 daily/YYYY-MM-DD.md 和 system/tasks.md。
6. 记录执行时间、结果和失败原因。
~~~

### 每周总结

~~~text
你是 Hyac Life Coach 2.0 的每周复盘任务。
1. 读取 WORKSPACE_ROOT 下本周 daily/ 记录、schedule/current-week.md、active-plan.md 和 learner-profile.md。
2. 统计完成率、实际时长、听说读写覆盖、输出次数、支出合计、重复错误和主要阻碍。
3. 生成 WORKSPACE_ROOT/reviews/weekly/YYYY-Www.md。
4. 只调整未来任务，更新 current-week.md、active-plan.md 和 system/tasks.md。
5. 把调整原因写入 WORKSPACE_ROOT/system/plan-changelog.md。
6. 使用 post_notification 告知用户周总结已完成，并提示下周第一项任务。
7. 记录任务执行结果。
~~~

### 每月总结

~~~text
你是 Hyac Life Coach 2.0 的每月复盘任务。
1. 读取 WORKSPACE_ROOT 下本月 reviews/weekly/、daily/、active-plan.md、schedule/six-month-plan.md 和 learner-profile.md。
2. 汇总英语主线进度、听说读写变化、学习时长、支出合计、重复卡点和有效方法。
3. 生成 WORKSPACE_ROOT/reviews/monthly/YYYY-MM.md。
4. 更新下个月计划和后续六个月里程碑，历史记录保持原样。
5. 把变更原因、日期、内容和影响写入 WORKSPACE_ROOT/system/plan-changelog.md。
6. 使用 post_notification 告知用户月总结和下月重点。
7. 记录任务执行结果；失败时保留历史并安排下一次执行。
~~~

## 提醒台账格式

~~~markdown
# Nexus 提醒台账

- 工作区根目录：WORKSPACE_ROOT
- Skill 版本：2.0

| 任务名 | 任务 ID | 时间 | 周期 | 状态 | 最近执行 | 最近结果 | 失败重试 |
|---|---|---|---|---|---|---|---|
| 开始学习提醒 | 待创建 | 用户时间 | 每日 | active | - | - | - |
| 提交学习汇报 | 待创建 | 用户时间 | 每日 | active | - | - | - |
| 每周总结 | 待创建 | 周日 21:00 | 每周 | active | - | - | - |
| 每月总结 | 待创建 | 月末 21:30 | 每月 | active | - | - | - |
~~~

## 失败和去重

- 创建失败：写入台账并保留待重试状态。
- 执行失败：保留历史结果，记录下一次重试动作。
- 发现多个同名任务：保留唯一的最新配置，其他任务暂停或删除并记录。
- 工作区文件缺失：先在 WORKSPACE_ROOT 创建缺失目录和文件。
- 用户时间改变：先更新提醒台账，再重建对应任务。