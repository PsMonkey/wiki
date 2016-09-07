陣列
----

* `get_object_vars($object)` 可以把一個 object 變成一個 key-value 格式的 array。
* `array_values($array)` 可以把 key-value 格式的 array 的 value 變成一個 array。


JSON
----

* `json_decode($foo)` 會回傳 Object，改成用 `json_decode($foo, true)` 就會回傳 key-value 格式的 array。
* `json_decode()` 在 32bit 的機器上，處理夠大的 long 值會出現流失精準度的問題，解法是換成字串（WTF）
 
		function json_decode_suck($json) {
			return json_decode(
				preg_replace(
					'/("\w+"):(\d+)/', '\\1:"\\2"', 
					$json
				)
			);
		}

	不確定在 64bit 的機器上是不是絕對不會出現這問題。
	參考資料：[StackOverflow](http://stackoverflow.com/questions/1777382/php-json-decode-on-a-32bit-server)


PDO
----

* `PDOStatement::execute($array)` 的 array 長度，必須與 SQL 指令中用到的 parameter 數量相同，
不然會炸 error。