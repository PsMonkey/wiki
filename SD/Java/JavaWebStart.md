> 現在大家都會忘記 JWS 是代表 Java Web Start，檔名取 JNLP 又很點點點...

`.jnlp` 檔案會被 cache 起來（jar 檔應該也有），在 client side 的作法是執行 `javaws -uninstall`
或是用 `javaws -viewer` 叫出 GUI 介面操作。
	
看起來比較正規的作法是 `.jnlp` 檔案裡頭寫 `update` element，例如

	<update check="always" policy="always"/>

但是實測結果好像沒啥鳥用... 不確定是少了什麼... ＝＝"

目前最實在的方法是讓 JNLP 中 `<jar>` 的 `version` 值持續遞增，
這樣除了 Chrome 會有重複下載的小問題外，Firefox 跟 IE 貌似都能正常運作。


Maven
-----
大概也只有 [Webstart Maven Plugin][MWebStart] 這個可以用，以下簡稱 `MWebStart`。
它的文件寫的不是很好，實務上建議直接跳 [JnlpDownloadServlet Example] 這頁。

MWebStart 可以：

* 把 Maven Project 產出的 jar，包進 web 的 Maven Project 當中
* 以先 gen 好的 key 自動 sign jar
* gen `version.xml`


[MWebstart]: http://www.mojohaus.org/webstart/webstart-maven-plugin/
[JnlpDownloadServlet Example]: http://www.mojohaus.org/webstart/webstart-maven-plugin/examples/advanced_jnlp_download_servlet.html


### 失敗 case ###

假設 `Foo` 這個專案是由幾個 Maven Project 組成的：

	/Foo 
		/FooJWS
		/FooModel
		/FooWeb


原本的設計是在 `Foo/pom.xml` 當中掛進 `FooJWS`、`FooModel`、`FooWeb` 的 module，
如此就能確保 `FooWeb` 在 build 之前就先 build `FooJWS`。
但實際上卻會發現 JWS 出現無法順利載入的狀況，
最終發現原因是出在 MWebStart 產生的 `version.xml` 不正確。

假設 `FooWeb` 中設定使用 `FooJWS` 9.7.8 版，`version.xml` 預期是長這樣：

	<?xml version="1.0"?>
	<jnlp-versions>
		<resource>
			<pattern>
				<name>FooJWS.jar</name>
				<version-id>9.7.8</version-id>
			</pattern>
			<file>FooJWS-9.7.8.jar</file>
		</resource>
	</jnlp-versions>


但是實際上產生的 `<file>` 值卻是 `FooJWS.jar`，沒有帶版號
（但是 `FooJWS-9.7.8.jar` 還是會 sign、會複製到 `webstart` 目錄）。
更有趣的是，是對 `Foo` 作 maven install 才會錯，
如果對 `FooWeb` 作 maven install 就會正常...... 
（就是這樣所以 debug 了老半天，還以為是 JNLP Download Servlet 出了啥問題 ＝＝"）

所以，在不知道 MWebStart 用了啥神奇邏輯的情況下，
就是移除 `Foo/pom.xml` 的 `FooJWS`，一切事情就沒事啦......
至於 Jenkins 連動 build 的事情就是另一個故事了 [遠目]


參考資料
---------

1. http://stackoverflow.com/questions/8828643/java-web-start-how-to-clear-cache-or-update-the-app-from-users-perspective
1. http://docs.oracle.com/javase/7/docs/technotes/guides/javaws/developersguide/syntax.html#update