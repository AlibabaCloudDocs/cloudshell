# 使用Packer创建ECS自定义镜像 {#concept_265198 .concept}

Packer是一款轻量级的镜像定义工具，能够运行在常用的主流操作系统上。本教程介绍了通过创建一个JSON格式的Packer模板文件来构建ECS自定义镜像。

## 教程介绍 {#section_x50_p5o_syt .section}

以下步骤及示例均已在Alibaba Cloud Shell教程目录（tutorial-hashicorp-packer）中集成。您可以通过点击代码块右上角的**试用**按钮，快速体验通过Packer模板文件来构建您的ECS自定义镜像。

## 安装Packer {#section_6nn_a3w_g99 .section}

您可以在 [Packer官网](https://www.packer.io/downloads.html?spm=5176.12026607.tutorial.1.9c7732e2JOvQCp)页面获取不同平台最新版本的Packer，选择下载与您操作系统对应的版本。并通过以下示例或者访问[Packer官方安装说明](https://www.packer.io/intro/getting-started/install.html)来安装Packer。

``` {#codeblock_cgw_bfj_1ea}
mkdir -p $HOME/bin
curl https://shell-image-softwares-cn-shanghai.oss-cn-shanghai.aliyuncs.com/packer/packer_1_3_5.tar.gz | tar zxv -C $HOME/bin
```

## 定义Packer模板 {#section_qnk_6jm_rta .section}

使用Packer创建自定义镜像时，需要先创建一个JSON格式的模板文件。在该模板文件中，您需要指定创建自定义镜像的[Alicloud Image Builder（生成器）](https://www.packer.io/docs/builders/alicloud-ecs.html?spm=5176.12026607.tutorial.3.9c7732e2JOvQCp)和[Provisioners（配置器）](https://www.packer.io/docs/provisioners/index.html?spm=5176.12026607.tutorial.4.9c7732e2JOvQCp)。Packer具有多种配置器，可用于配置自定义镜像的内容生成方式。

本教程以常用的[Shell](https://www.packer.io/docs/provisioners/shell.html?spm=5176.12026607.tutorial.5.9c7732e2JOvQCp)配置器为例，定义了一个名为alicloud.json的Packer模板，模板内容如下：

``` {#codeblock_8rn_0nn_gq5}
{
  "variables": {
    "access_key": "{{env `ALICLOUD_ACCESS_KEY`}}",
    "secret_key": "{{env `ALICLOUD_SECRET_KEY`}}"
  },
  "builders": [{
    "type":"alicloud-ecs",
    "access_key":"{{user `access_key`}}",
    "secret_key":"{{user `secret_key`}}",
    "region":"cn-beijing",
    "image_name":"packer_basic",
    "source_image":"centos_7_02_64_20G_alibase_20170818.vhd",
    "ssh_username":"root",
    "instance_type":"ecs.n1.tiny",
    "internet_charge_type":"PayByTraffic",
    "io_optimized":"true"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "yum install redis.x86_64 -y"
    ]
  }]
}
```

**说明：** 

-   您可以执行cat alicloud.json命令查看模板详情。
-   本示例中，将创建一个含Redis的自定义镜像。

您可以通过以下参数自定义您的模板文件：

|参数名|说明|
|---|--|
|access\_key|您的AccessKey ID。|
|secret\_key|您的AccessKey Secret。|
|region|创建自定义镜像时使用临时资源的地域。|
|image\_name|自定义镜像的名称。|
|source\_image|基础镜像的名称，可以从阿里云公共镜像列表获得。|
|instance\_type|创建自定义镜像时生成的临时实例的类型。|
|internet\_charge\_type|创建自定义镜像时临时实例的公网带宽付费类型。|
|provisioners|创建自定义镜像时使用的Packer配置器类型。|

## 使用Packer创建自定义镜像 {#section_sp3_tsd_yvy .section}

在使用上述Packer模板文件创建ECS自定义镜像之前，您需要确保您已有ECS的相关操作权限。如果已开放权限，在Alibaba Cloud Shell环境下，可以直接执行如下命令来构建镜像。

``` {#codeblock_qbf_jgh_077}
packer build alicloud.json
```

示例运行结果如下：

``` {#codeblock_ux0_ewh_74t}
alicloud-ecs output will be in this color.
==> alicloud-ecs: Prevalidating alicloud image name...
alicloud-ecs: Found image ID: centos_7_02_64_20G_alibase_20170818.vhd
==> alicloud-ecs: Start creating temporary keypair: packer_59e44f40-c8d6-0ee3-7fd8-b1ba08ea94b8
==> alicloud-ecs: Start creating alicloud vpc
---------------------------
==> alicloud-ecs: Provisioning with shell script: /var/folders/3q/w38xx_js6cl6k5mwkrqsnw7w0000gn/T/packer-shell257466182
alicloud-ecs: Loaded plugins: fastestmirror
---------------------------
alicloud-ecs: Total                                              1.3 MB/s | 650 kB 00:00
alicloud-ecs: Running transaction check
---------------------------
==> alicloud-ecs: Deleting temporary keypair...
Build 'alicloud-ecs' finished.
==> Builds finished. The artifacts of successful builds are:
--> alicloud-ecs: Alicloud images were created:
cn-beijing: m-2ze12578be1oa4ovs6r9
```

## 更多信息 {#section_xff_jiq_hxt .section}

-   获取更多信息，请参见阿里云GitHub Packer仓库[packer-provider](https://github.com/alibaba/packer-provider?spm=5176.12026607.tutorial.7.9c7732e2JOvQCp)。
-   了解更多Packer使用详情，请参见[Packer官方文档](https://www.packer.io/docs/index.html?spm=5176.12026607.tutorial.8.9c7732e2JOvQCp)。

