---
title: チュートリアル:NetSuite OneWorld を構成し、Azure Active Directory を使用してユーザーが自動的にプロビジョニングされるようにする | Microsoft Docs
description: Azure Active Directory と NetSuite OneWorld の間でシングル サインオンを構成する方法について説明します。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: a5b995fd273fcfa231e101bc77b11d268be728fb
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359137"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>チュートリアル:NetSuite を構成し、ユーザーが自動的にプロビジョニングされるようにする

このチュートリアルでは、Azure AD から NetSuite OneWorld にユーザー アカウントが自動的にプロビジョニング/プロビジョニング解除されるために NetSuite と Azure AD で実行する必要がある手順について説明します。

> [!NOTE]
> 現在この統合は、基本認証 (ユーザー名とパスワード) を使用して認証されています。 NetSuite では、この統合の使用に多要素認証の要件を実装しているため、この要件が免除されている場合を除いて、顧客はこの要件を満たさないとこの統合を使用することはできません。 マイクロソフトは NetSuite と協力し、免除されていない顧客も再度使用できるように、この統合を新しい認証方法に更新しようとしています。 利用できるようになった場合、このドキュメントを ETA と共に更新します。

推奨される操作:この統合の認証動作に対する更新プログラムがリリースされるまでお待ちいただくか、または NetSuite のサポートに、多要素認証要件の免除についてお問い合わせてください。

## <a name="prerequisites"></a>前提条件

このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

*   Azure Active Directory テナント。
*   NetSuite OneWorld のサブスクリプション。 現在、自動ユーザー プロビジョニングは NetSuite OneWorld でのみサポートされていることに注意してください。
*   管理者アクセス許可を持つ Netsuite のユーザー アカウント。
*   Azure AD との統合には、2FA の免除が必要です。 この免除を要求するには、NetSuite のサポート チームにお問い合わせください。

## <a name="assigning-users-to-netsuite-oneworld"></a>NetSuite OneWorld へのユーザーの割り当て

Azure Active Directory では、選択されたアプリへのアクセスが付与されるユーザーを決定する際に "割り当て" という概念が使用されます。 自動ユーザー アカウント プロビジョニングのコンテキストでは、Azure AD 内のアプリケーションに "割り当て済み" のユーザーとグループのみが同期されます。

プロビジョニング サービスを構成して有効にする前に、どの Azure AD ユーザーおよびグループが、お使いの NetSuite アプリにアクセスする必要があるか決定する必要があります。 決定し終えたら、次の手順でこれらのユーザーをお使いの NetSuite アプリに割り当てます。

[エンタープライズ アプリケーションにユーザーまたはグループを割り当てる](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>ユーザーを NetSuite OneWorld に割り当てる際の重要なヒント

*   プロビジョニングの構成をテストする場合、1 人の Azure AD ユーザーを NetSuite に割り当てることをお勧めします。 後でユーザーやグループを追加で割り当てられます。

*   NetSuite にユーザーを割り当てる場合には、有効なユーザー ロールを選択する必要があります。 "既定のアクセス" ロールはプロビジョニングでは使えません。

## <a name="enable-user-provisioning"></a>ユーザー プロビジョニングの有効化

このセクションでは、お使いの Azure AD を NetSuite のユーザー アカウント プロビジョニング API に接続する手順のほか、プロビジョニング サービスを構成して、Azure AD のユーザーとグループの割り当てに基づいて割り当て済みのユーザー アカウントを NetSuite で作成、更新、無効化する手順を説明します。

> [!TIP] 
> NetSuite では SAML ベースのシングル サインオンを有効にすることもできます。これを行うには、[Azure portal](https://portal.azure.com) で説明されている手順に従ってください。 シングル サインオンは自動プロビジョニングとは別に構成できますが、これらの 2 つの機能は相補的な関係にあります。

### <a name="to-configure-user-account-provisioning"></a>ユーザー アカウント プロビジョニングを構成するには

このセクションでは、Active Directory のユーザー アカウントを NetSuite にプロビジョニングする方法の概要を説明します。

1. [Azure Portal](https://portal.azure.com) で、 **[Azure Active Directory]、[エンタープライズ アプリ]、[すべてのアプリケーション]** セクションの順に移動します。

1. NetSuite に既にシングル サインオンを構成している場合は、検索フィールドからご自分の NetSuite インスタンスを検索します。 構成していない場合は、 **[追加]** を選択してアプリケーション ギャラリーで **NetSuite** を検索します。 検索結果から NetSuite を選択して、自分のアプリケーション一覧に追加します。

1. 自分の NetSuite のインスタンスを選択してから、 **[プロビジョニング]** タブを選択します。

1. **[プロビジョニング モード]** を **[自動]** に設定します。 

    ![スクリーンショットには、[自動] に設定された [プロビジョニング モード] と、ユーザーが設定するその他の値が NetSuite プロビジョニング ページが示されています。](./media/netsuite-provisioning-tutorial/provisioning.png)

1. **[管理者資格情報]** セクションに次の構成設定を指定します。
   
    a. **[管理ユーザー名]** テキストボックスに、NetSuite.com に **システム管理者** プロファイルが割り当てられている NetSuite アカウント名を入力します。
   
    b. **[管理パスワード]** テキストボックスに、このアカウントのパスワードを入力します。
      
1. Azure portal で、 **[テスト接続]** をクリックして Azure AD がお使いの NetSuite アプリに接続できることを確認します。

1. プロビジョニングのエラー通知を受け取るユーザーまたはグループの電子メール アドレスを **[通知用メール]** フィールドに入力して、チェック ボックスをオンにします。

1. **[保存]** をクリックします。

1. [マッピング] セクションの **[Synchronize Azure Active Directory Users to NetSuite]\(Azure Active Directory ユーザーを NetSuite に同期する\)** を選びます。

1. **[属性マッピング]** セクションで、Azure AD から NetSuite に同期されるユーザー属性を確認します。 **[照合]** プロパティとして選択されている属性は、更新処理で NetSuite のユーザー アカウントとの照合に使用されることに注意してください。 [保存] ボタンをクリックして変更をコミットします。

1. NetSuite で Azure AD プロビジョニング サービスを有効にするには、[設定] セクションで **[プロビジョニングの状態]** を **[オン]** に変更します。

1. **[保存]** をクリックします。

[ユーザーとグループ] セクションで NetSuite に割り当てたユーザーやグループの初期同期が開始されます。 初期同期は後続の同期よりも実行に時間がかかることに注意してください。後続の同期は、サービスが実行されている限り約 40 分ごとに実行されます。 **[同期の詳細]** セクションを使用すると、進行状況を監視できるほか、リンクをクリックしてプロビジョニング アクティビティ ログを取得できます。このログには、プロビジョニング サービスによって NetSuite アプリに実行されたすべてのアクションが記載されています。

Azure AD プロビジョニング ログの読み取りの詳細については、「[自動ユーザー アカウント プロビジョニングについてのレポート](../app-provisioning/check-status-user-account-provisioning.md)」をご覧ください。

## <a name="additional-resources"></a>その他のリソース

* [エンタープライズ アプリのユーザー アカウント プロビジョニングの管理](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)
* [シングル サインオンの構成](netsuite-tutorial.md)
