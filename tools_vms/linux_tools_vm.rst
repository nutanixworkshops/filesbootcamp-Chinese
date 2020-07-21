.. _linux_tools_vm:

---------------
Linux Tools VM
---------------

概述
+++++++++

此CentOS VM镜像将与用于支持多个实验练习的软件包一起制备。

如果在 **Lab Setup** 中指示这么做，请在指定的集群上部署这个VM。

.. raw:: html

  <strong><font color="red">只部署VM一次，不需要在任何实验完成时对其进行清理。</font></strong>

部署CentOS
++++++++++++++++

在 **Prism Central** > select :fa:`bars` **> Virtual Infrastructure > VMs** 中, 点击 **Create VM**.

填写以下字段:

- **Name** - *Initials*-Linux-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 2 GiB

- Select **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - Linux_ToolsVM.qcow2
    - Select **Add**

- Select **Add New NIC**
    - **VLAN Name** - Secondary
    - Select **Add**

点击 **Save** 以创建VM.

将 VM开机.
