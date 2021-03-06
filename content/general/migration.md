---
weight: 40
title: 备份与迁移
summary: 介绍如何进行备份与迁移
type: docs
---

# 备份与迁移

假设您已根据「服务器配置」中所介绍的方法切换至 `scp` 用户。本文将以备份、迁移全部机器人，和备份、迁移 `SCP-079-PM` 为例，介绍如何操作，其他机器人的备份迁移的流程基本相同。

---

## 备份

执行命令：

{{< tabs "uniqueid1" >}}
{{< tab "全部备份" >}}
```bash
backup
```
{{< /tab >}}

{{< tab "选择性备份" >}}
以 `PM` 为例：

```bash
backup pm
```
{{< /tab >}}
{{< /tabs >}}

此命令执行后，将在以下位置生成备份文件：

{{< tabs "uniqueid2" >}}
{{< tab "全部备份" >}}
```bash
~/scp-079.tar.gz
```

{{< hint danger >}}
**注意**  

1. 进行备份后，所有机器人将停止运行。请根据实际需要，选择是否要在备份后在服务器上使用 `refresh` 命令重启所有服务。
2. 备份文件中包含配置、数据、密码、登录会话等，请妥善保管和传输。
<br /><br />
{{< /hint >}}
{{< /tab >}}

{{< tab "选择性备份" >}}
以 `PM` 为例：

```bash
~/scp-079-pm.tar.gz
```

{{< hint danger >}}
**注意**  

1. 进行备份后，所指定备份的机器人 `PM` 将停止运行。请根据实际需要，选择是否要在备份后在服务器上使用 `start pm` 命令重启该服务。
2. 备份文件中包含配置、数据、密码、登录会话等，请妥善保管和传输。
<br /><br />
{{< /hint >}}
{{< /tab >}}
{{< /tabs >}}

---

## 迁移

请确保已按照[「服务器配置」](/general/server/)一节，对新的服务器进行必要的配置。

首先，将备份文件（`*.tar.gz`）传输至新服务器的 `/home/scp` 目录下，文件所有者应为用户 `scp`。

然后，在新服务器的 `scp` 用户下执行以下命令：

{{< tabs "uniqueid3" >}}
{{< tab "全部迁移" >}}
```bash
source <(curl -s https://raw.githubusercontent.com/scp-079/scripts/master/restore.sh)
```
{{< /tab >}}

{{< tab "选择性迁移" >}}
以 `PM` 为例：

```bash
source <(curl -s https://raw.githubusercontent.com/scp-079/scripts/master/restore.sh) pm
```
{{< /tab >}}
{{< /tabs >}}

---

### 设置每日定时重启

在新服务器下执行以下命令：

```bash
enable 00:13:00
```

请务必确保参数格式与示例一致，例如 `00:13:00` 代表在系统时间的 `0 点 13 分 0 秒` 重启所有机器人。

可根据需要自行调整此参数，建议将重启时间设置为机器人负载较低的时间段。建议分钟数保持为 `13`，以免重启时间影响到机器人某个整点或半点的定时任务。

---

### 启动服务

在新服务器下执行以下命令：

{{< tabs "uniqueid4" >}}
{{< tab "全部迁移" >}}
```bash
refresh
```
{{< /tab >}}

{{< tab "选择性迁移" >}}
以 `PM` 为例：

```bash
start pm
```
{{< /tab >}}
{{< /tabs >}}

---

至此，机器人已应成功运行，迁移过程已结束。
