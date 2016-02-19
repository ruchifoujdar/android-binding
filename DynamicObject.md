Dynamic Object is an Observable that contains other Observable objects. It is called Dynamic because it is an object without a fixed type declaration, and it's very similar to Javascript objects. It's notation, is also very close to JSON declaration:

```
    { ATTRIBUTE_NAME1=VALUE1,
      ATTRIBUTE_NAME2=VALUE2, ... }
```

The sequence of attributes is not important. For example, in [ADAPTER](BindingListOfObjects.md) it accepts dynamic object as parameter:

```
    ADAPTER({template=@layout/list_item, source=Demos})
```
```
    ADAPTER({source=Demos, template=@layout/list_item})
```

which the above two versions are yielding the same outcome.