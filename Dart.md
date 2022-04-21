# Dart语言中有意思的地方

## var的语法糖和dynamic

var在赋值是才自推到出类型

dynamic是动态生命，运行时检测

## 各类操作符

执行的时候首先是判断 AA 如果为空，就返回 999 ；
之后如果 AA 为空，就为 AA 赋值 999；
之后对 AA 进行整除 999 ，输出结果 10 。

    var AA；
    init(){
        AA??999;
        AA??=999;
        var result = AA ~/99;
        print(result);
        //输出10
    }

## 支持操作符重载

## 方法当参数传递

main(){
    doWhat(String name) {
        print(name);
        return "this $name";
    }

    doNext(int data) {
        print(data);
    }

    doSomething(doWhat, doNext);
}

doSomeThing(String doWhat(String name),void doNext(int data)) {
    var result = doWhat("YUAN");
    print(result);
    doNext(10);
}

## async await/async*yield
在 Dart 中 async await / async* yield 等语法糖，代表 Dart 中的 Future 和 Stream 操作，它们对应 Dart 中的异步逻辑支持。

## Mixins
在 Dart 中支持混入的模式，如下图所示，混入时的基础顺序是从右到左依次执行的，而且和 super 有关，同时 Dart 还支持 mixin 关键字的定义。

## isolate
Dart 中单线程模式中增加了 isolate 提供跨线程的真异步操作，而因为 Dart 中线程不会共享内存，所以也不存在死锁，从而也导致了 isolate 之间数据只能通过 port 的端口方式发送接口，类似于 Scoket 的方式，同时提供了 compute 的封装接口方便调用。

## call
Dart 为了让类可以像函数一样调用，默认都可以实现 call() 方法，同样 typedef 定义的方法也是具备 call() 条件。

## 参考

https://juejin.cn/post/6844903853482049544?utm_source=gold_browser_extension
