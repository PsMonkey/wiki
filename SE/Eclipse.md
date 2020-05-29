Eclipse 版本：2020-03

初始環境設定
============

1. import [PsMonkey.epf](Eclipse/PsMonkey.epf)

下面全部都是在改 Preferences 設定

1. General：勾選 Show heap status
	* Appearance / Colors and Fonts
		* Basic / Text Font：改成 [Monaco](Eclipse/MONACO.TTF)
	* Editors / Text Editors：
		勾選 Show whitespace characters（visibility 細節略），
		取消勾選 Show cursor position in the status line / Show selection size in the status line
		* Spelling：關掉
	* Startup and Shutdown
		* 勾選 Refresh workspace on startup
		* Plug-ins activated on startup
			* 取消 Mylyn*
		* Web Browser：Use external web browser
	* Workspace
		* Text file encoding：Other / UTF-8
1. Java
	* Editor
		* Save Actions
			* 勾選 Perform the selected action on save
			* 勾選 Organize imports
			* 勾選 Additional actions，加上 Remove trailing white spaces on all lines
		* Syntax Coloring：Comments 的 Task Tags 改成紅色
	* Installed JREs：換成 JDK
1. Maven / Installations：改用外部 Maven


Eclipse Marketplace
===================

* GWT Eclipse Plugin 3.0.0
