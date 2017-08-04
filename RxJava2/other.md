# Single Completable Subject/Processor

## Single

不同于RxJava1.x中的SingleSubscriber,RxJava2中的SingleObserver多了一个回调方法onSubscribe。

```
interface SingleObserver<T> {
    void onSubscribe(Disposable d);
    void onSuccess(T value);
    void onError(Throwable error);
}
```

## Completable

同Single，Completable也被重新设计为Reactive-Streams架构，RxJava1.x的CompletableSubscriber改为CompletableObserver，源码如下：

```
interface CompletableObserver<T> {
    void onSubscribe(Disposable d);
    void onComplete();
    void onError(Throwable error);
}
```

## Subject/Processor

Processor和Subject的作用是相同的。关于Subject部分，RxJava1.x与RxJava2.x在用法上没有显著区别，这里就不介绍了。其中Processor是RxJava2.x新增的，继承自Flowable,所以支持背压控制。而Subject则不支持背压控制。使用如下：

```
        //Subject
        AsyncSubject<String> subject = AsyncSubject.create();
        subject.subscribe(o -> Log.d("JG",o));//three
        subject.onNext("one");
        subject.onNext("two");
        subject.onNext("three");
        subject.onComplete();
       //Processor
        AsyncProcessor<String> processor = AsyncProcessor.create();
        processor.subscribe(o -> Log.d("JG",o)); //three
        processor.onNext("one");
        processor.onNext("two");
        processor.onNext("three");
        processor.onComplete();
```

## Maybe
2.X新增，行为类似Observable，可能会有一个数据或一个错误，也可能什么都没有。可以将其视为一种返回可空值的方法。这种方法如果不抛出异常的话，将总是会返回一些东西，但是返回值可能为空，也可能不为空。按照Reactive Streams规范设计，遵循协议onSubscribe (onSuccess/onError/onComplete)

```
Maybe.just(1)
.map(v -> v + 1)
.filter(v -> v == 1)
.defaultIfEmpty(2)
.test()
.assertResult(2);
```



