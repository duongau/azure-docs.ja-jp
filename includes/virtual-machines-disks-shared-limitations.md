---
title: インクルード ファイル
description: インクルード ファイル
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 76e52758bf88dde9cdc68dad974f0b65fd55140f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025946"
---
一部の種類のディスクでのみ、共有ディスクを有効にできます。 現在のところ、Ultra ディスクと Premium SSD のみで共有ディスクを有効にできます。 共有ディスクが有効になっているマネージド ディスクはそれぞれ、次の制約を受けます。制限はディスクの種類別に整理されています。

### <a name="ultra-disks"></a>Ultra ディスク

Ultra ディスクには、共有ディスクとは関係なく、独自の制限リストがあります。 Ultra ディスクの制限については、「[Azure Ultra ディスクの使用](../articles/virtual-machines/disks-enable-ultra-ssd.md)」を参照してください。

Ultra ディスクを共有するとき、次の追加制限があります。

- Azure Resource Manager または SDK サポートに現在、制限されています。 
- Windows Server フェールオーバー クラスターの一部のバージョンでは、ベーシック ディスクのみを使用できます。詳細については、「[フェールオーバー クラスタリングのハードウェア要件と記憶域オプション](/windows-server/failover-clustering/clustering-requirements)」を参照してください。

共有 Ultra ディスクは、既定で、Ultra ディスクをサポートするすべてのリージョンで利用でき、それらを使用するためのアクセスに対してサインアップする必要はありません。

### <a name="premium-ssds"></a>Premium SSD

- Azure Resource Manager または SDK サポートに現在、制限されています。 
- OS ディスクではなく、データ ディスクでのみ有効にすることができます。
- **ReadOnly** ホスト キャッシュは、`maxShares>1` の Premium SSD では使用できません。
- ディスク バーストは、`maxShares>1` の Premium SSD では使用できません。
- Azure 共有ディスクで可用性セットと仮想マシン スケール セットを使用している場合、仮想マシン障害ドメインによる[ストレージ障害ドメイン配置](../articles/virtual-machines/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set)は共有データ ディスクには適用されません。
- [近接配置グループ (PPG)](../articles/virtual-machines/windows/proximity-placement-groups.md) を使用する場合は、ディスクを共有するすべての仮想マシンが同じ PPG に含まれている必要があります。
- Windows Server フェールオーバー クラスターの一部のバージョンでは、ベーシック ディスクのみを使用できます。詳細については、「[フェールオーバー クラスタリングのハードウェア要件と記憶域オプション](/windows-server/failover-clustering/clustering-requirements)」を参照してください。
- Azure Backup および Azure Site Recovery のサポートは、まだ使用できません。

#### <a name="regional-availability"></a>リージョン別の提供状況

共有 Premium SSD は、マネージド ディスクが使用可能なすべてのリージョンで利用できます。