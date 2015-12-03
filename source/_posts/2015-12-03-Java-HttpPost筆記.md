title: Java HttpPost筆記
categories: [computer science, java]
tags: [java]
date: 2015-12-03 15:40:50
---

just a very easy httppost example for java
<!-- more -->
``` java
public class HttpPostTest {
	private Logger log = LoggerFactory.getLogger(this.getClass());
	
	@Test
	public void test() throws Exception {
		HttpClient client = HttpClientBuilder.create().build();
		HttpPost req = new HttpPost("http://posttestserver.com/post.php");
		req.setHeader("User-Agent", "ocsg");
		List<NameValuePair> urlParameters = new ArrayList<NameValuePair>();
		urlParameters.add(new BasicNameValuePair("sid", "C02G8416DRJM"));
		urlParameters.add(new BasicNameValuePair("type", ""));
		urlParameters.add(new BasicNameValuePair("key", "ADDON"));
		urlParameters.add(new BasicNameValuePair("content", "中文"));
	 
		req.setEntity(new UrlEncodedFormEntity(urlParameters, Consts.UTF_8.toString()));
		HttpResponse response = client.execute(req);
		log.info("Response Code : " + response.getStatusLine().getStatusCode());

		HttpEntity entity = response.getEntity();
		String responseString = EntityUtils.toString(entity, "UTF-8");
		log.info("response body: " + responseString);
		
	}
}
```
