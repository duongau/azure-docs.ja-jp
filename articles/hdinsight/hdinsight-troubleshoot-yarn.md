---
title: Azure HDInsight での YARN のトラブルシューティング
description: Apache Hadoop YARN と Azure HDInsight の操作についてよく寄せられる質問とその回答を示します。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: b8dc63030c6791779ad90500747e4e2f5c4ffeef
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022668"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Azure HDInsight を使用した Apache Hadoop YARN のトラブルシューティング

Apache Ambari で Apache Hadoop YARN ペイロードを操作するときに発生する主な問題とその解決策について説明します。

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>クラスターで新しい YARN キューを作成する方法

### <a name="resolution-steps"></a>解決手順

新しい YARN キューを作成し、すべてのキューの間で容量の割り当てのバランスを取るには、Ambari で次の手順を実行します。

この例では、次の 2 つの既存のキュー (**default** と **thriftsvr**) 両方の容量を 50% から 25% に変更し、新しいキュー (Spark) が容量の 50% を使用できるようにします。

| キュー | 容量 | 最大容量 |
| --- | --- | --- |
| 既定値 (default) | 25% | 50% |
| thrftsvr | 25% | 50% |
| spark | 50% | 50% |

1. **[Ambari Views]** アイコンを選択し、グリッド パターンを選択します。 次に、 **[YARN Queue Manager]\(YARN キュー マネージャー\)** を選択します。

    ![Apache Ambari ダッシュボードの YARN Queue Manager](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-1.png)
2. **default** キューを選びます。

    ![Apache Ambari YARN での default キューの選択](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-2.png)
3. **default** キューで、**容量** を 50% から 25% に変更します。 **thriftsvr** キューで、**容量** を 25% に変更します。

    ![default キューと thriftsvr キューの容量を 25% に変更する](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-3.png)
4. 新しいキューを作成し、 **[キューの追加]** を選択します。

    ![Apache Ambari YARN ダッシュボードのキューの追加](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-4.png)

5. 新しいキューの名前を指定します。

    ![Apache Ambari YARN ダッシュボードのキューの名前](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-5.png)  

6. **[容量]** の値は 50% のままにして、 **[アクション]** ボタンをクリックします。

    ![Apache Ambari YARN でのアクションの選択](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-6.png)  
7. **[Save and Refresh Queues]\(キューの保存と更新\)** を選択します。

    ![[Save and Refresh Queues]\(キューの保存と更新\) を選択する](media/hdinsight-troubleshoot-yarn/apache-yarn-create-queue-7.png)  

これらの変更は YARN Scheduler UI にすぐに表示されます。

### <a name="additional-reading"></a>その他の情報

- [Apache Hadoop YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)

## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>クラスターから YARN ログをダウンロードする方法

### <a name="resolution-steps"></a>解決手順

1. Secure Shell (SSH) クライアントで HDInsight クラスターに接続します。 詳細については、「[その他の情報](#additional-reading-2)」をご覧ください。

1. 現在実行しているすべての YARN アプリケーションのアプリケーション ID を一覧で表示するには、次のコマンドを実行します。

    ```apache
    yarn top
    ```

    ID が **APPLICATIONID** 列に一覧で表示されます。 ログを **APPLICATIONID** 列からダウンロードできます。

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

1. すべてのアプリケーション マスターの YARN コンテナー ログをダウンロードするには、次のコマンドを使用します。

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    このコマンドによって、amlogs.txt という名前のログ ファイルが作成されます。

1. 最新のアプリケーション マスターの YARN コンテナー ログだけをダウンロードするには、次のコマンドを使用します。

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    このコマンドによって、latestamlogs.txt という名前のログ ファイルが作成されます。

1. 最初の 2 つのアプリケーション マスターの YARN コンテナー ログをダウンロードするには、次のコマンドを使用します。

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

    このコマンドによって、first2amlogs.txt という名前のログ ファイルが作成されます。

1. すべての YARN コンテナー ログをダウンロードするには、次のコマンドを使用します。

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    このコマンドによって、logs.txt という名前のログ ファイルが作成されます。

1. 特定のコンテナーの YARN コンテナー ログをダウンロードするには、次のコマンドを使用します。

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

    このコマンドによって、containerlogs.txt という名前のログ ファイルが作成されます。

### <a name="additional-reading"></a><a name="additional-reading-2"></a>その他の情報

- [SSH を使用して HDInsight (Apache Hadoop) に接続する](./hdinsight-hadoop-linux-use-ssh-unix.md)
- [Apache Hadoop YARN の概念とアプリケーション](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html#Concepts_and_Flow)

## <a name="next-steps"></a>次のステップ

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]