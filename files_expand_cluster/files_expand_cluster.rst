.. _files_expand_cluster:

------------------------
Files: 集群扩展
------------------------

概述
++++++++

Files提供了横向扩展和纵向扩展部署的能力。 扩展Files VM的CPU和内存可以使环境支持更高的存储吞吐量和并发会话数。 当前，文件VM最多可以扩展到12个vCPU和96GB RAM。

Files可伸缩性的真正强大功能是能够简单地添加更多Files VM，并像底层Nutanix分布式存储结构一样横向扩展。 单个Files群集可以扩展到Nutanix群集中的物理节点数，从而确保在正常操作期间在单个节点上运行的File VM不超过1个。

扩展 Files 集群
++++++++++++++++++++++++++++++++++++

#. 返回 **Prism > File Server** 并选择 **BootcampFS**.

#. 点击 **Update > Number of File Server VMs**.

   .. figure:: images/25.png

#. 将Files VMs 的数据从 3 增加到 4 然后点击 **Next**。

   .. figure:: images/26.png

   请注意，客户端和存储网络都将使用额外的IP，以支持添加的Files VM。

#. 点击 **Next > Save**.

   群集现在将部署并打开第4个Files VM。 可以在 **Prism > Tasks** 中监视状态。

   .. note::

     Files群集扩展大约需要10分钟才能完成。

     扩展之后，验证客户端连接现在可以负载均衡到新的VM。

#. 通过RDP 或者控制台连接到你的 *姓名缩写*\ **-ToolsVM** 。

#. 打开 **Control Panel > Administrative Tools > DNS**.

#. 填写以下字段，然后点击 **OK**:

   - 选择 **The following computer**
   - 指定 **dc.ntnxlab.local**
   - 选择 **Connect to the specified computer now**

   .. figure:: images/28.png

#. 打开 **DC.ntnxlab.local > Forward Lookup Zones > ntnxlab.local** 并确认 **BootcampFS** 现在有四个条目。 Files 利用轮询DNS来平衡Files VM之间的连接。

   .. figure:: images/29.png

   .. note::

     如果仅存在三个条目，则可以通过选择Files集群并单击 **DNS** 来自动从 **Prism > File Server** 更新DNS条目。

重点回顾
+++++++++

关于 **Nutanix Files** 您应该了解哪些关键知识？

- Files 可以通过一键式性能优化来横向和纵向扩展。
