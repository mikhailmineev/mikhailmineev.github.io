 Standard generic types
 
 It is possible to put Object into List<Number>, but you shouldn't do that to prevent ClassCastException and compiler will try to stop you.
  
 Let's see how simple generic type works
 
 ```java
   {
        List<Number> numberList = new ArrayList<>();
        List<Integer> integerList = new ArrayList<>();
        List<Object> objectList = new ArrayList<>();
        all(numberList);
        all(integerList);
        all(objectList); // error
    }

    public void all(List<Number> l) {
        // normal OOP, I can only and subtypes of number not to break my list
        l.add((Number) 1);
        l.add(1);
        l.add(new Object()); // error
        l.add(null);

        // also normal OOP, I can only be sure, that get list with Numbers
        Integer a = l.get(1); // error
        Number b = l.get(1);
        Object c = l.get(1);
    }
```

Upper bound generic

```java
    {
        List<Number> numberList = new ArrayList<>();
        List<Integer> integerList = new ArrayList<>();
        List<Object> objectList = new ArrayList<>();
        a_whatewer_method(numberList);
        a_whatewer_method(integerList); // error
        a_whatewer_method(objectList);
    }

    public void a_whatewer_method(List<? super Number> l) {
        // can add only subtypes of Number because outside the strongest type can be Number
        // if have List<Number> and add Object into it, lager list.get can return Object, which is bad
        l.add((Number) 1);
        l.add(1);
        l.add(new Object()); // error
        l.add(null);

        // can add only super root type because it can be passed
        Integer a = l.get(1); // error
        Number b = l.get(1); // error
        Object c = l.get(1);
    }
```

Looks like it can only consume data

Lower bound generic

```java
    {
        List<Number> numberList = new ArrayList<>();
        List<Integer> integerList = new ArrayList<>();
        List<Object> objectList = new ArrayList<>();
        another_whaterwer_method(numberList);
        another_whaterwer_method(integerList);
        another_whaterwer_method(objectList); // error
    }

    public void another_whaterwer_method(List<? extends Number> l) {
        // can add only null otherwise can break list by adding unexpected type
        // if have List<Number> and add Object into it, lager list.get can return Object, which is bad
        l.add((Number) 1); // error
        l.add(1); // error
        l.add(new Object()); // error
        l.add(null);

        // normal OOP, I can only be sure, that get list with Numbers
        Integer a = l.get(1); // error
        Number b = l.get(1);
        Object c = l.get(1);
    }
```

Looks like it can only produce data

So, we have producer extends and consumer super whith results in PECS rule
If we need both to produce and consume data of a method parameter, we can't use extends or super
