# Single

單一傳送

```java
 Single.just("Hello").map(String::length)
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                System.out.println(integer); 
            }}, 
            new Consumer<Throwable>() {
                @Override
                public void accept(Throwable throwable) throws Exception {
                    throwable.printStackTrace(); 
                }
            });
```

output:

```
5
```

## 與 Observable.first\(\) 不同

Observable.first\(\) 可以指定預設值

```java
Observable<String> source2 = Observable.empty();
source2.first("Nil").subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) throws Exception {
        System.out.println(s);
    }
});
```

output:

```
Nil
```



