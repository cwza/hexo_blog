title: Java Annotation筆記
categories: java
tags: [java]
date: 2015-12-03 15:31:12
---

<!-- more -->
``` java
public class AnnotationTest {
	private Logger log = LoggerFactory.getLogger(this.getClass());
	
	@Test
	public void test() throws Exception {
		Field f_b = Test1.class.getDeclaredField("b");
		Annotation annotation_b = f_b.getAnnotation(Ignore.class);
		
		Field f_a = Test1.class.getDeclaredField("a");
		Annotation annotation_a = f_a.getAnnotation(Ignore.class);
		log.info("" + annotation_a);
		log.info("" + annotation_b);
	}
}

class Test1 {
	String a;
	@Ignore
	String b;
}
```
