---
title: MySQL异常错误码
date: 2015-05-11 13:10:39
tags: 数据库
---


Server抛给Client错误的 消息信息/错误值/SQLSTATE值 均列在源码`share/messages_to_clients.txt`文件中。“%d”和“%s”分别代表编号和字符串，显示时，它们将被消息值取代。



由于MySQL源码更新较频繁,以下内容可能缺失或有变更,虽然概率较小.

最新参考: [mysql-server/blob/8.0/share/messages_to_clients.txt](https://github.com/mysql/mysql-server/blob/8.0/share/messages_to_clients.txt)


<br>

---

<br>



· 错误：1000 SQLSTATE: HY000 (ER_HASHCHK)

消息：hashchk

· 错误：1001 SQLSTATE: HY000 (ER_NISAMCHK)

消息：isamchk

· 错误：1002 SQLSTATE: HY000 (ER_NO)

消息：NO

· 错误：1003 SQLSTATE: HY000 (ER_YES)

消息：YES

· 错误：1004 SQLSTATE: HY000 (ER_CANT_CREATE_FILE)

消息：无法创建文件'%s' (errno: %d)

· 错误：1005 SQLSTATE: HY000 (ER_CANT_CREATE_TABLE)

消息：无法创建表'%s' (errno: %d)

· 错误：1006 SQLSTATE: HY000 (ER_CANT_CREATE_DB)

消息：无法创建数据库'%s' (errno: %d)

· 错误：1007 SQLSTATE: HY000 (ER_DB_CREATE_EXISTS)

消息：无法创建数据库'%s'，数据库已存在。

· 错误：1008 SQLSTATE: HY000 (ER_DB_DROP_EXISTS)

消息：无法撤销数据库'%s'，数据库不存在。

· 错误：1009 SQLSTATE: HY000 (ER_DB_DROP_DELETE)

消息：撤销数据库时出错（无法删除'%s'，errno: %d）

· 错误：1010 SQLSTATE: HY000 (ER_DB_DROP_RMDIR)

消息：撤销数据库时出错（can't rmdir '%s', errno: %d）

· 错误：1011 SQLSTATE: HY000 (ER_CANT_DELETE_FILE)

消息：删除'%s'时出错 (errno: %d)

· 错误：1012 SQLSTATE: HY000 (ER_CANT_FIND_SYSTEM_REC)

消息：无法读取系统表中的记录。

· 错误：1013 SQLSTATE: HY000 (ER_CANT_GET_STAT)

消息：无法获取'%s'的状态(errno: %d)

· 错误：1014 SQLSTATE: HY000 (ER_CANT_GET_WD)

消息：无法获得工作目录(errno: %d)

· 错误：1015 SQLSTATE: HY000 (ER_CANT_LOCK)

消息：无法锁定文件(errno: %d)

· 错误：1016 SQLSTATE: HY000 (ER_CANT_OPEN_FILE)

消息：无法打开文件：'%s' (errno: %d)

· 错误：1017 SQLSTATE: HY000 (ER_FILE_NOT_FOUND)

消息：无法找到文件： '%s' (errno: %d)

· 错误：1018 SQLSTATE: HY000 (ER_CANT_READ_DIR)

消息：无法读取'%s'的目录 (errno: %d)

· 错误：1019 SQLSTATE: HY000 (ER_CANT_SET_WD)

消息：无法为'%s'更改目录 (errno: %d)

· 错误：1020 SQLSTATE: HY000 (ER_CHECKREAD)

消息：自上次读取以来表'%s'中的记录已改变。

· 错误：1021 SQLSTATE: HY000 (ER_DISK_FULL)

消息：磁盘满(%s)；等待某人释放一些空间...

· 错误：1022 SQLSTATE: 23000 (ER_DUP_KEY)

消息：无法写入；复制表'%s'的 键。

· 错误：1023 SQLSTATE: HY000 (ER_ERROR_ON_CLOSE)

消息：关闭'%s'时出错 (errno: %d)

· 错误：1024 SQLSTATE: HY000 (ER_ERROR_ON_READ)

消息：读取文件'%s'时出错 (errno: %d)

· 错误：1025 SQLSTATE: HY000 (ER_ERROR_ON_RENAME)

消息：将'%s'重命名为'%s'时出错 (errno: %d)

· 错误：1026 SQLSTATE: HY000 (ER_ERROR_ON_WRITE)

消息：写入文件'%s'时出错 (errno: %d)

· 错误：1027 SQLSTATE: HY000 (ER_FILE_USED)

消息：'%s'已锁定，拒绝更改。

· 错误：1028 SQLSTATE: HY000 (ER_FILSORT_ABORT)

消息：分类失败

· 错误：1029 SQLSTATE: HY000 (ER_FORM_NOT_FOUND)

消息：对于'%s'，视图'%s'不存在。

· 错误：1030 SQLSTATE: HY000 (ER_GET_ERRNO)

消息：从存储引擎中获得错误%d。

· 错误：1031 SQLSTATE: HY000 (ER_ILLEGAL_HA)

消息：关于'%s'的表存储引擎不含该选项。

· 错误：1032 SQLSTATE: HY000 (ER_KEY_NOT_FOUND)

消息：无法在'%s'中找到记录。

· 错误：1033 SQLSTATE: HY000 (ER_NOT_FORM_FILE)

消息：文件中的不正确信息：'%s'

· 错误：1034 SQLSTATE: HY000 (ER_NOT_KEYFILE)

消息：对于表'%s'， 键文件不正确，请尝试修复。

· 错误：1035 SQLSTATE: HY000 (ER_OLD_KEYFILE)

消息：旧的键文件，对于表'%s'，请修复之！

· 错误：1036 SQLSTATE: HY000 (ER_OPEN_AS_READONLY)

消息：表'%s'是只读的。

· 错误：1037 SQLSTATE: HY001 (ER_OUTOFMEMORY)

消息：内存溢出，重启服务器并再次尝试（需要%d字节）。

· 错误：1038 SQLSTATE: HY001 (ER_OUT_OF_SORTMEMORY)

消息：分类内存溢出，增加服务器的分类缓冲区大小。

· 错误：1039 SQLSTATE: HY000 (ER_UNEXPECTED_EOF)

消息：读取文件'%s'时出现意外EOF (errno: %d)

· 错误：1040 SQLSTATE: 08004 (ER_CON_COUNT_ERROR)

消息：连接过多。

· 错误：1041 SQLSTATE: HY000 (ER_OUT_OF_RESOURCES)

消息：内存溢出，请检查是否mysqld或其他进程使用了所有可用内存，如不然，或许应使用'ulimit'允许mysqld使用更多内存，或增加交换空间的大小。

· 错误：1042 SQLSTATE: 08S01 (ER_BAD_HOST_ERROR)

消息：无法获得该地址给出的主机名。

· 错误：1043 SQLSTATE: 08S01 (ER_HANDSHAKE_ERROR)

消息：不良握手

· 错误：1044 SQLSTATE: 42000 (ER_DBACCESS_DENIED_ERROR)

消息：拒绝用户'%s'@'%s'访问数据库'%s'。

· 错误：1045 SQLSTATE: 28000 (ER_ACCESS_DENIED_ERROR)

消息：拒绝用户'%s'@'%s'的访问（使用密码：%s）

· 错误：1046 SQLSTATE: 3D000 (ER_NO_DB_ERROR)

消息：未选择数据库。

· 错误：1047 SQLSTATE: 08S01 (ER_UNKNOWN_COM_ERROR)

消息：未知命令。

· 错误：1048 SQLSTATE: 23000 (ER_BAD_NULL_ERROR)

消息：列'%s'不能为空。

· 错误：1049 SQLSTATE: 42000 (ER_BAD_DB_ERROR)

消息：未知数据库'%s'。

· 错误：1050 SQLSTATE: 42S01 (ER_TABLE_EXISTS_ERROR)

消息：表'%s'已存在。

· 错误：1051 SQLSTATE: 42S02 (ER_BAD_TABLE_ERROR)

消息：未知表'%s'。

· 错误：1052 SQLSTATE: 23000 (ER_NON_UNIQ_ERROR)

消息：%s中的列'%s'不明确。

· 错误：1053 SQLSTATE: 08S01 (ER_SERVER_SHUTDOWN)

消息：在操作过程中服务器关闭。

· 错误：1054 SQLSTATE: 42S22 (ER_BAD_FIELD_ERROR)

消息：'%s'中的未知列'%s'。

· 错误：1055 SQLSTATE: 42000 (ER_WRONG_FIELD_WITH_GROUP)

消息：'%s'不在GROUP BY中。

· 错误：1056 SQLSTATE: 42000 (ER_WRONG_GROUP_FIELD)

消息：无法在'%s'上创建组。

· 错误：1057 SQLSTATE: 42000 (ER_WRONG_SUM_SELECT)

消息：语句中有sum函数和相同语句中的列。

· 错误：1058 SQLSTATE: 21S01 (ER_WRONG_VALUE_COUNT)

消息：列计数不匹配值计数。

· 错误：1059 SQLSTATE: 42000 (ER_TOO_LONG_IDENT)

消息：ID名称'%s'过长。

· 错误：1060 SQLSTATE: 42S21 (ER_DUP_FIELDNAME)

消息：重复列名'%s'。

· 错误：1061 SQLSTATE: 42000 (ER_DUP_KEYNAME)

消息：重复键名称'%s'。

· 错误：1062 SQLSTATE: 23000 (ER_DUP_ENTRY)

消息：键%d的重复条目'%s'。

· 错误：1063 SQLSTATE: 42000 (ER_WRONG_FIELD_SPEC)

消息：对于列'%s'，列分类符不正确。

· 错误：1064 SQLSTATE: 42000 (ER_PARSE_ERROR)

消息：在行%d上，%s靠近'%s'。

· 错误：1065 SQLSTATE: 42000 (ER_EMPTY_QUERY)

消息：查询为空。

· 错误：1066 SQLSTATE: 42000 (ER_NONUNIQ_TABLE)

消息：非唯一的表/别名：'%s'

· 错误：1067 SQLSTATE: 42000 (ER_INVALID_DEFAULT)

消息：关于'%s'的无效默认值。

· 错误：1068 SQLSTATE: 42000 (ER_MULTIPLE_PRI_KEY)

消息：定义了多个主键。

· 错误：1069 SQLSTATE: 42000 (ER_TOO_MANY_KEYS)

消息：指定了过多键：允许的最大键数是%d。

· 错误：1070 SQLSTATE: 42000 (ER_TOO_MANY_KEY_PARTS)

消息：指定了过多键部分：允许的最大键部分是%d。

· 错误：1071 SQLSTATE: 42000 (ER_TOO_LONG_KEY)

消息：指定的键过长，最大键长度是%d字节。

· 错误：1072 SQLSTATE: 42000 (ER_KEY_COLUMN_DOES_NOT_EXITS)

消息：键列'%s'在表中不存在。

· 错误：1073 SQLSTATE: 42000 (ER_BLOB_USED_AS_KEY)

消息：BLOB列'%s'不能与已使用的表类型用在 键说明中。

· 错误：1074 SQLSTATE: 42000 (ER_TOO_BIG_FIELDLENGTH)

消息：对于列'%s'，列长度过大 (max = %d)，请使用BLOB或TEXT取而代之。

· 错误：1075 SQLSTATE: 42000 (ER_WRONG_AUTO_KEY)

消息：不正确的表定义，只能有1个auto列，而且必须将其定义为 键。

· 错误：1076 SQLSTATE: HY000 (ER_READY)

消息：%s，连接就绪。版本：'%s'，套接字：'%s'，端口：%d

· 错误：1077 SQLSTATE: HY000 (ER_NORMAL_SHUTDOWN)

消息：%s，正常关闭。

· 错误：1078 SQLSTATE: HY000 (ER_GOT_SIGNAL)

消息：%s，获得信号%d。放弃！

· 错误：1079 SQLSTATE: HY000 (ER_SHUTDOWN_COMPLETE)

消息：%s，关闭完成

· 错误：1080 SQLSTATE: 08S01 (ER_FORCING_CLOSE)

消息：%s，强制关闭线程%ld，用户：'%s'

· 错误：1081 SQLSTATE: 08S01 (ER_IPSOCK_ERROR)

消息：无法创建IP套接字

· 错误：1082 SQLSTATE: 42S12 (ER_NO_SUCH_INDEX)

消息：表'%s'中没有与CREATE INDEX中索引类似的索引，重新创建表。

· 错误：1083 SQLSTATE: 42000 (ER_WRONG_FIELD_TERMINATORS)

消息：字段分隔符参量不是预期的，请参考手册。

· 错误：1084 SQLSTATE: 42000 (ER_BLOBS_AND_NO_TERMINATED)

消息：不能与BLOB一起使用固定行长度，请使用'fields terminated by'。

· 错误：1085 SQLSTATE: HY000 (ER_TEXTFILE_NOT_READABLE)

消息：文件'%s'必须在数据库目录下，或能被所有人读取。

· 错误：1086 SQLSTATE: HY000 (ER_FILE_EXISTS_ERROR)

消息：文件'%s'已存在。

· 错误：1087 SQLSTATE: HY000 (ER_LOAD_INFO)

消息：记录，%ld；已删除，%ld；已跳过，%ld；警告，%ld

· 错误：1088 SQLSTATE: HY000 (ER_ALTER_INFO)

消息：记录，%ld；重复，%ld

· 错误：1089 SQLSTATE: HY000 (ER_WRONG_SUB_KEY)

消息：不正确的子部分键，使用的键部分不是字符串，所用的长度长于键部分，或存储引擎不支持唯一子键。

· 错误：1090 SQLSTATE: 42000 (ER_CANT_REMOVE_ALL_FIELDS)

消息：不能用ALTER TABLE删除所有列，请使用DROP TABLE取而代之。

· 错误：1091 SQLSTATE: 42000 (ER_CANT_DROP_FIELD_OR_KEY)

消息：不能撤销'%s'，请检查列/键是否存在。

· 错误：1092 SQLSTATE: HY000 (ER_INSERT_INFO)

消息：记录，%ld；复制，%ld；告警，%ld

· 错误：1093 SQLSTATE: HY000 (ER_UPDATE_TABLE_USED)

消息：不能在FROM子句中制定要更新的目标表'%s'。

· 错误：1094 SQLSTATE: HY000 (ER_NO_SUCH_THREAD)

消息：未知线程ID：%lu

· 错误：1095 SQLSTATE: HY000 (ER_KILL_DENIED_ERROR)

消息：你不是线程%lu的所有者。

· 错误：1096 SQLSTATE: HY000 (ER_NO_TABLES_USED)

消息：未使用任何表。

· 错误：1097 SQLSTATE: HY000 (ER_TOO_BIG_SET)

消息：列%s和SET的字符串过多。

· 错误：1098 SQLSTATE: HY000 (ER_NO_UNIQUE_LOGFILE)

消息：不能生成唯一的日志文件名%s.(1-999)

· 错误：1099 SQLSTATE: HY000 (ER_TABLE_NOT_LOCKED_FOR_WRITE)

消息：表'%s'已用READ锁定，不能更新。

· 错误：1100 SQLSTATE: HY000 (ER_TABLE_NOT_LOCKED)

消息：未使用LOCK TABLES锁定表'%s'。

· 错误：1101 SQLSTATE: 42000 (ER_BLOB_CANT_HAVE_DEFAULT)

消息：BLOB/TEXT列'%s'不能有默认值。

· 错误：1102 SQLSTATE: 42000 (ER_WRONG_DB_NAME)

消息：不正确的数据库名'%s'。

· 错误：1103 SQLSTATE: 42000 (ER_WRONG_TABLE_NAME)

消息：不正确的表名'%s'。

· 错误：1104 SQLSTATE: 42000 (ER_TOO_BIG_SELECT)

消息：SELECT将检查超过MAX_JOIN_SIZE的行，如果SELECT正常，请检查WHERE，并使用SET SQL_BIG_SELECTS=1或SET SQL_MAX_JOIN_SIZE=#。

· 错误：1105 SQLSTATE: HY000 (ER_UNKNOWN_ERROR)

消息：未知错误。

· 错误：1106 SQLSTATE: 42000 (ER_UNKNOWN_PROCEDURE)

消息：未知过程'%s'

· 错误：1107 SQLSTATE: 42000 (ER_WRONG_PARAMCOUNT_TO_PROCEDURE)

消息：对于过程'%s'，参数计数不正确

· 错误：1108 SQLSTATE: HY000 (ER_WRONG_PARAMETERS_TO_PROCEDURE)

消息：对于过程'%s'，参数不正确

· 错误：1109 SQLSTATE: 42S02 (ER_UNKNOWN_TABLE)

消息：%s中的未知表%s

· 错误：1110 SQLSTATE: 42000 (ER_FIELD_SPECIFIED_TWICE)

消息：列'%s'被指定了两次。

· 错误：1111 SQLSTATE: HY000 (ER_INVALID_GROUP_FUNC_USE)

消息：无效的分组函数使用

· 错误：1112 SQLSTATE: 42000 (ER_UNSUPPORTED_EXTENSION)

消息：表'%s'使用了该MySQL版本中不存在的扩展。

· 错误：1113 SQLSTATE: 42000 (ER_TABLE_MUST_HAVE_COLUMNS)

消息：1个表至少要有1列。

· 错误：1114 SQLSTATE: HY000 (ER_RECORD_FILE_FULL)

消息：表'%s'已满。

· 错误：1115 SQLSTATE: 42000 (ER_UNKNOWN_CHARACTER_SET)

消息：未知字符集'%s'。

· 错误：1116 SQLSTATE: HY000 (ER_TOO_MANY_TABLES)

消息：表过多，MySQL在1个联合操作中只能使用%d个表。

· 错误：1117 SQLSTATE: HY000 (ER_TOO_MANY_FIELDS)

消息：列过多。

· 错误：1118 SQLSTATE: 42000 (ER_TOO_BIG_ROWSIZE)

消息：行的大小过大。对于所使用的表类型，不包括BLOB，最大行大小为%ld。必须将某些列更改为TEXT或BLOB。

· 错误：1119 SQLSTATE: HY000 (ER_STACK_OVERRUN)

消息：线程堆栈溢出，已使用，%ld堆栈的%ld。如果需要，请使用'mysqld -O thread_stack=#'指定较大的堆栈。

· 错误：1120 SQLSTATE: 42000 (ER_WRONG_OUTER_JOIN)

消息：在OUTER JOIN中发现交叉关联，请检查ON条件。

· 错误：1121 SQLSTATE: 42000 (ER_NULL_COLUMN_IN_INDEX)

消息：列'%s'与UNIQUE或INDEX一起使用，但未定义为NOT NULL。

· 错误：1122 SQLSTATE: HY000 (ER_CANT_FIND_UDF)

消息：无法加载函数'%s'。

· 错误：1123 SQLSTATE: HY000 (ER_CANT_INITIALIZE_UDF)

消息：无法初始化函数'%s'; %s

· 错误：1124 SQLSTATE: HY000 (ER_UDF_NO_PATHS)

消息：对于共享库，不允许任何路径。

· 错误：1125 SQLSTATE: HY000 (ER_UDF_EXISTS)

消息：函数'%s'已存在。

· 错误：1126 SQLSTATE: HY000 (ER_CANT_OPEN_LIBRARY)

消息：不能打开共享库'%s' (errno: %d %s)

· 错误：1127 SQLSTATE: HY000 (ER_CANT_FIND_DL_ENTRY)

消息：不能发现库中的符号'%s'。

· 错误：1128 SQLSTATE: HY000 (ER_FUNCTION_NOT_DEFINED)

消息：函数'%s'未定义。

· 错误：1129 SQLSTATE: HY000 (ER_HOST_IS_BLOCKED)

消息：由于存在很多连接错误，主机'%s'被屏蔽，请用'mysqladmin flush-hosts'解除屏蔽。

· 错误：1130 SQLSTATE: HY000 (ER_HOST_NOT_PRIVILEGED)

消息：不允许将主机'%s'连接到该MySQL服务器。

· 错误：1131 SQLSTATE: 42000 (ER_PASSWORD_ANONYMOUS_USER)

消息：你正在已匿名用户身份使用MySQL，不允许匿名用户更改密码。

· 错误：1132 SQLSTATE: 42000 (ER_PASSWORD_NOT_ALLOWED)

消息：必须有更新mysql数据库中表的权限才能更改密码。

· 错误：1133 SQLSTATE: 42000 (ER_PASSWORD_NO_MATCH)

消息：无法在用户表中找到匹配行。

· 错误：1134 SQLSTATE: HY000 (ER_UPDATE_INFO)

消息：行匹配，%ld；已更改，%ld；警告，%ld

· 错误：1135 SQLSTATE: HY000 (ER_CANT_CREATE_THREAD)

消息：无法创建新线程(errno %d)，如果未出现内存溢出，请参阅手册以了解可能的与操作系统有关的缺陷。

· 错误：1136 SQLSTATE: 21S01 (ER_WRONG_VALUE_COUNT_ON_ROW)

消息：列计数不匹配行%ld上的值计数。

· 错误：1137 SQLSTATE: HY000 (ER_CANT_REOPEN_TABLE)

消息：无法再次打开表'%s'。

· 错误：1138 SQLSTATE: 22004 (ER_INVALID_USE_OF_NULL)

消息：NULL值使用无效。

· 错误：1139 SQLSTATE: 42000 (ER_REGEXP_ERROR)

消息：获得来自regexp的错误'%s'。

· 错误：1140 SQLSTATE: 42000 (ER_MIX_OF_GROUP_FUNC_AND_FIELDS)

消息：如果没有GROUP BY子句，GROUP列 (MIN(),MAX(),COUNT(),...)与非GROUP列的混合不合法。

· 错误：1141 SQLSTATE: 42000 (ER_NONEXISTING_GRANT)

消息：没有为主机'%s'上的用户'%s'定义这类授权。

· 错误：1142 SQLSTATE: 42000 (ER_TABLEACCESS_DENIED_ERROR)

消息：拒绝用户'%s'@'%s'在表'%s'上使用%s命令。

· 错误：1143 SQLSTATE: 42000 (ER_COLUMNACCESS_DENIED_ERROR)

消息：拒绝用户'%s'@'%s'在表'%s'的'%s'上使用%s命令。

· 错误：1144 SQLSTATE: 42000 (ER_ILLEGAL_GRANT_FOR_TABLE)

消息：非法GRANT/REVOKE命令，请参阅手册以了解可使用那种权限。

· 错误：1145 SQLSTATE: 42000 (ER_GRANT_WRONG_HOST_OR_USER)

消息：GRANT的主机或用户参量过长。

· 错误：1146 SQLSTATE: 42S02 (ER_NO_SUCH_TABLE)

消息：表'%s.%s'不存在。

· 错误：1147 SQLSTATE: 42000 (ER_NONEXISTING_TABLE_GRANT)

消息：在表'%s'上没有为主机'%s'上的用户'%s'定义的这类授权。

· 错误：1148 SQLSTATE: 42000 (ER_NOT_ALLOWED_COMMAND)

消息：所使用的命令在该MySQL版本中不允许。

· 错误：1149 SQLSTATE: 42000 (ER_SYNTAX_ERROR)

消息：存在SQL语法错误，请参阅与你的MySQL版本对应的手册，以了解正确的语法。

· 错误：1150 SQLSTATE: HY000 (ER_DELAYED_CANT_CHANGE_LOCK)

消息：对于表%s，延迟的插入线程不能获得请求的锁定。

· 错误：1151 SQLSTATE: HY000 (ER_TOO_MANY_DELAYED_THREADS)

消息：使用的延迟线程过多。

· 错误：1152 SQLSTATE: 08S01 (ER_ABORTING_CONNECTION)

消息：与数据库'%s'和用户'%s'的连接%ld失败 (%s)

· 错误：1153 SQLSTATE: 08S01 (ER_NET_PACKET_TOO_LARGE)

消息：获得信息包大于'max_allowed_packet'字节。

· 错误：1154 SQLSTATE: 08S01 (ER_NET_READ_ERROR_FROM_PIPE)

消息：获得来自连接管道的读错误。

· 错误：1155 SQLSTATE: 08S01 (ER_NET_FCNTL_ERROR)

消息：获得来自fcntl()的错误。

· 错误：1156 SQLSTATE: 08S01 (ER_NET_PACKETS_OUT_OF_ORDER)

消息：获得信息包无序。

· 错误：1157 SQLSTATE: 08S01 (ER_NET_UNCOMPRESS_ERROR)

消息：无法解压缩通信信息包。

· 错误：1158 SQLSTATE: 08S01 (ER_NET_READ_ERROR)

消息：读取通信信息包时出错。

· 错误：1159 SQLSTATE: 08S01 (ER_NET_READ_INTERRUPTED)

消息：读取通信信息包时出现超时。

· 错误：1160 SQLSTATE: 08S01 (ER_NET_ERROR_ON_WRITE)

消息：写入通信信息包时出错。

· 错误：1161 SQLSTATE: 08S01 (ER_NET_WRITE_INTERRUPTED)

消息：写入通信信息包时出现超时。

· 错误：1162 SQLSTATE: 42000 (ER_TOO_LONG_STRING)

消息：结果字符串长于'max_allowed_packet'字节。

· 错误：1163 SQLSTATE: 42000 (ER_TABLE_CANT_HANDLE_BLOB)

消息：所使用的表类型不支持BLOB/TEXT列。

· 错误：1164 SQLSTATE: 42000 (ER_TABLE_CANT_HANDLE_AUTO_INCREMENT)

消息：所使用的表类型不支持AUTO_INCREMENT列。

· 错误：1165 SQLSTATE: HY000 (ER_DELAYED_INSERT_TABLE_LOCKED)

消息：由于用LOCK TABLES锁定了表，INSERT DELAYED不能与表'%s'一起使用。

· 错误：1166 SQLSTATE: 42000 (ER_WRONG_COLUMN_NAME)

消息：不正确的列名'%s'。

· 错误：1167 SQLSTATE: 42000 (ER_WRONG_KEY_COLUMN)

消息：所使用的存储引擎不能为列'%s'编制索引。

· 错误：1168 SQLSTATE: HY000 (ER_WRONG_MRG_TABLE)

消息：MERGE表中的所有表未同等定义。

· 错误：1169 SQLSTATE: 23000 (ER_DUP_UNIQUE)

消息：由于唯一性限制，不能写入到表'%s'。

· 错误：1170 SQLSTATE: 42000 (ER_BLOB_KEY_WITHOUT_LENGTH)

消息：在未指定键长度的键说明中使用了BLOB/TEXT列'%s'。

· 错误：1171 SQLSTATE: 42000 (ER_PRIMARY_CANT_HAVE_NULL)

消息：PRIMARY KEY的所有部分必须是NOT NULL，如果需要为NULL的关键字，请使用UNIQUE取而代之。

· 错误：1172 SQLSTATE: 42000 (ER_TOO_MANY_ROWS)

消息：结果有1个以上的行组成。

· 错误：1173 SQLSTATE: 42000 (ER_REQUIRES_PRIMARY_KEY)

消息：该表类型要求主键。

· 错误：1174 SQLSTATE: HY000 (ER_NO_RAID_COMPILED)

消息：该MySQL版本是未使用RAID支持而编译的。

· 错误：1175 SQLSTATE: HY000 (ER_UPDATE_WITHOUT_KEY_IN_SAFE_MODE)

消息：你正在使用安全更新模式，而且试图在不使用WHERE的情况下更新使用了KEY列的表。

· 错误：1176 SQLSTATE: HY000 (ER_KEY_DOES_NOT_EXITS)

消息：在表'%s'中，键'%s'不存在。

· 错误：1177 SQLSTATE: 42000 (ER_CHECK_NO_SUCH_TABLE)

消息：无法打开表。

· 错误：1178 SQLSTATE: 42000 (ER_CHECK_NOT_IMPLEMENTED)

消息：用于表的引擎不支持%s。

· 错误：1179 SQLSTATE: 25000 (ER_CANT_DO_THIS_DURING_AN_TRANSACTION)

消息：不允许在事务中执行该命令。

· 错误：1180 SQLSTATE: HY000 (ER_ERROR_DURING_COMMIT)

消息：在COMMIT期间出现错误%d。

· 错误：1181 SQLSTATE: HY000 (ER_ERROR_DURING_ROLLBACK)

消息：在ROLLBACK期间出现错误%d。

· 错误：1182 SQLSTATE: HY000 (ER_ERROR_DURING_FLUSH_LOGS)

消息：在FLUSH_LOGS期间出现错误%d。

· 错误：1183 SQLSTATE: HY000 (ER_ERROR_DURING_CHECKPOINT)

消息：在CHECKPOINT期间出现错误%d。

· 错误：1184 SQLSTATE: 08S01 (ER_NEW_ABORTING_CONNECTION)

消息：与数据库'%s'、用户'%s'和主机'%s'的连接%ld失败 (%s)。

· 错误：1185 SQLSTATE: HY000 (ER_DUMP_NOT_IMPLEMENTED)

消息：针对表的存储引擎不支持二进制表转储。

· 错误：1186 SQLSTATE: HY000 (ER_FLUSH_MASTER_BINLOG_CLOSED)

消息：Binlog已关闭，不能RESET MASTER。

· 错误：1187 SQLSTATE: HY000 (ER_INDEX_REBUILD)

消息：重新创建转储表'%s'的索引失败。

· 错误：1188 SQLSTATE: HY000 (ER_MASTER)

消息：来自主连接'%s'的错误。

· 错误：1189 SQLSTATE: 08S01 (ER_MASTER_NET_READ)

消息：读取主连接时出现网络错误。

· 错误：1190 SQLSTATE: 08S01 (ER_MASTER_NET_WRITE)

消息：写入主连接时出现网络错误。

· 错误：1191 SQLSTATE: HY000 (ER_FT_MATCHING_KEY_NOT_FOUND)

消息：无法找到与列列表匹配的FULLTEXT索引。

· 错误：1192 SQLSTATE: HY000 (ER_LOCK_OR_ACTIVE_TRANSACTION)

消息：由于存在活动的锁定表或活动的事务，不能执行给定的命令。

· 错误：1193 SQLSTATE: HY000 (ER_UNKNOWN_SYSTEM_VARIABLE)

消息：未知的系统变量'%s'。

· 错误：1194 SQLSTATE: HY000 (ER_CRASHED_ON_USAGE)

消息：表'%s'被标记为崩溃，应予以修复。

· 错误：1195 SQLSTATE: HY000 (ER_CRASHED_ON_REPAIR)

消息：表'%s'被标记为崩溃，而且上次修复失败（自动？）

· 错误：1196 SQLSTATE: HY000 (ER_WARNING_NOT_COMPLETE_ROLLBACK)

消息：不能回滚某些非事务性已变动表。

· 错误：1197 SQLSTATE: HY000 (ER_TRANS_CACHE_FULL)

消息：多语句事务要求更多的'max_binlog_cache_size'存储字节，增大mysqld变量，并再次尝试。

· 错误：1198 SQLSTATE: HY000 (ER_SLAVE_MUST_STOP)

消息：运行从实例时不能执行该操作，请首先运行STOP SLAVE。

· 错误：1199 SQLSTATE: HY000 (ER_SLAVE_NOT_RUNNING)

消息：该操作需要运行的从实例，请配置SLAVE并执行START SLAVE。

· 错误：1200 SQLSTATE: HY000 (ER_BAD_SLAVE)

消息：服务器未配置为从服务器，请更正config文件，或使用CHANGE MASTER TO。

· 错误：1201 SQLSTATE: HY000 (ER_MASTER_INFO)

消息：无法初始化主服务器信息结构，在MySQL错误日志中可找到更多错误消息。

· 错误：1202 SQLSTATE: HY000 (ER_SLAVE_THREAD)

消息：无法创建从线程，请检查系统资源。

· 错误：1203 SQLSTATE: 42000 (ER_TOO_MANY_USER_CONNECTIONS)

消息：用户%s已有了超过'max_user_connections'的活动连接。

· 错误：1204 SQLSTATE: HY000 (ER_SET_CONSTANTS_ONLY)

消息：或许仅应与SET一起使用常量表达式。

· 错误：1205 SQLSTATE: HY000 (ER_LOCK_WAIT_TIMEOUT)

消息：超过了锁定等待超时，请尝试重新启动事务。

· 错误：1206 SQLSTATE: HY000 (ER_LOCK_TABLE_FULL)

消息：总的锁定数超出了锁定表的大小。

· 错误：1207 SQLSTATE: 25000 (ER_READ_ONLY_TRANSACTION)

消息：在READ UNCOMMITTED事务期间，无法获得更新锁定。

· 错误：1208 SQLSTATE: HY000 (ER_DROP_DB_WITH_READ_LOCK)

消息：当线程保持为全局读锁定时，不允许DROP DATABASE。

· 错误：1209 SQLSTATE: HY000 (ER_CREATE_DB_WITH_READ_LOCK)

消息：当线程保持为全局读锁定时，不允许CREATE DATABASE。

· 错误：1210 SQLSTATE: HY000 (ER_WRONG_ARGUMENTS)

消息：为%s提供的参量不正确。

· 错误：1211 SQLSTATE: 42000 (ER_NO_PERMISSION_TO_CREATE_USER)

消息：不允许'%s'@'%s'创建新用户。

· 错误：1212 SQLSTATE: HY000 (ER_UNION_TABLES_IN_DIFFERENT_DIR)

消息：不正确的表定义，所有的MERGE表必须位于相同的数据库中。

· 错误：1213 SQLSTATE: 40001 (ER_LOCK_DEADLOCK)

消息：试图获取锁定时发现死锁，请尝试重新启动事务。

· 错误：1214 SQLSTATE: HY000 (ER_TABLE_CANT_HANDLE_FT)

消息：所使用的表类型不支持FULLTEXT索引。

· 错误：1215 SQLSTATE: HY000 (ER_CANNOT_ADD_FOREIGN)

消息：无法添加外键约束。

· 错误：1216 SQLSTATE: 23000 (ER_NO_REFERENCED_ROW)

消息：无法添加或更新子行，外键约束失败。

· 错误：1217 SQLSTATE: 23000 (ER_ROW_IS_REFERENCED)

消息：无法删除或更新父行，外键约束失败。

· 错误：1218 SQLSTATE: 08S01 (ER_CONNECT_TO_MASTER)

消息：连接至主服务器%s时出错。

· 错误：1219 SQLSTATE: HY000 (ER_QUERY_ON_MASTER)

消息：在主服务器%s上执行查询时出错。

· 错误：1220 SQLSTATE: HY000 (ER_ERROR_WHEN_EXECUTING_COMMAND)

消息：执行命令%s: %s时出错。

· 错误：1221 SQLSTATE: HY000 (ER_WRONG_USAGE)

消息：%s和%s的用法不正确。

· 错误：1222 SQLSTATE: 21000 (ER_WRONG_NUMBER_OF_COLUMNS_IN_SELECT)

消息：所使用的SELECT语句有不同的列数。

· 错误：1223 SQLSTATE: HY000 (ER_CANT_UPDATE_WITH_READLOCK)

消息：由于存在冲突的读锁定，无法执行查询。

· 错误：1224 SQLSTATE: HY000 (ER_MIXING_NOT_ALLOWED)

消息：禁止混合事务性表和非事务性表。

· 错误：1225 SQLSTATE: HY000 (ER_DUP_ARGUMENT)

消息：在语句中使用了两次选项'%s'。

· 错误：1226 SQLSTATE: 42000 (ER_USER_LIMIT_REACHED)

消息：用户'%s'超出了'%s'资源（当前值：%ld）。

· 错误：1227 SQLSTATE: 42000 (ER_SPECIFIC_ACCESS_DENIED_ERROR)

消息：拒绝访问，需要%s权限才能执行该操作。

· 错误：1228 SQLSTATE: HY000 (ER_LOCAL_VARIABLE)

消息：变量'%s'是1种SESSION变量，不能与SET GLOBAL一起使用。

· 错误：1229 SQLSTATE: HY000 (ER_GLOBAL_VARIABLE)

消息：变量'%s'是1种GLOBAL变量，应使用SET GLOBAL来设置它。

· 错误：1230 SQLSTATE: 42000 (ER_NO_DEFAULT)

消息：变量'%s'没有默认值。

· 错误：1231 SQLSTATE: 42000 (ER_WRONG_VALUE_FOR_VAR)

消息：变量'%s'不能设置为值'%s'。

· 错误：1232 SQLSTATE: 42000 (ER_WRONG_TYPE_FOR_VAR)

消息：变量'%s'的参量类型不正确。

· 错误：1233 SQLSTATE: HY000 (ER_VAR_CANT_BE_READ)

消息：变量'%s'只能被设置，不能被读取。

· 错误：1234 SQLSTATE: 42000 (ER_CANT_USE_OPTION_HERE)

消息：不正确的'%s'用法/位置。

· 错误：1235 SQLSTATE: 42000 (ER_NOT_SUPPORTED_YET)

消息：该MySQL版本尚不支持'%s'。

· 错误：1236 SQLSTATE: HY000 (ER_MASTER_FATAL_ERROR_READING_BINLOG)

消息：从二进制日志读取数据时，获得来自主服务器的致命错误%d: '%s'。

· 错误：1237 SQLSTATE: HY000 (ER_SLAVE_IGNORED_TABLE)

消息：由于“replicate-*-table”规则，从SQL线程忽略了查询。。

· 错误：1238 SQLSTATE: HY000 (ER_INCORRECT_GLOBAL_LOCAL_VAR)

消息：变量'%s'是一种%s变量。

· 错误：1239 SQLSTATE: 42000 (ER_WRONG_FK_DEF)

消息：对于 '%s': %s， 外键定义不正确。

· 错误：1240 SQLSTATE: HY000 (ER_KEY_REF_DO_NOT_MATCH_TABLE_REF)

消息：键引用和表引用不匹配。

· 错误：1241 SQLSTATE: 21000 (ER_OPERAND_COLUMNS)

消息：操作数应包含%d列。

· 错误：1242 SQLSTATE: 21000 (ER_SUBQUERY_NO_1_ROW)

消息：子查询返回1行以上。

· 错误：1243 SQLSTATE: HY000 (ER_UNKNOWN_STMT_HANDLER)

消息：指定给%s的未知预处理语句句柄。

· 错误：1244 SQLSTATE: HY000 (ER_CORRUPT_HELP_DB)

消息：帮助数据库崩溃或不存在。

· 错误：1245 SQLSTATE: HY000 (ER_CYCLIC_REFERENCE)

消息：对子查询的循环引用。

· 错误：1246 SQLSTATE: HY000 (ER_AUTO_CONVERT)

消息：将列'%s'从%s转换为%s。

· 错误：1247 SQLSTATE: 42S22 (ER_ILLEGAL_REFERENCE)

消息：引用'%s'不被支持 (%s)。

· 错误：1248 SQLSTATE: 42000 (ER_DERIVED_MUST_HAVE_ALIAS)

消息：所有的导出表必须有自己的别名。

· 错误：1249 SQLSTATE: 01000 (ER_SELECT_REDUCED)

消息：在优化期间简化了选择%u。

· 错误：1250 SQLSTATE: 42000 (ER_TABLENAME_NOT_ALLOWED_HERE)

消息：来自某一SELECT的表'%s'不能在%s中使用。

· 错误：1251 SQLSTATE: 08004 (ER_NOT_SUPPORTED_AUTH_MODE)

消息：客户端不支持服务器请求的鉴定协议，请考虑升级MySQL客户端。

· 错误：1252 SQLSTATE: 42000 (ER_SPATIAL_CANT_HAVE_NULL)

消息：SPATIAL索引的所有部分必须是NOT NULL。

· 错误：1253 SQLSTATE: 42000 (ER_COLLATION_CHARSET_MISMATCH)

消息：对于CHARACTER SET '%s'，COLLATION '%s'无效。

· 错误：1254 SQLSTATE: HY000 (ER_SLAVE_WAS_RUNNING)

消息：从服务器正在运行。

· 错误：1255 SQLSTATE: HY000 (ER_SLAVE_WAS_NOT_RUNNING)

消息：从服务器已停止。

· 错误：1256 SQLSTATE: HY000 (ER_TOO_BIG_FOR_UNCOMPRESS)

消息：解压的数据过大，最大大小为%d（也可能是，解压数据的长度已损坏）。

· 错误：1257 SQLSTATE: HY000 (ER_ZLIB_Z_MEM_ERROR)

消息：ZLIB，无足够内存。

· 错误：1258 SQLSTATE: HY000 (ER_ZLIB_Z_BUF_ERROR)

消息：ZLIB，输出缓冲区内无足够空间（也可能是，解压数据的长度已损坏）。

· 错误：1259 SQLSTATE: HY000 (ER_ZLIB_Z_DATA_ERROR)

消息：ZLIB，输入数据已损坏。

· 错误：1260 SQLSTATE: HY000 (ER_CUT_VALUE_GROUP_CONCAT)

消息：%d行被GROUP_CONCAT()截去。

· 错误：1261 SQLSTATE: 01000 (ER_WARN_TOO_FEW_RECORDS)

消息：行%ld不包含所有列的数据。

· 错误：1262 SQLSTATE: 01000 (ER_WARN_TOO_MANY_RECORDS)

消息：行%ld被解短，它包含的数据大于输入列中的数据。

· 错误：1263 SQLSTATE: 22004 (ER_WARN_NULL_TO_NOTNULL)

消息：列被设为默认值，在行%ld上将NULL提供给了NOT NULL列。

· 错误：1264 SQLSTATE: 22003 (ER_WARN_DATA_OUT_OF_RANGE)

消息：为行%ld上的列'%s'调整超出范围的值。

· 错误：1265 SQLSTATE: 01000 (WARN_DATA_TRUNCATED)

消息：为行%ld上的列'%s'截短数据。

· 错误：1266 SQLSTATE: HY000 (ER_WARN_USING_OTHER_HANDLER)

消息：为表%s使用存储引擎%s。

· 错误：1267 SQLSTATE: HY000 (ER_CANT_AGGREGATE_2COLLATIONS)

消息：对于操作'%s'，非法混合了校对(%s,%s)和(%s,%s)。

· 错误：1268 SQLSTATE: HY000 (ER_DROP_USER)

消息：无法撤销1个或多个请求的用户。

· 错误：1269 SQLSTATE: HY000 (ER_REVOKE_GRANTS)

消息：无法撤销所有权限，为1个或多个请求的用户授权。

· 错误：1270 SQLSTATE: HY000 (ER_CANT_AGGREGATE_3COLLATIONS)

消息：对于操作'%s'，非法混合了校对(%s,%s)、(%s,%s)和(%s,%s)。

· 错误：1271 SQLSTATE: HY000 (ER_CANT_AGGREGATE_NCOLLATIONS)

消息：对于操作'%s'，非法混合了校对。

· 错误：1272 SQLSTATE: HY000 (ER_VARIABLE_IS_NOT_STRUCT)

消息：变量'%s'不是变量组分（不能用作XXXX.variable_name）。

· 错误：1273 SQLSTATE: HY000 (ER_UNKNOWN_COLLATION)

消息：未知校对'%s'。

· 错误：1274 SQLSTATE: HY000 (ER_SLAVE_IGNORED_SSL_PARAMS)

消息：由于该MySQL从服务器是在不支持SSL的情况下编译的，CHANGE MASTER中的SSL参数被忽略，随后，如果启动了具备SSL功能的MySQL，可使用这些参数。

· 错误：1275 SQLSTATE: HY000 (ER_SERVER_IS_IN_SECURE_AUTH_MODE)

消息：服务器正运行在“--secure-auth”模式下，但'%s'@'%s'有1个采用旧格式的密码，请将密码更改为新格式。

· 错误：1276 SQLSTATE: HY000 (ER_WARN_FIELD_RESOLVED)

消息：SELECT #%d的字段或引用'%s%s%s%s%s'是在SELECT #%d中确定的。

· 错误：1277 SQLSTATE: HY000 (ER_BAD_SLAVE_UNTIL_COND)

消息：对于START SLAVE UNTIL，不正确的参数或参数组合。

· 错误：1278 SQLSTATE: HY000 (ER_MISSING_SKIP_SLAVE)

消息：与START SLAVE UNTIL一起执行按步复制时，建议使用“--skip-slave-start”，否则，如果发生未预料的从服务器mysqld重启，间出现问题。

· 错误：1279 SQLSTATE: HY000 (ER_UNTIL_COND_IGNORED)

消息：SQL线程未启动，因而UNTIL选项被忽略。

· 错误：1280 SQLSTATE: 42000 (ER_WRONG_NAME_FOR_INDEX)

消息：不正确的索引名'%s'。

· 错误：1281 SQLSTATE: 42000 (ER_WRONG_NAME_FOR_CATALOG)

消息：不正确的目录名'%s'。

· 错误：1282 SQLSTATE: HY000 (ER_WARN_QC_RESIZE)

消息：查询高速缓冲设置大小%lu时失败，新的查询高速缓冲的大小是%lu。

· 错误：1283 SQLSTATE: HY000 (ER_BAD_FT_COLUMN)

消息：列'%s'不能是FULLTEXT索引的一部分。

· 错误：1284 SQLSTATE: HY000 (ER_UNKNOWN_KEY_CACHE)

消息：未知的键高速缓冲'%s'。

· 错误：1285 SQLSTATE: HY000 (ER_WARN_HOSTNAME_WONT_WORK)

消息：MySQL是在“--skip-name-resolve”模式下启动的，必须在不使用该开关的情况下重启它，以便该授权能起作用。

· 错误：1286 SQLSTATE: 42000 (ER_UNKNOWN_STORAGE_ENGINE)

消息：未知的表引擎'%s'。

· 错误：1287 SQLSTATE: HY000 (ER_WARN_DEPRECATED_SYNTAX)

消息：'%s'已过时，请使用'%s'取而代之。

· 错误：1288 SQLSTATE: HY000 (ER_NON_UPDATABLE_TABLE)

消息：%s的目标表%s不可更新。

· 错误：1289 SQLSTATE: HY000 (ER_FEATURE_DISABLED)

消息：'%s'特性已被禁止，要想使其工作，需要用'%s'创建MySQL。

· 错误：1290 SQLSTATE: HY000 (ER_OPTION_PREVENTS_STATEMENT)

消息：MySQL正使用%s选项运行，因此不能执行该语句。

· 错误：1291 SQLSTATE: HY000 (ER_DUPLICATED_VALUE_IN_TYPE)

消息：列'%s'在%s中有重复值'%s'。

· 错误：1292 SQLSTATE: 22007 (ER_TRUNCATED_WRONG_VALUE)

消息：截短了不正确的%s值: '%s'

· 错误：1293 SQLSTATE: HY000 (ER_TOO_MUCH_AUTO_TIMESTAMP_COLS)

消息：不正确的表定义，在DEFAULT或ON UPDATE子句中，对于CURRENT_TIMESTAMP，只能有一个TIMESTAMP列。

· 错误：1294 SQLSTATE: HY000 (ER_INVALID_ON_UPDATE)

消息：对于'%s'列，ON UPDATE子句无效。

· 错误：1295 SQLSTATE: HY000 (ER_UNSUPPORTED_PS)

消息：在预处理语句协议中，尚不支持该命令。

· 错误：1296 SQLSTATE: HY000 (ER_GET_ERRMSG)

消息：从%s获得错误%d '%s'。

· 错误：1297 SQLSTATE: HY000 (ER_GET_TEMPORARY_ERRMSG)

消息：从%s获得临时错误%d '%s'。

· 错误：1298 SQLSTATE: HY000 (ER_UNKNOWN_TIME_ZONE)

消息：未知或不正确的时区: '%s'

· 错误：1299 SQLSTATE: HY000 (ER_WARN_INVALID_TIMESTAMP)

消息：在行%ld的列'%s'中存在无效的TIMESTAMP值。

· 错误：1300 SQLSTATE: HY000 (ER_INVALID_CHARACTER_STRING)

消息：无效的%s字符串: '%s'

· 错误：1301 SQLSTATE: HY000 (ER_WARN_ALLOWED_PACKET_OVERFLOWED)

消息：%s()的结果大于max_allowed_packet (%ld)，已截短

· 错误：1302 SQLSTATE: HY000 (ER_CONFLICTING_DECLARATIONS)

消息：冲突声明：'%s%s'和'%s%s'

· 错误：1303 SQLSTATE: 2F003 (ER_SP_NO_RECURSIVE_CREATE)

消息：不能从另一个存储子程序中创建%s。

· 错误：1304 SQLSTATE: 42000 (ER_SP_ALREADY_EXISTS)

消息：%s %s已存在。

· 错误：1305 SQLSTATE: 42000 (ER_SP_DOES_NOT_EXIST)

消息：%s %s不存在。

· 错误：1306 SQLSTATE: HY000 (ER_SP_DROP_FAILED)

消息：DROP %s %s失败

· 错误：1307 SQLSTATE: HY000 (ER_SP_STORE_FAILED)

消息：CREATE %s %s失败。

· 错误：1308 SQLSTATE: 42000 (ER_SP_LILABEL_MISMATCH)

消息：%s无匹配标签: %s

· 错误：1309 SQLSTATE: 42000 (ER_SP_LABEL_REDEFINE)

消息：重新定义标签%s

· 错误：1310 SQLSTATE: 42000 (ER_SP_LABEL_MISMATCH)

消息：末端标签%s无匹配项

· 错误：1311 SQLSTATE: 01000 (ER_SP_UNINIT_VAR)

消息：正在引用未初始化的变量%s。

· 错误：1312 SQLSTATE: 0A000 (ER_SP_BADSELECT)

消息：PROCEDURE %s不能在给定场景下返回结果集。

· 错误：1313 SQLSTATE: 42000 (ER_SP_BADRETURN)

消息：仅在FUNCTION中允许RETURN。

· 错误：1314 SQLSTATE: 0A000 (ER_SP_BADSTATEMENT)

消息：在存储程序中不允许%s。

· 错误：1315 SQLSTATE: 42000 (ER_UPDATE_LOG_DEPRECATED_IGNORED)

消息：更新日志已被放弃，并用二进制日志取代，SET SQL_LOG_UPDATE被忽略。

· 错误：1316 SQLSTATE: 42000 (ER_UPDATE_LOG_DEPRECATED_TRANSLATED)

消息：更新日志已被放弃，并用二进制日志取代，SET SQL_LOG_UPDATE已被截短为SET SQL_LOG_BIN。

· 错误：1317 SQLSTATE: 70100 (ER_QUERY_INTERRUPTED)

消息：查询执行被中断。

· 错误：1318 SQLSTATE: 42000 (ER_SP_WRONG_NO_OF_ARGS)

消息：对于%s %s，参量数目不正确，预期为%u，但却是%u。

· 错误：1319 SQLSTATE: 42000 (ER_SP_COND_MISMATCH)

消息：未定义的CONDITION: %s

· 错误：1320 SQLSTATE: 42000 (ER_SP_NORETURN)

消息：在FUNCTION %s中未发现RETURN。

· 错误：1321 SQLSTATE: 2F005 (ER_SP_NORETURNEND)

消息：FUNCTION %s结束时缺少RETURN。

· 错误：1322 SQLSTATE: 42000 (ER_SP_BAD_CURSOR_QUERY)

消息：光标语句必须是SELECT。

· 错误：1323 SQLSTATE: 42000 (ER_SP_BAD_CURSOR_SELECT)

消息：光标SELECT不得有INTO。

· 错误：1324 SQLSTATE: 42000 (ER_SP_CURSOR_MISMATCH)

消息：未定义的CURSOR: %s

· 错误：1325 SQLSTATE: 24000 (ER_SP_CURSOR_ALREADY_OPEN)

消息：光标已打开

· 错误：1326 SQLSTATE: 24000 (ER_SP_CURSOR_NOT_OPEN)

消息：光标未打开

· 错误：1327 SQLSTATE: 42000 (ER_SP_UNDECLARED_VAR)

消息：未声明的变量：%s

· 错误：1328 SQLSTATE: HY000 (ER_SP_WRONG_NO_OF_FETCH_ARGS)

消息：不正确的FETCH变量数目。

· 错误：1329 SQLSTATE: 02000 (ER_SP_FETCH_NO_DATA)

消息：FETCH无数据。

· 错误：1330 SQLSTATE: 42000 (ER_SP_DUP_PARAM)

消息：重复参数: %s

· 错误：1331 SQLSTATE: 42000 (ER_SP_DUP_VAR)

消息：重复变量: %s

· 错误：1332 SQLSTATE: 42000 (ER_SP_DUP_COND)

消息：重复条件: %s

· 错误：1333 SQLSTATE: 42000 (ER_SP_DUP_CURS)

消息：重复光标: %s

· 错误：1334 SQLSTATE: HY000 (ER_SP_CANT_ALTER)

消息：ALTER %s %s失败。

· 错误：1335 SQLSTATE: 0A000 (ER_SP_SUBSELECT_NYI)

消息：不支持Subselect值。

· 错误：1336 SQLSTATE: 0A000 (ER_STMT_NOT_ALLOWED_IN_SF_OR_TRG)

消息：在存储函数或触发程序中，不允许%s。

· 错误：1337 SQLSTATE: 42000 (ER_SP_VARCOND_AFTER_CURSHNDLR)

消息：光标或句柄声明后面的变量或条件声明。

· 错误：1338 SQLSTATE: 42000 (ER_SP_CURSOR_AFTER_HANDLER)

消息：句柄声明后面的光标声明。

· 错误：1339 SQLSTATE: 20000 (ER_SP_CASE_NOT_FOUND)

消息：对于CASE语句，未发现Case。

· 错误：1340 SQLSTATE: HY000 (ER_FPARSER_TOO_BIG_FILE)

消息：配置文件'%s'过大。

· 错误：1341 SQLSTATE: HY000 (ER_FPARSER_BAD_HEADER)

消息：文件'%s'中存在残缺的文件类型标题。

· 错误：1342 SQLSTATE: HY000 (ER_FPARSER_EOF_IN_COMMENT)

消息：解析'%s'时，文件意外结束。

· 错误：1343 SQLSTATE: HY000 (ER_FPARSER_ERROR_IN_PARAMETER)

消息：解析参数'%s'时出错（行：'%s'）。

· 错误：1344 SQLSTATE: HY000 (ER_FPARSER_EOF_IN_UNKNOWN_PARAMETER)

消息：跳过未知参数'%s'时，文件意外结束。

· 错误：1345 SQLSTATE: HY000 (ER_VIEW_NO_EXPLAIN)

消息：EXPLAIN/SHOW无法发出，缺少对基本表的权限。

· 错误：1346 SQLSTATE: HY000 (ER_FRM_UNKNOWN_TYPE)

消息：文件'%s'在其题头中有未知的类型'%s'。

· 错误：1347 SQLSTATE: HY000 (ER_WRONG_OBJECT)

消息：'%s.%s'不是%s。

· 错误：1348 SQLSTATE: HY000 (ER_NONUPDATEABLE_COLUMN)

消息：列'%s'不可更新。

· 错误：1349 SQLSTATE: HY000 (ER_VIEW_SELECT_DERIVED)

消息：视图的SELECT在FROM子句中包含子查询。

· 错误：1350 SQLSTATE: HY000 (ER_VIEW_SELECT_CLAUSE)

消息：视图的SELECT包含'%s'子句。

· 错误：1351 SQLSTATE: HY000 (ER_VIEW_SELECT_VARIABLE)

消息：视图的SELECT包含1个变量或参数。

· 错误：1352 SQLSTATE: HY000 (ER_VIEW_SELECT_TMPTABLE)

消息：视图的SELECT引用了临时表'%s'。

· 错误：1353 SQLSTATE: HY000 (ER_VIEW_WRONG_LIST)

消息：视图的SELECT和视图的字段列表有不同的列计数。

· 错误：1354 SQLSTATE: HY000 (ER_WARN_VIEW_MERGE)

消息：此时，不能在这里使用视图合并算法（假定未定义算法）。

· 错误：1355 SQLSTATE: HY000 (ER_WARN_VIEW_WITHOUT_KEY)

消息：正在更新的视图没有其基本表的完整键。

· 错误：1356 SQLSTATE: HY000 (ER_VIEW_INVALID)

消息：视图'%s.%s'引用了无效的表、列、或函数，或视图的定义程序／调用程序缺少使用它们的权限。

· 错误：1357 SQLSTATE: HY000 (ER_SP_NO_DROP_SP)

消息：无法从另一个存储子程序中撤销或更改%s。

· 错误：1358 SQLSTATE: HY000 (ER_SP_GOTO_IN_HNDLR)

消息：在存储子程序句柄中不允许GOTO。

· 错误：1359 SQLSTATE: HY000 (ER_TRG_ALREADY_EXISTS)

消息：触发程序已存在。

· 错误：1360 SQLSTATE: HY000 (ER_TRG_DOES_NOT_EXIST)

消息：触发程序不存在。

· 错误：1361 SQLSTATE: HY000 (ER_TRG_ON_VIEW_OR_TEMP_TABLE)

消息：触发程序的'%s'是视图或临时表。

· 错误：1362 SQLSTATE: HY000 (ER_TRG_CANT_CHANGE_ROW)

消息：在%strigger中，不允许更新%s行。

· 错误：1363 SQLSTATE: HY000 (ER_TRG_NO_SUCH_ROW_IN_TRG)

消息：在%s触发程序中没有%s行。

· 错误：1364 SQLSTATE: HY000 (ER_NO_DEFAULT_FOR_FIELD)

消息：字段'%s'没有默认值。

· 错误：1365 SQLSTATE: 22012 (ER_DIVISION_BY_ZERO)

消息：被0除。

· 错误：1366 SQLSTATE: HY000 (ER_TRUNCATED_WRONG_VALUE_FOR_FIELD)

消息：不正确的%s值，'%s'，对于行%ld 上的列'%s'。

· 错误：1367 SQLSTATE: 22007 (ER_ILLEGAL_VALUE_FOR_TYPE)

消息：解析过程中发现非法%s '%s'值。

· 错误：1368 SQLSTATE: HY000 (ER_VIEW_NONUPD_CHECK)

消息：不可更新视图'%s.%s'上的CHECK OPTION。

· 错误：1369 SQLSTATE: HY000 (ER_VIEW_CHECK_FAILED)

消息：CHECK OPTION失败，'%s.%s'

· 错误：1370 SQLSTATE: 42000 (ER_PROCACCESS_DENIED_ERROR)

消息：对于子程序'%s'，拒绝用户'%s'@'%s'使用%s命令。

· 错误：1371 SQLSTATE: HY000 (ER_RELAY_LOG_FAIL)

消息：清除旧中继日志失败，%s

· 错误：1372 SQLSTATE: HY000 (ER_PASSWD_LENGTH)

消息：密码混编应是%d位的十六进制数。

· 错误：1373 SQLSTATE: HY000 (ER_UNKNOWN_TARGET_BINLOG)

消息：在binlog索引中未发现目标日志。

· 错误：1374 SQLSTATE: HY000 (ER_IO_ERR_LOG_INDEX_READ)

消息：读取日志索引文件时出现I/O错误。

· 错误：1375 SQLSTATE: HY000 (ER_BINLOG_PURGE_PROHIBITED)

消息：服务器配置不允许binlog清除。

· 错误：1376 SQLSTATE: HY000 (ER_FSEEK_FAIL)

消息：fseek()失败。

· 错误：1377 SQLSTATE: HY000 (ER_BINLOG_PURGE_FATAL_ERR)

消息：在日志清除过程中出现致命错误。

· 错误：1378 SQLSTATE: HY000 (ER_LOG_IN_USE)

消息：可清除的日志正在使用，不能清除。

· 错误：1379 SQLSTATE: HY000 (ER_LOG_PURGE_UNKNOWN_ERR)

消息：在日志清除过程中出现未知错误。

· 错误：1380 SQLSTATE: HY000 (ER_RELAY_LOG_INIT)

消息：初始化中继日志位置失败，%s

· 错误：1381 SQLSTATE: HY000 (ER_NO_BINARY_LOGGING)

消息：未使用二进制日志功能。

· 错误：1382 SQLSTATE: HY000 (ER_RESERVED_SYNTAX)

消息：'%s'语法保留给MySQL服务器内部使用。

· 错误：1383 SQLSTATE: HY000 (ER_WSAS_FAILED)

消息：WSAStartup失败。

· 错误：1384 SQLSTATE: HY000 (ER_DIFF_GROUPS_PROC)

消息：尚不能用不同的组处理过程。

· 错误：1385 SQLSTATE: HY000 (ER_NO_GROUP_FOR_PROC)

消息：对于该过程，SELECT必须有1个组。

· 错误：1386 SQLSTATE: HY000 (ER_ORDER_WITH_PROC)

消息：不能与该过程一起使用ORDER子句。

· 错误：1387 SQLSTATE: HY000 (ER_LOGGING_PROHIBIT_CHANGING_OF)

消息：二进制日志功能和复制功能禁止更改全局服务器%s。

· 错误：1388 SQLSTATE: HY000 (ER_NO_FILE_MAPPING)

消息：无法映射文件: %s, errno: %d

· 错误：1389 SQLSTATE: HY000 (ER_WRONG_MAGIC)

消息：%s中有错

· 错误：1390 SQLSTATE: HY000 (ER_PS_MANY_PARAM)

消息：预处理语句包含过多的占位符。

· 错误：1391 SQLSTATE: HY000 (ER_KEY_PART_0)

消息：键部分'%s'的长度不能为0。

· 错误：1392 SQLSTATE: HY000 (ER_VIEW_CHECKSUM)

消息：视图文本校验和失败。

· 错误：1393 SQLSTATE: HY000 (ER_VIEW_MULTIUPDATE)

消息：无法通过联合视图'%s.%s'更改1个以上的基本表。

· 错误：1394 SQLSTATE: HY000 (ER_VIEW_NO_INSERT_FIELD_LIST)

消息：不能在没有字段列表的情况下插入联合视图'%s.%s'。

· 错误：1395 SQLSTATE: HY000 (ER_VIEW_DELETE_MERGE_VIEW)

消息：不能从联合视图'%s.%s'中删除。

· 错误：1396 SQLSTATE: HY000 (ER_CANNOT_USER)

消息：对于%s的操作%s失败。

· 错误：1397 SQLSTATE: XAE04 (ER_XAER_NOTA)

消息：XAER_NOTA: 未知XID

· 错误：1398 SQLSTATE: XAE05 (ER_XAER_INVAL)

消息：XAER_INVAL: 无效参量（或不支持的命令）

· 错误：1399 SQLSTATE: XAE07 (ER_XAER_RMFAIL)

消息：XAER_RMFAIL: 当全局事务处于%s状态时，不能执行命令。

· 错误：1400 SQLSTATE: XAE09 (ER_XAER_OUTSIDE)

消息：XAER_OUTSIDE: 某些工作是在全局事务外完成的。

· 错误：1401 SQLSTATE: XAE03 (ER_XAER_RMERR)

消息：XAER_RMERR: 在事务分支中出现致命错误，请检查数据一致性。

· 错误：1402 SQLSTATE: XA100 (ER_XA_RBROLLBACK)

消息：XA_RBROLLBACK: 回滚了事务分支。

· 错误：1403 SQLSTATE: 42000 (ER_NONEXISTING_PROC_GRANT)

消息：在子程序'%s'上没有为主机'%s'上的用户'%s'定义的这类授权。

· 错误：1404 SQLSTATE: HY000 (ER_PROC_AUTO_GRANT_FAIL)

消息：无法授予EXECUTE和ALTER ROUTINE权限。

· 错误：1405 SQLSTATE: HY000 (ER_PROC_AUTO_REVOKE_FAIL)

消息：无法撤销已放弃子程序上的所有权限。

· 错误：1406 SQLSTATE: 22001 (ER_DATA_TOO_LONG)

消息：对于行%ld上的列'%s'来说，数据过长。

· 错误：1407 SQLSTATE: 42000 (ER_SP_BAD_SQLSTATE)

消息：不良SQLSTATE: '%s'

· 错误：1408 SQLSTATE: HY000 (ER_STARTUP)

消息：%s，连接就绪；版本，'%s'；套接字，'%s'；端口，%d %s

· 错误：1409 SQLSTATE: HY000 (ER_LOAD_FROM_FIXED_SIZE_ROWS_TO_VAR)

消息：不能从具有固定大小行的文件中将值加载到变量。

· 错误：1410 SQLSTATE: 42000 (ER_CANT_CREATE_USER_WITH_GRANT)

消息：不允许用GRANT创建用户。

· 错误：1411 SQLSTATE: HY000 (ER_WRONG_VALUE_FOR_TYPE)

消息：不正确的%s值，'%s'，对于函数%s

· 错误：1412 SQLSTATE: HY000 (ER_TABLE_DEF_CHANGED)

消息：表定义已更改，请再次尝试事务。

· 错误：1413 SQLSTATE: 42000 (ER_SP_DUP_HANDLER)

消息：在相同块中声明了重复句柄。

· 错误：1414 SQLSTATE: 42000 (ER_SP_NOT_VAR_ARG)

消息：子程序%s的OUT或INOUT参量不是变量。

· 错误：1415 SQLSTATE: 0A000 (ER_SP_NO_RETSET)

消息：不允许从%s返回结果集。

· 错误：1416 SQLSTATE: 22003 (ER_CANT_CREATE_GEOMETRY_OBJECT)

消息：不能从发送给GEOMETRY字段的数据中获取几何对象。

· 错误：1417 SQLSTATE: HY000 (ER_FAILED_ROUTINE_BREAK_BINLOG)

消息：1个子程序失败，在其声明没有NO SQL或READS SQL DATA，而且二进制日志功能已启用，如果更新了非事务性表，二进制日志将丢失其变化信息。

· 错误：1418 SQLSTATE: HY000 (ER_BINLOG_UNSAFE_ROUTINE)

消息：在该子程序的在其声明没有DETERMINISTIC、NO SQL或READS SQL DATA，而且二进制日志功能已启用（你或许打算使用不太安全的log_bin_trust_routine_creators变量）。

· 错误：1419 SQLSTATE: HY000 (ER_BINLOG_CREATE_ROUTINE_NEED_SUPER)

消息：你没有SUPER权限，而且二进制日志功能已启用（你或许打算使用不太安全的log_bin_trust_routine_creators变量）。

· 错误：1420 SQLSTATE: HY000 (ER_EXEC_STMT_WITH_OPEN_CURSOR)

消息：不能执行该预处理语句，该预处理语句有与之相关的打开光标。请复位语句并再次执行。

· 错误：1421 SQLSTATE: HY000 (ER_STMT_HAS_NO_OPEN_CURSOR)

消息：语句(%lu)没有打开的光标。

· 错误：1422 SQLSTATE: HY000 (ER_COMMIT_NOT_ALLOWED_IN_SF_OR_TRG)

消息：在存储函数或触发程序中，不允许显式或隐式提交。

· 错误：1423 SQLSTATE: HY000 (ER_NO_DEFAULT_FOR_VIEW_FIELD)

消息：视图'%s.%s'基本表的字段没有默认值。

· 错误：1424 SQLSTATE: HY000 (ER_SP_NO_RECURSION)

消息：不允许递归存储子程序。

· 错误：1425 SQLSTATE: 42000 (ER_TOO_BIG_SCALE)

消息：为列'%s'指定了过大的标度%d。最大为%d。

· 错误：1426 SQLSTATE: 42000 (ER_TOO_BIG_PRECISION)

消息：为列'%s'指定了过高的精度%d。最大为%d。

· 错误：1427 SQLSTATE: 42000 (ER_M_BIGGER_THAN_D)

消息：对于float(M,D)、double(M,D)或decimal(M,D)，M必须>= D (列'%s')。

· 错误：1428 SQLSTATE: HY000 (ER_WRONG_LOCK_OF_SYSTEM_TABLE)

消息：不能将系统'%s.%s'表的写锁定与其他表结合起来。

· 错误：1429 SQLSTATE: HY000 (ER_CONNECT_TO_FOREIGN_DATA_SOURCE)

消息：无法连接到外部数据源，数据库'%s'！

· 错误：1430 SQLSTATE: HY000 (ER_QUERY_ON_FOREIGN_DATA_SOURCE)

消息：处理作用在外部数据源上的查询时出现问题。数据源错误：'%s'

· 错误：1431 SQLSTATE: HY000 (ER_FOREIGN_DATA_SOURCE_DOESNT_EXIST)

消息：你试图引用的外部数据源不存在。数据源错误：'%s'

· 错误：1432 SQLSTATE: HY000 (ER_FOREIGN_DATA_STRING_INVALID_CANT_CREATE)

消息：无法创建联合表。数据源连接字符串'%s'格式不正确。

· 错误：1433 SQLSTATE: HY000 (ER_FOREIGN_DATA_STRING_INVALID)

消息：数据源连接字符串'%s'格式不正确。

· 错误：1434 SQLSTATE: HY000 (ER_CANT_CREATE_FEDERATED_TABLE)

消息：无法创建联合表。外部数据源错误：'%s'

· 错误：1435 SQLSTATE: HY000 (ER_TRG_IN_WRONG_SCHEMA)

消息：触发程序位于错误的方案中。

· 错误：1436 SQLSTATE: HY000 (ER_STACK_OVERRUN_NEED_MORE)

消息：线程堆栈溢出，%ld字节堆栈用了%ld字节，并需要%ld字节。请使用'mysqld -O thread_stack=#'指定更大的堆栈。

· 错误：1437 SQLSTATE: 42000 (ER_TOO_LONG_BODY)

消息：'%s'的子程序主体过长。

· 错误：1438 SQLSTATE: HY000 (ER_WARN_CANT_DROP_DEFAULT_KEYCACHE)

消息：无法撤销默认的keycache。

· 错误：1439 SQLSTATE: 42000 (ER_TOO_BIG_DISPLAYWIDTH)

消息：对于列'%s'，显示宽度超出范围(max = %d)

· 错误：1440 SQLSTATE: XAE08 (ER_XAER_DUPID)

消息：XAER_DUPID: XID已存在

· 错误：1441 SQLSTATE: 22008 (ER_DATETIME_FUNCTION_OVERFLOW)

消息：日期时间函数，%s字段溢出。

· 错误：1442 SQLSTATE: HY000 (ER_CANT_UPDATE_USED_TABLE_IN_SF_OR_TRG)

消息：由于它已被调用了该存储函数／触发程序的语句使用，不能在存储函数／触发程序中更新表'%s'。

· 错误：1443 SQLSTATE: HY000 (ER_VIEW_PREVENT_UPDATE)

消息：表'%s'的定义不允许在表'%s上执行操作%s。

· 错误：1444 SQLSTATE: HY000 (ER_PS_NO_RECURSION)

消息：预处理语句包含引用了相同语句的存储子程序调用。不允许以这类递归方式执行预处理语句。

· 错误：1445 SQLSTATE: HY000 (ER_SP_CANT_SET_AUTOCOMMIT)

消息：不允许从存储函数或触发程序设置autocommit。

· 错误：1446 SQLSTATE: HY000 (ER_NO_VIEW_USER)

消息：视图定义人不完全合格。

· 错误：1447 SQLSTATE: HY000 (ER_VIEW_FRM_NO_USER)

消息：视图%s.%s没有定义人信息（旧的表格式）。当前用户将被当作定义人。请重新创建视图！

· 错误：1448 SQLSTATE: HY000 (ER_VIEW_OTHER_USER)

消息：需要SUPER权限才能创建具有%s@%s定义器的视图。

· 错误：1449 SQLSTATE: HY000 (ER_NO_SUCH_USER)

消息：没有注册的%s@%s。

· 错误：1450 SQLSTATE: HY000 (ER_FORBID_SCHEMA_CHANGE)

消息：不允许将方案从'%s'变为'%s'。

· 错误：1451 SQLSTATE: 23000 (ER_ROW_IS_REFERENCED_2)

消息：不能删除或更新父行，外键约束失败(%s)。

· 错误：1452 SQLSTATE: 23000 (ER_NO_REFERENCED_ROW_2)

消息：不能添加或更新子行，外键约束失败(%s)。

· 错误：1453 SQLSTATE: 42000 (ER_SP_BAD_VAR_SHADOW)

消息：必须用`...`引用变量，或重新命名变量。

· 错误：1454 SQLSTATE: HY000 (ER_PARTITION_REQUIRES_VALUES_ERROR)

消息：对于每个分区，%s PARTITIONING需要VALUES %s的定义。

· 错误：1455 SQLSTATE: HY000 (ER_PARTITION_WRONG_VALUES_ERROR)

消息：在分区定义中，只有%s PARTITIONING能使用VALUES %s。

· 错误：1456 SQLSTATE: HY000 (ER_PARTITION_MAXVALUE_ERROR)

消息：MAXVALUE只能在最后1个分区定义中使用。

· 错误：1457 SQLSTATE: HY000 (ER_PARTITION_SUBPARTITION_ERROR)

消息：子分区只能是哈希分区，并按键分区。

· 错误：1458 SQLSTATE: HY000 (ER_PARTITION_WRONG_NO_PART_ERROR)

消息：定义了错误的分区数，与前面的设置不匹配。

· 错误：1459 SQLSTATE: HY000 (ER_PARTITION_WRONG_NO_SUBPART_ERROR)

消息：定义了错误的子分区数，与前面的设置不匹配。

· 错误：1460 SQLSTATE: HY000 (ER_CONST_EXPR_IN_PARTITION_FUNC_ERROR)

消息：在分区（子分区）函数中不允许使用常量／随机表达式。

· 错误：1461 SQLSTATE: HY000 (ER_NO_CONST_EXPR_IN_RANGE_OR_LIST_ERROR)

消息：RANGE/LIST VALUES中的表达式必须是常量。

· 错误：1462 SQLSTATE: HY000 (ER_FIELD_NOT_FOUND_PART_ERROR)

消息：在表中未发现分区函数字段列表中的字段。

· 错误：1463 SQLSTATE: HY000 (ER_LIST_OF_FIELDS_ONLY_IN_HASH_ERROR)

消息：仅在KEY分区中允许使用字段列表。

· 错误：1464 SQLSTATE: HY000 (ER_INCONSISTENT_PARTITION_INFO_ERROR)

消息：frm文件中的分区信息与能够写入到frm文件中的不一致。

· 错误：1465 SQLSTATE: HY000 (ER_PARTITION_FUNC_NOT_ALLOWED_ERROR)

消息：%s函数返回了错误类型。

· 错误：1466 SQLSTATE: HY000 (ER_PARTITIONS_MUST_BE_DEFINED_ERROR)

消息：对于%s分区，必须定义每个分区。

· 错误：1467 SQLSTATE: HY000 (ER_RANGE_NOT_INCREASING_ERROR)

消息：对于各分区，VALUES LESS THAN值必须严格增大。

· 错误：1468 SQLSTATE: HY000 (ER_INCONSISTENT_TYPE_OF_FUNCTIONS_ERROR)

消息：VALUES值必须与分区函数具有相同的类型。

· 错误：1469 SQLSTATE: HY000 (ER_MULTIPLE_DEF_CONST_IN_LIST_PART_ERROR)

消息：Multiple definition of same constant in list partitioning

· 错误：1470 SQLSTATE: HY000 (ER_PARTITION_ENTRY_ERROR)

消息：在查询中，不能独立使用分区功能。

· 错误：1471 SQLSTATE: HY000 (ER_MIX_HANDLER_ERROR)

消息：在该MySQL版本中，不允许分区中的句柄组合。

· 错误：1472 SQLSTATE: HY000 (ER_PARTITION_NOT_DEFINED_ERROR)

消息：对于分区引擎，有必要定义所有的%s。

· 错误：1473 SQLSTATE: HY000 (ER_TOO_MANY_PARTITIONS_ERROR)

消息：定义了过多分区。

· 错误：1474 SQLSTATE: HY000 (ER_SUBPARTITION_ERROR)

消息：对于子分区，仅能将RANGE/LIST分区与HASH/KEY分区混合起来。

· 错误：1475 SQLSTATE: HY000 (ER_CANT_CREATE_HANDLER_FILE)

消息：无法创建特定的句柄文件。

· 错误：1476 SQLSTATE: HY000 (ER_BLOB_FIELD_IN_PART_FUNC_ERROR)

消息：在分区函数中，不允许使用BLOB字段。

· 错误：1477 SQLSTATE: HY000 (ER_CHAR_SET_IN_PART_FIELD_ERROR)

消息：如果为分区函数选择了二进制校对，才允许使用VARCHAR。

· 错误：1478 SQLSTATE: HY000 (ER_UNIQUE_KEY_NEED_ALL_FIELDS_IN_PF)

消息：在分区函数中，%s需要包含所有文件。

· 错误：1479 SQLSTATE: HY000 (ER_NO_PARTS_ERROR)

消息：%s的数目= 0不是允许的值。

· 错误：1480 SQLSTATE: HY000 (ER_PARTITION_MGMT_ON_NONPARTITIONED)

消息：无法在非分区表上进行分区管理。

· 错误：1481 SQLSTATE: HY000 (ER_DROP_PARTITION_NON_EXISTENT)

消息：分区列表中的错误出现变化。

· 错误：1482 SQLSTATE: HY000 (ER_DROP_LAST_PARTITION)

消息：不能删除所有分区，请使用DROP TABLE取而代之。

· 错误：1483 SQLSTATE: HY000 (ER_COALESCE_ONLY_ON_HASH_PARTITION)

消息：COALESCE PARTITION仅能在HASH/KEY分区上使用。

· 错误：1484 SQLSTATE: HY000 (ER_ONLY_ON_RANGE_LIST_PARTITION)

消息：%s PARTITION仅能在RANGE/LIST分区上使用。

· 错误：1485 SQLSTATE: HY000 (ER_ADD_PARTITION_SUBPART_ERROR)

消息：试图用错误的子分区数增加分区。

· 错误：1486 SQLSTATE: HY000 (ER_ADD_PARTITION_NO_NEW_PARTITION)

消息：必须至少添加1个分区。

· 错误：1487 SQLSTATE: HY000 (ER_COALESCE_PARTITION_NO_PARTITION)

消息：必须至少合并1个分区。

· 错误：1488 SQLSTATE: HY000 (ER_REORG_PARTITION_NOT_EXIST)

消息：重组的分区数超过了已有的分区数。

· 错误：1489 SQLSTATE: HY000 (ER_SAME_NAME_PARTITION)

消息：在表中，所有分区必须有唯一的名称。

· 错误：1490 SQLSTATE: HY000 (ER_CONSECUTIVE_REORG_PARTITIONS)

消息：重组分区集合时，它们必须连续。

· 错误：1491 SQLSTATE: HY000 (ER_REORG_OUTSIDE_RANGE)

消息：新分区的范围超过了已重组分区的范围。

· 错误：1492 SQLSTATE: HY000 (ER_DROP_PARTITION_FAILURE)

消息：在该版本的句柄中，不支持撤销分区。

· 错误：1493 SQLSTATE: HY000 (ER_DROP_PARTITION_WHEN_FK_DEFINED)

消息：在表上定义了外键约束时，不能舍弃分区。

· 错误：1494 SQLSTATE: HY000 (ER_PLUGIN_IS_NOT_LOADED)

消息：未加载插件'%s'

B.2. 客户端错误代码和消息
客户端错误信息来自下述源文件：

· 圆括号中的错误值和符号与include/errmsg.h MySQL源文件中的定义对应。

· 消息值与libmysql/errmsg.c文件中列出的错误消息对应。%d和%s分别代表数值和字符串，显示时，它们将被消息值取代。

由于更新很频繁，这些文件中可能包含这里未列出的额外错误消息。

· 错误：2000 (CR_UNKNOWN_ERROR)

消息：未知MySQL错误。

· 错误：2001 (CR_SOCKET_CREATE_ERROR)

消息：不能创建UNIX套接字(%d)

· 错误：2002 (CR_CONNECTION_ERROR)

消息：不能通过套接字'%s' (%d)连接到本地MySQL服务器。

· 错误：2003 (CR_CONN_HOST_ERROR)

消息：不能连接到'%s' (%d)上的MySQL服务器。

· 错误：2004 (CR_IPSOCK_ERROR)

消息：不能创建TCP/IP套接字(%d)

· 错误：2005 (CR_UNKNOWN_HOST)

消息：未知的MySQL服务器主机'%s' (%d)

· 错误：2006 (CR_SERVER_GONE_ERROR)

消息：MySQL服务器不可用。

· 错误：2007 (CR_VERSION_ERROR)

消息：协议不匹配，服务器版本= %d，客户端版本= %d

· 错误：2008 (CR_OUT_OF_MEMORY)

消息：MySQL客户端内存溢出。

· 错误：2009 (CR_WRONG_HOST_INFO)

消息：错误的主机信息

· 错误：2010 (CR_LOCALHOST_CONNECTION)

消息：通过UNIX套接字连接的本地主机。

· 错误：2011 (CR_TCP_CONNECTION)

消息：%s，通过TCP/IP

· 错误：2012 (CR_SERVER_HANDSHAKE_ERR)

消息：服务器握手过程中出错。

· 错误：2013 (CR_SERVER_LOST)

消息：查询过程中丢失了与MySQL服务器的连接。

· 错误：2014 (CR_COMMANDS_OUT_OF_SYNC)

消息：命令不同步，你现在不能运行该命令。

· 错误：2015 (CR_NAMEDPIPE_CONNECTION)

消息：命名管道，%s

· 错误：2016 (CR_NAMEDPIPEWAIT_ERROR)

消息：无法等待命名管道，主机，%s；管道，%s (%lu)

· 错误：2017 (CR_NAMEDPIPEOPEN_ERROR)

消息：无法打开命名管道，主机，%s；管道，%s (%lu)

· 错误：2018 (CR_NAMEDPIPESETSTATE_ERROR)

消息：无法设置命名管道的状态，主机，%s；管道，%s (%lu)

· 错误：2019 (CR_CANT_READ_CHARSET)

消息：无法初始化字符集%s (路径：%s)

· 错误：2020 (CR_NET_PACKET_TOO_LARGE)

消息：获得的信息包大于'max_allowed_packet'字节。

· 错误：2021 (CR_EMBEDDED_CONNECTION)

消息：嵌入式服务器。

· 错误：2022 (CR_PROBE_SLAVE_STATUS)

消息：SHOW SLAVE STATUS出错：

· 错误：2023 (CR_PROBE_SLAVE_HOSTS)

消息：SHOW SLAVE HOSTS出错：

· 错误：2024 (CR_PROBE_SLAVE_CONNECT)

消息：连接到从服务器时出错：

· 错误：2025 (CR_PROBE_MASTER_CONNECT)

消息：连接到主服务器时出错：

· 错误：2026 (CR_SSL_CONNECTION_ERROR)

消息：SSL连接错误

· 错误：2027 (CR_MALFORMED_PACKET)

消息：残缺信息包。

· 错误：2028 (CR_WRONG_LICENSE)

消息：该客户端库仅授权给具有'%s'许可的MySQL服务器使用。

· 错误：2029 (CR_NULL_POINTER)

消息：空指针的无效使用。

· 错误：2030 (CR_NO_PREPARE_STMT)

消息：语句未准备好。

· 错误：2031 (CR_PARAMS_NOT_BOUND)

消息：没有为预处理语句中的参数提供数据。

· 错误：2032 (CR_DATA_TRUNCATED)

消息：数据截短。

· 错误：2033 (CR_NO_PARAMETERS_EXISTS)

消息：语句中不存在任何参数。

· 错误：2034 (CR_INVALID_PARAMETER_NO)

消息：无效的参数编号。

· 错误：2035 (CR_INVALID_BUFFER_USE)

消息：不能为非字符串／非二进制数据类型发送长数据（参数：%d）。

· 错误：2036 (CR_UNSUPPORTED_PARAM_TYPE)

消息：正使用不支持的缓冲区类型， %d （参数：%d）

· 错误：2037 (CR_SHARED_MEMORY_CONNECTION)

消息：共享内存，%s

· 错误：2038 (CR_SHARED_MEMORY_CONNECT_REQUEST_ERROR)

消息：不能打开共享内存，客户端不能创建请求事件(%lu)

· 错误：2039 (CR_SHARED_MEMORY_CONNECT_ANSWER_ERROR)

消息：不能打开共享内存，未收到服务器的应答事件(%lu)

· 错误：2040 (CR_SHARED_MEMORY_CONNECT_FILE_MAP_ERROR)

消息：不能打开共享内存，服务器不能分配文件映射(%lu)

· 错误：2041 (CR_SHARED_MEMORY_CONNECT_MAP_ERROR)

消息：不能打开共享内存，服务器不能获得文件映射的指针(%lu)

· 错误：2042 (CR_SHARED_MEMORY_FILE_MAP_ERROR)

消息：不能打开共享内存，客户端不能分配文件映射(%lu)

· 错误：2043 (CR_SHARED_MEMORY_MAP_ERROR)

消息：不能打开共享内存，客户端不能获得文件映射的指针(%lu)

· 错误：2044 (CR_SHARED_MEMORY_EVENT_ERROR)

消息：不能打开共享内存，客户端不能创建%s事件(%lu)

· 错误：2045 (CR_SHARED_MEMORY_CONNECT_ABANDONED_ERROR)

消息：不能打开共享内存，无来自服务器的应答 (%lu)

· 错误：2046 (CR_SHARED_MEMORY_CONNECT_SET_ERROR)

消息：不能打开共享内存，不能将请求事件发送到服务器(%lu)

· 错误：2047 (CR_CONN_UNKNOW_PROTOCOL)

消息：错误或未知协议

· 错误：2048 (CR_INVALID_CONN_HANDLE)

消息：无效的连接句柄

· 错误：2049 (CR_SECURE_AUTH)

消息：拒绝使用旧鉴定协议（早于4.1.1）的连接（开启了客户端'secure_auth'选项）。

· 错误：2050 (CR_FETCH_CANCELED)

消息：行检索被mysql_stmt_close()调用取消。

· 错误：2051 (CR_NO_DATA)

消息：在未事先获取行的情况下试图读取列。

· 错误：2052 (CR_NO_STMT_METADATA)

消息：预处理语句不含元数据。

· 错误：2053 (CR_NO_RESULT_SET)

消息：在没有与语句相关的结果集时试图读取行。

· 错误：2054 (CR_NOT_IMPLEMENTED)

消息：该特性尚未实施。

分类: dateBase