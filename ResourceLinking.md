# Introduction #

In Android, you can define separate resources such as String, Integers etc in XML resource files. This provide good separation and easier porting of languages. From v0.3 of Android Binding, some of those resources can be included in Markup.

# Supported resources #

We support a subset of those [Resource type supported by Android](http://developer.android.com/guide/topics/resources/more-resources.html).

All following will be supported, and interpreted by Android Binding as `ConstantObservable<OutputType>`.

| Type | syntax | OutputType |
|:-----|:-------|:-----------|
| String | @string/abc | String     |
| bool | @bool/def | Boolean    |
| color | @color/red | Integer    |
| dimension | @dimen/fontSize | TypedValue |
| Integer | @integer/speed | Integer    |
| Raw  | @raw/data | Integer    |
| LayoutId | @layout/adapter\_view | Layout (Type defined in Android Binding) |

Also note that the syntax for getting from external package is also allowed, though the ? prefix is **not** supported:

```
    binding:text="@android:string/abc"
```
is OK while:
```
    binding:text="?android:string/abc"
```
is not.