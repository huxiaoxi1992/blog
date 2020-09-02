---
title: udemy 课程下载
date: 2020-09-02 23:15:25
tags:
  - 教程
  - 软件
categories: 软件
---

# udemy 课程下载

1. 下载工具 `udemy-dl` 下载

   `git clone https://github.com/r0oth3x49/udemy-dl`

1. 安装依赖

   `pip3 install -r requirement.txt`

1. 查看课程 cookie 中 access_token 保存至 cookie.txt 文件

   `access_token=XXXXXXXXX`

1. 命令行运行

   `python3 udemy-dl.py https://{cource_url} -k cookie.txt --sub-lang en`
