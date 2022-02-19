
Class Diagram
=============

分為兩個部份：「class 定義」、「class 間的關係」。

由於 generic type 的定義哏（後敘），
先寫「class 定義」再寫「class 間的關係」比較保險。


class 定義
----------

有兩種方式，這裡只使用 `{}` 的定義法：

```
class C_NAME ~G_TYPE~ {
    <<M_TEXT>>
	F_TYPE F_NAME
	M_NAME(M_ARGS) R_TYPE
}
```

+ `C_NAME`：class 名稱，case sensitive
+ `G_TYPE`：generic type，前後用 `~` 夾起，
	+ 只有 class 第一次出現時要加、也只有第一次出現時加才有用
	+ 多個 type 用 `,` 分隔（單純的字串處理）
	+ 不支援 nested type（generic type 裡頭還有 generic type）
	+ **會** 忽略跟 `C_NAME` 之間的空格
+ `M_TEXT`：class 的 metadata（官方用「annotation」），前後用 `<<` / `>>` 夾起
	+ 可以寫多個，但是只有第一個會顯示
	+ 可以不用寫在 class 定義的第一行
+ `F_NAME` / `M_NAME()`：field / method 的名稱
	+ 每行第一個字串如果有 `()` 就是 method，沒有就是 field
+ `F_TYPE` / `R_TYPE`：field 的 type / method 的 return type
	+ 可以加上 generic type，一樣用 `~` 夾起，
		但是 **不會** 忽略 跟 `*_TYPE` 之間的空格，
		**不會** 跟 diagram 的 class 定義有關聯，
		單純的字串處理。
	+ `R_TYPE` 可不寫
	+ 官方文件寫「`R_TYPE` 跟 `)` 之間要有空格」，實測結果是不用... :roll_eyes:
+ `M_ARGS`：method 的參數，單純的字串處理
+ `F_TYPE`、`M_NAME` 前面可以加上符號表現 visibility，單純的字串處理

	 visibility | 符號
	------------|------
	 public     | + 
	 private    | - 
	 protected  | # 
	 package    | ~ 

+ `F_NAME` / `M_NAME()`後面可以加 `$` 表示為 static
+ `M_NAME()` 後面可以加 `*` 表示為 abstract method


class 間的關係
--------------

完整語法：

```
CLASS_A "CM_TEXT" A_TYPE LINK A_TYPE "CM_TEXT" CLASS_B : L_TEXT
```

+ `CLASS_A` / `CLASS_B`：class 名稱，case sensitive
	+ 如果給沒定義過的 class 就會視同定義一個新的 class
	（下接 generic type 哏）
	+ `CLASS_A` / `CLASS_B` 可以相同
+ `A_TYPE`：端點類型（可以沒有）

	 端點類型 | 符號
	----------|----------
     實心三角 | <| 或 |>
	 實心菱形 | *
	 空心菱形 | o
	 箭頭     | < 或 >
	 
	 + 對，**沒有** 空心三角形
+ `LINK`：線條樣式，限定只能兩個字元

	 端點類型 | 符號
	----------|----------
	 實線     | --
	 虛線     | ..

+ `CM_TEXT` / `L_TEXT`：寫在連接線上的文字（可以沒有），單純的字串處理。
	+ `L_TEXT` 與 `CLASS_B` 中間需有 `:`，文字會顯示在連接線正中間，
	+ `CM_TEXT` 前後用 `"` 夾起，文字會顯示在靠近對應 class 的地方


#### 注意事項 ###

Mermain 只負責畫圖，不會判斷是否符合現實 / UML 規範。
要畫出 `Foo <|--|> WTF` 也是可以的 :dancer:
