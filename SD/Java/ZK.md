還有一些詰譙成份居多的散記在 [blameZK](https://plus.google.com/collection/wmvsb) 上。


ZUL Coding Style
================
* 使用 `<?import wtf.foo>` 而不是在 `<zscript>` 中寫 `import wtf.foo;`。


Eclipse
=======

測試環境：

* Eclipse Luna
* Maven project

用 Server View 指定外部 Tomcat、然後 project 用 Run on Server 的方法。


用內建 XML Editor 修改 ZUL
--------------------------

1. 開 Preferences
1. 左邊選擇 General / Content Types
	1. 右邊中間選擇 Text / XML (Illformed) 
	1. 新增 *.zul
1. 左邊選擇 General / File Associations
	1. 新增 *.zul
	1. 下方 Associated editors 選擇 XML Editor 後按下「Default」按鈕


其他哏
------

* 用 external editor 修改 ZUL，
	需要要求 Tomcat 重新 publish 才能讀得到，不然就是 Navigator View 重新 refresh。
	簡單地說就是 Eclipse 不知道 ZUL 改了，所以不會叫 Tomcat 更新 ZUL。
	* 另外要注意 Navigator View 當下必須包含該 ZUL（如果有作 zoom in 就會有這個問題），
		不然重新 publish 還是一樣讀到舊的... 
	* 有遇到 publish 也無法更新 ZUL 的狀況，一定要到 Navigator View 作 refresh 才行。
		沒特殊需求還是乖乖用 Eclipse 內建 XML editor 就算了... [死]
* 修改 MVVM 的 ViewModel，不用想了，一定要重新 restart Tomcat


MVVM
====

wire 相關
---------

使用 `@Wire`、`@WireVariable` 必須在 `@Init` 的 method 以後才會有實際值。

**注意**：`Window` 是獨立的 IdSpace，也就是你無法直接 wire `Window` 裡頭的 component，
而必須從 `Window` 開始指進去，例如 `@Wire("#fooWindow #foo")`


觸發 NotifyChange 的其他方法
----------------------------

透過 MVVM 觸發了一個 `@Command` method `doFoo()`，
在 `doFoo()` 中呼叫另一個 `@Command` method `doWTF()`，
這裡必須注意在 `doFoo()` 中呼叫 `doWTF()` 是單純的 Java method call，
跟 MVVM 一點關係也沒有，所以 `doWTF()` 掛的那堆 `@NotifyChange` 完全不會被觸發。

解決方法是自己發 event 給 MVVM 的機制，例如：

	String[] propertyList = {};
	for (String property : propertyList) {
		BindUtils.postNotifyChange(null, null, this, property);
	}
	
另一種解法是 `doFoo()` 必須長這樣

	@Command
	public void doFoo(@ContextParam(ContextType.BINDER) final Binder binder) {
		//......
		
		//doWTF()
		binder.postCommand("doWTF", null);
	}
	
這樣是透過 MVVM 底層機制去觸發 `doWTF()`，相關的 `@NotifyChange` 自然也會觸發了。


Form Binding
------------

假設 `form` 長這樣：

	form="@id('fx') @load(vm.currentData) @save(vm.currentData, before='save')"
	
如果有對 `currentData` 作 notify change，那有 load `fx` 或是 `fxStatus` 也會隨之 notify change。

假設有一個 field 的 binding 對象是用 `ListModelList`，像這樣：

	<combobox model="@load(vm.fooList)" value="@bind(fx.foo)">
		<template name="model">
			<comboitem label="@load(each.name)" />
		</template>
	</combobox>

在 VM 當中作 `fooList.setSelection(Arrays.asList(aFoo))` 只會讓畫面顯示正常，
並不會讓 `fx.foo` 的值變成 `aFoo`，
必須自己作 `form.setField("foo", aFoo)`。


### 動態改變 middle object 的 class ###

如果 middle object 的 class 會在操作當中改變
（你問為什麼會有這種需求？這是一個很長的故事... [淚目]），
這時會炸出一個 exception，炸點在 ZUL 宣告 form binding 的附近，
內容主要是 property binding 錯誤。
目前找到唯一的解法，就是自己實作一個 `MyForm`，內容完全照抄 `org.zkoss.bind.impl.FormImpl`，然後加上：

	public void clear() {
		_saveFieldNames.clear();
		_loadFieldNames.clear();
		_fields.clear();
		_initFields.clear();
		_dirtyFieldNames.clear();
	}


這樣在切換時先呼叫 `clear()` 再 notify change binding 的 instance，
就不會炸了 \囧/
	

雜項
----
如果 bind `ParentVM` 的 component 裡頭有一個 child component 是 bind `ChildVM`，
而 `ParentVM` 跟 `ChildVM` 都有掛 `@Command` 的 method 假設叫做 `wtf()`。
那麼，在 child component 裡頭的 `@command('wtf')` 只會觸發 `ChildVM.wtf()`，不會連帶觸發 `ParentVM.wtf()`

`@load()` 可以用來呼叫 VM 的 method，
例如 `@load(vm.foo(each))` 就是呼叫 `foo()`，然後傳入 `each`。

這樣子作可以取得發出 event 的 component

	@Command
	public void wtf(@ContextParam(ContextType.COMPONENT) Component component) { }

這樣子作可以取得發出 event 的 event

	@Command
	public void wtf(@ContextParam(ContextType.TRIGGER_EVENT) Event event) { }
	
當然，可以指定 child class、也可以兩個同時一起用。


Layout 之謎
===========

在 `listbox` 的 `auxhead` 中，各個 component 要（視覺上合理地）撐滿，
學理上給 `width="100%"` 或是 `hflex="1"`，當然以 render 速度來說應該設 width 會比較快，不過：

* textbox：只能用 `hflex="1"`
* combobox：都可以


Component
=========

Borderlayout
------------

margins 的順序居然是「上、左、右、下」

`setCollapsible()` 的前提條件是 `getTitle` 必須有值。


Cell
----
用了 Cell 會讓 DOM 結構與原本（不用 Cell）的 DOM 結構不一樣 [ref][Cell]。
這就會導致 Grid 原本賦予的 style 掛不上去。
解決方法大概有這幾種：

1. 每個 Row 裡頭 component 統一都用 Cell 包起來。
1. 寫一個 global 的 CSS 對應
1. 不管什麼 deprecated，繼續用 Row.setSpans()

結果似乎最保險的是最後那個不管 deprecated... 真是幹他媽的好 ZK 阿...


[Cell]: http://books.zkoss.org/wiki/ZK%20Component%20Reference/Supplementary/Cell


Listbox
-------

`row` 會蓋掉 `vflex` 的設定。不過如果有觸發 resize，那 `vflex` 的設定還是會再蓋回來。

`auxheader` 才能作 colspan，`listheader` 不行；
要出現 `auxheader` 必須要有 `listhead`（空的也無所謂）。
`auxheader` 設定寬度無效，會依照 `listheader` 的設定跑。

`auxhead` 如果塞 `textbox` 之類的元件，建議下 `hflex="1"` 會比 `width="100%"` 好看。

目前找到畫面載入時預設全選的最簡單方法：

	<listbox id="wtf" onAfterRender="wtf.selectAll()" ... />

`onAfterRender` 的時候，資料已經 ready 了（先不考慮動態載入資料的問題 Zzzz），
所以 `selectAll()` 可以運作。

在 EE 版，一個有設定 `rod=true` 的 Listbox 是不會出現 select all 的 checkbox。
如果不是在 EE 版，設定 `rod=true`（無論在 zk.xml 還是 ZUL）都會被忽略掉，
所以一定會出現 select all 的 checkbox。


### nonselectableTags ###

Listbox 中的某些 **HTML tag** 的 `onClick` 行為，天生就不會連帶觸發 Listbox 的 `onSelect` 行為，
例如 `<button>`、`<input>`、`<textarea>`、`<a>`。
然後透過 `nonselectableTags` 這個 attribute，可以重新決定哪些 **HTML tag** 會 / 不會觸發 `onSelect`。
例如希望圖片跟按鈕都不要觸發 `onSelect`，就要這樣寫

	<listbox nonselectableTags="img, button">
		<!-- 略 -->
	</listbox>
	

`nonselectableTags` 也可以給 `*`，表示不管阿貓阿狗都不會觸發 `onSelect`。
然後建議搭配 `checkmark="true"`

最後，請注意，`nonselectableTags` 接受的值是**幹他媽的 HTML tag**，
不是 ZUL component 名稱，也就是說，你得知道哪些 component 實際上是由什麼 HTML 湊出來的。


Textbox
-------

ZUL 的 `onOK` 是按下 enter 時候會做的事情。

如果設定 `rows/cols`，那麼就會無視 `width/height/hflex/vflex` 的設定，
Textbox 的大小會直接以 `rows/cols` 為準。
所以如果希望 Textbox 是 liquid（大小隨 parent 的大小而變動）
就是設定 `multiline="true"`，然後不要給 `rows/cols`。

Textbox 很容易被 parent 截掉、但是視覺上看不出來（例如在 Grid 當中）。
只能說當 Textbox 是多行狀態（等同於 HTML 的 textarea），
在 Chrome / Firefox 當中，右下角都會出現可以調整大小的 button，
以此作為判斷依據，也可以避免發生「怎麼都沒有 scroll bar」的誤判狀況 Orz。


Window
------
要有設定 `title` 的情況下 `closable="true"` 才會有效果... ＝＝"