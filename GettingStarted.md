# Table of Content #



# Introduction #

Getting started to Android Binding. This is the introduction guide on how to use the Android Binding Library.

# Demo Project #

```
// Code needed
```

# Basic Markup #

Markup is done in standard Android Layout xml file. While keeping original tags untouched, additional markup tags is used to indicate what the view is going bind. For example:

```
<TextView android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    binding:text="FirstName"
/>
```

Where "binding" is the recommended (but not restricted) namespace alias used for binding View widgets with Model. In the above example, _FirstName_ is an Observable Field that is available in the provided Model.

# MVVM Pattern #

The binding helps in achieving MVVM pattern.

# Implementations #

## Observable ##

## Binded View ##

All Android default view widgets are considered to be 'bindable', and each view widget contains a collection of _Observable_ attributes. The binding engine will setup the necessary listeners to observes the change of widget attributes.

For example, a CheckBox widget will have a _checked_ attribute (which is boolean); the corresponding checked state is triggered by the _OnCheckedChange_ event. When such attribute is binded to a model, the _OnCheckedChange_ Event is automatically wired-up by the binding engine.

## Command ##

Command is the common interface that the View will triggers on certain events, like clicking on a button will fire a Command.

## Multicast Listener ##

By default, most Android views accepts one listener to one type of event. This is normally enough, but in case that you need extra listeners to subscribe to one event, that may be problematic.

## Dependent Observable ##

Sometimes a property value is dependent to one or more other variables, in such case, a DependentObservable is used.

```
// Example needed
```

## Converter ##

Converter is a sub class of DependentObservable. It also act as converting the value back to the dependent observable objects.

```
// Example needed
```