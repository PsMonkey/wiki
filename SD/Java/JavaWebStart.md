> 現在大家都會忘記 JWS 是代表 Java Web Start，檔名取 JNLP 又很點點點...

`.jnlp` 檔案會被 cache 起來（jar 檔應該也有），在 client side 的作法是執行 `javaws -uninstall`
或是用 `javaws -viewer` 叫出 GUI 介面操作。
	
看起來比較正規的作法是 `.jnlp` 檔案裡頭寫 `update` element，例如

	<update check="always" policy="always"/>

但是實測結果好像沒啥鳥用... 不確定是少了什麼... ＝＝"

目前最實在的方法是讓 JNLP 中 `<jar>` 的 `version` 值持續遞增，
這樣除了 Chrome 會有重複下載的小問題外，Firefox 跟 IE 貌似都能正常運作。


### 參考資料 ###

1. http://stackoverflow.com/questions/8828643/java-web-start-how-to-clear-cache-or-update-the-app-from-users-perspective
1. http://docs.oracle.com/javase/7/docs/technotes/guides/javaws/developersguide/syntax.html#update