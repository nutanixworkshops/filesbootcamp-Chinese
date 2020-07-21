.. title:: Nutanix Files Bootcamp

.. toctree::
  :maxdepth: 2
  :caption:  Files Labs
  :name: _files_labs
  :hidden:

  files_smb_share/files_smb_share
  files_nfs_export/files_nfs_export
  files_file_blocking/files_file_blocking
  files_multiprotocol/files_multiprotocol

.. toctree::
  :maxdepth: 2
  :caption:  File Analytics Labs
  :name: _file_analytics_labs
  :hidden:

  file_analytics_scan/file_analytics_scan
  file_analytics_anomaly/file_analytics_anomaly

.. toctree::
   :maxdepth: 2
   :caption: Bonus Labs
   :name: _bonus
   :hidden:

   #peer/peer

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  files_deploy/files_deploy
  file_analytics_deploy/file_analytics_deploy
  files_expand_cluster/files_expand_cluster

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm

.. _getting_started:

---------------
Files 入门
---------------

欢迎来到 Nutanix Files 训练营!

该实验对应了一个由讲师指导的章节，其中介绍了Nutanix Files和许多常见的管理任务。 每个部分都有一个课程和一个练习，可为您提供动手练习。 讲师会讲角解练习并回答您可能有的其他问题。

通常来讲，文件存储一直是IT部门中的另一个孤岛，引入了不必要的复杂性，并且遭受了SAN存储中相同的扩展问题和缺乏持续创新的困扰。 Nutanix相信企业云中没有孤岛的空间。 通过将文件存储作为一种应用程序，并在经过验证的HCI Core之上运行于软件中，Nutanix Files通过一键式管理提供了高性能，可伸缩性和快速创新。

**在本实验中，您将逐步管理SMB共享和NFS导出，扩展环境并探索即将使用的Files功能。 该实验将提供有关部署，配置的关键注意事项，和应用场景。**

更新
++++++++++

- Workshop 更新了以下软件版本:
    - AOS & PC 5.11.2.x
    - Files 3.6.1.2
    - File Analytics 2.1.0

- 可选实验室更新:

议程
++++++

- Nutanix Files 实验
    - Files: 创建 SMB 共享
    - Files: 创建 NFS Export
    - Files: 选择性地文件屏蔽
    - Files 分析: Review Initial Scan（查看初始扫描）
    - Files 分析: Anomaly Rules（异常规则）


- 可选实验
    - Files: 部署
    - Files: 扩展集群
    - File Analytics: 部署

引言
+++++++++++++

- 姓名
- 熟悉Nutanix

初始化设置
+++++++++++++

- 注意使用的 *Passwords* 。
- 登录到你的实验环境 (连接信息如下所示)

实验环境信息
+++++++++++++++++++

Nutanix 研讨会是在Nutanix托管POC环境中运行。 将为您的群集提供完成练习所需的所有必要镜像，网络和虚拟机。

网络
..........

托管POC集群遵循标准的命名规则：

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

如果是从marketing pool配置:

- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

示例:

- **Cluster Name** - POC055
- **Subnet** - 10.21.55.0
- **Cluster IP** - 10.21.55.37

在整个实验中，有多个实例需要用正确的子网替换* XYZ *，例如：

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

每个群集配置有2个可用于虚拟机的VLAN：

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

用户凭证
...........

.. note::

 *<Cluster Password>* 对于每个集群都是唯一的，并将由讲师提供。

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

每个群集都有一个专用的域控制器虚拟机 **DC** ，负责为 **NTNXLAB.local** 域提供AD服务。 域中设置了以下用户和组：

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

访问说明
+++++++++++++++++++

可以通过多种不同方式访问Nutanix托管POC环境:

实验访问用户凭证
...........................

PHX 集群:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP 集群:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame VDI
.........

登录: https://frame.nutanix.com/x/labs

**Nutanix 员工** - 使用你的 **NUTANIXDC** 用户凭证
**合作伙伴** - 使用 **Lab Access User** 用户凭证

Parallels VDI
.................

PHX 集群登录: https://xld-uswest1.nutanix.com

RTP 集群登录: https://xld-useast1.nutanix.com

**Nutanix 员工** - 使用你的 **NUTANIXDC** 用户凭证
**合作伙伴** - 使用 **Lab Access User** 用户凭证

员工 Pulse Secure VPN
..........................

下载客户端:

PHX 集群登录: https://xlv-uswest1.nutanix.com

RTP 登录: https://xlv-useast1.nutanix.com

**Nutanix 员工** - 使用你的 **NUTANIXDC** 用户凭证
**合作伙伴** - 使用 **Lab Access User** 用户凭证

合伙伙伴可以使用百度盘下载安装对应版本的VPN客户端，链接:https://pan.baidu.com/s/1fG4baeO26XKHuzNjvsVHGA  密码:8zi8

安装客户端.

在Pulse Secure 客户端中, 点击 **Add** 添加一个连接:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanix 版本信息
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.16.x
- **PC Version** - 5.16.x
