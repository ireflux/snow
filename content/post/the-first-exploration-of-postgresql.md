---
title: "初探PostgreSQL"
date: 2019-05-29
lastmod: 2019-05-29
draft: false
categories: ["随笔"]
author: "sherry"
---
在群友的安利（~~传教~~）下，成功引起了我的兴趣...

在作了一些了解之后，记录如下。由于本文的基础是在Arch Linux上进行的，其他发行版暂不作讨论，不过应该也普遍适用。

安装没什么可说的，直接 `sudo pacman -S postgresql` 就好了。(其他发行版请使用对应的包管理器)

安装 PostgreSQL 的时候会同时创建一个名为 postgres 的系统账户。这个账户同时也是 PostgreSQL 数据库中的 Superuser, 因此创建数据库账户或者创建数据库都需要由这个账户来进行操作。

如果没有指定配置文件直接在 terminal 中键入 postgres 后显示可能会显示如下结果：

<!--more-->

```
postgres does not know where to find the server configuration file.  
You must specify the --config-file or -D invocation option or set the PGDATA environment variable.
```

因此需要先切换到 postgres 账户：

```bash
$ sudo -iu postgres
```

然后初始化配置：

```bash
$ initdb -D /var/lib/postgres/data
```

值得一提的是，默认情况下，数据库的编码和语言环境会使用本机的系统编码和语言环境，因此如果系统不是英文环境或者编码不是 utf-8 的还是建议通过指定编码和语言环境的方式来覆盖掉默认值。就像这洋：

```bash
$ initdb --locale en_US.UTF-8 -E UTF8 -D '/var/lib/postgres/data'
```

上条与上上条命令中的 -D 会指定默认(default)的存储位置。若无特殊需求，一般使用默认位置就行，如果想要修改存储位置，可以看[这里](https://wiki.archlinux.org/index.php/PostgreSQL#Change_default_data_directory)，此处不再赘述。

如果一切顺利，应该会出现以下结果：

```
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgres/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgres/data -l logfile start
```

然后启动 postgresql server 服务：

```bash
$ sudo systemctl start postgresql.service
```

到此处事实上已经完成了，但是这一切都是在 postgres 这个账户下进行操作的，为了便于自己本机日常使用的账户进行操作，可以在 postgresql 数据库中创建一个和自己本机账户同名的数据库账户，仍然需要 postgres 这个 Superuser 来继续操作：

```bash
$ createuser --interactive
```

上条命令执行后，在这里为了方便，我将其设置成了 superuser，交互如下：

```
Enter name of role to add: [此处填写你的本机账户名]
Shall the new role be a superuser? (y/n) y
```

创建完用户就可以输入 `exit` 来换回本来的账户了。接下来创建数据库：

```bash
$ createdb [此处填写数据库名称]
```

如果创建的数据库用户名称与本机不同名，此时通过 `createdb` 命令来创建数据库就会出现如下错误：

```
createdb: could not connect to database template1: FATAL:  role "[你的本机账户名]" does not exist
```

如果一切顺利，就可以通过 `psql -d [数据库名称]`来访问你的数据库了。

以下是进入数据库后的一些查询命令：

> \help -> 查看所有数据库的命令。  
\help [命令] -> 查看此命令的详细信息及用法。类似于 man page。  
\du -> 列出所有的账户及其权限。  
\dt -> 显示当前数据库所有表的摘要信息。  
\q -> 退出 

## 参考资料

[PostgreSQL Doc](https://www.postgresql.org/docs/11/)  
[Arch Wiki PostgreSQL](https://wiki.archlinux.org/index.php/PostgreSQL)