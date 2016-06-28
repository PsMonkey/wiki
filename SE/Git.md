基本概念
========

移動、改名要用 `git mv`，不然會被 git 認為是「刪除一個檔案 + 新增一個檔案」。

Eclipse（至少在 Mars）用 Refactor / Rename 修改 Java class，
會用 `git mv` 做，可以安心直接使用。


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

刪除 branch

	git branch -d wtfBranch
	
強制刪除 branch（沒有 merge 的話就需要）

	git branch -D wtfBranch
	
刪除 remote 上的 branch

	git push fooRemote :wtfBranch


pull / push
===========
`pull` 時不想產生一個 merge commit（也有[寫入設定檔的招式](http://ihower.tw/blog/archives/3843)）

	git pull --rebase
		
所有 branch 都 push 上 remote

	git push --all origin
	
