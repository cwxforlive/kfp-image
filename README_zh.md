<p align="center">
  <h1 align="center">开源镜像商品</h1>
  <p align="center">
    <a href="README.md"><strong>English</strong></a> | <strong>简体中文</strong>
  </p>
</p>

## 目录

- [仓库简介](#仓库简介)
- [前置条件](#前置条件)
- [镜像说明](#镜像说明)
- [获取帮助](#获取帮助)
- [如何贡献](#如何贡献)

## 仓库简介
kfp(Kubeflow pipelines)是使用 Kubeflow Pipelines SDK 构建的可复用的端到端 ML 工作流。kubeflow是一款机器学习 (ML) 工具包，致力于简化 Kubernetes 上 ML 工作流的部署，使其更加便捷、可移植且可扩展。

**核心特性：**

1.自动化与编排： 将复杂的多步骤ML任务自动化，减少人工干预，提高效率。

2.可复现性： 每次Pipeline运行（称为一个Run）的所有细节（代码、数据、参数、环境）都会被记录和版本化。你可以精确地复现任何一次实验的结果。

3.组件复用： 将每个任务封装成可复用的组件（Component），可以在不同Pipeline中像搭积木一样使用，促进团队协作和知识共享。

4.实验管理： 轻松比较不同Pipeline运行的结果（如不同模型、不同超参的效果），从而确定最佳模型。

5.简化部署： 提供将训练好的模型一键部署为推理服务的功能，打通从实验到生产的路径。

本项目提供的开源镜像商品 [**`kfp`**](https://marketplace.huaweicloud.com/contents/992480da-64a3-4ba8-90cb-686d1832e96a#productid=OFFI1111485128289529856)，已预先安装 chroma 软件及其相关运行环境，并提供部署模板。快来参照使用指南，轻松开启“开箱即用”的高效体验吧。

> **系统要求如下：**
> - CPU: 4GHz 或更高
> - RAM: 8GB 或更大
> - Disk: 至少100GB
## 前置条件
[注册华为账号并开通华为云](https://support.huaweicloud.com/usermanual-account/account_id_001.html)

## 镜像说明

| 镜像规格                                                                                                                     | 特性说明 | 备注 |
|--------------------------------------------------------------------------------------------------------------------------| --- | --- |
| [kfp-2.14.0-kunpeng](https://github.com/HuaweiCloudDeveloper/chroma-image/tree/Chroma-1.0.16-kunpeng) | 基于 鲲鹏服务器 + Huawei Cloud EulerOS 2.0 64bit 安装部署 |  |

## 获取帮助
- 更多问题可通过 [issue](当前仓库issue地址) 或 华为云云商店指定商品的服务支持 与我们取得联系
- 其他开源镜像可看 [open-source-image-repos](https://github.com/HuaweiCloudDeveloper/open-source-image-repos)

## 如何贡献
- Fork 此存储库并提交合并请求
- 基于您的开源镜像信息同步更新 README.md
