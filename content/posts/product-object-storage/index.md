---
title: "Product: Object Storage Comparison"
date: 2025-06-21T14:00:00+08:00
draft: false
author: "shenqianjin"
authorLink: "https://shenqianjin.github.io/"
description: ""
license: ""
images: []
tags: [Storage]
categories: [Storage]
#featuredImage: "cover.png"
featuredImagePreview: ""
---

## 什么是对象存储?

### 什么是 Amazon S3?
> Amazon Simple Storage Service（Amazon S3）是一项对象存储服务，在可扩展性、数据可用性、安全性和能效方面业界领先。
> 数百万不同规模和行业的客户可以为几乎任何应用场景存储、管理、分析和保护任意数量的数据，例如数据湖、云原生应用程序和移动应用程序。
> 借助高成本效益的存储类和易于使用的管理功能，您可以优化成本、组织并分析数据，以及配置精细调整过的访问控制，从而满足特定的业务和合规性要求。

## 存储管理

### 特殊说明: AWS Glacier


## 产品优势

### S3 优势
- `可扩展性`: 您可以使用 S3 存储几乎任何数量的数据，一直到 EB 级，性能无与伦比。S3 具有完全的弹性，可以随着您增减数据的操作自动扩展和收缩。
  无需预置存储，只需按实际使用量付费。
- `持久性和可用性`: Amazon S3 提供最持久的云端存储和业界领先的可用性。基于其独特的架构，S3 设计为默认提供 99.999999999%（11 个 9）的
  数据持久性和 99.99% 的可用性，并以云端最强的 SLA 为后盾。
- `安全性和数据保护`: 通过无与伦比的安全性、数据保护、合规性和访问控制功能保护您的数据。S3 是安全、私有且默认加密的，
  还支持多种审计功能来监控对 S3 资源的访问请求。
- `最低价格和最高性能`: S3 为任何工作负载和自动数据生命周期管理提供具有最佳性价比的多种存储类，因此您可以以经济高效的方式存储大量经常、
  不经常或很少访问的数据。S3 提供弹性、灵活性、延迟和吞吐量，确保存储永不限制性能。


### 数据管理
借助 S3 存储`桶名称`、`前缀`、`对象标签`、`S3 Metadata` 和 `S3 清单`，您可以通过各种方式来分类和报告数据，然后配置其他 S3 功能以执行操作。

Amazon S3 批量操作都可让您轻松管理 Amazon S3 中任意规模的数据。使用 S3 批量操作时，只需一个 S3 API 请求或者在 S3 控制台中操作几步，就可在存储桶之间复制对象，替换对象标签集，修改访问控制，以及从 Amazon S3 Glacier Flexible Retrieval 以及 Amazon S3 Glacier Deep Archive 存储类中恢复归档的对象。您还可以使用 S3 批量操作跨对象运行 AWS Lambda 函数，用于运行自定义的业务逻辑，例如处理数据或者转码图像文件。要开始使用，请选择源存储桶和筛选器，或者使用 S3 清单报告或提供自定义列表来指定目标对象列表，然后从预先填充的菜单中选择所需的操作。S3 分批操作请求完成后，您会收到通知以及有关全部更改的完成报告。观看视频教程以了解有关 S3 批量操作的更多信息。

Amazon S3 Metadata 提供准实时的可查询对象元数据，从而整理数据并加速数据发现。该功能可以帮助您整理和识别 S3 数据，并将这些数据用于业务分析、实时推理应用程序等。S3 Metadata 支持对象元数据，其中包括系统定义的详细信息（例如对象的大小和来源）和自定义元数据，允许您使用标签为对象添加商品 SKU、交易 ID、内容评级等信息作为注释。S3 Metadata 设计为在将对象上传到存储桶时自动捕获对象中的元数据，并使这些元数据可在只读表中查询。随着存储桶中的数据发生更改，S3 Metadata 在几分钟内即可更新表以反映最新更改。


## OSS
阿里云对象存储 OSS（Object Storage Service）是一款海量、安全、低成本、高可靠的云存储服务，提供最高可达 99.995 % 的服务可用性。多种存储类型供选择，全面优化存储成本。
### 存储类型
||标准类型||||
### 基础能力
### 数据迁移

### 数据处理
### 容灾备份
### 安全合规



Amazon S3
Google Cloud Storage（GCS）
Azure Blob Storage
OpenStack Swift

COS
Kodo

comparison

### Reference
https://aws.amazon.com/cn/s3/

https://www.aliyun.com/product/oss
https://cloud.tencent.com/product/cos
https://www.qiniu.com/products/kodo

- 使用Lambda在检索时动态添加水印: https://aws.amazon.com/cn/getting-started/hands-on/amazon-s3-object-lambda-to-dynamically-watermark-images/
- https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance.html