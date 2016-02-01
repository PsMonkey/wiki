初始環境設定
============

適用 Eclipse：

* Mars.1 EE 版
* Luna EE 版（未嚴格測試）

步驟：

1. import [PsMonkey.epf](Eclipse/PsMonkey.epf)
1. 下面全部都是在改 Preferences 設定
1. General：勾選 Show heap status
	* Appearance / Colors and Fonts
		* Basic / Text Font：改成 [Monaco](Eclipse/MONACO.TTF)
	* Editors / Text Editors：勾選 Show whitespace characters（visibility 細節略）
		* Spelling：關掉
	* Startup and Shutdown
		* 勾選 Refresh workspace on startup
		* 取消 Confirm exit when closing last window
		* Plug-ins activated on startup
			* 取消 Mylyn*
		* Web Browser：Use external web browser
	* Workspace
		* Text file encoding：Other / UTF-8
1. Java
	* Editor
		* Folding：勾選 Inner types
		* Save Actions
			* 勾選 Perform the selected action on save
			* 勾選 Organize imports
			* 勾選 Additional actions，加上 Remove trailing white spaces on all lines
		* Syntax Coloring：Comments 的 Task Tags 改成紅色
	* Installed JREs：換成 JDK
1. Maven / Installations：改用外部 Maven
1. Web（到底哪個白痴讓 default 值用 Big5 的... ＝＝"）
	* CSS Files：Encoding UTF-8
	* HTML Files：Encoding UTF-8
	* JSP Files：Encoding UTF-8


安裝 GPE
--------

不裝 AppEngine、Android 相關
