# 通过Terraform部署Kubernetes托管版集群 {#concept_zkx_sxd_kgb .concept}

您可以通过Terraform部署一个阿里云容器服务的Kubernetes托管版集群。

## 教程介绍 {#section_tsg_hyd_kgb .section}

本教程以下如下Terraform模板为例。该模板已经加载到Cloud Shell的教程目录中（terraform-kubernetes-wordpress/kubernetes）。

```
// Instance_types data source for instance_type
data "alicloud_instance_types" "default" {
  cpu_core_count = "${var.cpu_core_count}"
  memory_size    = "${var.memory_size}"
}

// Zones data source for availability_zone
data "alicloud_zones" "default" {
  available_instance_type = "${var.worker_instance_type == "" ? data.alicloud_instance_types.default.instance_types.0.id : var.worker_instance_type}"
}

resource "alicloud_cs_managed_kubernetes" "k8s" {
  name                  = "${var.k8s_name_prefix == "" ? format("%s-%s", var.example_name, format(var.number_format, count.index+1)) : format("%s-%s", var.k8s_name_prefix, format(var.number_format, count.index+1))}"
  availability_zone     = "${lookup(data.alicloud_zones.default.zones[count.index%length(data.alicloud_zones.default.zones)], "id")}"
  new_nat_gateway       = true
  worker_instance_types = ["${var.worker_instance_type == "" ? data.alicloud_instance_types.default.instance_types.0.id : var.worker_instance_type}"]
  worker_numbers        = ["${var.k8s_worker_number}"]
  worker_disk_category  = "${var.worker_disk_category}"
  worker_disk_size      = "${var.worker_disk_size}"
  password              = "${var.ecs_password}"
  pod_cidr              = "${var.k8s_pod_cidr}"
  service_cidr          = "${var.k8s_service_cidr}"
  install_cloud_monitor = true
  kube_config           = "~/.kube/config"
}
				
```

完成本教程后，您会创建以下资源。其中，容器服务没有任何附加费用，您只需要支付所使用资源（云服务器、 负载均衡等）的费用。

-   Worker 实例（ECS）
    -   实例规格：ecs.n2.medium
    -   实例数量：3
    -   系统盘：20G 高效云盘
-   负载均衡
    -   实例数量：2
    -   付费模式：按量付费
-   弹性公网 IP
    -   实例数量：1
    -   付费模式：使用流量计费
-   NAT 网关
    -   实例数量：1
    -   付费模式：按量付费

具体计费信息，参见 [ECS 计费概述](https://help.aliyun.com/document_detail/25398.html)、[负载均衡按量计费](https://help.aliyun.com/document_detail/27692.html)、[弹性公网 IP 按量计费](https://help.aliyun.com/document_detail/72156.html)、[NAT 网关按量计费](https://help.aliyun.com/document_detail/88658.html)。

## 使用限制 {#section_pbm_pyd_kgb .section}

在开始使用本教程前，确保您已经了解以下限制并满足相关要求：

-   保证您的账户有100元的余额并通过实名认证，否则无法创建按量付费的ECS实例和负载均衡。
-   随集群一同创建的负载均衡实例只支持按量付费的方式。
-   Kubernetes集群仅支持专有网络VPC。
-   您的每个账号默认可以创建的云资源有一定的配额，如果超过配额，集群创建失败。如果您需要提高配额，请提交工单申请。
    -   每个账号默认最多可以创建100个安全组。
    -   每个账号默认最多可以创建60个按量付费的负载均衡实例。
    -   每个账号默认最多可以创建20个EIP。
-   在开始之前，确保您已开通了以下云服务：
    -   [容器服务](https://cs.console.aliyun.com/)
    -   [资源编排](https://ros.console.aliyun.com/)
    -   [弹性伸缩](https://essnew.console.aliyun.com/)
    -   [访问控制](https://ram.console.aliyun.com/)

## 创建托管版 Kubernetes 集群 {#section_jhz_kzd_kgb .section}

完成以下操作，创建Kubernetes集群：

1.  执行以下命令定位到用来创建 Kubernetes集群的Terraform模板的目录。

    ```
    cd ~/terraform-kubernetes-wordpress/kubernetes
    ```

2.  执行init命令加载[Alibaba Cloud Providers](https://www.terraform.io/docs/providers/alicloud/index.html)。

    ```
    terraform init
    ```

3.  执行以下命令部署集群。

    ```
    terraform apply
    ```

    **说明：** 如果出现 `ErrManagedKuberneteRoleNotAttach` 的错误，请检查所需服务是否开通，以及您的账号是否通过了实名认证同时账户余额大于100元。

    部署成功后，系统会返回集群ID，如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/93413/155774184536879_zh-CN.png)

    Kubernetes的Kube Config文件会存储在~/.kube目录下。您可以登录[容器服务控制台](https://cs.console.aliyun.com/)查看通过Terraform创建的Kubernetes集群。

    您可以通过以下参数自定义您的Kubernetes集群：

    -   worker\_instance\_type：Worker实例规格
    -   worker\_disk\_category：Worker实例系统盘
    -   worker\_disk\_size：Worker实例系统盘容量
    -   ecs\_password：Worker实例登录密码
    -   k8s\_worker\_number：Worker实例数量
    -   k8s\_name\_prefix：集群名称前缀
4.  执行以下命令销毁创建的集群。

    ```
    cd ~/terraform-kubernetes-wordpress/kubernetes
    terraform destroy
    ```


