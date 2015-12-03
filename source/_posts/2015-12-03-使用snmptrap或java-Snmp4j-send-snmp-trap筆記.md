title: 使用snmptrap或java Snmp4j send snmp trap筆記
categories: [computer science, java]
tags: [java, linux, snmp]
date: 2015-12-03 15:46:03
---

## use snmptrap to send
``` bash
snmptrap -v 2c -c public 192.168.0.1 "" .1.3.6.1.4.1.89691.100.2.1
.1.3.6.1.4.1.89691.100.1.1 s “API_CATEGORY”
.1.3.6.1.4.1.89691.100.1.2 i 1
.1.3.6.1.4.1.89691.100.1.3 s “Alarm Caused by an exception” 
```
<!-- more -->
## use java Snmp4j to send
``` java
public class Snmp4jTest {
	private Logger log = LoggerFactory.getLogger(this.getClass());
	
	@Test
	public void test() throws Exception {
		// listen
		TransportMapping transport = new DefaultUdpTransportMapping();
		Snmp snmp = new Snmp(transport);
		transport.listen();

		// target
		CommunityTarget target = new CommunityTarget();
		target.setCommunity(new OctetString("public")); // public
		target.setAddress(new UdpAddress("192.168.0.59" + "/" + "162")); //new UdpAddress("172.16.72.49/162")
		target.setRetries(1);
		target.setTimeout(3000);
		target.setVersion(SnmpConstants.version2c);

		// pdu
		PDU pdu = new PDU();
		pdu.setType(PDU.TRAP);
		pdu.add(new VariableBinding(SnmpConstants.snmpTrapOID, new OID(".1.3.6.1.4.1.89691.100.2.1"))); // necessary for TRAP
		pdu.add(new VariableBinding(new OID(".1.3.6.1.4.1.89691.100.1.1"), new OctetString("API_CATEGORY")));
		pdu.add(new VariableBinding(new OID(".1.3.6.1.4.1.89691.100.1.2"), new Integer32(1)));
		pdu.add(new VariableBinding(new OID(".1.3.6.1.4.1.89691.100.1.3"), new OctetString("Alarm")));
		log.debug("pdu: " + pdu.toString());
		log.debug("target: " + target.toString());
		ResponseEvent event = snmp.send(pdu, target);
		log.debug("event: " + event);
	}
}
```
