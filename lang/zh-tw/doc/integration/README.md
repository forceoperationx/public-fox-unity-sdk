## Unity插件導入步驟

### 在項目裡添加Unity插件

1. 啟動Unity、選擇導入插件的Unity項目
2. 選擇菜單的「Assets」>「Import Package」>「Custom Package」
3. 選擇「FOX-UnityPlugin_&lt;version&gt;.unitypackage」
4. 按下「All」按鈕、全部勾選
5. 如果不需要iOS的插件、請去掉「Plugins/iOS」的選項。
6. 如果不需要Android的插件、請去掉「Plugins/Android」的選項。
7. 按下「Import」按鈕。

<img src="./img01.png" width="400px" />

> 如果不想進行F.O.X的Reengagement計測，請不要導入FoxReengagePlugin.h, FoxReengagePlugin.m

#### **在iOS9環境導入的注意點**

> 進行Cookie計測的時候，在iOS9環境裡使用SFSafariViewController。
F.O.X Unity SDK v2.16及以後版本，是靠FoxReengagePlugin做SFSafariViewController啟動後的控制，請務必導入。

> 如果到目前為止都沒有安裝，請導入同捆的FoxReengagePlugin到Unity SDK的unitypackage文件裡。

---
[TOP](/lang/zh-tw/README.md)
