---
title: Azure Cosmos DB Gremlin API のデータベースとグラフを作成する PowerShell スクリプト
description: Azure PowerShell script - Azure Cosmos DB Gremlin API のデータベースとグラフを作成する
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.openlocfilehash: 38ed741401740c60e9bdbe0869caa0f8a6a3c76d
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93099302"
---
# <a name="create-a-database-and-graph-for-azure-cosmos-db---gremlin-api"></a>Azure Cosmos DB - Gremlin API のデータベースとグラフを作成する
[!INCLUDE[appliesto-gremlin-api](../../../includes/appliesto-gremlin-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>サンプル スクリプト

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-create.ps1 "Create a database and graph for Gremlin API")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ

スクリプト サンプルの実行後は、次のコマンドを使用してリソース グループとすべての関連リソースを削除することができます。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | Notes |
|---|---|
|**Azure Cosmos DB**| |
| [New-AzCosmosDBAccount](/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Cosmos DB アカウントを作成します。 |
| [New-AzCosmosDBGremlinDatabase](/powershell/module/az.cosmosdb/new-azcosmosdbgremlindatabase) | Gremlin API データベースを作成します。 |
| [New-AzCosmosDBGremlinConflictResolutionPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbgremlinconflictresolutionpolicy) | Gremlin API 書き込みの競合の解決ポリシーを作成します。 |
| [New-AzCosmosDBGremlinGraph](/powershell/module/az.cosmosdb/new-azcosmosdbgremlingraph) | Gremlin API グラフを作成します。 |
|**Azure リソース グループ**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 入れ子になったリソースすべてを含むリソース グループを削除します。 |
|||

## <a name="next-steps"></a>次のステップ

Azure PowerShell の詳細については、[Azure PowerShell のドキュメント](/powershell/)を参照してください。