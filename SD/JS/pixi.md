> # pixi.js #


* 官網：http://www.pixijs.com/
* repo：https://github.com/pixijs/pixi.js
* API：http://pixijs.github.io/docs/
* ~~比文件好用的~~ Example：http://pixijs.github.io/examples


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

