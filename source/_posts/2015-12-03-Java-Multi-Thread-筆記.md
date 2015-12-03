title: Java Multi-Thread 筆記
categories: [computer science, java]
tags: [java]
date: 2015-12-03 15:37:35
---
just a code snippet about multi-thread in java
<!-- more -->
``` java
public class MultiThreadTest {
	@Test
	public void test() throws Exception {
		for(int i = 0; i < 5; ++i) {
			new Thread() {
	            public void run() {
	            	//dosomething;
	            }
	        }.start();
		}
		Thread.sleep((long)10000);
	}
}
```
