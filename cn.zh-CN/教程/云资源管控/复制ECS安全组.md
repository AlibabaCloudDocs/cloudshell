# 复制ECS安全组 {#task_1942382 .task}

云命令Cloud Shell中预装了一个复制ECS安全组的脚本。您可以通过运行该脚本快速创建一个新的安全组。

安全组是一种虚拟防火墙，具备状态检测和数据包过滤功能，用于在云端划分安全域。您可以通过配置安全组规则，允许或禁止安全组内的ECS实例对公网或私网的访问。

1.  在浏览器中输入[https://shell.aliyun.com](https://shell.aliyun.com/)或在[OpenAPI Explorer](https://pre-api.aliyun.com/new#/cli)中进入Cloud Shell页面。
2.  执行以下命令，复制脚本。 

    ``` {#codeblock_jb7_2t0_iku}
    git clone https://code.aliyun.com/labs/tutorial-ecs-copy-security-group.git
    ```

3.  执行以下命令进入tutorial-ecs-copy-security-group目录。 

    ``` {#codeblock_y7h_149_oxg}
    cd tutorial-ecs-copy-security-group
    ```

4.  执行以下命令复制安全组。 

    ``` {#codeblock_hn0_61r_vq7}
    sh CloneSecurityGroup.sh
    ```

5.  根据提示完成以下操作，创建一个和源安全组规相同的安全组： 
    1.  输入源安全组的所属地域，例如cn-hangzhou。您可以直接按Enter键查看地域信息。
    2.  输入源安全组的ID。 您可以直接按Enter键查看指定地域下所有的安全组。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1541077/156765472358532_zh-CN.png)

    3.  输入目标新创建的安全组的所属地域，例如cn-shanghai。您可以直接按Enter键查看地域信息。
    4.  输入目标新创建的安全组的所属VPC ID。您可以直接按Enter键查看目标地域下的VPC信息。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1541077/156765472458533_zh-CN.png)

    5.  输入要创建的安全组名称和描述。您可以按Enter键略过名称和描述，直接创建安全组。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1541077/156765472458534_zh-CN.png)


安全组创建成功后，系统会返回安全组ID。您可以执行以下命令查看创建的安全组规则详情。

``` {#codeblock_p49_kh9_rij}
aliyun ecs DescribeSecurityGroupAttribute --RegionId cn-shanghai --SecurityGroupId sg-adf13******
```

其中：

-   RegionId是已创建的安全组的所属地域。
-   SecurityGroupId是已创建的安全组ID。

