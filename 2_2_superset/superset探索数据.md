[TOC]

**主要参考superset文档：https://superset.apache.org/docs/creating-charts-dashboards/exploring-data**

## 简介

在本教程中，我们将通过探索一个真实的数据集来介绍Apache Superset中的关键概念，该数据集包含一个总部位于英国的组织的员工在2011年进行的航班。给出了有关每个航班的以下信息：

- 旅客部门。在本教程中，部门已重命名为“橙色”，“黄色”和“紫色”。
- 机票费用。
- 旅游舱（经济舱，高级经济舱，商务舱和头等舱）。
- 票是单张还是回程。
- 旅行日期。
- 有关始发地和目的地的信息。
- 起点和终点之间的距离，以公里（km）为单位。

## superset启动与设置

### superset启动

使用pip安装的superset启动，默认数据库只有python自带的sqlite3

![superset启动](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_082.png)

### 创建mysql数据库连接

**首先**，下载驱动，其他命令请参考上一个文档：Superset介绍与搭建

```
pip install mysqlclient
# conda install mysqlclient
```

之后选择mysql数据库并将其添加进superset，具体步骤如下：

（1）左侧选择database进入以下界面，右侧点击加号到达创建数据库连接界面

![选区_086](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_086.png)



（2）输入自己的数据库地址，并点击测试按钮（test connection）进行测试。同时勾选Allow Csv Upload，允许该数据库加载csv文件。

![选区_087](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_087.png)

**Note**:如果想用sqilte则需要先关闭superset，然后前往配置文件修改一个配置——`PREVENT_UNSAFE_DB_CONNECTIONS = False`,之后重新启动服务。

![image-20201211170128581](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/image-20201211170128581.png)

## 载入CSV文件

这里使用官方提供的文件（tutorial_flights.csv）进行讲解[官方下载地址](https://superset.apache.org/docs/creating-charts-dashboards/exploring-data)可能无法访问，请从该地址下载：https://github.com/FontTian/SDJTU_Training

然后在Data->Upload a CSV中加载CSV数据。**部分数据集在导入时可能会出现编码错误，有的与文件本身编码格式有关Python本身读取错误。有的错误则发生在数据库写入阶段。superset作为大数据的BI工具，上传CSV并非核心要求**

![superset 加载数据](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_083.png)

然后，输入**Table Name**作为*tutorial_flights，*并从您的计算机中选择CSV文件。

![选区_084](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_084.png)

接下来，在“**解析为日期（Parse Dates:A comma separated list of columns that should be parsed as dates.）”**字段中输入文本“*Travel Date*” 。

![选区_085](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/%E9%80%89%E5%8C%BA_085.png)

选择一个可用的数据库，同时将所有其他选项保留为默认设置，然后选择页面底部的“**保存**”。

### 表格可视化

创建第一个可视化文件：一个表格，以显示航班数量和每次旅行的费用，选择“**NEW”>“图表”**。

![image-20201211170430517](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/image-20201211170430517.png)

在“**创建新图表”**表单中，从“**选择数据源”** 下拉列表中选择“ *tutorial_flights* ” 。

![image-20201211170515342](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/image-20201211170515342.png)

可视化类型默认为表即可，点击“创建新图表”，进入下一页。在这一页，我们进行更多的图表创建设置，首先是time range（时间范围），默认为last work。我们改为no filter。分组选择travel class，指标则添加一个求和（sum），然后选择求和类型中选择cost。

![image-20201211171606918](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/image-20201211171606918.png)

完成后右键点击“运行查询”获取运行结果。

![image-20201211171937447](images/superset%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE/image-20201211171937447.png)

在运行界面，右键可以获取一些我们需要的内容，连接，嵌入代码，文件，图片等等。

