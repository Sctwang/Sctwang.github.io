# 20190714

### 搭建 OSS 图床

- 时间：2019 年 07 月 14 日
- 测试链接：[Picture.jpg](https://mortre-images.oss-cn-beijing.aliyuncs.com/737487.png?Expires=1563101273&OSSAccessKeyId=TMP.hWTvydxy3pNRymz4LUr4v46NTV6UxQ2YiGqWgHJPbM1BcnRaF2wDBe5yyHBd7L8gwMkSPUEu2bsSVLTmK5bUvwvCUVZCHw33rZWQUoTpUQ3LqpXYNZkzfu9yLkoQam.tmp&Signature=%2FUECAwz7hDXzEOFok5Po0vd%2B1j8%3D)

### 步骤

> 阿里云已经有非常完整，详细的教程：
>
> [阿里云 对象存储 OSS 教程](https://help.aliyun.com/document_detail/31883.html?spm=a2c4g.11186623.6.566.327b7f85jsYttr)、[阿里云视频链接](https://help.aliyun.com/document_detail/31883.html?spm=a2c4g.11186623.6.566.310528bcZIUbSZ)

- 开通 OSS 服务

  - 仔细阅读相关说明，在了解清楚的情况下选择是否开通

- 创建存储空间

  - 创建 Bucket

  ![创建 Bucket](https://mortre-images.oss-cn-beijing.aliyuncs.com/OSS/20190714173033_Bucket.jpg)

  - **注意：存储空间创建后其名称地域和存储类型无法修改。**

- 上传文件

  - ![oss_upload.png](https://mortre-images.oss-cn-beijing.aliyuncs.com/OSS/20190714173617_oss_upload.png)

- 下载文件

  - ![oss_download.png](https://mortre-images.oss-cn-beijing.aliyuncs.com/OSS/20190714173829_oss_download.png)

- 删除文件

  - ![oss_delete.png](https://mortre-images.oss-cn-beijing.aliyuncs.com/OSS/20190714174018_oss_delete.png)

- 删除存储空间

  - [阿里云教程](https://help.aliyun.com/document_detail/31889.html?spm=a2c4g.11186623.2.19.68c21c62bIOKn9#task-pkl-qdp-tdb)



### 图形化管理工具 ossbrowser

> 可以通过图形化根据管理图床应用

- [下载链接](https://help.aliyun.com/document_detail/61872.html?spm=a2c4g.11186623.2.20.68c21c62W1CfpE#concept-xmg-h33-wdb)

- [阿里云教程](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.19.37f61144Im19MH#concept-53045-zh)
- [创建AccessKey](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.19.37f61144Im19MH#concept-53045-zh)

### 图床工具 PicGo

- 安装包[下载链接](https://github.com/Molunerfinn/PicGo/releases)

- 云栖社区也进行了介绍：https://yq.aliyun.com/articles/609826

- 此软件可以配置多种图床

  - 七牛图床
  - 微博图床
  - 阿里云 OSS
  - SM.MS
  - 腾讯云 COS
  - 又拍云图床
  - Imgur 图床

- 阿里云 OSS 图床配置记录：

  > ![阿里云OSS设置](https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190714233635.png)

- KeyId 和 KeySecret 需要进行设置，[链接](https://usercenter.console.aliyun.com/#/manage/ak)

![设置KeyId和KeySecret](https://mortre-picgo.oss-cn-beijing.aliyuncs.com/20190714234054.png)

- 存储空间：新建一个 Bucket，建议权限为公共读
- 存储区域：根据自己的服务器和[访问域名和数据中心](https://www.alibabacloud.com/help/zh/doc-detail/31837.htm?spm=a2c63.p38356.a3.3.179112f0PBtYui)进行确认