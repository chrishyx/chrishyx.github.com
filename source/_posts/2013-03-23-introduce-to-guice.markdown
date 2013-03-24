---
layout: post
title: "introduce to guice"
date: 2013-03-23 22:54
comments: true
categories: java
---
很早之前就听说过google开源的IOC框架，轻量级、效率高，却一直没关注过（因为貌似用的人也不多）。这次因为看58同城开源的argo是采用guice的，所以决定先了解一下guice。

先看一个wiki上的例子，main函数是这样的
```java
public static void main(String[] args) {
    Injector injector = Guice.createInjector(new BillingModule());
    RealBillingService billingService = injector.getInstance(RealBillingService.class);
    ...
}
```
Injector是一个提供对bean factory访问的借口。从BillingModule的命名可以看出，Guice建议代码应该是模块化的，配置也应该是模块化的。Guice必然是支持模块化的，所以Guice的接口是
```java
public static Injector createInjector(Module... modules);
```
那么module的实现就应该是Guice指明接口和实现类对应关系的地方了。wiki上例子中的BillingModule是这样的
```java
public class BillingModule extends AbstractModule{
    @Override
    protected void configure(){
        bind(TransactionLog.class).to(DatabaseTransactionLog.class);
        bind(CrediCardProcessor.class)to(PaypalCreditCardProcessor.class);
    }
}
```
Module类在configure方法中指明了接口和实现类的对应。这儿的设计有点借鉴objective c的味道，只是objective c中，bind xxx to xxx是一个函数，这是两个，如果只写bind没有to是没有语法错误的，不知道会不会有语意错？或者可以autowired？另外，这里还有一个in scope的概念，//TODO
业务类的实现
```java
class RealBillingService implements BillingService{
    private final CreditCardProcessor processor;
    private final TransactionLog transactionLog;

    @Inject
    RealBillingService(CreditCardProcessor processor, TransactionLog transactionLog){
        this.processor = processor;
        this.transactionLog = transactionLog;
    }
    ...
}
```
