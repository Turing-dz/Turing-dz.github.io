---
title: "不翻墙使用Claude-code"
date: '2026-02-21T12:31:27+08:00'
draft: false
github_link: "https://turing-dz.github.io/"
author: "Zoe"
tags:
  - life
image: /images/blogs/claudecode.jpeg
description: "不翻墙使用Claude-code"
toc: 

---

claude经常封号，所以索性使用第三方api，不用翻墙使用。

# 1.安装claude_code
首先去[官网](https://code.claude.com/docs/en/overview)根据设备型号进行安装。
```bash
curl -fsSL https://claude.ai/install.sh | bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
claude -v
```


# 2.接入第三方API

1.使用[第三方中转](https://referer.shadowai.xyz/r/1044709)，购买key。然后进行如下配置

```bash
mkdir -p ~/.claude
vi  ~/.claude/settings.json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://api.openai-proxy.org/anthropic"
  }
}
```
2.进行免登录配置。

```bash
vi ~/.claude.json
"hasCompletedOnboarding": true
```
3.最后在自己的项目目录下启动cluade使用。

```bash
claude
```
