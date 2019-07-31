# 使用Terraform管理阿里云资源 {#concept_ekq_whc_kgb .concept}

云命令行\(Cloud Shell\)中预装了Terraform，Terraform是一种开源工具，用于安全高效地预配和管理云基础结构。您可以通过Terraform管理阿里云资源。

## 启动Cloud Shell {#section_nv3_43c_kgb .section}

选择一种方式启动云命令行：

-   在控制台中运行

    单击[控制台首页](https://home.console.aliyun.com/new?cloudshell=true)头部导航的命令行按钮，启动云命令行。

-   独立运行

    在浏览器中输入[https://shell.aliyun.com](https://shell.aliyun.com)或在 [OpenAPI Explorer](https://pre-api.aliyun.com/new#/cli) 中打开云命令行操作界面。

    您可以根据实际需要打开多个命令行窗口，单最多可同时打开5个云命令行窗口。


在启动云命令时，请注意：

-   第一次连接云命令行时会为您创建虚拟机，会消耗一些时间，最长不超过40秒。

-   打开多个云命令行窗口时，所有窗口都会连接到同一台虚拟机。虚拟机数量不会因为您打开新的命令行窗口而增加。


## 管理云资源 {#section_kfk_l3c_kgb .section}

1.  在Cloud Shell中编写Terraform模板。

    您可以使用vim命令直接编写模板，如果开通了OSS存储，您可以直接将配置模板上传到为Cloud Shell创建的bucket中。

    执行如下命令创建一个工程目录及模板文件

    ``` {#codeblock_rwg_wy6_9jo}
    mkdir terraform-project
    cd terraform-project 
    touch main.tf
    ```

    以下代码示例是一个创建ECS实例的Terraform模板，请将内容粘贴到 main.tf 中

    ``` {#codeblock_626_efe_3wq}
    provider "alicloud" {}
    
    resource "alicloud_vpc" "vpc" {
      name       = "tf_test_foo"
      cidr_block = "172.16.0.0/12"
    }
    
    resource "alicloud_vswitch" "vsw" {
      vpc_id            = "${alicloud_vpc.vpc.id}"
      cidr_block        = "172.16.0.0/21"
      availability_zone = "cn-beijing-b"
    }
    
    
    resource "alicloud_security_group" "default" {
      name = "default"
      vpc_id = "${alicloud_vpc.vpc.id}"
    }
    
    
    resource "alicloud_instance" "instance" {
      # cn-beijing
      availability_zone = "cn-beijing-b"
      security_groups = ["${alicloud_security_group.default.*.id}"]
    
      # series III
      instance_type        = "ecs.n2.small"
      system_disk_category = "cloud_efficiency"
      image_id             = "ubuntu_140405_64_40G_cloudinit_20161115.vhd"
      instance_name        = "test_foo"
      vswitch_id = "${alicloud_vswitch.vsw.id}"
      internet_max_bandwidth_out = 10
    }
    
    
    resource "alicloud_security_group_rule" "allow_all_tcp" {
      type              = "ingress"
      ip_protocol       = "tcp"
      nic_type          = "intranet"
      policy            = "accept"
      port_range        = "1/65535"
      priority          = 1
      security_group_id = "${alicloud_security_group.default.id}"
      cidr_ip           = "0.0.0.0/0"
    }
    ```

2.  执行init命令初始化。

    ``` {#codeblock_f8h_0wy_kdz}
    terraform init
    ```

3.  执行plan命令预览配置。

    ``` {#codeblock_40c_6m7_hww}
    terraform plan
    ```

4.  执行apply命令创建ECS实例。

    ``` {#codeblock_gs1_t3t_bhe}
    terraform apply
    ```


## 相关文档 {#section_e5p_lkc_kgb .section}

-   [什么是Terraform](../../../../../cn.zh-CN/Terraform/Terraform介绍/什么是Terraform.md#)
-   [Alibaba Cloud Module Registry](https://registry.terraform.io/browse?spm=a2c4g.11186623.2.11.59631190yBYmLL&provider=alicloud)

