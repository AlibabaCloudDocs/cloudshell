# 编写Cloud Shell教程 {#concept_101359_zh .concept}

您可以在Cloud Shell中编写和启动教程，以帮助用户快速的熟悉您的项目。

单击[这里](https://shell.aliyun.com/?action=git_open&git_repo=https%3A%2F%2Fcode.aliyun.com%2Flabs%2Ftutorial-aliyun-cli.git&tutorial=tutorial.md)打开 Cloud Shell 教程模式。教程模式下，用户可以直接单击帮助文档中的命令行，在 Cloud Shell 中运行。

## 步骤一 创建教程文件 {#section_zjg_sfc_kgb .section}

完成以下操作，创建存储教程文档的文件夹：

1.  在Cloud Shell中创建一个文件夹用来存储创建的教程文档。

    **示例**

    ```
    mkdir tutorials
    ```

2.  在文件夹根目录中添加一个Markdown文件，比如`tutorial.md`。

    ```
    touch tutorial.md
    ```


## 步骤二 编写教程 { .section}

你可以使用Markdown语法来编写教程文档。在编写教程时，请遵循以下约定：

-   H1 \(\#\) 为教程标题。一个教程文档中仅能有一个H1标题。
-   H2 \(\#\#\) 为步骤标题。

-   您可以使用Markdown中的code语法在文档中增加一段代码。最终在您的教程文档中会渲染成一个代码块，并且带有一个复制按钮，点击会复制内容到Cloud Shell输入位置上。

    ```
    
    
    ```bash
    aliyun help
    ```    
    ```


参考以下示例，创建您的教程文档。您可以通过vim命令编写，也可以使用Cloud Shell的文本编辑器编写教程。

```
# 使用阿里云CLI来管理云资源

## 查看阿里云 CLI 帮助信息
```bash
aliyun help
```
### 首先
相关说明
### 然后
相关说明
```bash
echo world
```
## 恭喜完成教程
恭喜完成了学习如何使用阿里云 CLI的教程。
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/92352/155774032036789_zh-CN.png)

## 步骤三 发布教程 { .section}

完成以下操作，发布教程：

1.  在git push之前，你可以使用如下命令来预览测试您编写的教程，以检查是否还存在问题。

    ```
    teachme tutorial.md
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/92352/155774032136767_zh-CN.png)

2.  编写完成后，您就可以使用Cloud Shell内置的Git命令，将您的教程作为一个Git仓库，推送到您使用的Git中。

    **说明：** 您需要将仓库推送到一个公网可访问的仓库中。否则阅读您的教程的用户，可能无法将仓库clone到他的Cloud Shell中。


