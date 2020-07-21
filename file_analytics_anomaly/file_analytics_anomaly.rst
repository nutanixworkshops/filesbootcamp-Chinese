.. _file_analytics_anomaly:

--------------------------------
File 分析: Anomaly Rules（异常规则）
--------------------------------

概述
++++++++



定义异常规则
+++++++++++++++++++++

#. 通过从齿轮图标下转到 **Define Anomaly Rules** 来创建两个异常规则

   .. figure:: images/39.png

#. 选择 **Define Anomaly Rules** 并使用以下设置创建规则

   - **Events:** Delete
   - **Minimum Operation %:** 1
   - **Minimum Operation Count:** 10
   - **User:** All Users
   - **Type:** Hourly
   - **Interval:** 1

#. 为该异常表条目选择 **Save**

#. 选择 **+ Configure new anomaly** 并使用以下设置创建第二条规则

   - **Events**: Create
   - **Minimum Operation %**: 1
   - **Minimum Operation Count**: 10
   - **User**: All Users
   - **Type**: Hourly
   - **Interval**: 1

#. 为该异常表条目选择 **Save**

   .. figure:: images/40.png

#. 选择 **Save** 以退出定义异常规则窗口

加载示例数据
+++++++++++++++++++++

#. 转到Marketing共享中的Sample Data文件夹并复制，然后将该文件夹粘贴到同一共享中。

   .. figure:: images/42.png

#. 现在删除原始的Sample Data文件夹。

导致错误条件
+++++++++++++++++++++

#. 在等待异常警报发出时，我们将创建一个拒绝权限。

   .. note:: 异常引擎每30分钟运行一次。 虽然可以从File Analytics VM配置此设置，但是修改此变量不在本练习的范围之内。

#. 在Marketing共享中创建一个名为 **RO** 的新目录

#. 在 **RO** 目录中创建一个文本文件，其中包含诸如 “hello world” 之类的名为 **myfile.txt** 的文本

#. 转到 **RO** 文件夹的 **Properties** 并选择Security选项卡

#. 选择 **Advanced**

#. 选择 **Disable inheritance** 然后选择 **Convert…** 选项

#. 然后添加 **Everyone** 权限如下:

   - Read & Execute
   - List folder contents
   - Read

   .. figure:: images/43.png

#. 选择 **OK** 然后再次选择 **OK**

#. 以特定用户的身份打开PowerShell窗口

   - 按住 **Shift** 然后 **right click** 任务栏上的 **PowerShell icon**
   - 选择 **Run as different user**

   .. figure:: images/44.png

#. 输入以下信息

   - **User name**: devuser01
   - **Password**: nutanix/4u

#. 更改目录为 Marketing share 和 **RO** 目录

     .. code-block:: bash

        cd \\xyz-files.ntnxlab.local\marketing\RO

#. 执行以下命令，第一个应该成功，第二个应该失败:

     .. code-block:: bash

        more .\myfile.txt
        rm .\myfile.txt

   .. figure:: images/45.png

#. 大约一分钟后，您将在仪表板和 **Audit Trails** 视图中看到 **Permission Denials** 。 您可能需要刷新您的浏览器。

   .. figure:: images/46.png

   .. note:: 容量趋势面板每24小时更新一次。
