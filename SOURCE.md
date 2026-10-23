# 协议来源与支持范围

2026-09-28 对照当前 GSUID Core 的 [models.py](https://github.com/Genshin-bots/gsuid_core/blob/master/gsuid_core/models.py)、[segment.py](https://github.com/Genshin-bots/gsuid_core/blob/master/gsuid_core/segment.py) 和 [bot.py](https://github.com/Genshin-bots/gsuid_core/blob/master/gsuid_core/bot.py)，以及 [参考适配器入口](https://github.com/KimigaiiWuyi/astrbot_plugin_gscore_adapter/blob/master/main.py)。本接口按协议编写。

- 发送身份为onebot和当前真实 bot_id 对应的 OneBot 账号；/gs只提交命令文字、实际@及引用reply_id，user_pm固定普通用户6，QQ群管理员在Core里同样是普通用户。Core自己的权限与账号状态由Core管理。
- 目标只接明确启用的group/direct；支持text、at、base64://图片、Core当前URL转码分支的裸base64、link://HTTP(S)图片及仅供尺寸提示的image_size。文件、音频、合并转发、撤回、禁言等其他段目前不支持，收到时整帧报错并记录原文片段。
- MessageSend.echo可以没有，正常帧仍发送；有值时，recall_message_id.data.id回传实际确认的平台ID列表，无确认则null。当前Core支持列表并会展开；它用于取回实际发送ID。协议错误无法解析整帧时记录原文片段。
- 启动一次连接，断连停止常驻读取并显露错误；/gs连接 建立一条新连接。若第一次启动就失败，此插件由宿主标为失败，在 Core 就绪后点击该插件的「重载」。Core 重连后重送的帧按收到的内容处理。

HTTP图片可来自运营者信任的Core提供的地址，单次读取，限时限量。Core令牌只发给配置的WebSocket端点。
