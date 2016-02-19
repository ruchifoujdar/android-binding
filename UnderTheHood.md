# Introduction #

Android-Binding (A-B) works along with the standard Android SDK.

# Glossary #

  * View: Most often refers to widget classes (such as TextView, ImageView) in Android, but any subclass from _android.view.View_ is also considered as View. It is also the same as the View in MVVM

  * View Model: Class that contains the data and action, which will be supplied to the View for user interaction

  * Layout: The XML markup declaring how the user interface looks like.

  * View Attribute: The bindable attribute of the View. It can be either for display purpose (e.g. Text, Background Color) or behavioral (adapter)

  * View Event: A special type of View Attribute, which binds to View Model Command. In A-B, their name must starts with _on-_, for example, `onClick`, `onLongClick`. Whenever the described event happens on View, the command will fire.

  * Command: The action defined in View Model, similar to public methods.

  * Multicast Listener: Most widget in Android SDK accepts only one listener, in each of these listener, A-B provides a mechanism to make those widget can _multicast_ the event to more than one listener, through the Multicast Listener

# The binding process #

To help illustrate the binding process, assume following xml declaration is processed:

```
<LinearLayout xmlns:android=...
    xmlns:binding="http://www.gueei.com/android-binding/"
    android:layout_width...>
    <Button
        android:text="hello"
        binding:text="ShowText"
        binding:onClick="Show"/>
</LinearLayout>
```

Android uses the `LayoutInflater` to inflate the layout from XML file. Android-Binding supplies its own `LayoutInflater.Factory` to the default inflater, so it can read the additional tags in `binding` namespace. During inflating, the View (object) hierarchy is built, and the tags (such as onClick) are put in a map with the View.

Following is the board break down of the two phases: inflate and binding:

  * Inflate Phase
    * Binder: Inflate Layout
      * Factory: Inflate View
        * Attach The tags map to the View
    * The root View returned
  * Binding Phase
    * Attribute Binder: For each provider, handle all the tags known
      * Binding SyntaxResolver: create an Observable for each known tag (text, onClick)
      * Binding Provider: Create the View Attribute for the given tag name
      * Subscribe the View Attribute to the Observable

Note that any `android:` prefix is handled by Android SDK, any unknown tag with `binding:` prefix is ignored. While `android:` tags and `binding:` tags can stand side-by-side, in case of any contradicting tags, `binding:` tags will take the precedence, in above example, the button will have `ShowText` (An Observable declared in ViewModel) value instead of 'hello'.

# Attribute, Multicast Listener Attachment #

A-B tries not to touch the Android SDK widget classes. In order to inject the observer-observable behavior to the widget classes, A-B attach the collection of all View Attributes as Tag to the View through:

```
   View.setTag(id)
```

There are quite a few tags used in Android-Binding, one for the view attribute collection, one of Multicast listeners, one for the tags map and some for Cursor/Object Collection adapter use.

The Attribute or Multicast Listener can be retrieved with `Binder` class's static methods.