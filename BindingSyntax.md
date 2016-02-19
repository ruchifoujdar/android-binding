# Introduction #

Currently, all binding is defined in XML namespace: `http://www.gueei.com/android-binding/`, so, a typical way is include this namespace in each layout file, alongside with the android one:

```
<LinearLayout xmlns:android="http://...."
  xmlns:binding="http://www.gueei.com/android-binding/"
..>
```

Binding is by default, to the **public observable** fields of the supplied ViewModel. For example:

```
    <TextView binding:text="FirstName" ...
```

Means the text of that TextView is binded to an Observable named `FirstName`. Note that the naming is **Case Sensitive**.

# Binding Types #

| Type | Meaning |
|:-----|:--------|
| One-way | Change on value will be reflected in User Interface, but not returned to Model |
| Two-way | Same as One-way, but changes in UI also reflected in Model |
| One-shot | Only one-time value-assignment from model to UI, UI will not acknowledged to further changes |

Not all attribute of Views will support all binding modes. For example, the `clickedItem` of AdapterViews is a read-only attribute, that the assignment of value to clickedItem will be ignored by the UI.

# Syntaxes #

| Syntax | Example | Effect | type | Supported |
|:-------|:--------|:-------|:-----|:----------|
| '_string_' |         | String constant | one-shot | yes       |
| _number_ | 123     | Integer constant | one-shot | 0.22      |
| @_id_  | @layout/main | [Read Separate Document](ResourceLinking.md) | one-shot | 0.3       |
| .      | binding:text="." | binding to Model itself | one-shot | 0.17      |
| _Converter_(_Observable_, ...) | binding:visibility="NOT(EditMode)", binding:text="CONCAT(A,B,C...)" | Perform the Converter Operation | One-way/two-way (depending on Converter) | from 0.17 |
| com.example.package.CustomConverter |         | Custom converter must use full path of package | depends | 0.17      |
| _Converter_('String') | binding:text="CONCAT('abc', _Observable_)"  | _results_ abc\_ValueOfObservable|      | 0.17      |
| {_argName1_=_argVal1_, _argName2_=_argVal2_ ... } | {template=@layout/main, source=Demos} | [Dynamic Object](DynamicObject.md) | one-shot | v0.2      |
| =ID.ViewAttribute | =id/edtext.text | [Referencing](Referencing.md) | One/two-way | 0.4       |