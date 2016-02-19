# Release History #

## 0.45 ##

  * [Referencing](Referencing.md) to other widget's view attribute
  * Refined [CustomView](CustomView.md) API. Not compatible to previous versions.
  * [OptionsMenu](OptionsMenu.md) and [ContextMenu](ContextMenu.md) binding
  * Deprecated CursorObservable, use CursorObservableCollection instead or TrackedCursorObservableCollection for automatic uri tracking.

## 0.3a ##

  * [Custom View API](CustomView.md)
  * ExpandableListView added onChildClick, clickedChild attributes
  * [ResourceLinking](ResourceLinking.md)
  * Major refactoring of the binding process, removed redundant codes

## 0.23a ##

v0.23a fixes some issues and may not stable enough:

issue:

#3 IObservable and similar is now exposing Collection instead of AbstractCollection

#4 The Observable implementation change to WeakList instead of ArrayList. This would help release the reference once the View is disposed by the GC. In this case, re-Binding on ViewModel would be fine
[Seriously, this one need extensive check]

#7 Removed the redundant call.


## 0.22 ##
  * Added support to some more View Attributes
  * Added IF converter and ARGB converter

## 0.2 ##

  * IMPORTANT! Framework Namespace prefix changed to gueei.binding, which is less verbose and easier to code
  * Refactor (and deprecated) ArraySource to ObservableCollection
  * CursorSource will be deprecated too, it will change to CursorObservable
  * Built-in ArrayListObservable, HashSetObservable
  * Experimental: ExpandableListView support
  * Modifying view model must done on UI thread
  * Text Attribute (of TextViews) changed to CharSequence instead of String: you can format the text in HTML
  * CursorFields now support creation via Column Name (alongside with column Index)
  * As usual, looking the source code in Markup Demo would help learning the latest additions

## 0.175 ##

  * Update to 0.17 that Converter can be nested

## 0.17 ##

  * Converter supported in XML layout

## 0.15 ##

  * Bug fix in Array Source
  * Added Debugger for dumping object tree
  * Added IRowModel Interface
  * Experimental: Pojo View Model Support (Not working on ArraySource, CursorSource at the moment)

## 0.11 ##

  * initial release