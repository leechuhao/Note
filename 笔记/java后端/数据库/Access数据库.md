|  数据类型  |                             描述                             |                         范围                         |
| :--------: | :----------------------------------------------------------: | :--------------------------------------------------: |
| Short Text | 文字或文字和数字的组合，包括不需要计算的数字（例如电话号码）。 |                   最多255个字符。                    |
| Long Text  |                  长文本或文本和数字的组合。                  |                 最多63个，999个字符                  |
|   Number   |                  数学计算中使用的数字数据。                  | 1，2，4或8个字节（如果设置为复制ID，则为16个字节）。 |
| Date/Time  |                 100到9999年的日期和时间值。                  |                        8 字节                        |
|  Currency  | 用于涉及1到4位小数位数据的数学计算中使用的货币值和数字数据。 |                       8个字节                        |
| AutoNumber | 将新记录添加到表中时，由Microsoft Access分配的唯一序列（由1递增）数字或随机数。 |       4字节（如果设置为复制ID，则为16字节）。        |
|   Yes/No   | 是和否只包含两个值之一（Yes / No，True / False或On / Off）的值和字段。 |                         1 位                         |

驱动

```
<!-- https://mvnrepository.com/artifact/net.sf.ucanaccess/ucanaccess -->
<dependency>
    <groupId>net.sf.ucanaccess</groupId>
    <artifactId>ucanaccess</artifactId>
    <version>4.0.4</version>
</dependency>

```

C#驱动

分为ACE和Jet两种，不知道有什么区别，现在用的Ace

如果配置语句写不对就会报找不到ISAM的bug

```
connection = new OleDbConnection(@"Provider=Microsoft.ACE.OLEDB.12.0; 
Data Source=C:\Users\brain\Desktop\downs\db_prisca4rpt.mdb;
jet oledb:database password=prisca;");
```

