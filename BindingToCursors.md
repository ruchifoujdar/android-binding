#How Android Binding Binds to Cursors

# Deprecation #

CursorObservable is deprecated and will be removed. It is safely replaced by CursorCollection.

# Introduction #

Binding to Cursors is a bit tricky, but it is very important concept in Android application development. It doesn't require all data to be loaded in memory, and thus save memory and enhance performance.

Binding of Cursors in Android binding requires deriving a special class, called `CursorRowModel`, which is a object representation of a row of the Cursor data. Here, suppose we are retrieving all the Music item from the phone:

```
Cursor c = activity.getContentResolver().query(
    MediaStore.Audio.Media.INTERNAL_CONTENT_URI, 
    new String[]{MediaStore.Audio.Media._ID, MediaStore.Audio.Media.TITLE}, 
    null, null, null);
```

Above cursor queries the MediaStore for all Audio's Title within our phone. Next, we need a object representation for that particular cursor row:

```
        public static class MusicItem extends RowModel{
		public final IdField Id = new IdField(MediaStore.Audio.Media._ID);
		public final StringField Title = new StringField(MediaStore.Audio.Media.TITLE);
	}
```

As mentioned earlier, `RowModel` representing each row of cursor content. You can imagine RowModel is the ViewModel of _each_ individual row. Since it is a ViewModel, you can put anything valid in Android Binding just like normal ViewModel, for example, you can put Commands in it, or other Observables too! But in the above example, the `IdField` and `StringField` is specifically designed to work in RowModel, which Android Binding will automatically fill in the value. All primitives supported by Cursor.get() is supported, like BlobField -> Blob, IntegerField -> Integer etc, LongField -> Long.

IdField is a special instance of LongField, when an IdField is declared in CursorRowModel, the value of that field will be considered by Android-Binding the ID of that row (which is sometimes useful).

# Binding to View #

After declaring the CursorRowModel, you need to create CursorCollection in ViewModel. For example:

```
public final CursorCollection<MusicItem> MusicList = new CursorObservable<MusicItem>(MusicItem.class);
```

There are two versions of constructor in CursorCollection, they are:

```
public CursorCollection(Class<T> rowModelType)

public CursorCollection(Class<T> rowModelType, IRowModelFactory<T> factory)
```

If using the first one, it is assumed that the CursorRowModel provided has a parameter-less constructor, while the second one you can use any constructor method.

Once everything set, you can already bind the CursorCollection to ListViews, Spinners, ExpandableListViews etc.

# Under the hood #

Android Lists is about recycling of views, and Cursors are about saving memory. With this in mind, Android Binding also recycles CursorRowModels.

In most cases, each _visible_ row in a ListView is mapped to one CursorRowModel, so if your layout have 10 rows on screen, about the same number of CursorRowModels is created.

If you scroll down, then the ListView will try to recycle the topmost view and place it to bottom, right before you can see it (this is how ListView works in Android), the attached CursorRowModel, will also be recycled in Android Binding.

# Stateless nature of RowModel #

So it is very important to know that the same instance of RowModel may mapped to position 1, and later mapped to position 10. All the values filled is temporary only, so, you should not store any state within the RowModel class.

For some reason if you need to clear the state whenever the RowModel is mapped to something new, you should override the onLoad() method.