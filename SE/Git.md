基本概念
========

如果用 `git mv` 移動一個檔案，`git status` 的結果會是

	renamed:	wtf.md -> foo/wtf.md
	
建議這時候就作 commit 動作（也符合「小步前進」原則），
因為如果沒 commit 就改變內容，則 `git status` 會認為是：

	deleted:	wtf.md
	new file:	foo/wtf.md


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
	
