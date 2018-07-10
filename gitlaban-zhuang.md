# GitLab的优点：Integrated teams working together

### GitLab是第一个针对产品，开发，QA，安全和运营团队的DevOps生命周期的所有阶段构建的单一应用程序，可以在同一个项目上同时工作。

### GitLab使团队能够通过单个对话进行协作和工作，而不是跨不同的工具管理多个线程。

### GitLab在DevOps生命周期内为团队提供单一数据存储，一个用户界面和一个权限模型，允许团队进行协作，显着缩短周期时间，并专注于快速构建优秀软件。

### [installation doc](https://about.gitlab.com/installation/#)

#### 这里以CentOS 7为例

* #### 安装必要依赖

  在CentOS 7（和RedHat / Oracle / Scientific Linux 7）上，以下命令还将在系统防火墙中打开HTTP和SSH访问。

```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```

接下来，安装Postfix以发送通知电子邮件。

如果要使用其他解决方案发送电子邮件，请跳过此步骤并在安装GitLab后[配置外部SMTP服务器](https://docs.gitlab.com/omnibus/settings/smtp.html)

```
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

在Postfix安装期间，可能会出现配置屏幕。

选择“Internet Site”并按Enter键。

使用服务器的外部DNS作为“邮件名称”，然后按Enter键。

如果出现其他屏幕，请继续按Enter键接受默认值。

* #### 添加GitLab软件包存储库并安装软件包

添加GitLab包存储库。

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

接下来，安装GitLab包。将'http://gitlab.example.com'更改为您要访问GitLab实例的URL。安装将自动配置并启动该URL的GitLab。HTTPS在安装后需要[其他配置](https://docs.gitlab.com/omnibus/settings/nginx.html#enable-https)。

```
sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee
```

* #### 浏览到主机名并登录

首次访问时，您将被重定向到密码重置屏幕。提供初始管理员帐户的密码，您将被重定向回登录屏幕。使用默认帐户的用户名`root`登录。

有关[安装和配置的详细说明，](https://docs.gitlab.com/omnibus/README.html#installation-and-configuration-using-omnibus-package)请参阅[文档](https://docs.gitlab.com/omnibus/README.html#installation-and-configuration-using-omnibus-package)。

