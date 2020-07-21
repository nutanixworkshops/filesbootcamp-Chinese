.. _files_multiprotocol:

------------------------
Files: 多协议
------------------------

多协议
++++++++++++++

在本练习中，您将配置现有的SMB共享以也支持NFS。 启用多协议访问要求您配置用户映射并定义共享的本机和非本机协议。

配置用户映射
.......................

Nutanix Files共享具有本机和非本机协议的概念。 使用本地协议应用所有权限。
使用非本机协议的任何访问请求都需要用户或组映射到从本机端应用的权限。
有多种应用用户和组映射的方法，包括基于规则的映射，显式映射和默认映射。 您将首先配置默认映射。

#. 在 **Prism** > **File Server** > 中选择你的文件服务器，点击 **Protocol Management** > 然后点击 **User Mapping**

   .. figure:: images/53.png

#. 在 **User Mapping** 对方框中，至少两次单击 **Next** ， 直到您进入 **Default Mapping** 页面。

#. 在 **Default Mapping** 页面上，选择 **Deny access to NFS export** 和 **Deny access to SMB share** 作为未找到映射的默认值。

   .. figure:: images/54.png

#. 选择 **Next** 然后在 **Summary** 页面上选择 **Save** 以完成初始映射。

#. 在 **Prism** > **File Server** > **Share/Export** > 单击 Marketing share 然后选择 **Update**。

#. 在 **Basics** 页面的底部，选中 **Enable multiprotocol access for NFS** 复选框。

   .. figure:: images/55.png

#. 单击 **Next** 然后从 **Settings* 页面中检查 **Simultaneous access to the same files from both protocols**.

   .. figure:: images/56.png

#. 单击 **Next** ，然后从 **Summary** 页面中单击 **Save** 。

#. 通过SSH连接到 *姓名缩写*\ -NFS-Client VM.

#. 执行以下命令:

     .. code-block:: bash

       [root@CentOS ~]# mkdir /filesmulti
       [root@CentOS ~]# mount.nfs4 BootcampFS.ntnxlab.local:/Marketing /filesmulti
       [root@CentOS ~]# dir /filesmulti
       dir: cannot open directory /filesmulti: Permission denied
       [root@CentOS ~]#

   .. note:: mount操作区分大小写。

因为默认映射是拒绝访问，所以会出现“权限被拒绝”错误。 现在，您将添加一个显式映射，以允许访问非本机NFS协议用户。
我们将需要获取用户ID（UID）来创建显式映射。

#. 执行以下命令并注意 UID:

     .. code-block:: bash

       [root@CentOS ~]# id
       uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
       [root@CentOS ~]#

#. 在 **Prism** > **File Server** > 中，选择您的文件服务器，单击 **Protocol Management** > 然后单击 **User Mapping**

#. 单击 **Next** 直到进入 **Explicit Mapping** 页面

#. 单击 **+ Add one-to-one mapping**

#. 填入以下字段:

   - **SMB Name** - ntnxlab\\administrator
   - **NFS ID** - UID from previous step (0 if root)
   - **User/Group** - User

   .. figure:: images/57.png

#. 点击 **Actions** 列下的 **Save**

#. 点击 **Next** 直到进入 **Summary** 页面，然后单击 **Save**

#. 点击 **Close**

#. 返回到NFS-Client VM 并执行以下操作:

     .. code-block:: bash

       [root@CentOS ~]# dir /filesmulti
       MyMovie.flv Sample\ Data
       [root@CentOS ~]#
