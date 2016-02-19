# Introduction #

This is helpful for referring to other Widget's View Attribute, because they are purely display purpose, we don't need to create bridging Observables in the View Model. Thus, a cleaner View Model is resulted.

This is a new feature and will be included in v0.4 release of Android-Binding, it is currently available to Layout type binding, but not in Options Menu and Context Menus.

# Syntax #

```
    =ID.ViewAttribute
```

# Example #

Suppose we have a Check box and an Edit Text (maybe more than one), the Edit Text is enabled only if the check box is checked. Of course, we can do something like this:

```
    <CheckBox checked="BoxChecked" .../>
    <EditText enabled="BoxChecked" .../>
```

But this requires an additional Observable created in View Model, and in some cases, that `BoxChecked` is absolutely no use to View Model since it is purely used in Views.

A better way to do will be using the new Referencing syntax, which became:

```
    <CheckBox android:id="@+id/checkbox .../>
    <EditText enabled="=id/checkbox.checked" .../>
```

Note you have to supply the `android:id` to the CheckBox you want to reference, all the Referencing statements should start with `=` sign, and the id of the referenced field, a dot, and last the view attribute you want to bind to. If the view attribute you are referencing to is of the same type, it also supports two-way binding (Get the MarkupDemo and go to Referencing demo to see this!)

# Converters and other syntaxes #

The referencing is valid with combining other syntaxes, for example:

```
    <CheckBox android:id="@+id/checkbox .../>
    <CheckBox checked="NOT(=id/checkbox.checked)" .../>
```

is totally valid and the two Check Boxes are opposite to each other.