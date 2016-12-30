> # pixi.js #


* 官網：http://www.pixijs.com/
* repo：https://github.com/pixijs/pixi.js
* API：http://pixijs.github.io/docs/
* ~~比文件好用的~~ Example：http://pixijs.github.io/examples


PIXI
----

### autoDetectRenderer() ###

`options` 還可以有這些 field：

* backgroundColor: 0xFFFFFF


### loader.add() ###

第一個參數可以直接給 object，field 如下：

* url
* name / key：如果沒給的話就會用 url 的值
* onComplete / callback：load 完會觸發的 function


DisplayObject
-------------

`foo.interactive = true` 之後，`foo` 就可以發出 event，處理方式。

```JS
foo.click = wtfFunc;
foo.on("mouseup", wtfFunc)

function wtfFunc(event) {
	//event 是 pixi 重新包裝過的 `InteractionData`
}
```

其實起點都是 `InteractionManager.prototype.processMouseUp()`，行為好像也差不多，
只不過第二個 `EventEmitter` 的行為...... Zzz


瀏覽器差異
==========

圖檔無法顯示
------------

* 4.3.0
* Chrome / App WebViewer @ Android

如果要顯示的圖檔尺寸過大（不確定實際數字，4200 * 4200 確定炸、4000 * 4000 確定不會），
在 Android 上的 Chrome / App WebViewer 就會無法載入圖檔。

Desktop 的 Chrome 基本上沒有這個問題。
