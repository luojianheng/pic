# 维护说明 · pic 仓库

> 本文件是该仓库的**维护契约**。任何会话 / AI 改动本仓库前，请先读完本文件。

## 一、这是什么

「照片转手绘」站点源码仓库，GitHub Pages 托管，公开访问：

https://luojianheng.github.io/pic/

## 二、唯一权威代码源

| 项 | 值 |
|---|---|
| 仓库 | luojianheng/pic |
| 分支 | main |
| 部署 | GitHub Pages（分支直出，无构建步骤） |
| 入口 | index.html（单文件应用，含全部 HTML / CSS / JS） |

本仓库只有一份权威源。本机存在其他历史副本（腾讯云 IIS 版、云相册版），不属于本仓库；改动前务必确认改的是这一份。

## 三、写入通道

1. **SSH**：`git@github.com:luojianheng/pic.git`（推荐）
2. GitHub 官方 MCP 连接器：**只读**，写入会返回 403 `Resource not accessible by integration`

## 四、三条硬规矩

### 规矩 1 · 写前必读 SHA，写时必须带 SHA

更新已存在文件时必须提供当前 blob SHA。SHA 过期会被拒绝（409 Conflict），这是防误覆盖的保护，不要绕过。

### 规矩 2 · 改动后必须做字节校验

写完立刻比对三者一致才算完成：

```
本地文件 git blob SHA == 远端 blob SHA
线上文件 sha256       == 本地文件 sha256
```

体积相同不等于内容相同，只看「提交成功」不算验收通过。

### 规矩 3 · 不要用开启 autocrlf 的 git 操作本仓库

本仓库行尾是**混合**的：`index.html` 库内为 CRLF（1094 处），`README.md` 为纯 LF（78 行）。

Windows 上 `core.autocrlf` 常被系统级设为 `true`，一旦用它提交 `index.html`，会把整个文件转成 LF，**产生 1094 行假 diff** 污染历史。

- 克隆时即关闭：`git -c core.autocrlf=false clone <url>`
- 已克隆的仓库：`git config core.autocrlf false`，然后删掉被转换的文件重新检出

## 五、发布验收（三步）

1. 提交成功，记录新 commit SHA
2. 轮询线上 `https://luojianheng.github.io/pic/?cb=<随机数>`，直到响应字节数与本地一致（Pages 重建约 10~40 秒）
3. 本地 sha256 与线上 sha256 逐字节比对

## 六、能力边界

- 读 / 写 / 删除文件、查提交历史、按 SHA 回滚到任意历史版本
- 无法操作腾讯云服务器 129.204.124.78 上的 IIS 版（本机到该机网络不通）

---

最后更新：2026-09-20
