---
title: PowerShell サンプル - 証明書を使用しないアプリケーション プロキシ アプリ
description: カスタム ドメインを使用しているが、有効な TLS/SSL 証明書がアップロードされていないすべての Azure Active Directory (Azure AD) アプリケーション プロキシ アプリケーションを一覧表示する PowerShell の例。
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 49a7c0729fae8eb009bfcbe0661ecaca600dd3ec
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861665"
---
# <a name="get-all-azure-ad-proxy-application-apps-published-with-no-certificate-uploaded"></a>証明書をアップロードせずに公開されたすべての Azure AD プロキシ アプリケーション アプリを取得する

この PowerShell スクリプトの例では、カスタム ドメインを使用しているが、有効な TLS/SSL 証明書がアップロードされていないすべての Azure Active Directory (Azure AD) アプリケーション プロキシ アプリを一覧表示します。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

このサンプルには、[Graph 用 AzureAD V2 PowerShell モジュール](/powershell/azure/active-directory/install-adv2) (AzureAD) または [Graph 用 AzureAD V2 PowerShell モジュール プレビュー バージョン](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) が必要です。

## <a name="sample-script"></a>サンプル スクリプト

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-custom-domain-no-cert.ps1 "Get all Azure AD Proxy application apps published with no certificate uploaded")]

## <a name="script-explanation"></a>スクリプトの説明

| command | Notes |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | サービス プリンシパルを取得します。 |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Azure AD アプリケーションを取得します。 |
|[Get-AzureADApplicationProxyApplication](/powershell/module/azuread/get-azureadapplicationproxyapplication) | Azure AD でアプリケーション プロキシ用に構成されたアプリケーションを取得します。 |

## <a name="next-steps"></a>次のステップ

Azure AD PowerShell モジュールの詳細については、[Azure AD PowerShell モジュールの概要](/powershell/azure/active-directory/overview)に関する記事を参照してください。

アプリケーション プロキシのその他の PowerShell の例については、[Azure AD アプリケーション プロキシの Azure AD PowerShell の例](../application-proxy-powershell-samples.md)に関する記事を参照してください。
