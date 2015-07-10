XML→Java
=========

還沒有測試過用 XSD 產生 entity 檔案，以下是手動寫 entity 的心得。

class 掛 `@XmlRootElement`，如果 class name 跟 XML root name 不一樣，加 `name = foo` 來指定。

field 掛 `@XmlElement`，如果 field name 跟 XML tag name 不一樣，加 `name = foo` 來指定。
如果該 field 有 setter，field 就不能掛 `@XmlElement`、setter 也不用掛。
如果 setter 名字跟 tag name 不一樣的解決招數相同。

如果 field 的 data type 需要參考到 `Foo` 這個 class，則需要用 `@XmlSeeAlso`：

	@XmlSeeAlso(Foo.class)
	public class Wtf {
		private Foo foo;
		public Foo getFoo() {}
		public void setFoo(Foo foo) {}
	}
	
當然，`Foo` 必須有掛 `@XmlRootElement`。

field / setter 如果掛 `@XmlAttribute`，則視為 root element 的 attribute。
（所以看起來如果 child element 有 attribute，就只能用 `@XmlSeeAlso` 的方法另外開一個 class）
** TODO Unmarshaller 還沒測試**


### 例外狀況 ###

目前測試的結果，只有 `@XmlRootElement` 的設定值跟 XML 對不上才會炸 exception，
如果 root 相同，其他 field 都對不上也不會怎樣，頂多值都是 null。
