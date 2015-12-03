title: Java使用自簽證書訪問https網站筆記
categories: [computer science, java]
tags: [java, https, ssl]
date: 2015-12-03 16:36:44
---

1. 用firefox瀏覽器取得https網站的server crt，並存成local.crt
```
google firefox get ssl cert
```
2. 利用keytool產生java client端要用的client keysotre
``` bash
keytool -import -alias ocsg.keystore -keystore ocsg.keystore -file localhost.crt
 #建立的同時要輸入密碼，密碼會在下一步用到
```
<!-- more -->
3. Java程式建立ttps connection時將ocsg.keystore讀入
``` java java-https-client-code
@Test
public void test3() throws Exception {
        X509TrustManager sunJSSEX509TrustManager;
        // 加载 Keytool 生成的證書文件
        char[] passphrase;
        String p = "ocsgks"; //keystore 密碼
        passphrase = p.toCharArray();
        File file = new File("/ocsg.keystore"); //keystore檔案位置
        System.out.println("Loading KeyStore " + file + "...");
        InputStream in = new FileInputStream(file);
        KeyStore ks = KeyStore.getInstance(KeyStore.getDefaultType());
        ks.load(in, passphrase);
        in.close();
        // 建立 javax.net.ssl.TrustManager
        TrustManagerFactory tmf = TrustManagerFactory.getInstance("SunX509", "SunJSSE");
        tmf.init(ks);
        TrustManager tms [] = tmf.getTrustManagers();
        // 使用TrustManager 訪問https網站
        SSLContext sslContext = SSLContext.getInstance("SSL", "SunJSSE");
        sslContext.init(null, tms, new java.security.SecureRandom());
        SSLSocketFactory ssf = sslContext.getSocketFactory();
        URL myURL = new URL("https://localhost:9191/ocsg-cht/"); //https網站url
        HttpsURLConnection httpsConn = (HttpsURLConnection) myURL.openConnection();
        httpsConn.setSSLSocketFactory(ssf);
        InputStreamReader insr = new InputStreamReader(httpsConn.getInputStream());
        int respInt = insr.read();
        while (respInt != -1) {
            System.out.print((char) respInt);
            respInt = insr.read();
        }
	}
```
