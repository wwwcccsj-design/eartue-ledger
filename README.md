# EARTUE 积分账本公示

本仓库只保存 EARTUE 社区积分账本的**每小时总指纹**，由社区 Worker 自动提交，用来让任何人确认历史积分没有被悄悄修改。这里不包含任何用户、帖子或积分明细。

## 文件

`checkpoints/00000001.json` 起每期一个文件：

| 字段 | 含义 |
|---|---|
| `seq` | 期号，从 1 起连续递增 |
| `firstRowid` / `lastRowid` | 本期覆盖的流水范围 |
| `entryCount` | 本期流水条数 |
| `adminCount` / `adminTotal` | 本期管理员手工调整的笔数与合计积分 |
| `merkleRoot` | 本期全部流水指纹的默克尔根 |
| `prevHash` | 上一期的 `checkpointHash`，各期因此串成一条链 |
| `checkpointHash` | 本期指纹 |

## 计算方法

- 流水指纹：SHA-256，内容为 `eartue-ledger-entry-v1` 与流水各字段按换行拼接。
- 默克尔节点：SHA-256(`eartue-ledger-node-v1\n左\n右`)，落单的末尾节点原样上移。
- 本期指纹：SHA-256，内容为 `eartue-ledger-checkpoint-v1`、期号、范围、条数、管理员调整笔数与合计、默克尔根、上一期指纹按换行拼接。

社区用户可在 EARTUE 社区「积分与等级 → 验证我的积分」中，由浏览器下载本人流水的证明，在本机重算后与本仓库的文件比对。

## 承诺

文件只增不改。若任何一期文件被修改或删除，提交历史与后续各期的 `prevHash` 都会暴露。
