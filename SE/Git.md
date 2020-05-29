基本概念
========

移動、改名要用 `git mv`，不然會被 git 認為是「刪除一個檔案 + 新增一個檔案」。

Eclipse（至少在 Mars）用 Refactor / Rename 修改 Java class，
會用 `git mv` 做，可以安心直接使用。


設定
====

Windows 上設定 Notepad++ 為 commit editor
（在 2.26.x 安裝時可以設定連結的 editor，不用再自己搞了），

	git config --global core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -nosession -noPlugin"


設定 username 跟 email

	git config user.name Foo
	git config user.email foo@wtf.org


gitk 使用 UTF-8 編碼（在 2.26.x 被炸到）

	git config --global gui.encoding UTF-8


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
	git push fooRemote --delete wtfBranch
	

抓 remote 的某個 branch 變成 local 的 branch

	git fetch fooRemote remoteBranch:localBranch


pull / push
===========

`pull` 時不想產生一個 merge commit（也有[寫入設定檔的招式](http://ihower.tw/blog/archives/3843)）

	git pull -r
	git pull --rebase


所有 branch 都 push 上 remote

	git push --all origin


版本推進流程
============

1. pom 檔版號拿掉 `SNAPSHOT`、寫 `Release.md`，commit
1. 掛上版號 tag

		git tag wtf.foo.bar

1. pom 檔版號進版，加上 `SNAPSHOT`，commit
1. 推上 remote repo（所有 tag 都上）

		git push origin --tags


SVN repo 匯入
=============

1. 用 git svn 取出 repo

		git svn clone SVN_URL REPO_NAME
		
1. 用 git log 找出系統產生的 commiter email
	（假設為 `foo <foo@26f1cbcb-2ce9-9940-8e71-79534620cc70>`）
1. 用 git filter-branch 修正

		git filter-branch --commit-filter '
			if [ "$GIT_AUTHOR_EMAIL" = "foo@26f1cbcb-2ce9-9940-8e71-79534620cc70" ];
			then
				GIT_AUTHOR_NAME="FOO";
				GIT_AUTHOR_EMAIL="foo@DontCareAbout.us";
				git commit-tree "$@";
			else
				git commit-tree "$@";
			fi' HEAD
1. 因為不知道怎麼改上面那段程式碼，所以只好有幾個 author 就做幾次 [死]
1. 確定改完後作 git push
