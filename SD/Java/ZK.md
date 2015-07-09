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


雜項
----
如果 bind `ParentVM` 的 component 裡頭有一個 child component 是 bind `ChildVM`，
而 `ParentVM` 跟 `ChildVM` 都有掛 `@Command` 的 method 假設叫做 `wtf()`。
那麼，在 child component 裡頭的 `@command('wtf')` 只會觸發 `ChildVM.wtf()`，不會連帶觸發 `ParentVM.wtf()`


Component
=========

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



Borderlayout
------------

margins 的順序居然是「上、左、右、下」

`setCollapsible()` 的前提條件是 `getTitle` 必須有值。


Textbox
-------

ZUL 的 `onOK` 是按下 enter 時候會做的事情。