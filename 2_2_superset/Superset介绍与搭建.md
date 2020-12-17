[TOC]



## Superset是什么?

 [Apache Superset](https://airbnb.io/projects/superset/) （孵化中）是一种现代的，可用于企业的商业智能Web应用程序，换句话说就是一种轻量级的BI工具。由 Airbnb 开源，截至2020年十二月，其[github](https://github.com/apache/incubator-superset星数已超过31k。
## Super能干什么？
### 探索
使用数据可视化进行数据探索

![superset-使用数据可视化进行数据探索](images/Superset%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97/explorer.png)

### 视图

通过交互式仪表盘查看数据

![superset-dashboard](images/Superset%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97/dashboard3.png)

### 调查

使用SQL Lab编写查询以探索数据

![superset-sqllab](images/Superset%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97/sqllab1.png)


## Superset的优点？

- 直观的界面，用于浏览和可视化数据集，并创建交互式仪表板。
- 各种精美的可视化效果可展示您的数据。
- 简单，无代码的用户流程，可以对裸露的仪表板下的数据进行细分和切分。仪表板和图表充当更深入分析的起点。
- 先进的SQL编辑器/ IDE公开了丰富的元数据浏览器，以及轻松的工作流程，可从任何结果集中创建可视化内容。
- 可扩展的高粒度安全模型，允许有关谁可以访问哪些产品功能和数据集的复杂规则。与主要的身份验证后端集成（数据库，OpenID，LDAP，OAuth，REMOTE_USER等）
- 轻量级语义层，允许通过定义维度和指标来控制如何向用户公开数据源
- 对大多数使用SQL的数据库提供开箱即用的支持
- 与Druid的深度集成使Superset能够在切片和切块大型实时数据集时能够保持超快的速度
- 具有可配置缓存的快速加载仪表板

## Superset怎么搭建？

### pip安装

#### 1.创建虚拟环境并激活（可选）

```shell
conda create -n superset
activate superset
```
#### 2.下载Superset

```shell
pip install apache-superset
```

#### 3.初始化数据库

官方文档请见：https://superset.apache.org/docs/intro

```shell
superset db upgrade
```

#### 4.运行下列命令完成最后安装

```shell
# Create an admin user (you will be prompted to set a username, first and last name before setting a password)
$ export FLASK_APP=superset
superset fab create-admin

# Load some data to play with
superset load_examples

# Create default roles and permissions
superset init

# To start a development web server on port 8088, use -p to bind to another port
superset run -p 8088 --with-threads --reload --debugger

# 如果出现
pip install  pillow
```

#### 5.登录

按照命令行提示在浏览器打开对应链接（默认英文，右上角修改）

### docker与docker compose安装

#### 1.安装docker与docker compose

[Docker for Mac](https://docs.docker.com/docker-for-mac/install/)安装后请选择分配内存到6GB（默认2GB）

LInux：选择任意风格安装即可

Windows：目前没有正式支持，官方建议为使用VirtualBox安装Ubuntu虚拟机，官方建议保留8GB内存以及40G硬盘。

#### 2.克隆Superset的Github存储库

```shell
 git clone https://github.com/apache/incubator-superset.git
```

#### 3.通过Docker Compose启动Superset

```shell
cd incubator-superset
docker-compose up
# 结束后应当有一个实例运行，可通过命令docker ps -a 查询
```

注意：使用docker-compose启动的superset需要下载较多的镜像，目前默认使用的docker-compose版本较高，由于ubuntu默认商店docker-compose版本可能较低，建议使用docker-compose官方推荐方式，或者命令`pip install docker-compose`进行安装。

#### 4.登录

本地的Superset实例还包括一个Postgres服务器来存储您的数据，并且已经预先加载了Superset附带的一些示例数据集。请通过Web浏览器访问Superset `http://localhost:8088`，并确保使用`http`

默认账号/密码：`admin/admin`

## Superset连接到数据库

### 安装驱动

Superset需要为要连接的每种其他类型的数据库安装Python数据库驱动程序。

Superset使用提供的SQL接口（通常通过SQLAlchemy库）与基础数据库进行交互。

除了Sqlite（它是Python标准库的一部分）外，Superset并没有捆绑与数据库的连接。您需要为要用作元数据数据库的数据库安装必需的软件包，以及与要通过Superset访问的数据库连接所需的软件包。

常用数据包如下



| 数据库        | PyP包                             | 连接字符串                                                   |
| ------------- | --------------------------------- | ------------------------------------------------------------ |
| druid         | pip install pydruid               | druid://<User>:<password>@<Host>:<Port-default-9088>/druid/v2/sql |
| hive          | pip install pyhive                | hive://hive@{hostname}:{port}/{database}                     |
| kylin         | pip install kylinpy               | kylin://<username>:<password>@<hostname>:<port>/<project>?<param1>=<value1>&<param2>=<value2> |
| clickhouse    | pip install sqlalchemy-clickhouse | clickhouse://{username}:{password}@{hostname}:{port}/{database} |
| mysql         | pip install mysqlclient           | mysql://<UserName>:<DBPassword>@<Database Host>/<Database Name> |
| Oracle        | pip install cx_Oracle             | oracle://                                                    |
| postgresql    | pip install psycopg2              | postgresql://<UserName>:<DBPassword>@<Database Host>/<Database Name> |
| presto        | pip install pyhive                | presto://                                                    |
| elasticsearch | pip install elasticsearch-dbapi   | elasticsearch+http://{user}:{password}@{host}:9200/          |

