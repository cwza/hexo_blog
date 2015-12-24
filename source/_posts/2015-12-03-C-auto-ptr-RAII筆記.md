title: 'C++ auto_ptr & RAII筆記'
categories: c-plus-plus
tags: [c++]
date: 2015-12-03 15:24:15
---
學生時代筆記，可能有誤？

## Resource Acquisition Is Initialization
C++中利用物件離開scope即會自動呼叫destructor的特性(stack)，使資源類型物件在使用完或有exception跳出時能自動將資源釋放
Java中常見的try catch block中finally{close資源}的寫法在C++中就不需要了
若是必須使用pointer而非物件時，可以在資源物件中宣告該pointer，實際使用還是用資源物件即可
<!-- more -->
``` cpp
  class AutoFile {
    AutoFile(String path) {
    File *file = open(path);
    }
    ~AutoFile() {
    file.close()
    }
  }
  private foo() {
    //即使不呼叫close()釋放file，當程式離開foo()時自然會呼叫
    AutoFile file = AutoFile("/temp/test.txt");
    //dosomething
  }
```
## C++ auto_ptr
利用RAII包出來的pointer，實際上仍是個物件，利用operator overloading實作pointer的oprator，藉以模擬pointer行為
由於仍然是個物件，並會在destructor時執行delete *ptr，所以能做到不需手動呼叫delete的功能
需注意的是因destructor呼叫的一律是delete *ptr，所以陣列無法使用auto_ptr(需要delete []*)
``` cpp
     private foo() {
          Auto_ptr<Object> a = new Object();
          cout<<a->output()
          //用完不需delete他，當成是離開foo()即會自動呼叫auto_ptr中destructor裡面的delete
     }
```
