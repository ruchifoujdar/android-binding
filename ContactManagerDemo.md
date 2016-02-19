

# Introduction #

[Contact Manager](http://developer.android.com/resources/samples/ContactManager/index.html) is one of the sample code in android's developers' page. This demo is a re-write on the sample code, using the separated presentation and business logic supported by Android Binding.

# Source code #

[Source code](http://code.google.com/p/android-binding/source/browse/#svn%2FDemos) is available. This document will hightlight some of the most interesting point and compare with the original sample.

# Layout #

## Layout (portion) in original Android-way ##
```
    <ListView android:layout_width="match_parent"
              android:id="@+id/contactList"
              android:layout_height="wrap_content"
              android:layout_weight="1"/>
    <CheckBox android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:id="@+id/showInvisible"
              android:text="@string/showInvisible"/>
    <Button android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/addContactButton"
            android:text="@string/addContactButtonLabel"/>
```

## Layout(portion) in Android-Binding-way ##
```
    <ListView android:layout_width="fill_parent"
              android:layout_height="wrap_content"
              binding:adapter="ContactList"
              binding:clickedId="SelectedContactId"
              binding:itemClicked="ViewContact"
              android:layout_weight="1"/>
    <CheckBox android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              binding:checked="ShowInvisible"
              binding:checkedChange="PopulateList"
              android:text="@string/showInvisible"/>
    <Button android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            binding:click="AddContact"
            android:text="@string/addContactButtonLabel"/>
```

Which they look pretty much the same. While the **android** tag prefix is kept and function as always, additional **binding** prefix is to denote that what the view will bind to, in the model.

# Activity #

Activity is acting as purely a controller in Android Binding, what actually dictate the output of the view, is defined in the Model (or View Model in MVVM). The user interaction, like handling user-click  to a button, is setup by Android Binding and they no longer exists in the back end codes, everything about interaction is defined in the Layout.

## Original Android-way Activity ##

```
    public void onCreate(Bundle savedInstanceState){
        Log.v(TAG, "Activity State: onCreate()");
        super.onCreate(savedInstanceState);
        setContentView(R.layout.contact_manager);

        // Obtain handles to UI objects
        mAddAccountButton = (Button) findViewById(R.id.addContactButton);
        mContactList = (ListView) findViewById(R.id.contactList);
        mShowInvisibleControl = (CheckBox) findViewById(R.id.showInvisible);

        // Initialize class properties
        mShowInvisible = false;
        mShowInvisibleControl.setChecked(mShowInvisible);

        // Register handler for UI elements
        mAddAccountButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                Log.d(TAG, "mAddAccountButton clicked");
                launchContactAdder();
            }
        });
        mShowInvisibleControl.setOnCheckedChangeListener(new OnCheckedChangeListener() {
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                Log.d(TAG, "mShowInvisibleControl changed: " + isChecked);
                mShowInvisible = isChecked;
                populateContactList();
            }
        });

        // Populate the contact list
        populateContactList();
    }
```

The Activity need to handle the populating of items, referencing UI elements, and wired-up the UI elements.

## Activity (In Android Binding Way) ##

```
    public void onCreate(Bundle savedInstanceState)
    {
        Log.v(TAG, "Activity State: onCreate()");
        super.onCreate(savedInstanceState);
        Binder.init();
        ContactManagerModel model = new ContactManagerModel(this);
        Binder.setAndBindContentView(this, R.layout.contact_manager, model);
    }
```

Activity is very clean, it need to instruct which model the binder to bind with the layout, of course, data population are handled in the model, and the model is not view-aware:

# Model (or View Model) #

```
    public class ContactManagerModel {
	private Activity mContext;
	public Observable<Adapter> ContactList = new Observable<Adapter>();
	public Observable<Boolean> ShowInvisible = new Observable<Boolean>(false);
	public Observable<Object> SelectedContact = new Observable<Object>(new Object());
	public Observable<Long> SelectedContactId = new Observable<Long>(0l);

	public Command ViewContact = new Command(){
		public void Invoke(View view, Object... args) {
			Long x = SelectedContactId.get();
			toastContact(x.toString());
		}
	};
	
	public Command PopulateList = new Command(){
		public void Invoke(View view, Object... args) {
			populateContactList();
		}
	};
	public Command AddContact = new Command(){
		public void Invoke(View view, Object... args) {
			launchContactAdder();
		}
	};
	
	public ContactManagerModel(Activity context){
		mContext = context;
		populateContactList();
	}
  ...
```

A Model need to have basically two types of _public_ Fields:

  1. Observable
  1. Command

Observable<?> is a Generic type that denotes something within it is _Observable_, which changes to it will fire a notification to all its subscribers.

Command is an interface that defines the behavior, a command is normally bind to View actions, such as click on a button or selection of a spinner.