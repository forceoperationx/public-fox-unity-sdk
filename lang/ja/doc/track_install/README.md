[TOP](../../README.md)　>　**インストール計測の詳細**

---

# インストール計測のオプション機能

オプション機能として以下の機能があります。<br>
必要に応じて起動時に実行されるスクリプトから以下のコードが呼ばれるよう実装を行ってください。<br>

### 特定のURLヘ遷移させたい場合
特定のURLヘ遷移させたい場合や、アプリケーションで動的にURLを生成したい場合には、URLの文字列を設定してください。

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  // URLを指定
  option.redirectURL = "http://yourhost.com/yourpage.html";
  Fox.trackInstall(option);
```

### Buid (広告主端末ID)を指定する

`FoxTrackOption`のbuidに広告主端末IDを渡すことができます。<br>例えば、アプリ起動時にSDKがUUIDを生成し、初回起動の成果と紐付けて管理したい場合等に、利用できます。

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  option.redirectURL = "http://yourhost.com/yourpage.html";
  // BUIDを指定
  option.buid = "USER_001"
  Fox.trackInstall(option);
```

<div id="check_track"></div>

### インストール計測が完了したかをチェックする

初回起動時のFox.trackInstallが行われたかの情報をboolで取得することができます。<br>
以下メソッドを用いることでインストール計測が完了し、２回目以降の起動時に特定の処理を行うことが出来るようになります。

```cs
using Cyz;
...
  // 計測済みかどうか
  bool isComplete = Fox.isConversionCompleted();
```


<div id="receive_callback"></div>

### コールバックを受け取る

`FoxTrackOption`の`onTrackComplete`（イベントハンドラ）を用いることで、F.O.X SDKの計測処理が完了したタイミングのコールバックを受け取ることが可能です。

```cs
using Cyz;
...

  FoxTrackOption option = new FoxTrackOption();
  option.redirectURL = "http://yourhost.com/yourpage.html";
  option.buid = "USER_001"
  // イベントハンドラを指定
  option.onTrackComplete += HandleFoxTrackComplete;
  Fox.trackInstall(option);
...

// SDKの起動計測が完了したタイミングに呼ばれます
public void HandleFoxTrackComplete(object sender, EventArgs args) {
		print ("FoxTrack tracked complete!!");
}
```

> ※ バージョン4.1.1以降においてAndroidでコールバックを受け取るの場合、`UnityPlayerActivity`を編集する必要がありますので[こちら](../integration/android/README.md#receive_callback)をご確認ください。

---
[TOP](../../README.md)
