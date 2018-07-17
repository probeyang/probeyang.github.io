# mysqldump命令参数介绍

## 使用
注意：mysqldump不用在mysql命令行模式下执行，它是个单独的命令，直接在windows或者linux的命令行模式下执行即可。
'''mysqldump -uroot -p123456 -h127.0.0.1 -P3306 --default-character-set=utf8 --set-gtid-purged=ON -B blogs > blogs.sql'''
上面示例中参数一看就明了，只有两个参数需要解释一下：
1. –databases, -B
导出几个数据库。参数后面所有名字参量都被看作数据库名。

2. —set-gtid-purged
  GTID即全局事务ID（global transaction identifier），GTID实际上是由UUID+TID组成的。其中UUID是一个MySQL实例的唯一标识。TID代表了该实例上已经提交的事务数量，并且随着事务提交单调递增，所以GTID能够保证每个MySQL实例事务的执行（不会重复执行同一个事务，并且会补全没有执行的事务）
具体内容参考文献：[http://blog.51cto.com/changjianglinux/2059419]

## 参数详解
其他参数解释：
–password, -p 连接数据库密码
–port, -P 连接数据库端口号
–user, -u 指定连接的用户名。
\*\*–all-databases , -A \*\* 导出全部数据库 mysqldump -uroot -p –all-databases
–all-tablespaces , -Y 导出全部表空间 mysqldump -uroot -p –all-databases –all-tablespaces
–no-tablespaces , -y 不导出任何表空间信息 mysqldump -uroot -p –all-databases –no-tablespaces
–add-drop-database 每个数据库创建之前添加drop数据库语句 mysqldump -uroot -p –all-databases –add-drop-database
–add-drop-table 每个数据表创建之前添加drop数据表语句。(默认为打开状态，使用–skip-add-drop-table取消选项) mysqldump -uroot -p –all-databases (默认添加drop语句) mysqldump -uroot -p –all-databases –skip-add-drop-table (取消drop语句)
–add-locks 在每个表导出之前增加LOCK TABLES并且之后UNLOCK TABLE。(默认为打开状态，使用–skip-add-locks取消选项) mysqldump -uroot -p –all-databases (默认添加LOCK语句) mysqldump -uroot -p –all-databases –skip-add-locks (取消LOCK语句)
–comments 附加注释信息。 默认为打开，可以用–skip-comments取消mysqldump -uroot -p –all-databases (默认记录注释)mysqldump -uroot -p –all-databases –skip-comments (取消注释)
–compact 导出更少的输出信息(用于调试),去掉注释和头尾等结构,可以使用选项：–skip-add-drop-table –skip-add-locks –skip-comments –skip-disable-keys mysqldump -uroot -p –all-databases –compact
–complete-insert, -c 使用完整的insert语句(包含列名称)。这么做能提高插入效率，但是可能会受到max\_allowed\_packet参数的影响而导致插入失败。 mysqldump -uroot -p –all-databases –complete-insert
–compress, -C在客户端和服务器之间启用压缩传递所有信息mysqldump -uroot -p –all-databases –compress
–databases, -B 导出几个数据库。参数后面所有名字参量都被看作数据库名。 mysqldump -uroot -p –databases test mysql
–debug 输出debug信息，用于调试。 默认值为：d:t:o,/tmp/mysqldump.trace mysqldump -uroot -p –all-databases –debug mysqldump -uroot -p –all-databases –debug="d:t:o,/tmp/debug.trace"
–debug-info 输出调试信息并退出 mysqldump -uroot -p –all-databases –debug-info
–default-character-set 设置默认字符集，默认值为utf8 mysqldump -uroot -p –all-databases –default-character-set=latin1
–delayed-insert 采用延时插入方式（INSERT DELAYED）导出数据 mysqldump -uroot -p –all-databases –delayed-insert
–events, -E 导出事件 mysqldump -uroot -p –all-databases –events
–flush-logs 开始导出之前刷新日志 请注意：假如一次导出多个数据库(使用选项–databases或者–all-databases)，将会逐个数据库刷新日志。除使用–lock-all-tables或者–master-data外。在这种情况下，日志将会被刷新一次，相应的所以表同时被锁定。因此，如果打算同时导出和刷新日志应该使用–lock-all-tables 或者–master-data 和–flush-logs。 mysqldump -uroot -p –all-databases –flush-logs
–flush-privileges 在导出mysql数据库之后，发出一条FLUSH PRIVILEGES 语句。为了正确恢复，该选项应该用于导出mysql数据库和依赖mysql数据库数据的任何时候。 mysqldump -uroot -p –all-databases –flush-privileges
–force 在导出过程中忽略出现的SQL错误 mysqldump -uroot -p –all-databases –force
–host, -h 需要导出的主机信息 mysqldump -uroot -p –host=localhost –all-databases
–ignore-table 不导出指定表。 指定忽略多个表时，需要重复多次，每次一个表。每个表必须同时指定数据库和表名。 例如：–ignore-table=database.table1 –ignore-table=database.table2 …… mysqldump -uroot -p –host=localhost –all-databases –ignore-table=mysql.user
–lock-all-tables, -x 提交请求锁定所有数据库中的所有表，以保证数据的一致性。 这是一个全局读锁，并且自动关闭–single-transaction 和–lock-tables 选项。 mysqldump -uroot -p –host=localhost –all-databases –lock-all-tables
–lock-tables, -l 开始导出前，锁定所有表。 用READ LOCAL锁定表以允许MyISAM表并行插入。对于支持事务的表例如InnoDB和BDB，–single-transaction是一个更好的选择，因为它根本不需要锁定表。 请注意当导出多个数据库时，–lock-tables分别为每个数据库锁定表。因此，该选项不能保证导出文件中的表在数据库之间的逻辑一致性。不同数据库表的导出状态可以完全不同。 mysqldump -uroot -p –host=localhost –all-databases –lock-tables
–no-create-db, -n 只导出数据，而不添加CREATE DATABASE 语句。 mysqldump -uroot -p –host=localhost –all-databases –no-create-db
–no-create-info, -t只导出数据，而不添加CREATE TABLE 语句 mysqldump -uroot -p –host=localhost –all-databases –no-create-info
–no-data, -d 不导出任何数据，只导出数据库表结构 mysqldump -uroot -p –host=localhost –all-databases –no-data
参考：[https://blog.csdn.net/linuxlsq/article/details/52606317]




