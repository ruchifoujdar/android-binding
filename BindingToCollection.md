# Introduction #

Since v0.2, a new way of binding to collection of objects is introduced. This is through the `ArrayListObservable<T>` class. The signature of ArrayListObservable is as following:

```
public class ArrayListObservable<T> extends ObservableCollection<T> implements Collection<T>
```

Since it is implemented `Collection<T>`, that means you can do most of the normal ArrayList operations on `ArrayListObservable`.

# Usage #

The constructor is pretty much similar to simple observable properties, notice you must pass the class of the generic type to it (just like observables).

```
public ArrayListObservable<String> NameOfList = 
			new ArrayListObservable<String>(String.class);
```

With above declaration, you can do something like:

```
NameOfList.setArray(new String[] { ... });
NameOfList.add("abc");
//etc
```

just like normal ArrayList, and if you bind it to `itemSource` of ListViews or Spinners, the change on the list will be reflected to the view directly.