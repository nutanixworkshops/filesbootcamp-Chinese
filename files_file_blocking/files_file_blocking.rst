.. _files_file_blocking:

------------------------
Files: 文件屏蔽
------------------------

选择性的文件屏蔽
+++++++++++++++++++++++

在本练习中，您将配置Files以阻止File server和 Marketing share和特定文件扩展名。

#.  在 **Prism** > **File Server** >中选择您的 files server 点击 **Update** > 然后点击 **Blocked File Types**

   .. figure:: images/47.png

#. 在 **Blocked File Types** 下，输入逗号分隔的扩展名列表例如 .flv,.mov 然后点击 **Save**

   .. figure:: images/48.png

#. 通过单击任务栏上的 **PowerShell icon** 打开PowerShell窗口。 输入以下命令，您将在其中看到拒绝访问错误消息：

   .. code-block:: bash

	 new-item \\xyz-files.ntnxlab.local\marketing\MyMovie.flv

   .. figure:: images/49.png

#. 在 **Prism** > **File Server** > **Share/Export** > 单击 Marketing share 并选择 **Update**

   .. figure:: images/50.png

#. 选择 **Next** 以进入 **Settings** 页面。

#. 选中 **Blocked File Types** ，然后输入.none 作为文件扩展名。

   .. figure:: images/51.png

#. 选择 **Next** ，然后在 **Summary** 页面上选择 **Save** 以完成更新。

#. 共享级别的阻止的文件类型设置将覆盖服务器级别的设置。 使用PowerShell发出与上一步相同的命令。 该命令现在将成功完成。

   .. figure:: images/52.png
