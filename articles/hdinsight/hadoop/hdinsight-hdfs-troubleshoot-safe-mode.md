---
title: Azure HDInsight クラスター上のローカル HDFS がセーフ モードでスタックする
description: Azure HDInsight の Apache クラスター上でセーフ モードでスタックするローカル Apache HDFS のトラブルシューティング
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: 0f4a7702c4f85c162b0485e06cabc99b963ff210
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96014705"
---
# <a name="scenario-local-hdfs-stuck-in-safe-mode-on-azure-hdinsight-cluster"></a>シナリオ:Azure HDInsight クラスター上のローカル HDFS がセーフ モードでスタックする

この記事では、Azure HDInsight クラスターと対話するときの問題のトラブルシューティング手順と可能な解決策について説明します。

## <a name="issue"></a>問題

HDInsight クラスター上でローカル Apache Hadoop 分散ファイル システム (HDFS) のセーフ モードが解除されない。 次のようなエラー メッセージが表示されます。

```output
hdiuser@spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
...
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

## <a name="cause"></a>原因

HDFS レプリケーション係数未満か、それに近いごく少数のノードにまで、HDInsight クラスターがスケール ダウンされました。

## <a name="resolution"></a>解像度

1. 次のコマンドを使用して、HDInsight クラスター上の HDFS の状態をレポートします。

    ```bash
    hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
    ```

1. 次のコマンドで、HDInsight クラスター上の HDFS の整合性をチェックします。

    ```bash
    hdiuser@spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
    ```

1. 欠落しているブロックや破損しているブロック、レプリケート数不足のブロックが存在しないことがわかっている場合や、それらのブロックを無視できる場合は、次のコマンドを実行して、ネーム ノードのセーフ モードを解除します。

    ```bash
    hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
    ```

## <a name="next-steps"></a>次のステップ

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]