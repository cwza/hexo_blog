title: 用JAVA來做Remote Dynamic DNS Update
categories: java
tags: [java, bind9]
date: 2015-12-02 19:36:58
---

## Library
[DNSJAVA](http://www.dnsjava.org/)
[DNSPython](http://www.dnspython.org/)

<!-- more -->
## Dnsjava example
``` xml
<dependency>
	<groupId>dnsjava</groupId>
    <artifactId>dnsjava</artifactId>
    <version>2.1.6</version>
</dependency> 
```
``` java
	@Test
	public void add() throws Exception{
    private String dnsServer = "192.168.30.132";
		Name zone = Name.fromString("abc.com.tw.");
		Name host = Name.fromString("aaa", zone);
		log.info("" + host);
		Update update = new Update(zone);
		update.add(host, Type.CNAME, DClass.IN, 400, "sdp", Name.fromString("cs."));
		log.info("update: " + update);
		
		Resolver res = new SimpleResolver(dnsServer);
		res.setTSIGKey(new TSIG("example.com.","Roa6+m+40vuEHNJbMVTRjg=="));
		res.setTCP(true);

		Message response = res.send(update);
		log.info("message: " + response.toString());
	}
```
``` java
    @Test
	public void delete() throws Exception{
    private String dnsServer = "192.168.30.132";
		Name zone = Name.fromString("abc.com.tw.");
		Name host = Name.fromString("aaa", zone);
		log.info("" + host);
		Update update = new Update(zone);
		update.delete(host, Type.CNAME, DClass.IN, 400, "sdp", Name.fromString("cs."));
		log.info("update: " + update);
		
		Resolver res = new SimpleResolver(dnsServer);
		res.setTSIGKey(new TSIG("example.com.","Roa6+m+40vuEHNJbMVTRjg=="));
		res.setTCP(true);

		Message response = res.send(update);
		log.info("message: " + response.toString());
	}
```
``` java
    @Test
	public void get() throws Exception {
    private String dnsServer = "192.168.30.132";
		Name zone = Name.fromString("abc.com.tw.");
		Name host = Name.fromString("aaa", zone);
		Lookup l = new Lookup(host, Type.CNAME, DClass.IN);
		l.setResolver(new SimpleResolver(dnsServer));
		Record [] records = l.run();
		for (int i = 0; i < records.length; i++) {
			CNAMERecord r = (CNAMERecord) records[i];
			log.info("record: " + r);
		}
	} 
```
