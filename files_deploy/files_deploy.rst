.. _files_deploy:

-------------
Files: 部署
-------------

.. _deploying_files:

部署 Files
++++++++++++

#. 在 **Prism > File Server** 点击 **+ File Server** 以打开 **New File Server Pre-Check** 窗口。

   .. figure:: images/1.png

   为了节省时间，文件已被上传到您的集群。 文件二进制文件可以直接通过Prism下载，也可以手动上传。

   .. figure:: images/2.png

   此外，群集的 Data Services IP地址已配置 (10.XX.YY.38)。 在文件群集中，存储通过iSCSI作为卷组提供给Files虚拟机，因此依赖于数据服务IP。

   .. note::

     如果你要搭建自己的环境， 则可以通过以下方式轻松配置数据服务IP :fa:`gear` **> Cluster Details**, 指定 **iSCSI Data Services IP**, 然后点击 **Save**. 当前, 数据服务IP必须与CVM在同一子网中。

     最后，Files将确保在群集上至少配置了1个网络。 建议至少有2个网络在客户端网络和存储网络之间进行分段。

#. 点击 **Continue**.

   .. figure:: images/3.png

#. 填写以下字段：

   - **Name** - *姓名缩写*-Files (e.g. XYZ-Files)
   - **Domain** - ntnxlab.local
   - **File Server Size** - 1 TiB

   .. figure:: images/4.png

   .. note::

     点击 **Custom Configuration** 将允许您根据用户和吞吐量目标横向和纵身扩展Files VM配置。 还允许手动调整Files群集的大小。

     .. figure:: images/5.png

#. 点击 **Next**.

#. 选择 **Secondary - Managed** VLAN 的 **Client Network**.

    每个Files VM将在客户端网络上使用一个IP。

   .. note::

     在HPOC环境中，如果使用单独的客户端网络和存储网络，则对于客户端网络使用Secondary VLAN至关重要。

     在生产环境中，通常需要使用专用虚拟网络部署文件以用于客户端和存储流量。 当使用两个网络时，根据设计，Files将禁止客户端访问存储网络，这意味着分配给主网络的VM将无法访问共享。

   .. note::

     由于这是AHV管理的网络，因此不需要配置单个IP。 在ESXi环境中，或使用不受管理的AHV网络，您将指定网络详细信息和可用IP，如下所示。

     .. figure:: images/6.png

#. 指定你集群的 **Domain Controller** VM IP (found in :ref:`stagingdetails`) 作为 **DNS Resolver IP** (例如 10.XX.YY.40). 保留默认的 (cluster) NTP 服务器。

   .. raw:: html

     <strong><font color="red">为了使Files群集成功找到并加入NTNXLAB.local域，将DNS解析服务器IP设置为您的集群的域控制器VM IP是至关重要的。 默认情况下，此字段设置为Nutanix群集配置的主要DNS服务器IP，此值不正确，将不起作用。</font></strong>

   .. figure:: images/7.png

#. 点击 **Next**.

#. 选择 **Primary - Managed** VLAN 用于存储网络。

   每个Files VM将在存储网络上使用一个IP，另外再为群集增加1个其他IP。

   .. figure:: images/8.png

#. 点击 **Next**.

#. 填定以下字段:

   - 选择 **Use SMB Protocol**
   - **Username** - Administrator@ntnxlab.local
   - **Password** - nutanix/4u
   - 选择 **Make this user a File Server admin**
   - 选择 **Use NFS Protocol**
   - **User Management and Authentication** - Unmanaged

   .. figure:: images/9.png

   .. note:: 在非托管模式下, 仅通过 UID/GID标识用户。 在 Files 3.5中, Files 支持 NFSv3 和 NFSv4。

#. 点击 **Next**.

   默认情况下，Files 将自动创建一个保护域，以获取Files群集的每日快照并保留前两个快照。 部署后，可以修改快照计划并定义远程复制站点。

   .. figure:: images/10.png

#. 点击 **Create** 开始部署Files。

#. 在 **Prism > Tasks** 中监视部署进度。

   部署大约需要10分钟。

   .. figure:: images/11.png

   .. note::

   如果您收到有关DNS记录验证失败的警告，则可以安全地将其忽略。 共享群集不使用与Files群集相同的DNS服务器，因此无法解析在部署Files时创建的DNS条目。

#. 转到 **Prism > File Server** 然后选择 *姓名缩写*\ **-Files** 服务器单击 **Protect**.

   .. figure:: images/12.png

#. 遵守默认的自助服务还原计划，此功能控制Windows以前版本功能的快照计划。 支持早期版本允许最终用户回滚对文件的更改，而无需聘请存储或备份管理员。 请注意，这些本地快照不能保护Files服务器群集免受本地故障的影响，并且可以将整个Files服务器群集复制到远程Nutanix群集。 点击闭 **Close** 。

   .. figure:: images/13.png

重点回顾
+++++++++

关于 **Nutanix Files** 您应该了解哪些关键知识？

- Files可以快速部署在现有Nutanix群集之上，从而为用户共享，主目录，部门共享，应用程序和任何其他通用文件存储需求提供SMB和NFS存储。
- Files不是单一解决方案。 VM，文件，块和对象存储都可以在同一平台上使用相同的管理工具来交付，从而降低了复杂性和管理孤岛。
