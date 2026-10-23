# KodakkuRecordOpcodeMapLibrary

KodakkuAssist「录像转换」使用的跨版本录像包映射库。插件启动时读取 `index.json`，按 sha256 与本地缓存比对，只下载有变化的文件。

## 结构

```
index.json                       各区服、各版本条目
<区服>/tables/<区服>_<修订号>.json        包表（opcode → 跨版本包编号 + 布局 + 默认值）
<区服>/descriptors/<区服>_<修订号>.json.gz 版本描述符（推下一版本时使用）
<区服>/manual/<区服>_<修订号>.json        人工配对：旧 opcode（十六进制）→ 新 opcode 或 "none"
```

`index.json` 条目字段：`Revision`、`GameVersion`、`Parent`、`Algo`、`Status`（`confirmed` 为开发者已核对，`auto` 为机器人按共识自动发布）、`Missing`、`Reporters`、`Table`/`TableSha256`、`Descriptor`/`DescriptorSha256`、`PublishedAt`。

## 更新方式

- **自动**：游戏更新后，玩家插件在本机解析新版本并上报描述符到验证机器人；不同 Discord 账号上报的描述符有 5 份一致时，机器人重算表并提交到这里（`auto`）。有未配上的包时，机器人会在 Discord @服务器拥有者。
- **人工**：开发者用 OpcodeChain 生成并核对后，执行 `OpcodeChain library <数据目录> <本库目录>` 合并进来再提交（`confirmed`）。

不要修改已有版本之后还有新版本的表的包编号：后续版本都依赖它。
