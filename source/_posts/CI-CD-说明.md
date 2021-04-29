---
title: CI/CD 说明
date: 2021-04-29 15:17:08
tags:
  - CI/CD
categories: CI/CD
---

CI/CD 包括`持续集成（CI）`, `持续部署（CD）`两个内容。

- `持续集成`的工作原理是将小的代码块推送到 Git 存储库中，并在每次推送时运行脚本管道来构建，测试和验证代码更改，然后再将其合并到主分支中。
- `持续部署`可在每次推送到存储库指定分支时将应用程序部署到不同的生产环境。

CI/CD 具有`细粒度`，`流程化`，`自动化`的特点。

因此使用 CI/CD 可以在开发周期内进行代码`检查与测试`，从而更有效的发现 BUG 和问题，确保生产环境下的运行稳定性；另一方面，CI/CD 能够`自动`完成产品的构建与发布，简化部署流程，提升开发与测试效率。

<!-- more -->

## CI/CD 的使用方法

### 前置基础知识

1. {% post_link Git-使用规范 Git %}

   CI/CD 工具都是通过 Git 代码仓库的提交，合并等事件触发。

1. {% post_link Linux-常用命令 命令行与脚本 %}

   CI/CD 所有步骤均通过编写 shell 脚本来指定，完成从代码下载到产品发布的整个流程。

1. [docker](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)
   CI/CD 服务通常运行在指定的 docker 镜像中，减少对环境的依赖。

### 常用的 CI 服务

- GitHub Actions
- Jenkins
- GitLab CI/CD
- CircleCI
- Travis CI
- Drone CI

### CI/CD 配置文件

几乎所有的 CI 服务都是通过 `yaml` 类型的配置文件完成对整个产品的测试，构建，部署流程。通常包含以下主要组成部分

#### 使用镜像

声明当前任务的运行环境，如前端应用的`node`环境，Spring 应用的`maven`环境，安卓应用的`gradle+androidSdk` 环境，以及上传 docker 镜像需要的`docker`环境。

#### 环境变量

环境变量会储存一些需要隐藏显示的账号，密码，token 等字符串，然后通过声明导入当前任务中。

#### 运行脚本

CI/CD 服务均通过编写的脚本逐行执行，自动地完成设计好的流程。

## CI/CD 的工作流程

![CI/CD](gitlab_workflow_example_11_9.png)

![DevOps](gitlab_workflow_example_extended_v12_3.png)

1. 验证

   - 通过持续集成自动构建和测试应用程序。
   - 使用代码质量分析源代码质量。
   - 使用浏览器性能测试评估代码更改对浏览器性能的影响。

1. 包管理

   - 存储并发布 Docker 镜像。
   - 使用 NPM Registry 存储并发布 NPM 包。

1. 发布

   - 持续部署，自动将应用程序部署到生产环境。
   - 完成安卓 apk 的自动构建与发布。
   - 部署前端产品。
   - 部署静态页面。
   - 将功能一部分进行更新，并让一定比例的用户群访问临时部署的功能。
   - 添加发行说明。
