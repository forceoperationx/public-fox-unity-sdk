## (任意)Opt-Out的安裝

有些廣告公司會允許用戶選擇不接受瞄準投放的廣告。在APP啟動時，用戶若在顯示隱私條例和使用規範的視窗中選擇不參加的選項，接收到效果測量通知的同時，F.O.X會把此用戶選擇不參加的資訊，發送給廣告公司。

若要對應Opt-Out，請按照下面，在「Install計測安裝」階段的FoxPlugin.sendConversion之前設定。

```cs
// 如果用戶選擇了Opt-Out需要把setOptout設置為有效
if(user.optout) {
	FoxPlugin.setOptout(true);
}

FoxPlugin.sendConversion("default");
```

---
[TOP](/lang/zh-tw/README.md)
