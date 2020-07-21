.. _windows_tools_vm:

----------------
Windows Tools VM
----------------

概述
+++++++++

这个Windows Server 2012 R2镜像预装了一些工具，包括:

- Microsoft Remote Server Administration Tools (RSAT)
- PuTTY, CyberDuck, WinSCP
- Sublime Text 3, Visual Studio Code
- OpenOffice
- Python
- pgAdmin
- Chocolatey Package Manager

如果在 **Lab Setup** 中指示这么做，请在指定的集群上部署这个VM。

.. raw:: html

  <strong><font color="red">只部署VM一次，不需要在任何实验完成时对其进行清理。</font></strong>

部署 Tools VM
++++++++++++++++++

在 **Prism Central** > select :fa:`bars` **> Virtual Infrastructure > VMs** 中, 点击 **Create VM**.

填写以下字段:

- **Name** - *Initials*-Windows-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 4 GiB

- Select **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - WinToolsVM.qcow2
    - Select **Add**

- Select **Add New NIC**
    - **VLAN Name** - Secondary
    - Select **Add**

点击 **Save** 以创建此 VM.

打开VM电源.

通过RDP或控制台登录到VM，使用以下凭证:

- **Username** - NTNXLAB\\Administrator
- **password** - nutanix/4u
