

Spring Boot
===========

提供一個 all-in-one 的開發環境（設定），
基本上是下面幾個要點組成，
所以不一定要用 [spring initializr] 產生：

+ pom.xml
	```XML
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>版本號碼</version>
		<relativePath/>
	</parent>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!-- 開發階段用 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>	
		<dependency>
			<!-- 偵測檔案變動自動重起 Tomcat -->
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
	</dependencies>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>	
	```
+ 傳統 Java application class
	```Java
	@SpringBootApplication
	public class SpringApp {
		public static void main(String[] args) {
			SpringApplication.run(SpringApp.class, args);
		}
	}
	```

也不需要 `mvnw`，直接下 `mvn spring-boot:run` 就行。


[spring initializr]: http://start.spring.io