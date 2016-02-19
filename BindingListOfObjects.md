# Introduction #

Android Binding is capable of binding to Object Array or Cursors. Binding to array supports full two-way binding just like the normal view models, but for cursor, it's a little bit different.

# Markup #

For all subclasses of Adapter View(including ListView, Spinner etc), it supports Binding to list of objects. In order to bind, it needs a pair of markups to achieve:

```
	binding:itemSource="MusicList"
	binding:itemTemplate="@layout/music_row"
```

`itemSource` is the 'List' to bind to. It accepts ObservableCollections and CursorObservables. (It also accepts any Observable

&lt;Adapter&gt;

, in such case, the templates will be ignored)

`itemTemplate` is the layout that each item will display with. All binding syntax can be used there (even binding with Command)

For spinners, the `spinnerTemplate` attribute is also supported. When working with spinner, the spinnerTemplate is the 'normal' template when spinner is not active, while itemTemplate in this case maps to drop-down layout.

And the music\_row.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:binding="http://www.gueei.com/android-binding/"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    binding:onClick="Play"
    >
    <TextView
        android:layout_width="fill_parent"
    	android:layout_height="wrap_content"
    	android:textSize="30dip"
    	binding:text="Title"
     />
     <TextView
        android:layout_width="wrap_content"
    	android:layout_height="wrap_content"
    	android:textSize="12dip"
    	binding:text="Artist"
     />
     <RatingBar
        android:layout_width="wrap_content"
    	android:layout_height="wrap_content"
    	android:numStars="5"
    	binding:rating="Rating"
    	binding:visibility="DisplayRating"
    	binding:onRatingChanged="Save"
     />
</LinearLayout>
```

# Converters supported in itemSource #

Since `itemSource` accepts Observable

&lt;Adapters&gt;

 as parameter, converters can be used to more sophisticated control on appearances on ListViews.

Up to version 0.2, two converters for itemSource is provided:
  * ADAPTER
  * STITCH

The `ADAPTER` is provided as an alternative to replace `itemTemplate` and `spinnerTemplate` all in once. Syntax is as follow:

```
    ADAPTER({
      template=@layout/LAYOUT_ID, 
      source=SOURCE_OF_COLLECTION, 
      spinnerTemplate=@layout/LAYOUT_ID })
```

the curly brace notation is a new addition in version 0.2, which makes everything inside to be ONE single object (in [DynamicObject](DynamicObject.md) type). The template, source and spinnerTemplate is representing itemTemplate, itemSource and spinnerTemplate respectively, which spinnerTemplate is optional.

The `ADAPTER` by itself do nothing more than separating markups to different part, but it is a base on more complicated case, for example:

## STITCH ##

`STITCH` is a converter accepts _any_ number of adapter, and resulting a single adapter the stitches all the adapters supplied into one. (Under the hood it is creating a `gueei.binding.CombinedAdapter`)
This is useful when you need to create a list that needs to blend several collections together.

For example, you can do something like:
```
    STITCH(
      ADAPTER({template=@layout/list_item, source=CURSOROBSERVABLE}),
      ADAPTER({template=@layout/list_item2, source=ARRAYLISTOBSERVABLE})
    )
```
to put two lists of content, one is from cursor, one is from array list, into one single list view.

The above two Converters are just the basic, more will be coming.