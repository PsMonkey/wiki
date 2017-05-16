Coding Style
============
即使簡單如 markdown，還是需要 coding style。[死]

1. 縮排用 tab
1. header
	1. h1, h2 用 Setext-stye（`====` 與 `----`），要跟 header 一樣長，但不得少於四個。
	1. 除了文件第一行之外，其餘 header 的上頭都要空兩行
		1. 例外：如果連續兩個 header，則 header 之間只要空一行
		1. 例外：作為 blockquote 的 title
	1. header 下頭要空一行
		1. 例外：作為 blockquote 的 title
1. code block
	1. 上頭空一行、下頭空兩行
1. blockquote
	1. 上頭空一行、下頭空兩行
	1. 行首全部都要加上 `>`
	1. 用 blockquote 作諸如「備註」、「注意」的文字區塊，
		則用第六級的 header（稱之為 title），例如：
		
			> ###### title ######
			> 內容內容內容......
1. list
	1. list 區段結束空兩行
1. horizontal
	1. 上下都要空兩行
	1. 使用 70 個底線
 
			______________________________________________________________________
	
1. link
	1. *建議*使用 reference 方式
	1. reference link 段落上下都要空兩行
1. phrase emphasis 用 `*` 跟 `**` 而不用 `_` 跟 `__`


雜項 memo
---------
* 引言當中要用 header，`>` 跟 `#` 之間只能用一個空格、不能用 tab。
* 如果要在 list 當中用 code block，除了前後要留空行之外，還要比當下的縮排還要再多一個

		（這行有兩個縮排，會是 code block）

  原因是第一階的縮排（如果不是 \* 開頭）還是會被當作是同一個 item 的項目。
* 如果接連兩段引言，基本上 parser 會把他們黏在一起變成一個 blockquote，
	目前發現只有在之間插一個無意義的 reference link 才能分開：

        [MEANINGLESS]: http://
		

GitHub (GFM)
============

GitHub 發展出來的[自有規格][GFM]，基本上跟傳統版 markdown 相容，
但是多了：

* Table
* Task list
* 刪除線
* 自動轉 hyperlink
* 禁止原生 HTML 碼


[GFM]: https://github.github.com/gfm/


裏技？
------

如果在文件的最開頭這樣寫：

	---
	header: context
	標題: 內文
	---


會製造出一個 table：


| header  | 標題 |
| ------- | ---- |
| context | 內文 |