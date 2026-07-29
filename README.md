# httpload

一个零第三方依赖的命令行 HTTP GET 压测工具，支持有界并发、单请求超时、结果统计和 Ctrl+C 优雅退出。

## 环境要求

- Node.js 18.17 或更高版本
- 无需执行 `npm install`

## 快速运行

先在一个终端启动本地测试服务：

```powershell
npx serve -l 8080
```

再打开另一个 PowerShell：

```powershell
cd C:\Users\81984\Desktop\张浩
.\httpload.cmd -u http://localhost:8080/ -n 1000 -c 20 -t 2s
```

也可以直接运行 Node.js 入口：

```powershell
node .\bin\httpload.js -u http://localhost:8080/ -n 1000 -c 20 -t 2s
```

## 参数

| 参数 | 短参数 | 说明 |
|---|---|---|
| `--url` | `-u` | HTTP/HTTPS 目标地址 |
| `--requests` | `-n` | 计划请求总数，必须为正整数 |
| `--concurrency` | `-c` | 最大并发数，必须为正整数 |
| `--timeout` | `-t` | 单请求超时，支持 `ms`、`s`、`m`，如 `500ms`、`2s` |
| `--help` | `-h` | 显示帮助 |
| `--version` | `-v` | 显示版本 |

当并发数大于总请求数时，实际并发数会自动降为总请求数，报告中会同时显示请求的并发数和实际并发数。

## 结果与中止

- `Succeeded`：HTTP 200～299；`Non-2xx`：其他状态码。
- `Errors`：网络错误；`Timeouts`：单请求超时。
- 始终满足 `Completed = Succeeded + Non-2xx + Errors + Timeouts`。
- 延迟只统计 `Completed` 请求；用户中止的请求单独计入 `Cancelled`。

压测时按 Ctrl+C，工具会停止调度、取消在途请求并输出部分统计。

## 自动化测试

执行：

```powershell
npm test
```

共 10 项测试，使用本地随机端口且不依赖公网，覆盖参数校验、并发上限、结果分类、响应正文超时、统计守恒、延迟口径和主动中止。

## 完成情况

- 全部必做功能、运行说明、示例命令和自动化测试均已完成。
- 未完成的必做功能：无。
- 尚未上传 GitHub；该项是题目中的推荐项。
