# Force Operation X是什麼

Force Operation X (下面簡稱F.O.X)是基於智慧手機的，用來最大改善廣告效果的綜合解決方案平台。除了對APP下載量和網絡用戶操作的基本計測外，還能基於手機用戶行為特性採用獨自效果計測基準，實現了企業宣傳推廣时費用与效果比的最大改善。

在這個文檔裡，詳細講解了基於智慧手機平台優化廣告效果的F.O.X SDK的導入步驟。

## 目錄

* **[1. 導入](#install_sdk)**
	* [SDK下載](https://github.com/cyber-z/public-fox-unity-sdk/releases)
  * [Unity插件的導入方法](./doc/integration/README.md)
  * [iOS項目的設定](./doc/integration/ios/README.md)
  * [Android項目的設定](./doc/integration/android/README.md)
* **[2. Install計測的安裝](#tracking_install)**
* **[3. LTV計測的安裝](#tracking_ltv)**
	* [sendLtvの詳細](./doc/send_ltv_conversion/README.md)
* **[4. 流量分析的安裝](#tracking_analytics)**
  * [依靠流量分析進行Event計測](./doc/analytics_event/README.md)
* **[5. 進行疏通測試](#integration_test)**
  * [Reengagement計測時的疏通測試](./doc/reengagement_test/README.md)
* **[6. 其他機能的安裝](#other_function)**
  * [Opt-Out的安裝](./doc/optout/README.md)
* **[7. 最後請務必確認](#trouble_shooting)**

## F.O.X SDK是什麼

在APP中導入F.O.X，可以實現如下功能

* **Install計測**

能夠按不同的廣告流入來計測安裝數。

* **LTV計測**

按不同的廣告流入来計測Life Time Value。作為主要的成果地點，有會員登錄，教程突破，消费等。能夠按照不同廣告来監測登錄率，消費率和消費額等。

* **流量分析**

自然流入和廣告流入的APP安裝數比較。能夠計測APP的啟動數，唯一用戶數(DAU/MAU)，持續率等。

<div id="install_sdk"></div>

## 1. 導入

請從下面的頁面來下載最新的安定版(Latest release)SDK。

[SDK發布頁面](https://github.com/cyber-z/public-fox-unity-sdk/releases)

請展開下載的SDK「FOX_UnityPlugin_&lt;version&gt;.zip」，並導入到APP的項目裡。

[Unity插件的導入方法](./doc/integration/README.md)

### 不同OS的設定

* [iOS項目的設定](./doc/integration/ios/README.md)
* [Android項目的設定](./doc/integration/android/README.md)

<div id="tracking_install"></div>

## 2. Install計測的安裝

導入初次啟動的Install計測，就能夠監測廣告效果。請任選下面一個方法來進行安裝。

* 用GUI(Inspector)設定Install計測
* 書寫代碼

### 用GUI(Inspector)設定Install計測的場合

如果在APP啟動時有僅讀取一次的對象，可以使用GUI(Inspector)進行編輯。

例如）利用Main Camera的Inspector進行Install計測

1. 拖拽項目的「Plugins/FoxPlugin.cs」到Main Camera裡。
2. 在Main Camera的Inspector上，指定'default'這個字符串到Fox Plugin腳本的Url變量裡。

### 書寫代碼的場合

不使用GUI(Inspector)，如果使用腳本語言書寫Install計測處理，在啟動時從執行的腳本語言調用FoxPlugin.sendConversion。

在iOS9環境初回啟動時，從瀏覽器啟動到返回APP的時候，會跳出對話框。
在F.O.X SDK裡，從iOS9開始提供新WebView形式，在初回啟動時使用這個新形式的“SFSafariViewController”來計測的話，可以禁止彈出對話框來提高用戶體驗。

```cs
FoxPlugin.sendConversion("default");
```

通常在sendConversion的參數裡像上面那樣輸入"default"。默認是顯示準備好的標準頁面，可以在F.O.X管理畫面裡任意設定跳轉目標頁面的URL。

想要跳轉至特定URL，或者想用APP動態生成URL的時候，請設定URL字符串。

```cs
FoxPlugin.sendConversion("http://yourhost.com/yourpage.html");
```

sendConversion方法的第二個參數可以用來傳遞廣告主終端ID。<br>例如、希望把用户ID和SDK生成的UUID结合起来管理时可以使用这个参数。

```cs
FoxPlugin.sendConversion("default", "your unique id");
```

> ※如果指定了default，會表示標準的簡單的範例頁面，隨後鄙公司會在F.O.X的管理畫面上設定跳轉目的地URL或者HTML頁面。

> ※為了從跳轉目的地頁面回到APP需要URL Scheme，到發布到Market之前請告知鄙公司URL Scheme。

<div id="tracking_ltv"></div>

## 3. LTV計測的安裝

通過在會員登錄，教程突破，消費等任意的成果地點安裝LTV計測，能夠測定不同廣告流入的LTV。如果不做LTV計測，可以省略本項目的安裝。

在成果到達後執行的程式中加入SDK的處理程式。譬如說，會員登錄和APP內消費後的消費計測，是將LTV計測處理記述在登錄、消費處理實行後的回調方法內。請注意根據對象腳本語言(C#,或者JavaScript)，編輯內容會不同。

```cs
FoxPlugin.sendLtv(成果地点 ID);
```

為了進行LTV計測，必須指定識別各成果地點的成果地點ID。請指定到sendLtv的參數發行的ID。

進行消費計測的時候，請仿照下面的例子在完成消費處理的地方指定消費額和貨幣代碼。

```cs
// ...
FoxPlugin.addParameter(FoxPlugin.PARAM_CURRENCY, "USD");
FoxPlugin.addParameter(FoxPlugin.PARAM_PRICE, "20");
FoxPlugin.sendLtv(成果地点 ID);
```

> 如果用Javascript進行編輯、請把文中的「FoxPlugin」換成「FoxPluginJS」。

* [sendLtv的詳細](./doc/send_ltv_conversion/README.md)


<div id="tracking_analytics"></div>

## 4. 流量分析的安裝

自然流入和廣告流入的安裝數比較。能夠計測APP的啟動數，唯一用戶數(DAU/MAU)，持續率等。如果不做流量分析，可以省略本項目的安裝。

如果希望做流量分析計測，請任選下面一個方法來進行安裝。

* 使用Script
* 書寫代碼

### [使用Script]

把「`Plugins/FoxAnalyticsSession.cs`」拖拽到 `Main Camera` 裡。
APP啟動時或者從後台恢復時開始Session計測。

>
如果項目裡存在多個Scene，請在所有的MainCamera裡設定計測地點。在未設置計測地點的Scene顯示狀態從後台恢復時，將無法正確計測。


### [書寫代碼]

在Scene的啟動地點請安裝下面的方法。

```cs
FoxPlugin.sendStartSession();
```

> 如果項目裡存在多個Scene，請在所有的Scene必經處理裡安裝計測地點。

* **依靠流量分析進行消費計測**
希望進行依靠流量分析的Event計測，請參考下面的鏈接。

[依靠流量分析進行Event計測](./doc/analytics_event/README.md)

<div id="integration_test"></div>

## 5.進行疏通測試

在APP上架申請以前，在導入SDK的狀態請做充分的測試，以確保APP的動作沒有問題。

由於在啟動後只發生一次Install計測的通信，如果想要再次進行Install計測的話，請卸載APP再次安裝

**測試步驟**

1. 如果測試用的設備已安裝APP，請先卸載掉APP<br />
1. 清除測試移动终端默认浏览器的Cookie<br />
   ＊若是iOS的話，請按「設定」→「Safari」→「Cookie和數據消除」刪除Cookie<br />
   ＊若是Android的話，請查看默认浏览器的设置。一般點擊某個URL自動彈出的瀏覽器就是默認瀏覽器。<br />
1. 複製鄙司發行的【安装用测试URL】，粘貼到默認瀏覽器（標準瀏覽器）的URL欄裡進行訪問。<br />
＊請在管理畫面（SDK導入→平台的選擇→SDK導入文檔→测试URL→安装用测试URL）裡取得【安装用测试URL】。<br />
＊請一定在OS設定的默認瀏覽器裡粘貼測試URL來發出請求。郵件APP或QR碼讀取APP等這些APP內部會用WebView發生的畫面跳轉是無法計測的。<br />
1. 畫面移轉到Market<br />
＊使用測試URL，可能會因為沒有設定跳轉目的地（沒有在APP詳細裡設定「商城URL」）而彈出錯誤對話框，這個不影響測試。<br />
1. 在測試終端上安裝測試APP<br />
1. 啟動APP<br />
＊如果沒有勾選cookie計測手法，瀏覽器將不會彈跳出來。<br />
＊如果勾選了cookie計測手法，瀏覽器將自動打開，若流覽器無法啟動，說明沒有正常設定。請重新設定，若仍無法發現問題，請與弊司聯繫。<br />
1. 把畫面移轉到LTV計測地點<br />
＊如果登錄了LTV地點執行此步驟<br />
1. 結束並從後台關閉APP<br />
1. 再次啟動APP<br />

請告訴鄙司3，6，7，9的時間。在鄙司這邊會確認是否正常被計測。待確認沒有問題的時候，測試算正式完成。


[Reengagement計測時的疏通測試](./doc/reengagement_test/README.md)

<div id="other_function"></div>

## 6. 其他機能的安裝

* [Opt-Out的安裝](./doc/optout/README.md)

<div id="trouble_shooting"></div>

## 7. 最後請務必確認（到現在發生過的問題集）

### 7.1. 未設定URL Scheme發布的APP引起無法從瀏覽器跳轉到APP

為了進行Cookie計測，在啟動外部瀏覽器以後，要利用URL Scheme跳轉到APP來返回到原來的畫面。
這時有必要設定獨自的URL Scheme，未設定URL Scheme發布的APP將無法正常跳轉。

### 7.2. URL Scheme裡包含了大寫字母，無法正常跳轉回APP

由於環境的不同，可能無法判別URL Scheme裡的大小寫字母，進而引起不能正常跳轉。
因此URL Scheme請全部使用小寫字母來設定。

### 7.3. 由於設定的URL Scheme與其他APP的相同，導致了從瀏覽器跳轉到了其他APP

在iOS裡，如果設定同一個URL Scheme到多個APP，啟動哪個APP是不確定的。因此設定URL Scheme的時候，請使用唯一的有一定複雜度的字符串。

### 7.4. 進行了在短時間獲得大量用戶的宣傳推廣但無法正常計測

在iOS裡，啟動APP時一旦主線程被阻擋超過一定時間，APP獎被強制關閉。啓動時的初期化處理請不要在主線程裡向服務器進行同期通信。像成果報酬型廣告這類的在短時間獲取大量用戶的方式，會產生向服務器的集中訪問，通信響應變得非常差，APP的啟動會花費更長時間，這種狀況下啟動APP會發生強制關閉而無法計測成果的問題。

這種狀況下請按下面的步驟來測試，按照下面的設定確認APP可否正常啟動。

`iOS「設定」→「DEVELOPE」→「NETWORK LINK CONDITIONER」``

* 開啟「Enable」
* 勾選「Very Bad Network」

### 7.5. 用F.O.X計測的Install數值比Google Play Developer Console計測的數值要大

F.O.X使用了多種方式來監測終端的重複安裝。倘若設定了不進行重複監測，在相同終端再安裝時F.O.X會判定為新的安裝。
為了提高重複監測的精度，請進行如下設定。

* [導入Google Play Services SDK來使用廣告ID](./doc/integration/android/google_play_services/README.md)

* [（任意）利用外部存儲設定重複排除](./doc/integration/android/external_storage/README.md)

* [（任意）Android M(6.0) 利用自動備份功能](./doc/integration/android/auto_backup/README.md)

---
[TOP MENU](/README.md)
