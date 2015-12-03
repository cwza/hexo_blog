title: Tree with DB and JAVA
categories:
  - computer science
  - java
tags: [java, database]
date: 2015-12-03 17:58:46
---

以下轉貼至PTT JAVA討論版csieflyman
備份用
<!-- more -->

-------------------------------------------------------------------------------

這個需求其實跟 LDAP 做的事情一樣

如果想要攤平成一個字串，建議可以使用 JDK 預帶的 LdapName
字串格式長得像這樣 OU=cc,OU=bb,OU=aa,DC=google,DC=com
http://docs.oracle.com/javase/7/docs/api/javax/naming/ldap/LdapName.html
http://docs.oracle.com/javase/7/docs/api/javax/naming/ldap/Rdn.html

你不必自創字串格式 而且LdapName格式是RFC標準 大家都看懂得字串意義
所有的字串prefix suffix add remove 動作都有method幫你處理
嫌不夠的話 還可以用 spring-ldap 的 LdapUtils LdapNameBuilder
http://projects.spring.io/spring-ldap/

至於底層資料儲存方式
如果用資料庫的話，我不太確定你查詢需求
不過可以參考 Interval Tree，它可以一個SQL把所有 subtree node 就找出來
http://en.wikipedia.org/wiki/Interval_tree
每個 node 都有 low hi 二個值
例如 where條件找 2 ~ 7 之間就可以查出 b,c,d
         (1 a 10)
    (2 b 7)   (8 e 9)
(3 c 4) (5 d 6)

如果你有更多的查詢需求，例如往上往下找所有 node，甚至往上或往下找 n 層
那你可以參考以下的實作(Tree也是DAG) 這也是一個SQL 就查出來
http://www.codeproject.com/Articles/22824/A-Model-to-Represent-Directed-Acycli
c-Graphs-DAG-o

如果你的資料量大而且階層多，建議用上述方式實作
否則一層一層爬資料下SQL，效能會變差

如果你不喜歡資料庫 覺得操作起來卡卡的
那你可以用 Apache Jackrabbit，操作資料本身的概念就是以階層式的方式思考
使用 Xpath Query 來查詢
 http://jackrabbit.apache.org/

至於 In Memory Java 操作的話
實作Tree可以使用 javax.swing.tree 裡面的class
http://docs.oracle.com/javase/6/docs/api/javax/swing/tree/TreeNode.html
recursive…等操作都不必自己寫 而且你的類別也不必定義 parent child 欄位

如果你想要用 DAG 可以使用 JGrapht library
http://jgrapht.org/
有神人幫你實作好了 DirectedAcyclicGraph
http://jgrapht.org/javadoc/org/jgrapht/experimental/dag/DirectedAcyclicGraph.h
tml

要注意以上都沒有 thread safe
如果有多個 root node 你可以多建立一個最上層的 dummy root node 串起來


