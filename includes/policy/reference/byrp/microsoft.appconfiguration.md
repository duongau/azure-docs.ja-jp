---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a4a1627ddd5f02535d1221a6dd276e02a7ec3d78
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2021
ms.locfileid: "98804974"
---
|名前<br /><sub>(Azure portal)</sub> |説明 |効果 |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[App Configuration はカスタマー マネージド キーを使用する必要がある](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F967a4b4b-2da9-43c1-b7d0-f98d0d74d0b1) |カスタマー マネージド キーは、暗号化キーを管理できるようにすることで、データ保護を強化します。 これは、多くの場合、コンプライアンス要件を満たすために必要となります。 |Audit、Deny、Disabled |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Configuration/CustomerManagedKey_Audit.json) |
|[App Configuration ではプライベート リンクを使用する必要がある](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fca610c1d-041c-4332-9d88-7ed3094967c7) |Azure Private Link を使用すると、接続元または接続先にパブリック IP アドレスを使用せずに、仮想ネットワークを Azure サービスに接続できます。 プライベート リンク プラットフォームでは、Azure のバックボーン ネットワークを介してコンシューマーとサービスの間の接続が処理されます。 サービス全体ではなく、アプリ構成インスタンスにプライベート エンドポイントをマッピングすることで、データ漏えいのリスクからも保護されます。 詳細については、[https://aka.ms/appconfig/private-endpoint](https://aka.ms/appconfig/private-endpoint) を参照してください。 |AuditIfNotExists、Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Configuration/PrivateLink_Audit.json) |
