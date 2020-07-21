.. _file_analytics_deploy:

----------------------
Files 分析: 部署
----------------------

概述
++++++++

在这个练习中，您将部署File Analytics VM并扫描现有的共享以构建仪表板。您还将创建异常警报并查看文件服务器实例的审计详细信息。

部署 File Analytics
+++++++++++++++++++++

#. 在 **Prism** > **File Server** >中点击 **Deploy File Analytics**

   .. figure:: images/31.png

#. 选择 **Deploy**

   为了节省时间，File Analytics 2.0.0包已经上传到您的集群中了。二进制文件可以从Nutanix门户下载并手动上传。

#. 上传完成后选择 **Install**

#. 填写详细信息

   - **Name** - 姓名首字母缩写
   - **Storage Container** – 会自动选择文件服务器实例所使用的container
   - **Network List** – Primary - Managed

#. 选择 **Show Advanced Settings**

#. 确保将 **DNS Resolver IP** 设置为你的 Active Directory, ntnxlab.local, domain controller/DNS IP address 并且 **仅有** 该地址。

#. 选择 **Deploy**

#. 您可以从 **Tasks** 页面监视部署。  The Analytics VM 部署大约需要5分钟。

#. 在 **Prism** > **File Server** > 中点击 **File Analytics**

   .. figure:: images/33.png

#. 在启用 File Analytics 页面上输入您的域管理员，该域管理员也是您的文件服务器管理员。

   - **Username**: administrator
   - **Password**: nutanix/4u

   .. figure:: images/34.png

#. 选择 **Enable**
