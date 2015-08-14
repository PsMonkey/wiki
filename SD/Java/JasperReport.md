Subreport
---------
Subreport 也是對應到一個 .jrxml，
可以作到只切換 / 動態指定整個 report 其中一部分的需求，
提高 report 的重用率、減少維護成本。

在 main report 當中加入 Subreport 之後，
Subreport 的 `Subreport Expression` 指到一個 parameter，假設為 `$P{SubReport}`，
`$P{SubReport}` 的 `Parameter Class` 是 `net.sf.jasperreports.engine.JasperReport`；
Subreport 的 `Data Source Expression` 指到一個 parameter，假設為 `$P{ReportVO}`，
`$P{ReportVO}` 的 `Parameter Class` 要是 data source，個人習慣是用 `JRBeanCollectionDataSource`。

在 Java 當中：

	Map<String, Object> parameters = new HashMap<String, Object>();
	parameters.put(
		"ReportVO",
		new JRBeanCollectionDataSource(getSubreportData())
	);
	parameters.put(
		"SubReport",
		JRLoader.loadObject(
			loader.getResourceAsStream(getJasperFile())
		)
	);

	//把 parameters 丟進 main report 的 JasperFillManager.fillReport()

這樣 Subreport 就會以 `getJasperFile()` 的 .jasper 檔案
搭配 `getSubreportData()` 的內容值產生報表。
只不過不是獨立存在、而是 main report 的一部分。


iReport Editor
--------------

> ###### 注意 ######
> * 這裡是 4.6.0 版的使用經驗
> * 官方在 2016 年就不再維護 iReport，要改用 Jaspersoft Studio

要切換到 Preview 才會產生 .jasper 檔

如果修改 XML（尤其是 `<parameter>` 或是 `<field>`），最好存檔之後關掉重新載入。
千萬不要直接切到 Designer，不然不但 Designer 沒有載入修改後的 XML，
還有可能把之前改的全部復原回去...... Orz