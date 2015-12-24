title: 使用Spring Profile來簡化多開發環境的properties設定
categories: java
tags: [java, spring]
date: 2015-12-02 19:55:37
---

Spring要用3.1以上版本
``` xml applicationContex.xml
<beans>
  <beans profile="LOCAL">
    <context:property-placeholder location="classpath*:META-INF/spring/local/*.properties" />
  </beans>
  <beans profile="PRODUCTION">
    <context:property-placeholder location="classpath*:META-INF/spring/production/*.properties" />
  </beans>
</beans>
```
<!-- more -->
只要在applicationContex.xml設定上面
就可以讓spring在runtime時依據環境變數來決定要用哪些properties檔案
-Dspring.profiles.active="LOCAL" or -Dspring.profiles.active="PRODUCTION"
在Test class中也可以使用@ActiveProfiles("LOCAL")來直接指定profile
@ActiveProfiles("LOCAL")

以上面的例子來看
-Dspring.profiles.active="LOCAL"時
spring會使用resources/META-INF/spring/local/底下的.properties檔
-Dspring.profiles.active="PRODUCTION"時
spring會使用resources/META-INF/spring/production/底下的.properties檔

profile的用法其實並不局限於properties檔
也可在profile底下設定各個profile獨立使用的bean
這邊就不詳述了




