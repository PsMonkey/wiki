設定
====

設定 Notepad++ 為 commit editor（@Windows）

	git config --global core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -nosession -noPlugin"

設定 username 跟 email

	git config user.name Foo
	git config user.email foo@wtf.org


branch 相關
===========

開啟一個完全沒有 parent 的 branch（在這裡是 `gh-pages`）

	git checkout --orphan gh-pages


pull / push
===========
`pull` 時不想產生一個 merge commit（也有[寫入設定檔的招式](http://ihower.tw/blog/archives/3843)）

	git pull --rebase
		
所有 branch 都 push 上 remote

	git push --all origin
	
