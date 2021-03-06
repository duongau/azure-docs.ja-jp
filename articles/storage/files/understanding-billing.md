---
title: Azure Files の課金の概要 | Microsoft Docs
description: Azure ファイル共有のプロビジョニングおよび従量課金制モデルを解釈する方法について説明します。
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 12/1/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 880ec90ce1cf0efffce0cfd6800bdbaed23f8dd0
ms.sourcegitcommit: beacda0b2b4b3a415b16ac2f58ddfb03dd1a04cf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2020
ms.locfileid: "97831467"
---
# <a name="understanding-azure-files-billing"></a>Azure Files の課金の概要
Azure Files には、プロビジョニングと従量課金制という 2 つの異なる課金モデルが用意されています。 プロビジョニング モデルは Premium ファイル共有でのみ使用できます。これは、**FileStorage** ストレージ アカウントの種類でデプロイされたファイル共有です。 従量課金制モデルは Standard ファイル共有でのみ使用できます。Standard ファイル共有は、**汎用バージョン 2 (GPv2)** ストレージ アカウントの種類でデプロイされたファイル共有です。 この記事では、Azure Files の毎月の請求書を理解できるように、両方のモデルがどのように機能するかについて説明します。

Azure Files の現在の価格については、[Azure Files の価格ページ](https://azure.microsoft.com/pricing/details/storage/files/)を参照してください。

## <a name="provisioned-billing"></a>プロビジョニングの課金
Azure Files では、Premium ファイル共有にプロビジョニング モデルが使用されます。 プロビジョニング ビジネス モデルでは、使用量に基づいて請求されるのではなく、Azure Files サービスにストレージ要件を事前に指定します。 これはオンプレミスでハードウェアを購入する場合と似ています。この場合、特定の量のストレージを使用する Azure ファイル共有をプロビジョニングするときに、使用するかどうかに関係なく、そのストレージの料金を支払います。オンプレミスの物理メディアにかかるコストを支払い始めるのが、領域を使い始めるときではないことと同じです。 オンプレミスの物理メディアを購入する場合とは異なり、プロビジョニング ファイル共有は、ストレージと IO のパフォーマンス特性に応じて動的にスケールアップまたはスケールダウンすることができます。

Premium ファイル共有をプロビジョニングするときは、ワークロードに必要な GiB 数を指定します。 プロビジョニングする GiB ごとに、固定比率で追加の IOPS とスループットを使用できるようになります。 保証されているベースライン IOPS に加えて、各 Premium ファイル共有はベスト エフォート ベースでバーストをサポートしています。 IOPS とスループットの数式は次のとおりです。

- ベースライン IOPS = 400 + 1 * プロビジョニング済み GiB。 (最大 100,000 IOPS まで)。
- バースト限度 = MAX(4000, 3 * ベースライン IOPS)。
- エグレス レート = 60 MiB/s + 0.06 * プロビジョニング済み GiB。
- イングレス レート = 40 MiB/s + 0.04 * プロビジョニング済み GiB

プロビジョニングされたファイル共有のサイズはいつでも引き上げることができますが、引き下げは最後の引き上げから 24 時間後にのみ行うことができます。 クォータの拡大なしで 24 時間経過した後は、再び拡大するまで、共有クォータを何回でも縮小できます。 IOPS/スループットのスケールの変更は、プロビジョニングされたサイズの変更後、数分以内に有効になります。

プロビジョニングされた共有のサイズを、使用されている GiB 未満に縮小できます。 これを行った場合、データは失われませんが、使用されているサイズに対して引き続き課金され、使用されているサイズではなく、プロビジョニングされた共有のパフォーマンス (ベースライン IOPS、スループット、およびバースト IOPS) を受け取ります。

次の表は、プロビジョニングされた共有サイズについてのこれらの式の例をいくつか示しています。

|容量 (GiB) | ベースライン IOPS | バースト IOPS | エグレス (MiB/秒) | イングレス (MiB/秒) |
|---------|---------|---------|---------|---------|
|100         | 500     | 最大 4,000     | 66   | 44   |
|500         | 900     | 最大 4,000  | 90   | 60   |
|1,024       | 1,424   | 最大 4,000   | 122   | 81   |
|5,120       | 5,520   | 最大 15,360  | 368   | 245   |
|10,240      | 10,640  | 最大 30,720  | 675   | 450   |
|33,792      | 34,192  | 最大 100,000 | 2,088 | 1,392   |
|51,200      | 51,600  | 最大 100,000 | 3,132 | 2,088   |
|102,400     | 100,000 | 最大 100,000 | 6,204 | 4,136   |

ファイル共有の実効パフォーマンスは、他の多くの要因の中でも特にマシン ネットワークの制限、使用可能なネットワーク帯域幅、IO サイズ、並列処理の影響を受けます。 たとえば、8 KiB の読み取り/書き込み IO サイズでの内部テストに基づいて、SMB 経由で Premium ファイル共有に接続された、SMB マルチチャネルが有効になっていない単一の Windows 仮想マシン (*Standard F16s_v2*) は 20K の読み取り IOPS と 15K の書き込み IOPS を実現できます。 512 MiB の読み取り/書き込み IO サイズでは、同じ VM は 1.1 GiB/秒の送信スループットと 370 MiB/秒の受信スループットを実現できます。 Premium 共有で SMB マルチチャネルが有効になっている場合は、同じクライアントで 最大 \~3 倍のパフォーマンスを達成できます。 最大のパフォーマンス スケールを達成するには、[SMB マルチチャネルを有効にし](storage-files-enable-smb-multichannel.md)、負荷を複数の VM に分散します。 一般的なパフォーマンスの問題と回避策については、「[SMB マルチチャネルのパフォーマンス](storage-files-smb-multichannel-performance.md)」と[トラブルシューティング ガイド](storage-troubleshooting-files-performance.md)に関する記事を参照してください。

### <a name="bursting"></a>バースト
ピーク時の需要に対応するためにワークロードで追加のパフォーマンスが必要な場合、共有は、バースト クレジットを使用して共有のベースライン IOPS の限度を超えて、需要を満たすために必要な共有パフォーマンスを提供できます。 Premium ファイル共有は、IOPS を最大 4,000 または最大 3 倍のいずれか大きい方の値までバーストできます。 バーストは自動化され、クレジット システムに基づいて動作します。 バーストはベスト エフォートで動作し、バースト限度は保証されるものではなく、ファイル共有は、最大 60 分間、その限度 "*まで*" バーストできます。

クレジットは、ファイル共有のトラフィックがベースライン IOPS を下回るたびに、バースト バケットに蓄積されます。 たとえば、100 GiB 共有は 500 ベースライン IOPS を備えています。 共有の実際のトラフィックが特定の 1 秒間で 100 IOPS だった場合は、未使用の 400 IOPS がバースト バケットに補充されます。 同様に、アイドル状態の 1 TiB 共有では、1,424 IOPS でバースト クレジットが発生します。 これらのクレジットは、後で操作がベースライン IOPS を超えたときに使用されます。

共有は、ベースライン IOPS を超え、バースト バケット内にクレジットがあるときは常に、最大許可ピーク バースト率でバーストします。 クレジットが残っている限り、共有は最大 60 分間バーストを継続できますが、これは発生したバースト クレジットの数に基づきます。 ベースライン IOPS を超えた IO のそれぞれが 1 つのクレジットを消費し、すべてのクレジットが消費されると、共有はベースライン IOPS に戻ります。

共有のクレジットには次の 3 つの状態があります。

- 蓄積中 (ファイル共有が使用している量がベースライン IOPS より少ない場合)。
- 減少中 (ファイル共有がベースライン IOPS より多く使用しており、バースト モードである場合)。
- 一定 (ファイル共有がちょうどベースライン IOPS を使用していて、クレジットが発生したり、使用されたりしていない場合)。

新しいファイル共有は、そのバースト バケットに全数のクレジットが含まれると開始されます。 サーバーによる調整のために共有 IOPS がベースライン IOPS を下回った場合、バースト クレジットは発生しません。

## <a name="pay-as-you-go-billing"></a>従量課金制の課金
Azure Files には、Standard ファイル共有に従量課金制ビジネス モデルが使用されます。 従量課金制ビジネス モデルの場合、支払う金額は、プロビジョニングされた量ではなく、実際に使用する量によって決まります。 大まかに言えば、ディスクに格納されているデータの量と、そのデータの使用状況に基づいて追加のトランザクション セットに対してコストを支払います。 従量課金制モデルの方がコスト効率が高くなる可能性があります。なぜなら、ワークロードのデータ フットプリントが時間の経過と共に変化する場合、将来の成長、パフォーマンスの要件、またはプロビジョニング解除を考慮して、過剰なプロビジョニングを行う必要がないためです。 一方、従量課金制モデルはエンドユーザーの使用量によって決まるため、予算編成プロセスの一環として従量課金制モデルを計画することが困難な場合もあります。

### <a name="differences-in-standard-tiers"></a>Standard レベルの相違点
Standard ファイル共有を作成するときに、トランザクション最適化層、ホット層、およびクール層から選択します。 3 つの層はすべて、まったく同じ Standard ストレージ ハードウェアに格納されます。 これら 3 つの層の主な違いは、保存データのストレージ料金がクールな層では低く、トランザクション料金がクールな層では高いことです。 これは、以下を意味します。

- 名前が示すとおり、トランザクション最適化は、トランザクション ワークロードが大きい場合の料金を最適化します。 トランザクション最適化では、保存データのストレージ料金が最も高くなりますが、トランザクション料金は最も低くなります。
- ホットでは、多数のトランザクションを含まないアクティブなワークロードを対象としており、トランザクション最適化と比べて保存データのストレージ料金は若干低くなりますが、トランザクション料金が若干高くなります。 これは、トランザクション最適化層とクール層の間の中間点と考えてください。
- クールでは、多数のアクティビティを含まないワークロードの料金を最適化します。これにより、格納データのストレージ料金は最も低くなりますが、トランザクション料金は最も高くなります。

アクセス頻度の低いワークロードをトランザクション最適化層に配置すると、月に数回、共有に対してトランザクションを実行してもほとんど料金はかかりませんが、データ ストレージ コストには多額の料金がかかります。 この同じ共有をクール層に移動した場合、やはりトランザクション コストについてはほとんど料金がかかりません。これは、単にこのワークロードに対してトランザクションを行う頻度が非常に低いからです。ただし、クール層のデータ ストレージ料金ははるかに安価です。 ユース ケースに適したレベルを選択すると、コストを大幅に削減できます。 ユース ケースに適したレベルを選択すると、コストを大幅に削減できます。

同様に、アクセスの多いワークロードをクール層に配置すると、トランザクション コストは大幅に増加しますが、データ ストレージ コストは少なくなります。 これにより、トランザクション料金の上昇によるコストの増加が、データ ストレージ料金の低下による節約額を上回り、トランザクション最適化よりもクールの方がコストがかかる可能性があります。 一部の使用レベルでは、ホット層が最もコスト効率の高い層であっても、クール層がトランザクション最適化よりも高価になる可能性があります。

ワークロードとアクティビティ レベルによって、標準ファイル共有の最もコスト効率の高い層が決まります。 実際のところ、最もコスト効率の高い層を選択する最善の方法は、共有の実際のリソース消費量 (格納データ、トランザクションの書き込みなど) を調べることです。

### <a name="what-are-transactions"></a>トランザクションとは
トランザクションとは、ファイル共有の内容をアップロード、ダウンロード、またはその他の方法で操作するための Azure Files に対する操作または要求のことです。 ファイル共有に対して実行されるすべてのアクションは、1 つ以上のトランザクションに変換されます。また、従量課金制モデルを使用する Standard 共有では、トランザクション コストに変換されます。

書き込み、一覧表示、読み取り、その他、削除の 5 つの基本的なトランザクション カテゴリがあります。 REST API または SMB を介して実行されるすべての操作は、次の 4 つのカテゴリのいずれかにバケット処理されます。

| 操作の種類 | 書き込みトランザクション | 一覧表示トランザクション | 読み取りトランザクション | その他トランザクション | 削除トランザクション |
|-|-|-|-|-|-|
| 管理操作 | <ul><li>`CreateShare`</li><li>`SetFileServiceProperties`</li><li>`SetShareMetadata`</li><li>`SetShareProperties`</li></ul> | <ul><li>`ListShares`</li></ul> | <ul><li>`GetFileServiceProperties`</li><li>`GetShareAcl`</li><li>`GetShareMetadata`</li><li>`GetShareProperties`</li><li>`GetShareStats`</li></ul> | | <ul><li>`DeleteShare`</li></ul> |
| データ操作 | <ul><li>`CopyFile`</li><li>`Create`</li><li>`CreateDirectory`</li><li>`CreateFile`</li><li>`PutRange`</li><li>`PutRangeFromURL`</li><li>`SetDirectoryMetadata`</li><li>`SetFileMetadata`</li><li>`SetFileProperties`</li><li>`SetInfo`</li><li>`SetShareACL`</li><li>`Write`</li><li>`PutFilePermission`</li></ul> | <ul><li>`ListFileRanges`</li><li>`ListFiles`</li><li>`ListHandles`</li></ul>  | <ul><li>`FilePreflightRequest`</li><li>`GetDirectoryMetadata`</li><li>`GetDirectoryProperties`</li><li>`GetFile`</li><li>`GetFileCopyInformation`</li><li>`GetFileMetadata`</li><li>`GetFileProperties`</li><li>`QueryDirectory`</li><li>`QueryInfo`</li><li>`Read`</li><li>`GetFilePermission`</li></ul> | <ul><li>`AbortCopyFile`</li><li>`Cancel`</li><li>`ChangeNotify`</li><li>`Close`</li><li>`Echo`</li><li>`Ioctl`</li><li>`Lock`</li><li>`Logoff`</li><li>`Negotiate`</li><li>`OplockBreak`</li><li>`SessionSetup`</li><li>`TreeConnect`</li><li>`TreeDisconnect`</li><li>`CloseHandles`</li><li>`AcquireFileLease`</li><li>`BreakFileLease`</li><li>`ChangeFileLease`</li><li>`ReleaseFileLease`</li></ul> | <ul><li>`ClearRange`</li><li>`DeleteDirectory`</li></li>`DeleteFile`</li></ul> |

> [!Note]  
> NFS 4.1 は、プロビジョニング課金モデルを使用する Premium ファイル共有でのみ使用できます。トランザクションは Premium ファイル共有の課金に影響しません。

## <a name="see-also"></a>関連項目
- [「Azure Files 料金」ページ](https://azure.microsoft.com/pricing/details/storage/files/)。
- 「[Azure Files のデプロイの計画](./storage-files-planning.md)」と「[Azure File Sync のデプロイの計画](./storage-sync-files-planning.md)」。
- 「[ファイル共有を作成する](./storage-how-to-create-file-share.md)」と「[Azure File Sync のデプロイ](./storage-sync-files-deployment-guide.md)」。