title: C++ Class Memory Layout 筆記
categories: [computer science, c-plus-plus]
tags: [c++]
date: 2015-12-03 16:02:22
---
學生時代筆記，可能有誤？

<!-- more -->
## 不考慮virtual function
``` cpp c++ sample class
class Base {
    public:
        void foo();
    private:
        int i;
        static int si;
}
Base base = Base()
```
``` layout object memory layout
base:
  int i

Out Of base(like global):
  static int si
  void foo(this) //this pointer point to reference object
```

## 考慮繼承與virtual function
``` cpp c++ sample class
class Base {
    public:
        virtual void foo();
    private:
        int i;
}
class Derived : Base {
    public:
        virtual void foo();
    private:
        int j;
}
Base *base = new Derived();
base.foo(); //會呼叫Derived的foo
```
``` layout object memory layout
base:
  Vtable* vtable
  int i

derived:
  Base portion:
    Vtable* vtable
    int i
  int j

vtable:
  function pointer
  function pointer
  ...
```


Compiler在編譯期間會對Base與Derived的vtable塞入其foo的function pointer
Base vtable 塞 Base的foo的function pointer(compiled time)
Derived vtable 塞 Derived的foo的function pointer(compiled time)
當runtime時確定base的type後base.foo會根據derived裡的vtable去找到foo的真正位置

# Reference
[The virtual table](http://www.learncpp.com/cpp-tutorial/125-the-virtual-table/)

