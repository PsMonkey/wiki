SUMMARY.md
==========

一定要有，不然 GitBook 無法 build。

基本上只轉換第一個 unordered list 成左邊的目錄結構，其他完全無視。

如果 list 的第一個 item 不是 link 到 `README.md`，
GitBook 會自動生一個為 `Instruction` 的 item 指向 `README.md`。
所以反過來說，只要第一個 item link 到 `README.md`，
就可以指定要顯示的字串內容。

不是每個 item 都非得是 link（只不過目錄上的字樣會變成灰色）、
或著非得 link 到實際存在的 md（同前，不過有曾經亂 try 之後 GitBook build 失敗）。
