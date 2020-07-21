.. _files_smb_share:

----------------------------
Files: 创建 SMB 共享
----------------------------

概述
++++++++

在本练习中，您将创建和测试SMB共享，用于支持主目录，用户配置文件以及其他非结构化文件数据，例如Windows客户端通常访问的部门共享。

实验设置
+++++++++

本实验需要在 :ref:`windows_tools_vm` 中配置的应用程序。

如果尚未部署此VM，请在继续练习之前查看链接的步骤。

#. 在等待Files服务器部署时，如果尚未部署Windows Tools VM，则请进行此操作。

#. 通过RDP或控制台连接到Windows Tools VM

#. 将用于文件分析的示例文件下载到工具VM：

   - `https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip <https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>`_

使用 SMB 共享
++++++++++++++++

创建共享
..................

#. 在 **Prism > File Server** 中, 点击 **+ Share/Export**.

#. 填写以下字段:

   - **Name** - Marketing
   - **Description (Optional)** - Departmental share for marketing team
   - **File Server** - BootcampFS
   - **Share Path (Optional)** - 保持为空。 该字段允许您指定在其中创建嵌套共享的现有路径。
   - **Max Size (Optional)** - 保持为空。 该字段允许您为单个共享设置硬配额。
   - **Select Protocol** - SMB

   .. figure:: images/14.png

#. 点击 **Next**.

#. 选择 **Enable Access Based Enumeration** 和 **Self Service Restore**.

   .. figure:: images/15.png

   在创建部门共享时，应将其创建为 **Standard** 共享。 这意味着共享中的所有顶级目录和文件以及与共享的连接均由单个Files VM提供。

   **Distributed**共享适用于主目录，用户配置文件和应用程序文件夹。 这种类型的共享对所有文件VM的顶级目录进行分片，并在文件群集内的所有文件VM之间平衡连接的负载。

   **Access Based Enumeration (ABE)** 确保只有给定用户具有读取访问权限的文件和文件夹对该用户可见。 Windows文件共享通常启用此功能。

   **Self Service Restore** 允许用户利用Windows早期版本轻松地将单个文件还原到基于Nutanix快照的先前版本。

#. 点击 **Next**.

#. 查看 **Summary** 然后 **Create**.

   .. figure:: images/16.png

测试共享
.................

#. 通过RDP或控制台连接到您的 *姓名缩写*\ **-ToolsVM**

   .. note::

    工具虚拟机已加入** NTNXLAB.local **域。 您可以使用任何加入域的VM来完成以下步骤。

#. 在 **File Explorer** 中打开 ``\\BootcampFS.ntnxlab.local\`` .

   .. figure:: images/17.png

#. 通过将上一步中下载的SampleData_Small.zip文件解压缩到共享中，以测试对Marketing共享的访问。

   .. figure:: images/18.png

   - 在文件群集的部署过程中， **NTNXLAB\\Administrator** 用户被指定为文件管理员，默认情况下授予该用户对所有共享的读/写访问权限。
   - 管理其他用户的访问权限与任何其他SMB共享相同。

#. 右击 **Marketing > Properties**.

#. 选择 **Security** 选项卡，然后单击 **Advanced**.

   .. figure:: images/19.png

#. 选择 **Users (BootcampFS\\Users)** 然后单击 **Remove**.

#. 点击 **Add**.

#. 点击 **Select a principal** 然后在 **Object Name** 字段中指定 **Everyone** 。点击 **OK**.

   .. figure:: images/20.png

#. 填写以下字段，然后单击 **OK**:

   - **Type** - Allow
   - **Applies to** - This folder only
   - Select **Read & execute**
   - Select **List folder contents**
   - Select **Read**
   - Select **Write**

   .. figure:: images/21.png

#. 点击 **OK > OK > OK** 以保存权限更改。

   现在，所有用户都可以在Marketing共享中创建文件夹和文件。

   许多人利用配额来使用共享以确保公平使用资源是很常见的。 通过Files，可以为Active Directory中的单个用户或特定的Active Directory安全组基于共享设置软配额或硬配额。

#. 在 **Prism > File Server > Share > Marketing** 中, 单击 **+ Add Quota Policy**.

#. 填写以下字段，然后单击 **Save**:

   - Select **Group**
   - **User or Group** - SSP Developers
   - **Quota** - 10 GiB
   - **Enforcement Type** - Hard Limit

   .. figure:: images/22.png

#. 点击 **Save**.

#. 在选择 Marketing share 的情况下, 查看 **Share Details**, **Usage** 和 **Performance** 选项卡，以了解每个共享的可用性，包括文件和连接的数量、随时间变化的存储利用率、延迟、吞吐量和IOPS。

   .. figure:: images/23.png
