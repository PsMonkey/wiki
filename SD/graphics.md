> # 圖學 #
> 應該是啦... 不是就算了 [毆]


transform matrix
================

參考資料：https://www.siggraph.org/education/materials/HyperGraph/modeling/mod_tran/2dconc.htm

* Scale：`Sx` 為 x 軸倍率，`Sy` 為 y 軸倍率

		| Sx    0   0 |
		|  0   Sy   0 |
		|  0    0   0 |

* Rotate：`a` 為旋轉弳度

		|  cos(a)   sin(a)   0 |
		| -sin(a)   cos(a)   0 |
		|       0        0   1 |
		
* Translate（移動）：`Tx` 為 x 軸移動量，`Ty` 為 y 軸移動量

		|  1    0   0 |
		|  0    1   0 |
		| Tx   Ty   1 |
		

組合技
------

如果需要一次（依序）做多個 transform，就（依序）對多個 transform matrix 做矩陣乘法。
注意：矩陣乘法沒有交換律。

* 原地選轉：

		|                       cos(a)                         sin(a)   0 |
		|                      -sin(a)                         cos(a)   0 |
		| Tx(1 - cos(a)) + Ty * sin(a)   Ty(1 - cos(a)) - Tx * sin(a)   1 |
		
	1. 移動到原點
	1. 旋轉
	1. 移動回原始位置

