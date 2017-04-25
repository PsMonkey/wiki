變更 repo 名稱
==============

假設帳號 `SOMEONE` 下的 `WTF`（https://github.com/SOMEONE/WTF.git`）
要改名為 `Foo`（https://github.com/SOMEONE/Foo.git）。

最重要的是 client 的 remote 設定值要改，
假設之前的 remote name 是 `origin`，git 指令會是

	git remote set-url origin https://github.com/SOMEONE/Foo.git


如果原本有其他 repo 跟 `SOMEONE/WTF` 有關（無論誰 fork 誰），
它們不改名字 or 更著改名字都不會影響，最多就是 PR 的時候看起來很怪... XD
