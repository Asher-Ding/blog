---
title: "Hugo如何增加主题"
date: 2023-06-01T15:11:38+08:00
draft: true
---

1. 增加主题
[参考](https://themes.gohugo.io/themes/hugo-theme-terminal/#how-to-start)
```bash
git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal
```
2. 更新主题
```bash
cd themes/poison

git pull

```
2. 使用主题，修改配置文件
```bash
# config.toml
theme = "terminal"
```
3. 启动服务器查看效果