# QMT 策略 API Codex Skill

这是一个面向 Codex 的 QMT/迅投极速策略交易系统量化策略开发 skill。安装后，Codex 在编写、改造、审查或调试 QMT 策略时，会优先参考仓库内置的 QMT API 文档，而不是凭通用 Python 或其他量化平台经验猜测接口。

## 适用场景

这个 skill 适合用于：

- 编写 QMT K 线策略，包括 `init(ContextInfo)`、`handlebar(ContextInfo)`、`after_init(ContextInfo)` 和 `stop(ContextInfo)`。
- 编写基于 `subscribe_quote`、`subscribe_whole_quote` 的行情订阅回调逻辑。
- 编写 `run_time`、`schedule_run` 等定时任务。
- 迁移其他平台策略到 QMT，例如从 JoinQuant、PTRade、掘金或本地 Python 策略迁移。
- 查询 QMT 的行情数据、Tick 数据、财务数据、账号、持仓、委托、成交和撤单接口。
- 检查 `passorder` 下单参数，包括 `opType`、`orderType`、`prType`、`quickTrade`、`accountID` 和 `strAccountType`。
- 区分回测环境和实盘环境中的 API 差异。
- 让 Codex 生成更符合 QMT 嵌入式 Python 环境和接口约定的策略代码。

## 仓库内容

```text
skills/
  qmt-strategy-api/
    SKILL.md
    references/
      qmt_api_full.md
    agents/
      openai.yaml
```

文件说明：

- `skills/qmt-strategy-api/SKILL.md`：Codex skill 的主说明文件，定义何时使用、如何使用 QMT API 参考。
- `skills/qmt-strategy-api/references/qmt_api_full.md`：QMT API 本地参考文档，是该 skill 的核心资料。
- `skills/qmt-strategy-api/agents/openai.yaml`：Codex 界面展示用元数据。

## 安装前准备

请先确认：

- 已安装 Codex。
- 终端里可以运行 Python 3。
- 可以访问 GitHub。

如果你不确定 Python 是否可用，可以先运行：

```bash
python3 --version
```

Windows PowerShell 用户也可以运行：

```powershell
python --version
```

如果 Windows 上 `python` 不可用，可以试：

```powershell
py --version
```

## 安装方法

### macOS / Linux

在 Terminal 中运行：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo stepven8/qmt-strategy-api-skill \
  --path skills/qmt-strategy-api
```

### Windows

在 PowerShell 中运行：

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo stepven8/qmt-strategy-api-skill `
  --path skills/qmt-strategy-api
```

如果提示 `python` 无法识别，请改用 `py`：

```powershell
py "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo stepven8/qmt-strategy-api-skill `
  --path skills/qmt-strategy-api
```

安装完成后，请重启 Codex，让新的 skill 被加载。

安装后的目录通常是：

- macOS / Linux：`~/.codex/skills/qmt-strategy-api`
- Windows：`%USERPROFILE%\.codex\skills\qmt-strategy-api`

## 使用方法

重启 Codex 后，可以在对话中直接点名使用：

```text
$qmt-strategy-api
```

示例：

```text
使用 $qmt-strategy-api 写一个 QMT handlebar 策略，只在最新 K 线上判断信号并下单。
```

也可以这样描述任务：

```text
用 $qmt-strategy-api 检查这个 QMT 策略的 passorder 参数、账号类型和实盘回调是否写对。
```

Codex 使用该 skill 时，会优先搜索 `references/qmt_api_full.md`，再根据文档中的函数签名、参数联动、返回结构和注意事项生成代码或审查结果。

## 更新方法

如果之后仓库更新了 API 文档或 skill 内容，可以先删除旧版本，再重新安装。

macOS / Linux：

```bash
rm -rf ~/.codex/skills/qmt-strategy-api
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo stepven8/qmt-strategy-api-skill \
  --path skills/qmt-strategy-api
```

Windows PowerShell：

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\skills\qmt-strategy-api"
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo stepven8/qmt-strategy-api-skill `
  --path skills/qmt-strategy-api
```

更新后同样需要重启 Codex。

## 卸载方法

macOS / Linux：

```bash
rm -rf ~/.codex/skills/qmt-strategy-api
```

Windows PowerShell：

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\skills\qmt-strategy-api"
```

## 注意事项

- 该 skill 只提供 QMT API 使用指导和本地参考文档，不包含券商账号、交易权限、行情权限或任何实盘连接能力。
- 生成的策略代码在实盘使用前，必须先由使用者自行回测、仿真、审查风控逻辑，并确认目标 QMT 客户端支持相关 API。
- QMT 的嵌入式 Python 环境常见为 Python 3.6，策略代码应注意语法兼容性，并保留 `#coding:gbk` 编码声明。
- 不同 QMT 客户端版本、券商柜台、账号类型和 VIP 行情权限可能影响接口可用性，遇到差异时应以实际环境文档和测试结果为准。
- 涉及 `passorder`、撤单、资金、持仓、委托和成交查询的代码，需要特别核对参数联动、账号类型、数量单位、价格规则和状态枚举。

