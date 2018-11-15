# skywelld排除故障——了解日志消息

以下部分描述了一些最常见的日志消息类型，这些消息可以出现在`skywelld`服务器的调试日志中以及如何解释它们。

这是使用`skywelld`在[诊断问题]中的重要一步。

## 崩溃了

日志中提及运行时错误的消息可以指示服务器崩溃。这些消息通常以诸如以下示例之一的消息开头：

```
Throw<std::runtime_error>
```

```
Terminating thread skywelld: main: unhandled St13runtime_error
```

如果您的服务器在启动时总是崩溃，请参阅[服务器无法启动]以了解可能的情况。

如果您的服务器在操作期间或由于特定命令而随机崩溃，请确保您[更新]到最新的`skywelld`版本。如果您使用的是最新版本且服务器仍在崩溃，请检查以下内容：

- 您的服务器内存不足吗？在某些系统上，“skywelld”可能会被内存不足（OOM）结束掉或其他监控进程终止。
- 如果您的服务器在共享环境中运行，其他用户或管理员是否会重新启动计算机或服务？例如，某些托管提供程序会在很长一段时间内自动终止使用大量共享计算机资源的任何服务。
- 您的服务器是否满足[最低要求]来运行`skywelld`？ 生产服务器的配置怎么样？

如果以上都不适用，请将问题报告给skywelld作为安全敏感错误。


## 由对等方重置连接

以下日志消息表明对等“skywelld”服务器关闭了连接：

```
2018-Aug-28 22:55:41.738765510 Peer:WRN [012] onReadMessage: Connection reset by peer
```

对于任何对等网络，不时丢失连接是正常的。 **此类偶尔的消息并不表示存在问题。**

大约同一时间的大量这些消息可能表示存在问题，例如：

- 您与一个或多个特定同行的互联网连接被切断。
- 您的服务器可能已经使对等设备超载请求，导致它丢弃您的服务器。


## 没有提取包的哈希值

下载的消息是由于“历史分片”下载历史分类帐时“skywelld”更早版本中的错误引起的：

```文本
2018-Aug-28 22:56:21.397076850 LedgerMaster:ERR No hash for fetch pack. Missing Index 743892

```

这些可以安全地忽略。

## LoadMonitor作业

当函数需要很长时间才能运行时（例如，在此示例中超过11秒），会发生以下消息：

```
2018-Aug-28 22:56:36.180827973 LoadMonitor:WRN Job: gotFetchPack run: 11566ms wait: 0ms
```

当作业花费很长时间等待运行时，会发生以下类似消息（在此示例中，再次超过11秒）：

```
2018-Aug-28 22:56:36.180970431 LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181053831 LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181110594 LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181169931 LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
```

当长时间运行的作业导致其他作业等待很长时间才能完成时，这两种类型的消息通常会一起出现。

在启动服务器后的前几分钟中显示这些类型的几条消息**是正常的**。

如果消息在启动服务器后持续超过5分钟，特别是如果`run`时间超过1000毫秒，则可能表明**您的服务器没有足够的资源，例如磁盘I / O，RAM或中央处理器**。这可能是由于没有足够强大的硬件或者因为在同一硬件上运行的其他进程和资源与“skywelld”竞争而引起的。 （可能与“skywelld”竞争资源的其他进程的示例包括计划备份，病毒扫描程序和定期数据库清除程序。）

另一个可能的原因是尝试在旋转硬盘上使用NuDB; NuDB应仅用于固态驱动器（SSD）。 建议始终为“skywelld”的数据库使用SSD存储，但是能够使用RocksDB在机械磁盘上成功运行`skywelld`。如果您使用的是机械磁盘，请确保将`[node_db]`和`[shard_db]`（如果有的话）配置为使用RocksDB。例如：

```
[node_db]
type=RocksDB
# ... more config omitted

[shard_db]
type=RocksDB
```

## 在运行期间改变了共识的观点

当服务器与网络的其余部分不同步时。

在服务器启动后的前5到15分钟内，它与网络的其余部分不同步并打印诸如此类的消息是正常的。如果服务器在启动后长时间写入这些消息，则可能表示存在问题。常见原因包括不可靠的网络连接和不足的硬件规格。当在同一硬件上运行的其他进程与`skywelld`竞争资源时，也会发生这种情况。 （可能与“skywelld”竞争资源的其他进程的示例包括计划备份，病毒扫描程序和定期数据库清除程序。）


## 已经验证的序列在或过去

诸如以下的日志消息指示服务器不按顺序接收不同分类帐序列的验证。

```
2018-Aug-28 22:55:58.316094260 Validations:WRN Val for 2137ACEFC0D137EFA1D84C2524A39032802E4B74F93C130A289CD87C9C565011 trusted/full from nHUeUNSn3zce2xQZWNghQvd9WRH6FWEnCBKYVJu2vAizMxnXegfJ signing key n9KcRZYHLU9rhGVwB9e4wEMYsxXvUfgFxtmX25pc1QPNgweqzQf5 already validated sequence at or past 12133663 src=1
```

此类偶然消息通常不表示存在问题。如果使用相同的发送验证器频繁发生此类消息，则可能表示存在问题，包括以下任何一种情况（大致按照大多数情况下最不可能的顺序）：

- 写入消息的服务器存在网络问题。
- 消息中描述的验证器存在网络问题。
- 消息中描述的验证器具有恶意行为。


## 无法确定父类账本的哈希值

当服务器从对等方看到验证消息并且它不知道服务器正在构建的父分类帐版本时，会发生以下日志消息。当服务器同步到网络时，这是正常的。

```
2018-Aug-28 22:56:22.256065549 Validations:WRN Unable to determine hash of ancestor seq=3 from ledger hash=00B1E512EF558F2FD9A0A6C263B3D922297F26A55AEB56A009341A22895B516E seq=12133675
```

如果在启动服务器后的前5到15分钟内经常出现此消息，则可能表示存在问题。


## InboundLedger需要的哈希

如下所示的日志消息表明服务器正在从其他服务器请求分类帐数据：

```
InboundLedger:WRN Want: 5AE53B5E39E6388DBACD0959E5F5A0FCAF0E0DCBA45D9AB15120E8CDD21E019B
```

如果您的服务器正在同步，回填或下载历史记录分片，这是正常现象。


## InboundLedger 11分类帐超时

```
InboundLedger:WRN 11 timeouts for ledger 8265938
```

这表示您的服务器在从其对等方请求特定分类帐数据时遇到问题。如果分类帐索引远低于server_info方法报告的最新验证分类帐索引，则可能表示您的服务器正在下载历史记录分片。

这不是严格意义上的问题，但如果您想更快地获取分类帐历史记录，则可以skywelld通过添加或编辑[ips_fixed]配置节并重新启动服务器来配置连接到具有完整历史记录的对等方。例如，要始终尝试连接到skywelld的完整历史记录服务器之一：
```
[ips_fixed]
s.jingtum.com 50333
```
