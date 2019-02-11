# Observable.create\(\)

```java
Observable<String> source = Observable.create(new ObservableOnSubscribe<String>() {
        @Override
        public void subscribe(ObservableEmitter<String> e) {
            try {
                e.onNext("Alpha");
                e.onNext("Beta");
                e.onNext("Gamma");
                e.onNext("Delta");
                e.onNext("Epsilon");
                e.onComplete();
            } catch (Throwable t) {
                e.onError(t);
            }
        }
    });
source.subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
}, new Consumer<Throwable>() {
    @Override
    public void accept(Throwable throwable) {
        throwable.printStackTrace();
    }
});
```

output:

```
Alpha
Beta
Gamma
Delta
Epsilon
```

---

# Observable.just\(\)

```java
Observable<String> mString = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
mString.map(new Function<String, Character>() {
    @Override
    public Character apply(String s) throws Exception {
        return s.charAt(0);
    }
}).subscribe(System.out::println);
```

output:

```
A
B
G
D
E
```

supplemental:

> ```
> System.out::println 是 Java8 的 method references 使用於 lambda 表示法的內容僅有呼叫現有函示時使用
> https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html
> ```

---

# ConnectableObservable

類似廣播給所有 observers

```java
ConnectableObservable<String> connSource = Observable.just("Alpha-", "Beta-", "Gamma-", "Delta-", "Epsilon-")
                .publish();
connSource.subscribe(System.out::println); // 列印字串
connSource.map(String::length).subscribe(System.out::println); // 取得文字長度後列印
connSource.connect();
```

output:

```
Alpha-
6
Beta-
5
Gamma-
6
Delta-
6
Epsilon-
8
```

---

# Observable.range\(\)

```java
Observable.range(1, 10).subscribe(System.out::println);
```

output:

```
1
2
3
4
5
6
7
8
9
10
```

---

# Observable.interval\(\)

```java
Observable.interval(1, TimeUnit.SECONDS)
        .subscribe(new Consumer<Long>() {
            @Override
            public void accept(Long aLong) throws Exception {
                System.out.println(aLong + "sec(s)");
            }
        });
sleep(5000);
```

```java
public static void sleep(int millis) {
    try {
        Thread.sleep(millis);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

output:

```
0sec(s)
1sec(s)
2sec(s)
3sec(s)
4sec(s)
```

---

# Observable.feature\(\)

---

# Observable.empty\(\)

```
Observable<String> empty = Observable.empty();
empty.subscribe(
    System.out::println,
    Throwable::printStackTrace,
    () -> System.out.println("Done!"));
```

output:

```
Done!
```

---

# Observable.never\(\)

永遠不會傳任何資料，與 empty\(\) 差別就是永遠不會呼叫onComplete\(\)

---

# Observable.error\(\)

直接丟出宣告的錯誤並呼叫 onError\(\)

```java
Observable.error(new Exception("Test Error"))
        .subscribe(new Consumer<Object>() {
            @Override
            public void accept(Object o) throws Exception {
                System.out.println(o.toString());
            }
        }, new Consumer<Throwable>() {
            @Override
            public void accept(Throwable throwable) throws Exception {
                System.out.println(throwable.getMessage());
            }
        });
```

output:

```
Test Error
```

---

# Observable.defer\(\)

每一次的 subscription 都會產生一個新的 Observable，所以 Observable 有變更會被反映出來。

```java
private static int start = 0;
private static int count = 3;

public static void main(String[] args) {
    Observable<Integer> deferData = Observable.defer(new Callable<ObservableSource<? extends Integer>>() {
        @Override
        public ObservableSource<? extends Integer> call() throws Exception {
            return Observable.range(start, count);
        }
    });

    deferData.subscribe(System.out::println);
    count = 5;
    deferData.subscribe(System.out::println);
}
```

output:

```
0
1
2
0
1
2
3
4
```

---

# Observable.fromCallable\(\)

錯誤想要在執行期間被自訂的錯誤處理方法去處理

```java
Observable.fromCallable(new Callable<Object>() {
    @Override
    public Object call() {
        return 1 / 0;
    }
}).subscribe(new Consumer<Object>() {
    @Override
    public void accept(Object o) {
        System.out.println("Received: " + o);
    }
}, new Consumer<Throwable>() {
    @Override
    public void accept(Throwable throwable) {
        System.out.println("Error Captured: " + throwable.getLocalizedMessage());
    }
});
```

output:

```
Error Captured: / by zero
```



