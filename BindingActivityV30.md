# Introduction #

In the new AndroidBindingV30, original `setAndBindContentView()`(in BindingActivity) will be deprecated. In replacement, a new metadata xml file will be required.

# File structure #

The metadata file is required to be placed under res/xml folder, and following is the required structure:

```

<?xml version="1.0" encoding="utf-8"?>
<activity xmlns:binding="http://www.gueei.com/android-binding/">
	<actionBar/>
	<rootView
	    binding:dataSource="RootViewViewModel"
		binding:layoutId="@layout/root_view_layout"/>
	<optionsMenu
	    binding:dataSource="OptionsMenuViewModel"
	    binding:menu="@menu/optionsmenu"/>
</activity>

```

`<actionBar>` contains several other attributes, all of them are optional:

```
<actionBar
		binding:displayHomeAsUp="Boolean"
		binding:displayShowTitle="Boolean"
		binding:displayShowHome="Boolean"
		binding:navigationMode="Integer"
		binding:listNavigationAdapter="Adapter"
		binding:listNavigationOnItemSelected="Command"
		binding:listNavigationSelectedItem="Object"
		/>
```

Note: currently only list navigation mode is supported, works on tab navigation is in progress. All action bar attributes and usage is experimental and subject to change.

for `<rootView>` and `<optionsMenu>`, each of them can bind to separate View Models.

# Usage #

```

public class MainActivity extends BindingActivityV30 {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ViewModel vm = new ViewModel (this);
        this.inflateAndBind(R.xml.main_metadata, vm);
    }
}

```

The Activity class should be extended from `BindingActivityV30`, afterwards, in `onCreate()`, call `inflateAndBind(int, object)` will parse the above xml file and setup all the bindings.

# Rationale #

Since Android supports multiple configuration to XML files, the metadata  typed bindings will help the Activity to be configuration independent. For example, we are creating an app that utilizes Action Bar in ICS, but for lower versions, they use another layout to replace the Action Bar's function. In this case, we are creating two different `main_metadata`:

in res/xml (for other devices):

```
<?xml version="1.0" encoding="utf-8"?>
<activity xmlns:binding="http://www.gueei.com/android-binding/">
	<rootView
	    binding:dataSource="."
		binding:layoutId="@layout/main_without_actionbar"/>
	<optionsMenu
	    binding:dataSource="."
	    binding:menu="@menu/optionsmenu"/>
</activity>
```

in res/xml-v14 (for devices with api-14+):
```
<?xml version="1.0" encoding="utf-8"?>
<activity xmlns:binding="http://www.gueei.com/android-binding/">
        <actionBar>...</actionBar>
	<rootView
	    binding:dataSource="."
		binding:layoutId="@layout/main_with_actionbar"/>
	<optionsMenu
	    binding:dataSource="."
	    binding:menu="@menu/optionsmenu"/>
</activity>
```

Of course, the configuration options is unlimited. You can have different metadata for landscape, portrait, tablets/phones etc. In this way, we can target different configuration of devices with different settings, and the View Models and Activities are independent of them.