---
title: Azure IoT Central で RuuviTag を接続する | Microsoft Docs
description: RuuviTag 環境センサーを、お使いの IoT Central アプリケーションに接続する方法について説明します。
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 11/27/2019
ms.openlocfilehash: 7cf0d31895eaeaa41c23f9f07664b27bed1d3126
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006389"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>RuuviTag センサーを Azure IoT Central アプリケーションに接続する

*この記事は、ソリューション ビルダーを対象としています。*

この記事では、ソリューション ビルダーとして、RuuviTag センサーを、お使いの Microsoft Azure IoT Central アプリケーションに接続する方法について説明します。

RuuviTag とは

RuuviTag は、企業顧客、開発者、メーカー、学生、一般ユーザーのニーズを満たすように設計された高度なオープンソース センサー ビーコン プラットフォームです。 デバイスはすぐに使用できるように設定され、必要な場所に配置する準備もできています。 これは環境センサーと加速度計が組み込まれた Bluetooth LE ビーコンです。

RuuviTag は、BLE (Bluetooth Low Energy) で通信するため、Azure IoT Central に接続するにはゲートウェイ デバイスが必要です。 Rigado Cascade 500 などのゲートウェイ デバイスが、RuuviTag と IoT Central を接続できるように設定されていることを確認してください。

Rigado Cascade 500 ゲートウェイ デバイスを設定する場合は、[こちらの手順](./howto-connect-rigado-cascade-500.md)に従ってください。

## <a name="prerequisites"></a>前提条件

RuuviTag センサーを接続するには、次のリソースが必要です。

* RuuviTag センサー。 詳細については、[RuuviTag](https://ruuvi.com/) に関するページをご覧ください。
* Rigado Cascade 500 デバイスまたはその他の BLE ゲートウェイ。 詳細については、[Rigado](https://www.rigado.com/) に関するページをご覧ください。
* Azure IoT Central アプリケーション。 詳細については、[新しいアプリケーションの作成](./quick-deploy-iot-central.md)に関するページをご覧ください。

## <a name="add-a-ruuvitag-device-template"></a>RuuviTag デバイス テンプレートを追加する

RuuviTag センサーをお使いの Azure IoT Central アプリケーション インスタンスにオンボードするには、お使いのアプリケーション内で対応するデバイス テンプレートを構成する必要があります。

RuuviTag デバイス テンプレートを追加するには:

1. 左ウィンドウの **[Device Templates]\(デバイス テンプレート\)** タブに移動し、 _[+ 新規] を選択します。![新しいデバイス テンプレートを作成する](./media/howto-connect-ruuvi/devicetemplate-new.png) このページには、"*_新しいカスタム テンプレートを作成する_*"_ ためのオプション、または _"*_構成済みデバイス テンプレートを使用_*"_ するためのオプションが示されています
1. 次に示すように、構成済みデバイス テンプレートの一覧から RuuviTag デバイス テンプレートを選択します。![RuuviTag デバイス テンプレートを選択する](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. _*_[Next: Customize]\(次へ: カスタマイズ\)_*_ を選択して、次の手順に進みます。
1. 次の画面で、_*_[Create]\(作成\)_*_ を選択して、C500 デバイス テンプレートをお使いの IoT Central アプリケーションにオンボードします。

## <a name="connect-a-ruuvitag-sensor"></a>RuuviTag センサーを接続する

前述のように、RuuviTag とご自身の IoT Central アプリケーションを接続するには、ゲートウェイ デバイスを設定する必要があります。 以下の手順では、Rigado Cascade 500 ゲートウェイ デバイスが設定済みであることを前提としています。  

1. Rigado Cascade 500 デバイスの電源を入れ、ご自身のネットワークに接続します (イーサネットまたはワイヤレス経由)
1. RuuviTag のカバーを外し、プラスチックのタブを引いて、バッテリーとの接続を保護します。
1. ご自身の IoT Central アプリケーションで事前構成された Rigado Cascade 500 ゲートウェイのすぐ近くに RuuviTag を配置します。
1. わずか数秒で、ご自身の RuuviTag が、IoT Central 内のデバイスの一覧に表示されます。  
    ![RuuviTag デバイスの一覧](./media/howto-connect-ruuvi/ruuvi-devicelist.png)

これで、この RuuviTag が、ご自身の IoT Central アプリケーション内で使用できるようになりました。  

## <a name="create-a-simulated-ruuvitag"></a>シミュレートされた RuuviTag を作成する

物理 RuuviTag デバイスがない場合は、ご自身の Azure IoT Central アプリケーション内でテストに使用するシミュレートされた RuuviTag センサーを作成できます。

シミュレートされた RuuviTag を作成するには:

1. **[Devices]\(デバイス\) > [RuuviTag]** の順に選択します。
1. **[+新規]** を選択します。
1. 一意の **デバイス ID** と、わかりやすい **デバイス名** を指定します。  
1. **シミュレートされた** 設定を有効にします。
1. **［作成］** を選択します  

## <a name="next-steps"></a>次の手順

デバイス開発者にお勧めする次の手順は以下です。

- [Azure IoT Central のデバイス接続](./concepts-get-connected.md)について確認する
- [Azure CLI を使用してデバイスの接続性を監視する](./howto-monitor-devices-azure-cli.md)方法を確認する
