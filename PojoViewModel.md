# Introduction #

Currently experimenting Pojo-styled (Plain-Old-Java-Object) View Models. Such View Models no longer need the `Observables` nor `Commands` but simply defined their own binding-interfaces as normal object:

  * Commands: Plain 'void' Method with _No_ arguments
  * Observables: Private fields with `public` _getters_ and/or _setters_

The only requirement for the ViewModel is it need to _implements_ the `PojoViewModel` interface.

# Current state #

It will be included in v0.15 release, it is only working in flat view models (i.e. Array, Cursor are not supported)

# `PojoViewModel` interface #

The interface defined two contracts:

```
    public interface PojoViewModel {
	PojoViewModelHelper getHelper();
	void notifyPropertyChanged(String propertyName);
    }
```

`getHelper()` returns a PojoViewModelHelper, which requires the View Model to instantiate and keep the reference. `notifyPropertyChanged` is supposed to be called within your View Model, whenever a public property is changed. Following is a common implementations of the interface:

```
    public class SamplPojoViewModel implements PojoViewModel {
	private PojoViewModelHelper mHelper = new PojoViewModelHelper();
	public PojoViewModelHelper getHelper() {
		return mHelper;
	}

	public void notifyPropertyChanged(String propertyName) {
		mHelper.notifyPropertyChanged(propertyName);
	}
        // rest of the class declarations
    }
```

# Sample use case #

Suppose we are doing a simple login activity, so, our View Model needs to have following:

  * Login Name
  * Password
  * Submit (Action)

so, following will be the View Model class:

```
    public class LoginViewModel implements PojoViewModel {
	private PojoViewModelHelper mHelper = new PojoViewModelHelper();
	public PojoViewModelHelper getHelper() {
		return mHelper;
	}

	public void notifyPropertyChanged(String propertyName) {
		mHelper.notifyPropertyChanged(propertyName);
	}

        // View Model Public contracts
        public String getLoginName() { return loginName; }
        public String getPassword() { return password; }
        // setters
        public void setLoginName(String loginName){
            this.loginName = loginName;
            notifyPropertyChanged("LoginName");
        }

        public void setPassword(String password){
            this.password = password;
            notifyPropertyChanged("Password");
        }
        
        public void Submit(){ ... }
        
        // Private fields
        private String loginName;
        private String password;
        // ... rest of the stuffs
    }
```
# Binding #

After you have a Pojo-type View Model, you have to **wrap** it before passing to Binder, since Binder is not capable to recognize Pojo.

```
    PojoViewModelWrapper<?> wrapper = PojoViewModelWrapper.create(new SamplePojoViewModel());
    Binder.setAndBindContentView(this, R.layout.main, wrapper);
```

# Layout #

The layout file would be exactly the same as original Observable-Command binding. For instance, the above login example could looks like:

```
    <TextView android:...
     binding:text="LoginName"/>
    <TextView android:...
     binding:text="Password"/>
    <Button android:...
     binding:onClick="Submit"/>
```