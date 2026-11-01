# 1.2.0

清单声明 `chat_platforms = ["onebot"]`，由宿主拒绝在其他平台的群启用，面板只列出 QQ 群；插件里原来的手写检查去掉。需要支持 `chat_platforms` 的宿主（LenBot 分支 `feat/multi-platform`）。配置、数据格式和工具名称不变。

# 1.1.2

工具说明、配置说明和 README 改为直接陈述功能与数据来源；配置、数据格式和工具名称不变。

# 1.1.1

/gs 显式允许命令正文中的 @，把真实提及转交 Core；声明 send/network 宿主能力；只接入 OneBot 场景，其他平台加载时直接说明不支持。需要包含 command(allow_mentions=True) 的宿主插件接口 1。

# 1.1.0

需要 LenBot 0.2.0 起的插件接口 1（从 `len_bot.plugin` 导入）。插件名、配置和业务数据不变，无需数据迁移。

- 工具带显式简介和参数说明，相关用法写在按需加载的共享指南里，结果返回 JSON。
