# Options Menu #

Options menu is the menu that bring up from the 'menu' button in Android devices. The menu structure is declared in xml file under res/menu/

Binding to options menu, required two steps:
  1. Declare your xml file with custom binding namespace
> 2. Route your Activity's event to MenuBinder

# XML declaration #

Similar to layout file, the XML file requires a custom namespace: http://www.gueei.com/android-binding/

Following is a portion of a sample menu xml:

```
<menu xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:binding="http://www.gueei.com/android-binding/">
	<item android:id="@+id/button1"
		android:title="Enabled=BrowserVisible"
		binding:enabled="BrowserVisible"
	/>
	<item android:id="@+id/browser_visibility"
        binding:title="IF(BrowserVisible, 'Hide group', 'Show group')"
        binding:onClick="ToggleBrowser"
        />
    <group android:id="@+id/browser"
    	binding:visible="BrowserVisible">
        <item android:id="@+id/refresh"
            android:title="Refresh" />
        <item android:id="@+id/bookmark"
            android:title="Bookmark" />
    </group>
    <item android:id="@+id/submenu" android:title="Emotions">
        <menu>        
            <item android:id="@+id/happy" 
                android:title="Happy"
                binding:onClick="ARG(SubMenuClick,'happy')"
                />
            <item android:id="@+id/neutral"
                android:title="Neutral"
                binding:onClick="ARG(SubMenuClick,'netural')"
                />
            <item android:id="@+id/sad"
                android:title="Sad"
                binding:onClick="ARG(SubMenuClick,'sad')"
                />
        </menu>
    </item>
</menu>

```

The major difference, compare to layout binding, is that menu binding requires explicit `android:id` declared in each _binded_ item (well, virtually any item), currently we support group and items and submenus.

# Routing Activity's event #

Activity that needs binding to options menu, must create and keep an instance of `MenuBinder`, which in general it should be instantiated in `onCreate(bundle)` of the Activity; then, the other two options menu event must be routed to let MenuBinder to handle:

```
        public void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    menuBinder = new MenuBinder(R.menu.menu);
	}

        // Redirect the activity events to menu binder
	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		return menuBinder.onCreateOptionsMenu(this, menu, ViewModel);
	}

	@Override
	public boolean onPrepareOptionsMenu(Menu menu) {
		return menuBinder.onPrepareOptionsMenu(this, menu);
	}
```

That's it. Your ViewModel will already binded with the items with the Menu.

# BindingActivity #

From 0.4, we also provide a helper class for managing options menu, context menu, binding to view etc, to avoid coding above mentioned routing. To use options menu with BindingActivity, you can just declare:

```
        protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		this.setAndBindRootView(R.layout.optionsmenu, ViewModel);
		this.setAndBindOptionsMenu(R.menu.optionsmenu, ViewModel);
	}
```

More functions will be included in BindingActivity later on, for example the instantiation of everything through one single meta-data xml file.

# Additional Menu Item binding in ICS/HC #

Android 3.0 introduces a new [Action View](http://developer.android.com/guide/topics/ui/actionbar.html#ActionView) concept. In order to bind to Action View, you need to provide a layout file (with binding information) to it:

```
    <item android:id="..."
        binding:actionView="@layout/action_view"/>
```