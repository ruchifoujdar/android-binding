# Status #

Removed due to complete refactoring on Array and Cursor bindings.

# Details #

IRowModel defines the following operation:

```
public interface IRowModel{
    public void onAttachedToUI(int position);
}
```

Since Android is reusing views in ListView/Spinner etc, at a certain instance, not all the Models is bind to front end UI. The `onAttachedToUI()` method will be called once the particular model is loaded with UI.

One common scenario is you can do lazy loading only when the content is needed, for example, loading a Bitmap to the memory and display it.