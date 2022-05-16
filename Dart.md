# Dart语言中有意思的地方

## Hello world

    void main() {
        print('Hello, World!');
    }

## 变量
Dart 是类型安全语言，支持类型推断，因此大多数变量不需要显式指定类型

    var name = 'This is a var';
    var year = 2022;
    var cost = 100.2;
    var fruits = ['apple', 'orange', 'mango', 'banana'];
    var image = {
        'tags':'color',
        'url':'www.badu.com'
    }

## 控制流程

    if (year >= 2001) {
        print('21st century');
    } else if (year >= 1901) {
        print('20th century');
    }

    for (final object in flybyObjects) {
        print(object);
    }

    for (int month = 1; month <= 12; month++) {
        print(month);
    }

    while (year < 2016) {
        year += 1;
    }

## 函数

    int fibonacci(int n) {
    if (n == 0 || n == 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
    }

    var result = fibonacci(20);

## 注释

/// 这是一个文档注释。
/// 文档注释用于为库、类以及类的成员添加注释。
/// 像 IDE 和 dartdoc 这样的工具可以专门处理文档注释。

/* 也可以像这样使用单斜杠和星号的注释方式 */

## 导入 Import

使用 import 关键字来访问在其它库中定义的 API。

// Importing core libraries
import 'dart:math';

// Importing libraries from external packages
import 'package:test/test.dart';

// Importing files
import 'path/to/my_other_file.dart';


## 类 Class

    class Spacecraft {
    String name;
    DateTime? launchDate;

    // Read-only non-final property
    int? get launchYear => launchDate?.year;

    // Constructor, with syntactic sugar for assignment to members.
    Spacecraft(this.name, this.launchDate) {
        // Initialization code goes here.
    }

    // Named constructor that forwards to the default one.
    Spacecraft.unlaunched(String name) : this(name, null);

    // Method.
    void describe() {
        print('Spacecraft: $name');
        // Type promotion doesn't work on getters.
        var launchDate = this.launchDate;
        if (launchDate != null) {
        int years = DateTime.now().difference(launchDate).inDays ~/ 365;
        print('Launched: $launchYear ($years years ago)');
        } else {
        print('Unlaunched');
        }
    }
    }

使用：

    var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
    voyager.describe();

    var voyager3 = Spacecraft.unlaunched('Voyager III');
    voyager3.describe();

## 继承

Dart支持单继承

    class Orbiter extends Spacecraft {
    double altitude;

    Orbiter(String name, DateTime launchDate, this.altitude)
        : super(name, launchDate);
    }

## Mixins

Mixin 是一种在多个类层次结构中重用代码的方法。下面的是声明一个 Mixin 的做法：

    mixin Piloted {
    int astronauts = 1;

    void describeCrew() {
        print('Number of astronauts: $astronauts');
    }
    }

现在你只需使用 Mixin 的方式继承这个类就可将该类中的功能添加给其它类。


    class PilotedCraft extends Spacecraft with Piloted {
    // ···
    }

## 接口和抽象类

Dart 没有 interface 关键字。相反，所有的类都隐式定义了一个接口。因此，任意类都可以作为接口被实现。

    class MockSpaceship implements Spacecraft {
    // ···
    }

你可以创建一个被任意具体类扩展（或实现）的抽象类。抽象类可以包含抽象方法（不含方法体的方法）。

    abstract class Describable {
    void describe();

    void describeWithEmphasis() {
        print('=========');
        describe();
        print('=========');
    }
    }

任意一个扩展了 Describable 的类都拥有 describeWithEmphasis() 方法，这个方法又会去调用实现类中实现的 describe() 方法。

## 异步

使用 async 和 await 关键字可以让你避免回调地狱（Callback Hell）并使你的代码更具可读性。

    const oneSecond = Duration(seconds: 1);
    // ···
    Future<void> printWithDelay(String message) async {
    await Future.delayed(oneSecond);
    print(message);
    }


    Future<void> createDescriptions(Iterable<String> objects) async {
    for (final object in objects) {
        try {
        var file = File('$object.txt');
        if (await file.exists()) {
            var modified = await file.lastModified();
            print(
                'File for $object already exists. It was modified on $modified.');
            continue;
        }
        await file.create();
        await file.writeAsString('Start describing $object in this file.');
        } on IOException catch (e) {
        print('Cannot create description for $object: $e');
        }
    }
    }

## 异常

使用 throw 关键字抛出一个异常：

    if (astronauts == 0) {
    throw StateError('No astronauts.');
    }

使用 try 语句配合 on 或 catch（两者也可同时使用）关键字来捕获一个异常:

    try {
    for (final object in flybyObjects) {
        var description = await File('$object.txt').readAsString();
        print(description);
    }
    } on IOException catch (e) {
    print('Could not describe object: $e');
    } finally {
    flybyObjects.clear();
    }


## 空安全
什么是空安全？空安全有什么用？解决了什么问题？

## Dart 集成操作
list map set

## 箭头语法

=> 这种箭头语法是一种定义函数的方法，该函数将其右边执行的表达式返回给左边

例如 

bool hasEmpty = aListOfString.any((s){
    return s.isEmpty;
});

可以直接写成更简单的代码：

bool hasEmypty = aListOfString.any((s) => s.isEmpty);

## var的语法糖和dynamic

var在赋值时才自推到出类型

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
