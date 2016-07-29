記憶體
======

* 初始 heap 大小（Xms）：實體記憶體的 1/64。
* 最大 heap 大小（Xmx）：實體記憶體的 1/4。

資料來源： http://docs.oracle.com/javase/6/docs/technotes/guides/vm/gc-ergonomics.html

顯示目前設定值（Windows）

	java -XX:+PrintFlagsFinal -version | findstr HeapSize
