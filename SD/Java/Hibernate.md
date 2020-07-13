以程式初始化
------------

版本：5+

```Java
StandardServiceRegistry registry = new StandardServiceRegistryBuilder()
	.configure()	//非必須，有掛的話在 classpath 當中就得要有 hibernate.cfg.xml
	.applySetting(AvailableSettings.DRIVER, "org.h2.Driver")
	//apply... apply... apply...
	.build()
;
	
MetadataSources sources = new MetadataSources(registry)
	.addAnnotatedClass(Foo.class)	//等同 cfg.xml <mapping class="PACKAGE.Foo" />
	//目前看不出 Hibernate 有提供「載入指定 package 下所有 class」的方式... ＝＝"
;

SessionFactory sessionFactory  = sources.getMetadataBuilder().build()
	.getSessionFactoryBuilder().build();
```


----------------------------------------------------------------------
Entity 的哏
-----------

資料庫的欄位沒辦法直接 mapping 到 Java class，例如資料庫儲存的是「Y/N」實際上希望對應到 boolean。
目前發現兩招，第一招是掛 `@Basic`，例如

	@Basic
	private String enabled;
	
	public Boolean getEnabled() {
		return "TRUE".equalsIgnoreCase(enabled);
	}

	public void setEnabled(Boolean enabled) {
		this.enabled = enabled ? "TRUE" : "FALSE";
	}

第二招是掛 `@Converter`，還沒試過。
[reference](http://stackoverflow.com/questions/1154833/configure-hibernate-using-jpa-to-store-y-n-for-type-boolean-instead-of-0-1)


如果希望新增資料的時候，某個欄位直接使用 DB 預設值（而不是 entity field 當下的值），
在 `@Column` 裡頭加上 `insertable = false`：

	@Column(insertable = false)
	private Date createDate;


HQL 的哏
--------

Session.createQuery() 裡頭的字串邏輯：

* 不是用 DB 的 table / column name，而是用 Java 的 class / field name
* parameter name 用 `:` 開頭，實際參數不需要 `:`


### Sub Query ###

在 `IN` / `NOT IN` 使用 sub query 的時候，如果這樣寫

	FROM foo WHERE id NOT IN (FROM wtf)
	
`foo.id` 會比對的是 `wtf.id`。如果要作 NOT IN 的是 `wtf.fooId`，要這樣寫

	FROM foo WHERE id NOT IN (SELECT fooId FROM wtf)
	
