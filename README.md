# JdbcTiDBBackup

#使用JDBC备份和恢复TiDB数据库的Java工具。



## 工具人8

队长：黄金振

队员：黄应起

## 项目介绍

**使用JDBC备份和恢复TiDB数据库的Java工具。**

JdbcTiDBBackup可用于备份完整数据库或一组 ，并和datax一起组合使用，提高TiDB备份数据的能力

-仅一个或几个模式。出于性能原因，备份完整

-数据库不是事务安全的

TiDBWriter 插件已将数据写入 TiDB 主库的目标表实现的功能。在实现上，TiDBWriter 通过 Streamload 以csv格式导入数据至 TiDB。

**针对分区表：待定优化**

## 背景&动机

我们团队是做云数据库工具开发的，降低用户在云上使用数据库的成本只需要配置sql或者页面点击实现数据etl。

## 项目设计

## 详细设计


**SQL Parser模块,系统表模块:**

-当数据量比较少时，采用全量采如，我们需要指定在TiDB中每一天建立一个分区，但是我们不能写死，所以我们在脚本文件汇总需要定义一个变量，然后再从外部传入到脚本文件中，
需要优化这个脚本逻辑

**执行器模块：**

TiDBWriter 插件实现了写入数据到 TiDB 主库的目的表的功能。在底层实现上， TiDBWriter 通过 JDBC 连接远程 TiDB 数据库，并执行相应的 insert into ... 或者 ( replace into ...) 的 sql 语句将数据写入 TiDB，内部会分批次提交入库，需要数据库本身采用 innodb 引擎。

TiDBWriter 面向ETL开发工程师，他们使用 TiDBWriter 从数仓导入数据到 TiDB。同时 TiDBWriter 亦可以作为数据迁移工具为DBA等用户提供服务。


**优化器模块：**

- TiDBWriter 通过 Streamload 以 csv 格式导入至 TiDB，将内部reader读取的数据进行读取数据后导入至 TiDB，以提高写入性能。
