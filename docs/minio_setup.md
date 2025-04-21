# MinIO 对象存储配置指南

本文档提供如何使用 MinIO 作为图片上传的对象存储服务的详细指南。MinIO 是一个高性能的开源对象存储服务，兼容 Amazon S3 API。

## 1. 使用 Docker 部署 MinIO

以下是使用 Docker 快速部署 MinIO 服务器的方法：

```bash
# 创建MinIO数据目录
mkdir -p ~/minio/data

# 运行MinIO容器
docker run -d \
  --name minio \
  -p 9000:9000 \
  -p 9001:9001 \
  -e "MINIO_ROOT_USER=minioadmin" \
  -e "MINIO_ROOT_PASSWORD=minioadmin" \
  -v ~/minio/data:/data \
  minio/minio server /data --console-address ":9001"
```

运行后，可以通过以下方式访问：
- MinIO API 端点: http://localhost:9000
- MinIO 控制台: http://localhost:9001

默认的访问凭证：
- 用户名: `minioadmin`
- 密码: `minioadmin`

## 2. 创建存储桶

部署 MinIO 后，需要创建一个用于存储图片的存储桶（bucket）：

1. 访问 MinIO 控制台 (http://localhost:9001)
2. 使用默认凭证登录
3. 点击左侧导航栏的 "Buckets"
4. 点击 "Create Bucket+"
5. 输入存储桶名称（例如 "images"）
6. 点击 "Create Bucket" 创建

## 3. 配置存储桶访问策略

要使上传的图片可公开访问，需要配置存储桶策略：

1. 在控制台中选择刚创建的存储桶
2. 点击 "Manage"
3. 在 "Access Policy" 部分，选择 "public"
4. 点击 "Add" 保存策略

## 4. 在应用中配置 MinIO

在应用的配置中添加 MinIO 相关信息：

```
UPLOAD_PROVIDER=s3
S3_ACCESS_KEY=minioadmin
S3_SECRET_KEY=minioadmin
S3_BUCKET_NAME=images
S3_REGION=us-east-1
S3_ENDPOINT_URL=http://minio.example.com:9000
S3_PREFIX=uploads/
```

配置说明：
- `UPLOAD_PROVIDER`: 设置为 `s3`
- `S3_ACCESS_KEY`: MinIO 的访问密钥（默认为 minioadmin）
- `S3_SECRET_KEY`: MinIO 的秘密密钥（默认为 minioadmin）
- `S3_BUCKET_NAME`: 存储桶名称
- `S3_REGION`: 区域名称（对于 MinIO 可设为 us-east-1）
- `S3_ENDPOINT_URL`: MinIO 服务器的 URL
- `S3_PREFIX`: 可选，文件路径前缀
- `S3_CUSTOM_DOMAIN`: 可选，自定义域名

## 5. 生产环境注意事项

在生产环境中使用 MinIO 时，请注意以下几点：

1. **安全凭证**: 更改默认的用户名和密码
2. **HTTPS**: 配置 SSL/TLS 以确保数据传输安全
3. **持久化存储**: 确保数据目录使用持久化存储
4. **备份策略**: 实施定期备份策略
5. **资源限制**: 根据使用情况调整资源限制

## 6. 使用 MinIO Client (mc)

MinIO Client (mc) 是一个命令行工具，用于访问 MinIO 和其他兼容 S3 的服务：

```bash
# 安装 mc
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
sudo mv mc /usr/local/bin/

# 配置 MinIO 服务器
mc alias set myminio http://localhost:9000 minioadmin minioadmin

# 列出存储桶
mc ls myminio

# 上传文件
mc cp myimage.jpg myminio/images/
```

## 7. 故障排除

- **无法连接到 MinIO**: 检查防火墙设置和网络连接
- **上传失败**: 验证存储桶访问权限和凭证
- **图片不可访问**: 检查存储桶的访问策略是否配置为公开

## 8. 参考资源

- [MinIO 官方文档](https://min.io/docs/minio/container/index.html)
- [MinIO Docker Hub](https://hub.docker.com/r/minio/minio/)
- [MinIO Client 文档](https://min.io/docs/minio/linux/reference/minio-mc.html) 