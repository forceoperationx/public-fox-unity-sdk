## Unityプラグイン導入手順

### Unityプラグインのプロジェクトへの追加

1. Unityを起動し、プラグインを組み込むUnityプロジェクトを選択
2. メニューの「Assets」>「Import Package」>「Custom Package」を選択する
3. 「FOX-UnityPlugin_<version>.unitypackage」を選択する
4. 「All」ボタンを押下し、全てにチェックを付ける
5. iOS用プラグインが不要な場合は、`Fox/iOS`と`Plugins/iOS`のチェックを外す
6. Android用プラグインが不要な場合は、`Fox/Android`と`Plugins/Android`のチェックを外す
7. 「Import」ボタンを押下する

<img src="./img01.png" width="400px" />

> ※ `Fox_<version>.unitypackage`には最新のネイティブ版SDK(iOS/Android)が同梱されています。

> ※ iOSでF.O.Xのプッシュ通知を利用しない場合は、FoxNotifyPlugin.h, FoxNotifyPlugin.mをインポートしないでください。また、リエンゲージメント計測を実施しない場合は、FoxReengagePlugin.h, FoxReengagePlugin.mをインポートしないでください。

#### **iOS9における導入の注意点**

> Cookie計測を実施する際に、iOS9ではSFSafariViewControllerを使用します。
F.O.X Unity SDK v2.16以降では、SFSafariViewController起動後の制御をFoxReengagePluginで行うため導入が必須となります。

> これまで外されていた場合には、本Unity SDKのunitypackageファイルに同梱のFoxReengagePluginをご導入ください。


---
[TOPへ](/lang/ja/README.md)
