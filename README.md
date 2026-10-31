# GSUID Core 桥接

LenBot 的独立插件，需要 LenBot >=0.2,<1、Python >=3.13。

## 安装

在「插件」填写 `https://github.com/lendevs/lenbot-plugin-gscore-adapter`，准备后填写配置并应用，再选择启用的群。也可以下载仓库 ZIP 导入。插件名称仍为 `gscore_adapter`，从旧内置版迁移时先更新到已移除内置插件的宿主，再安装同名插件；保持原来的参数、选群与数据目录。

在根配置 `plugins.gscore_adapter` 显式填写 `ws_url`，按需要填写 `access_token`，并在相应 `scenes.<场景>.plugins` 加入 `gscore_adapter`。在「插件」保存配置会定向重载，再选择使用场景。本插件连接已运行的 Core；Core 的启动和账号库由 Core 自行管理。

- `/gs <命令>`：把命令提交到 Core，执行结果由 Core 返回。
- `/gs连接`：连接已断开时再连接一次，已连接时保持现有连接。
- 低频工具 `gscore_status`：实际连接、进程内计数、最近错误；仍受角色工具许可限制。

图文响应统一经宿主发送，原图保存在现有媒体存储，回执分别标出失败、未确认和模拟。支持范围及上游协议见 [SOURCE.md](SOURCE.md)。命令由插件直接转发；`gscore_status` 是提供给模型的状态工具。

Core 返回的 `link://` 图片只抓取公开 HTTP(S) 地址，每次重定向都检查目标地址，拒绝私网、环回和云元数据地址；遵守宿主 `network.fake_ip_networks` 配置和原图字节上限。部署在内网的 Core 可以继续通过 WebSocket 返回 `base64://` 图片。

配置示例（面板使用相同字段）：

```json
{"ws_url": "ws://127.0.0.1:8765/ws/lenbot", "access_token": "", "token_query_parameter": "token"}
```

将 `ws_url` 换成 Core 实际端点，令牌填在独立密钥字段。`timeout_seconds` 默认 10 秒，`max_frame_bytes` 默认 20,000,000 字节。

## 开发与验证

在仓库目录使用已安装 LenBot 的 Python 环境执行：

```sh
uv run --project /path/to/LenBot --no-sync pytest -q -c pyproject.toml tests
```

测试使用合成消息和本机服务。

## 工具接口

工具采用接口 1 的显式简介、Field 参数说明与 `prompts/tools.md` 共享指南，返回原生 JSON 或文本。用 `PluginTest.preview_tools()` 查看模型说明、参数与可用性；测试里模型服务默认关闭。兼容和更新事项见 [CHANGELOG](CHANGELOG.md)。

CI 调用 LenBot 的可复用工作流，跟随宿主 master 测试。catalog-entry.json 记录目录条目的安装来源。

本机生成 ZIP（LenBot 仓库与本仓库放在同一目录下）：`uv run --no-project --python 3.13 python ../LenBot/scripts/package_plugin.py . /tmp/plugin.zip`。打包取 Git 已跟踪的运行源码和资源，新增文件需先加入 Git。

## 宿主能力

清单声明 `send`、`network`。这些权限约束宿主提供的能力，插件自行执行的 Python 与网络访问不受其约束。插件运行在宿主进程中，当前功能使用工具或后台协程，`isolation = "inline"`。
