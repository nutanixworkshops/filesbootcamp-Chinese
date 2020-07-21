.. _files_nfs_export:

------------------------
Files: 创建 NFS Export
------------------------

概述
++++++++

在本练习中，您将创建和测试NFSv4导出，该导出用于支持集群应用程序，存储应用程序数据（例如日志记录）或存储Linux客户端通常访问的其他非结构化文件数据。

保用 NFS Exports
+++++++++++++++++

启用 NFS Protocol
.....................

.. note::

   每个Files服务器只需要执行一次NFS协议启用，并且在您的环境中可能已经完成。 如果已启用NFS，请继续执行 `Creating the Export`_.

#. 在 **Prism Element > File Server** 中,选择您的文件服务器，然后单击 **Protocol Management > Directory Services**.

   .. figure:: images/29b.png

#. 选择 **Use NFS Protocol** 和 **Unmanaged** 用户管理和身份验证，然后单击 **Update**.

   .. figure:: images/30b.png

创建 Export
...................

#. 在 **Prism > File Server** 中, 点击 **+ Share/Export**.

#. 填写以下字段:

   - **Name** - logs
   - **Description (Optional)** - File share for system logs
   - **File Server** - BootcampFS
   - **Share Path (Optional)** - Leave blank
   - **Max Size (Optional)** - Leave blank
   - **Select Protocol** - NFS

   .. figure:: images/24b.png

#. 点击 **Next**.

#. 填写以下字段:

   - Select **Enable Self Service Restore**
      - 这些快照显示为NFS客户端的.snapshot目录。
   - **Authentication** - System
   - **Default Access (For All Clients)** - No Access
   - Select **+ Add exceptions**
   - **Clients with Read-Write Access** - *The first 3 octets of your cluster network*\ .* (e.g. 10.38.1.\*)

   .. figure:: images/25b.png

   默认情况下，NFS Export将允许对安装该导出的任何主机进行读/写访问，但是可以将其限制为特定的IP或IP范围。

#. 点击 **Next**.

#. 查看 **Summary** 然后 **Create**.

测试Export
..................

首先，您将提供一个CentOS VM，以用作文件导出的客户端。

.. note:: 如果您已经将ref_linux_tools_vm部署为另一个实验的一部分，则可以将此VM用作NFS客户端。

#. 在 **Prism > VM > Table** 中, 点击 **+ Create VM**.

#. 填写以下字段:

   - **Name** - *姓名缩写*\ -NFS-Client
   - **Description** - CentOS VM for testing Files NFS export
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 2 GiB
   - Select **+ Add New Disk**
      - **Operation** - Clone from Image Service
      - **Image** - CentOS
      - Select **Add**
   - Select **Add New NIC**
      - **VLAN Name** - Secondary
      - Select **Add**

#. 点击 **Save**.

#. 选择 *姓名缩写*\ **-NFS-Client** VM 点击 **Power on**.

#. 在Prism中记下VM的IP地址，并使用以下凭据通过SSH连接：

   - **Username** - root
   - **Password** - nutanix/4u

#. 执行以下操作:

     .. code-block:: bash

       [root@CentOS ~]# yum install -y nfs-utils #This installs the NFSv4 client
       [root@CentOS ~]# mkdir /filesmnt
       [root@CentOS ~]# mount.nfs4 BootcampFS.ntnxlab.local:/ /filesmnt/
       [root@CentOS ~]# df -kh
       Filesystem                      Size  Used Avail Use% Mounted on
       /dev/mapper/centos_centos-root  8.5G  1.7G  6.8G  20% /
       devtmpfs                        1.9G     0  1.9G   0% /dev
       tmpfs                           1.9G     0  1.9G   0% /dev/shm
       tmpfs                           1.9G   17M  1.9G   1% /run
       tmpfs                           1.9G     0  1.9G   0% /sys/fs/cgroup
       /dev/sda1                       494M  141M  353M  29% /boot
       tmpfs                           377M     0  377M   0% /run/user/0
       BootcampFS.ntnxlab.local:/             1.0T  7.0M  1.0T   1% /afsmnt
       [root@CentOS ~]# ls -l /filesmnt/
       total 1
       drwxrwxrwx. 2 root root 2 Mar  9 18:53 logs

#. 注意 **logs** 目录已经挂载到  ``/filesmnt/logs``.

#. 重新启动VM，并观察到export不再挂载。 要保持挂载，通过执行以下命令将其添加到“ ``/etc/fstab`` 中:

     .. code-block:: bash

       echo 'BootcampFS.ntnxlab.local:/ /filesmnt nfs4' >> /etc/fstab

#. 以下命令会将100个2MB的随机数据文件添加到 ``/filesmnt/logs`` 中:

     .. code-block:: bash

       mkdir /filesmnt/logs/host1
       for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/filesmnt/logs/host1/file$i; done

#. 返回到 **Prism > File Server > Share > logs** 监视性能和使用情况。

   请注意，利用率数据每10分钟更新一次。
   
