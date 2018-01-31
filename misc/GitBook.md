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


GLOSSARY.md
===========

在 `GLOSSARY.md` 中撰寫詞彙與詞彙的定義，
若在書中出現該字詞，該字詞就會自動建立 hyper link 到 `GLOSSARY.md`，
滑鼠移上去還會以 `title` 的方式顯示定義。

不過在下列情況中，即使出現該字詞也不會有效果：

* 已經是 hyper link
* 在 `<code>` 或是 `<pre>` 當中

詞彙必須是 `h2`，詞彙的定義是該 h2 的第一個 `p` 也就是第一段文字。
撰寫定義時可以用其他 markdown 語法，
不過 `title` 只能顯示純文字。

詞彙可以包含空格。
如果同時要定義 `WTF` 與 `WTF Foo` 這兩個詞彙，
則 `WTF Foo` 要擺在 `WTF` 前面，否則會認定為只有 `WTF` 這個詞彙而已。

詞彙不分大小寫，`WTF` 跟 `wtf` 視為同一個字。
如果在 `GLOSSARY.md` 當中同時有 `WTF` 跟 `wtf` 這兩個詞彙，
則後面的定義會蓋過前面的定義。
