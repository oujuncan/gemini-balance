# S3 对象存储支持

本次更新添加了 S3 对象存储支持，使用户能够使用 Amazon S3 或任何兼容 S3 API 的服务（如 MinIO）作为图片上传的存储方案。

## 功能概述

- 支持 Amazon S3 原生服务
- 支持 MinIO 等 S3 兼容存储服务
- 可配置存储桶、区域、前缀等参数
- 支持自定义域名
- 完全集成到现有配置系统

## 变更摘要

1. 添加 `boto3` 依赖到 `requirements.txt`
2. 在 `app/utils/uploader.py` 中添加 `S3Uploader` 类
3. 在 `app/config/config.py` 中添加 S3 相关配置项
4. 更新 `app/templates/config_editor.html` 添加 S3 配置界面
5. 更新 `app/handler/response_handler.py` 中的图片处理
6. 在 `.env.example` 中添加 S3 配置示例
7. 添加 MinIO 配置文档 `docs/minio_setup.md`

## 配置参数说明

| 参数名 | 说明 | 示例值 |
|-------|------|-------|
| UPLOAD_PROVIDER | 设置为 `s3` 以启用 S3 存储 | s3 |
| S3_ACCESS_KEY | S3 访问密钥 ID | AKIAXXXXXXXXXXXXXXXX |
| S3_SECRET_KEY | S3 秘密访问密钥 | xxxxxxxxxxxxxxxxxxxxxxxx |
| S3_BUCKET_NAME | 存储桶名称 | my-images |
| S3_REGION | AWS 区域 | us-east-1 |
| S3_ENDPOINT_URL | 自定义终端节点 URL (适用于 MinIO) | http://minio.example.com:9000 |
| S3_PREFIX | 文件路径前缀 | uploads/ |
| S3_CUSTOM_DOMAIN | 自定义域名 | https://cdn.example.com |

## 使用方法

1. 在系统配置界面中，选择上传提供商为 "S3/MinIO"
2. 填写所需的 S3 配置参数
3. 保存配置
4. 现在所有图片上传将使用 S3 存储

## MinIO 特别说明

对于 MinIO 用户，必须设置以下参数：
- S3_ENDPOINT_URL: MinIO 服务器地址，例如 http://minio.example.com:9000
- S3_REGION: 通常设置为 us-east-1
- S3_BUCKET_NAME: 事先在 MinIO 中创建的存储桶名称

详细的 MinIO 配置指南可以参考 [docs/minio_setup.md](minio_setup.md) 文档。 