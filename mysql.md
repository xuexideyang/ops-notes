# MySQL

## MySQL 主从复制
- 主库将数据更改写入 binlog。
- 主库创建 dump 线程，将 binlog 推送给从库的 I/O 线程。
- 从库 I/O 线程接收并写入本地 relay log（中继日志）。
- 从库 SQL 线程读取 relay log 并执行变更。

## binlog 三种格式
- **Statement**：记录 SQL 语句，日志量小，但可能造成主从不一致（如含 `NOW()` 等函数）。
- **Row**：记录每行数据的前后变化，安全但日志量大。
- **Mixed**：MySQL 自行判断，在 Statement 和 Row 之间自动切换。

## MyISAM vs InnoDB
- MyISAM：表级锁，不支持事务，不支持外键，适合读多写少。
- InnoDB：行级锁，支持事务（ACID），支持外键，支持聚簇索引，宕机可恢复，5.6 后支持全文索引，推荐生产环境使用。

## ACID
- 原子性（Atomicity）：事务要么全部执行，要么全部不执行（通过 undo log 实现）。
- 一致性（Consistency）：事务执行前后数据保持合法状态。
- 隔离性（Isolation）：事务之间互相隔离，互不干扰。
- 持久性（Durability）：事务提交后数据永久保存（通过 redo log 实现）。

## 隔离级别
- **读未提交**（READ UNCOMMITTED）：可能脏读。
- **读已提交**（READ COMMITTED）：解决脏读。
- **可重复读**（REPEATABLE READ，默认）：解决不可重复读，InnoDB 用间隙锁防止幻读。
- **串行化**（SERIALIZABLE）：最高隔离，完全串行，并发最差。

## 脏读、不可重复读、幻读
- 脏读：读到未提交事务的数据。
- 不可重复读：同一事务两次读取同一行数据结果不同。
- 幻读：同一事务两次范围查询结果行数不同。

## 异步复制 vs 半同步复制
- 异步：主库提交事务后立即返回，不等从库确认，性能好但可能丢数据。
- 半同步：主库至少等待一个从库接收并写入 relay log 后才返回，提高数据一致性，有性能损耗。

## 覆盖索引 vs 回表
- 覆盖索引：索引包含了查询所需的所有列，无需回表，速度快。
- 回表：使用二级索引查询时，需要根据主键回到聚簇索引取完整行，速度慢。

## EXPLAIN 的 type 类型
- `const`：通过主键或唯一索引查找，最快。
- `ref`：通过普通索引查找。
- `ALL`：全表扫描，最慢。

## redo log vs undo log
- redo log：记录数据修改后的值，保证持久性。
- undo log：记录数据修改前的值，用于回滚和 MVCC。

## MVCC（多版本并发控制）
- 读旧版本，写新版本，读写互不冲突。
- 通过 undo log 保存历史版本，实现非锁定读。

## 慢查询日志
- 作用：记录执行时间超过指定阈值的 SQL，帮助定位性能瓶颈。
- 开启方式：
  ```sql
  SET GLOBAL slow_query_log = ON;
  SET GLOBAL long_query_time = 2;
  
## 忘记root密码
- 首先停止mysql,修改配置文件添加skip-grant-tables,保存退出重启服务,无密码登录并更新密码.刷新权限flush privileges,最后退出把原先配置文件添加的删除重启服务

## 主丛延迟的判断
- show processlist;

## 备份方式
- (mysqldump)逻辑备份,导出sql语句,备份恢复慢,适合小数据量。
- (xtrabackup)物理备份,直接复制数据文件,备份恢复快,适合生产大文件。