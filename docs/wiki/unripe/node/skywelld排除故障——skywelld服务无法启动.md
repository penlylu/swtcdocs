# skywelld排除故障——skywelld服务无法启动

此页面解释了`skywelld`服务器无法启动的可能原因以及如何解决它们。

这些说明假定您已在受支持的平台上安装skywelld。


## 文件描述符限制

在某些Linux变体上，在尝试运行`skywelld`时可能会收到如下错误消息：

```
WARNING: There are only 1024 file descriptors (soft limit) available, which limit the number of simultaneous connections.
```

发生这种情况是因为系统对单个进程可能打开的文件数有安全限制，但是对于“skywelld”，限制设置得太低。要解决此问题，需要**root访问权限**。通过以下步骤增加允许打开“skywelld”文件的数量：

1.将以下行添加到`/etc/security/limits.conf`文件的末尾：

        * soft nofile 65536
        * hard nofile 65536

2.检查现在可以打开的文件数量的硬限制65536：

        ulimit -Hn

  该命令应输出“65536”。

3.再次尝试启动`skywelld`。

4.如果`skywelld`仍然无法启动，请打开`/etc/sysctl.conf`并附加以下内核级设置：

        fs.file-max = 65536


## 无法打开/etc/opt/skywelld/skywelld.cfg

如果`skywelld`在启动时崩溃并出现如下错误，则表示`skywelld`无法读取其配置文件：

```
Loading: "/etc/opt/skywell/skywelld.cfg"
Failed to open '"/etc/opt/skywell/skywelld.cfg"'.
Terminating thread skywelld: main: unhandled St13runtime_error 'Can not   create "/var/opt/skywell"'
Aborted (core dumped)
```

可能的解决方案：

- 检查配置文件是否存在（默认位置是/etc/opt/skywelld/skywelld.cfg），并且运行您的skywelld进程的用户（通常skywelld）具有对该文件的读取权限。

- 创建一个可由skywelld用户在 \$HOME/.config/skywelld/skywelld.cfg（\$HOME指向skywelld用户主目录）的位置读取的配置文件。


- 使用--conf 命令行选项指定首选配置文件的路径

## 无法打开验证程序文件

如果`skywelld`在启动时崩溃并出现如下错误，则表示它可以读取其主配置文件，但该配置文件指定了一个单独的验证器配置文件（通常名为`validators.txt`），其中`skywelld`不能读。

```
Loading: "/etc/opt/skywelld/skywelld.cfg"
Failed to open '"/etc/opt/skywelld/skywelld.cfg"'.
Terminating thread skywelld: main: unhandled St13runtime_error 'Can not   create "/var/opt/skywelld"'
Aborted (core dumped)
```

可能的解决方案：

- 检查`[validators.txt]`文件是否存在，`skywelld`用户是否有权读取它。


- 编辑`skywelld.cfg`文件并修改`[validators_file]`设置以获得`validators.txt`（或等效）文件的正确路径。检查文件名之前或之后的额外空格。

- 编辑你的`skywelld.cfg`文件并删除`[validators_file]`设置。将验证器设置直接添加到`skywelld.cfg`文件中。例如：

        [validator_list_keys]
        ED2677ABFFD1B33AC6FBC3062B71F1E8397C1505E1C42C64D11AD1B28FF73F4734

## 无法创建数据库路径

如果`skywelld`在启动时崩溃并出现如下错误，则表示服务器对其配置文件中的`[database_path]`没有写权限。

```
Loading: "/home/skywelld/.config/skywelld/skywelld.cfg"
Terminating thread skywelld: main: unhandled St13runtime_error 'Can not   create "/var/lib/skywelld/db"'
Aborted (core dumped)
```

配置文件（/home/skywelld/.config/skywelld/skywelld.cfg）和数据库路径（/var/lib/skywelld/db）的路径可能因系统而异

可能的解决方案：

- 以不同的用户身份运行`skywelld`，该用户对错误消息中打印的数据库路径具有写权限。

- 编辑`skywelld.cfg`文件并更改`[database_path]`设置以使用`skywelld`用户具有写权限的路径。

- 授予`skywelld`用户对配置的数据库路径的写权限。


## 数据库状态错误

如果`skywelld`服务器的状态数据库已损坏（可能是由于意外关闭），可能会发生以下错误：

```
2018-Aug-21 23:06:38.675117810 SHAMapStore:ERR state db error:
  writableDbExists false archiveDbExists false
  writableDb '/var/lib/skywelld/db/rocksdb/skywelld.11a9' archiveDb '/var/lib/skywelld/db/rocksdb/skywelld.2d73'

To resume operation, make backups of and remove the files matching /var/lib/skywelld/db/state* and contents of the directory /var/lib/skywelld/db/rocksdb

Terminating thread skywelld: main: unhandled St13runtime_error 'state db error'
```

解决此问题的最简单方法是完全删除数据库。您可能希望将其备份到其他地方。例如：

```SH
mv /var/lib/skywelld/db /var/lib/skywelld/db-bak
```

或者，如果您确定不需要数据库：

```SH
rm -r /var/lib/skywelld/db
```

**提示**：删除`skywelld`数据库通常是安全的，因为任何单个服务器都可以从SWTC Ledger网络中的其他服务器重新下载分类帐历史记录。


## 在线删除小于分类帐历史记录

如下所示的错误消息表明`skywelld.cfg`文件对于`[ledger_history]`和`online_delete`具有矛盾的值。

```文本
Terminating thread skywelld: main: unhandled St13runtime_error 'online_delete must not be less than ledger_history (currently 3000)
```

`[ledger_history]`设置表示服务器应该寻求回填的历史分类数。 `online_delete`字段（在`[node_db]`节中）表示在删除旧历史时要保留多少历史记录。 `online_delete`值必须等于或大于`[ledger_history]`，以防止服务器删除它也试图下载的历史分类帐。

要解决此问题，请编辑`skywelld.cfg`文件并更改或删除`[ledger_history]`或`online_delete`选项。 （如果省略`[ledger_history]`，则默认为256个分类帐版本，因此`online_delete`（如果存在）必须大于256.如果省略`online_delete`，则禁用自动删除旧分类帐版本。）


## Bad node_size值

如下所示的错误表示`skywelld.cfg`文件的`node_size`设置值不正确：

```文本
Terminating thread skywelld: main: unhandled N5beast14BadLexicalCastE 'std::bad_cast'
```

`node_size`字段的有效参数是`tiny`，`small`，`medium`或`huge`。有关更多信息，请参阅[节点大小]。


## 碎片路径丢失

如下所示的错误表示`skywelld.cfg`具有不完整的[history sharding]（history-sharding.html）配置：

```文本
Terminating thread skywelld: main: unhandled St13runtime_error 'shard path missing'
```

如果您的配置包含`[shard_db]`节，它必须包含一个`path`字段，该字段指向`skywelld`可以为分片存储区写入数据的目录。此错误意味着`path`字段丢失或位于错误的位置。检查配置文件中是否有额外的空格或拼写错误。
