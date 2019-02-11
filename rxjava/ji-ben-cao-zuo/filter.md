# filter\(\)

對應成 Boolean 值的條件判斷式過濾元素

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .filter(s -> s.length() != 5)
        .subscribe(System.out::println);
```

output:

```
Beta
Epsilon
```



