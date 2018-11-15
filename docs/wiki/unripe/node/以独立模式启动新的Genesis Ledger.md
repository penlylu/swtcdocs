# 以独立模式启动新的Genesis Ledger

在独立模式下，您可以使用“skywelld”创建新的生成分类帐。这提供了一个已知状态，没有来自生产SWTC  Ledger的分类帐历史记录。 （除了其他方面，这对单元测试非常有用。）

* 要skywelld使用新的生成分类帐在独立模式下启动，请使用-a和--start选项：

```
skywelld -a --start --conf=/path/to/skywelld.cfg
```

在一个起源分类账中，起源地址拥有全部6000亿SWTC。创世地址的关键字硬编码如下：

**地址**：`jHb9CJAWyB4jr91VRWn96DkukG4bwdtyTh`

**秘钥**：`snoPBjXtMeMyMHUVTgbuqAfg1SUTb`（“masterpassphrase”）

## New Genesis Ledgers中的设置

在新的生成分类帐中，硬编码的默认[Reserve]最小为 **200 SWTC**，用于为新地址提供资金，在分类帐中每个对象增加**50 SWTC**。这些值高于生产网络的当前储备要求。 （另见：[收费表决]（费用表决））

默认情况下，新的生成分类帐未启用[修订]。如果你用`--start`启动一个新的创世总账，那么创世总账包含一个[EnableAmendment伪事务]来打开`skywelld`服务器原生支持的所有修改，除了你明确的修改在配置文件中禁用。这些修订的效果可从下一个分类帐版本开始提供。 （提醒：在独立模式下，您必须[手动推进分类帐]。
