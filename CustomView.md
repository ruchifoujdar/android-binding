The MarkupDemo provided a sample implementation of custom view:
http://code.google.com/p/android-binding/source/browse/trunk/AndroidBindingMarkupDemo/src/com/gueei/demos/markupDemo/viewModels/CustomViewWidget.java

## Custom View class ##

All custom view requires to implement the IBindableView interface and implement the function:

```
public ViewAttribute<CustomViewWidget, ?> 
    getViewAttribute(String attributeName)
```

The attributeName is the name appeared in the XML attribute name. If the attribute Name is something you want to handle, the custom view return the attribute; if it is not supported, or you want to let the super class declaration, just return `null`. In other word, if you want to change the default behavior of any attribute (say onClick), you can return your own implementation here.

## View Attribute ##

View Attribute is very similar to property (of any object), but it also handles the observer-observable pattern by itself. In custom view classes, you have to provide the View Attributes which binds to input from xml declaration. In View Attribute, you need to implement two methods:

```
// In case the observed observable changes, newValue is the value of the observable
protected void doSetAttributeValue(Object newValue);

public T get();
```

Each View Attribute must declares a type, take the `PasswordMaskViewAttribute` in Markup Demo as example, its type is CharSequence, so, the `get()` will return CharSequence. Although it is typed CharSequence, Android-Binding still accepts different type of object to bind to the View Attribute. For example, the `text` view attribute of `TextView`s is typed CharSequence, but if you put any other Object to it, it still accepts and the value will be Object.toString().

Because of this behavior, `doSetAttributeValue` takes Object as input, and implementation of ViewAttribute is responsible to type checking and casting.

### Binding Type ###

By default, the binding will be Two way if and only if the type of View Attribute is the same as the binded Observable, two way means, whatever change to the View Attribute will be reflected in Observable and vice versa. Binding will be One way (Only changes in Observable will change the view Attribute, but not backward) if the type of View Attribute is subclass of the binded Observable; there will be `NoBinding` in case it is not subclass.

If you wish, you can override this behavior by overriding:

```
protected BindingType AcceptThisTypeAs(Class<?> type)
```

You can return `BindingType.TwoWay`, `BindingType.OneWay` and `BindingType.NoBinding` in the above method. For example, the `itemSource` of `AdapterView`s accepts type of Adapter, ObservableCollection, CursorObservable as input.

## View Event ##

View Event is a specialized type of View Attribute. In bind to `Command` type of object. For example, `onClick`, `onChildClicked` are View Event attributes. In custom view, such View Event needed to be returned in the getViewAttribute() method as well.