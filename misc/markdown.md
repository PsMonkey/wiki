Coding Style
============
即使簡單如 markdown，還是需要 coding style。[死]

1. 縮排用 tab
1. header
    1. h1, h2 用 Setext-stye（`====` 與 `----`），要跟 header 一樣長，但不得少於四個。
    1. 除了文件第一行之外，其餘 header 的上頭都要空兩行
		1. 例外：如果連續兩個 header，則 header 之間只要空一行
	1. header 下頭要空一行
1. blockquote
    1. 上下都要空兩行
    1. 行首全部都要加上 `>`
	1. 用 blockquote 作諸如「備註」、「警告」的文字區塊，
		則「備註」、「警告」用第六級的 header，例如：
		
		> ###### 標題 ######
		> 內容內容內容......
		
1. horizontal
	1. 上下都要空兩行
	1. 使用 70 個底線
 
		______________________________________________________________________
	
1. link
	1. *建議*使用 reference 方式
	1. reference link 段落上下都要空兩行
1. phrase emphasis 用 `*` 跟 `**` 而不用 `_` 跟 `__`
1. 若要使用延伸語法，以 [Github] 為基準。


[Github]: (https://help.github.com/articles/github-flavored-markdown/)


雜項 memo
---------
* 引言當中要用 header，`>` 跟 `#` 之間只能用一個空格、不能用 tab。
* 如果要在 list 當中用 code block，除了前後要留空行之外，還要比當下的縮排還要再多一個

        （這行有兩個縮排，會是 code block）

  原因是第一階的縮排（如果不是 \* 開頭）還是會被當作是同一個 item 的項目。
* 如果接連兩段引言，基本上 parser 會把他們黏在一起變成一個 blockquote，目前只發現這招管用：

        > block A
        [MOCK LINK]: http://
        > block B