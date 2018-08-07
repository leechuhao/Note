|                     | MyISAM                                   | InnoDB                                   |
| ------------------- | ---------------------------------------- | ---------------------------------------- |
| 存储结构                | 每张表被存放在三个文件：frm-表格定义MYD(MYData)-数据文件MYI(MYIndex)-索引文件 | 所有的表都保存在同一个数据文件中（也可能是多个文件，或者是独立的表空间文件），InnoDB表的大小只受限于操作系统文件的大小，一般为2GB |
| 存储空间                | MyISAM可被压缩，存储空间较小                        | InnoDB的表需要更多的内存和存储，它会在主内存中建立其专用的缓冲池用于高速缓冲数据和索引 |
| 可移植性、备份及恢复          | 由于MyISAM的数据是以文件的形式存储，所以在跨平台的数据转移中会很方便。在备份和恢复时可单独针对某个表进行操作 | 免费的方案可以是拷贝数据文件、备份 binlog，或者用 mysqldump，在数据量达到几十G的时候就相对痛苦了 |
| 事务安全                | 不支持 每次查询具有原子性                            | 支持 具有事务(commit)、回滚(rollback)和崩溃修复能力(crash recovery capabilities)的事务安全(transaction-safe (ACID compliant))型表 |
| AUTO_INCREMENT      | MyISAM表可以和其他字段一起建立联合索引                   | InnoDB中必须包含只有该字段的索引                      |
| SELECT              | MyISAM更优                                 |                                          |
| INSERT              |                                          | InnoDB更优                                 |
| UPDATE              |                                          | InnoDB更优                                 |
| DELETE              |                                          | InnoDB更优 它不会重新建立表，而是一行一行的删除              |
| COUNT without WHERE | MyISAM更优。因为MyISAM保存了表的具体行数               | InnoDB没有保存表的具体行数，需要逐行扫描统计，就很慢了           |
| COUNT with WHERE    | 一样                                       | 一样，InnoDB也会锁表                            |
| 锁                   | 只支持表锁                                    | 支持表锁、行锁 行锁大幅度提高了多用户并发操作的新能。但是InnoDB的行锁，只是在WHERE的主键是有效的，非主键的WHERE都会锁全表的 |
| 外键                  | 不支持                                      | 支持                                       |
| FULLTEXT全文索引        | 支持                                       | 不支持 可以通过使用Sphinx从InnoDB中获得全文索引，会慢一点      |