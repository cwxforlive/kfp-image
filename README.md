<p align="center">
  <h1 align="center">Flink Stream Data Analysis Tool</h1>
  <p align="center">
    <a href="README_ZH.md"><strong>简体中文</strong></a> | <strong>English</strong>
  </p>
</p>

## Table of Contents

- [Repository Introduction](#repository-introduction)  
- [Prerequisites](#prerequisites)  
- [Image Specifications](#image-specifications)
- [Getting Help](#getting-help)
- [How to Contribute](#how-to-contribute)

## Repository Introduction  
Kubeflow is a machine learning (ML) toolkit that is dedicated to making deployments of ML workflows on Kubernetes simple, portable, and scalable.

Kubeflow pipelines are reusable end-to-end ML workflows built using the Kubeflow Pipelines SDK.

**Core Features:**

1.Automation and Orchestration: Automate complex multi-step ML tasks to reduce manual intervention and enhance efficiency.

2.Reproducibility: All details (code, data, parameters, environment) of each Pipeline run (referred to as a Run) are recorded and versioned. You can precisely reproduce the results of any experiment.

3.Component Reusability: Encapsulate each task into reusable components (Component), which can be used like building blocks in different Pipelines, promoting team collaboration and knowledge sharing.

4.Experiment Management: Easily compare the results of different Pipeline runs (such as the effects of different models and hyperparameters) to determine the best model.

5.Simplified Deployment: Provide the functionality to deploy trained models as inference services with a single click, bridging the gap from experimentation to production.

This project offers pre-configured [**Argo Workflows**](https://marketplace.huaweicloud.com/contents/992480da-64a3-4ba8-90cb-686d1832e96a#productid=OFFI1111485128289529856) images with Chroma and its runtime environment pre-installed, along with deployment templates. Follow the guide to enjoy an "out-of-the-box" experience.

> **System Requirements:**
> - CPU: 4GHz or higher  
> - RAM: 8GB or more  
> - Disk: At least 100GB  


## Prerequisites  
[Register a Huawei account and activate Huawei Cloud](https://support.huaweicloud.com/usermanual-account/account_id_001.html)

## Image Specifications  

| Image Version                                                                                                            | Description | Notes |  
|--------------------------------------------------------------------------------------------------------------------------|-------------|-------|  
| [kfp-2.14.0-kunpeng](https://github.com/HuaweiCloudDeveloper/argoWorkflows-image/tree/ArgoWorkflows-3.7.1-kunpeng)                    | Deployed on Kunpeng servers with Huawei Cloud EulerOS 2.0 64bit |  | 


## Getting Help
- Submit an [issue](https://github.com/HuaweiCloudDeveloper/kfp-image/issues)
- Contact Huawei Cloud Marketplace product support

## How to Contribute
- Fork this repository and submit a merge request.
- Update README.md synchronously based on your open-source mirror information.
