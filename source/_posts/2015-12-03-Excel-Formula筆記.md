title: Excel Formula筆記
categories:
  - computer science
  - windows
tags: [excel]
date: 2015-12-03 17:51:34
---

有點忘記當初這篇是在寫什麼了XD

<!-- more -->
return (A1 + " + B2 + " + , + .......)
=CONCATENATE($A$1,"(",B2,",","'",C2,"'",",","'",D2,"'",",","'",E2,"'",",","'",F2,"'",",","'",G2,"'",",","'",H2,"'",",","'",I2,"'",",","'",J2,"'",")",";")


if K2.contains("Consumer"): 
  return "CSP"
elif K2.contains("Enterprise"):
  return "ESP"
else:
  return "PP"
=IF(ISNUMBER(SEARCH("Consumer",K2)), "CSP", IF(ISNUMBER(SEARCH("Enterprise",K2)),"ESP","PP"))
