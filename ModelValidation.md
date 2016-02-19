# Status #

Following is documenting the Validation package under gueei.binding.labs.validation, which is expected to ship in a different package (as addon) after Android-Binding 0.5+, while the original one within Android-Binding will be removed. The design of this one is quite different than the original one, but unless you are making custom validation rules, you won't be much affected (other than changing package import names). The Message from resource is fixed (due to certain ADT Change), and standardized so it should be easier to use.

# Introduction #

Android Binding also supports for Model Validation through @Annotation syntax. You may also add custom validation rules in a few easy steps.
Following article is based on the Sample application on the market.

market://search?q=pname:com.gueei.demo.inputvalidation

or search

Android Binding

Please download the sample app and get a feel of how it works. Source code also available. Check the source code page -> Demos folder for it.

# Initial View Model #

Let's start with a simple Registration view model:

```
public class RegistrationViewModel{
    public final StringObservable Login;
    public final StringObservable  Password;
    public final StringObservable  ConfirmPassword;
    public final Comamnd Submit;
}
```

Remember, in Android Binding, it is expected the Observables are _public_ fields, it might be a good practice to decorate it as _final_ so no latter remapping will occur.

# Add validation rules #

OK, the login and password are obviously mandatory. It is called **Required** in Android Binding. All the validation classes in sitting in the package: com.gueei.android.binding.validation.validators; Remember, if you found the validators are not enough, you can always make a custom one, which will cover later in the post.

So, here's how we instruct the Android Binding that those fields are Required:

```
public class RegistrationViewModel{
    @Required
    public final Observable<CharSequence> Login;
    @Required
    public final Observable<CharSequence> Password;
    @Required
    public final Observable<CharSequence> ConfirmPassword;
}
```

That's it! We just need to put an **@Required** annotation attribute to the required field.

# Validate #

Validation is done by using ModelValidator. In order to construct the ModelValidator, you need to pass the Context, model and the R.string.class (will explain later):

```
ModelValidator mv = new ModelValidator(mContext, this, R.string.class);
ValidationResult result = mv.ValidateModel();
```

You might want to validate in whenever the user 'submits' the form. The above line of code will return an object of class: **ValidationResult** which contains whether the validation is success, if not, the error messages associated with it. If you don't like (yes, you shouldn't) the predefined messages, you may put in your own:

```
...
@Required(ErrorMessage="You must put the login name!")
public final StringObservable Login;
...
```

# More Built-in validation rules #

The Confirm Password is something you wanna make sure your user didn't mistype a password. It must be the same as the Password field. Here's how we do:

```
	@Required
	@EqualsTo(Observable = "Password", ErrorMessage="Confirm Password must match Password.")
	public final StringObservable ConfirmPassword = new StringObservable();
```

Simple! We tell the Framework that confirm password must be equals to another observable, that is, the Password field. One more thing, the attributes are 'stackable', that means, one Observable property can have multiple validation rules applied on it. Just like the above example, that means:

> The _ConfirmPassword_ is required and it must equals to _Password_

Ok, we are done with password, how about the user name? We only want to accept alphabets, numbers and, underscore; and it must be at least 3 characters to most 8 characters. Simplest way to check everything like this, is using Regular Expression.

```
        @Required(ErrorMessage="Login is required")
	@RegexMatch(Pattern = "^[A-Za-z0-9_]{3,8}$", ErrorMessage="Login need to contains alphanumeric characters and must be 3-8 characters long.")
	public final StringObservable Login = new StringObservable("");
```

# Local Language Support for Validation Message #

Most of the time, your application can go multilingual, and the validation message shouldn't be set static like the example above. In Android, we use the XML file to define Strings which can be tailored for different language settings. In the code, we access the String in terms of `R.string.name`. Suppose we defined an error message in our R.string, which the name is: `validation_password_length` (i.e., access with R.string.validation\_password\_length), we can use it to decorate the validation rule:

```
	@MinLength(Length=5, ErrorMessageRes="validation_password_length")
	public final StringObservable Password = new StringObservable();
```

The parameter is called `ErrorMessageRes`, it will look for the string in `R.string` (which is the class we pass in constructing the ModelValidator class).

# Currently supported validation #

  * Required
  * RegexMatch
  * EqualsTo
  * MinLength
  * MaxLength

# Custom Validation #

Since Java does not support Inheritance on @interface, working with custom validation is a little bit tricky. To simplify the explanation, I'd start with the implementation of @Required annotation:

```
@Retention(RetentionPolicy.RUNTIME)
public @interface Required{
	public Class<?> Validator() default RequiredValidator.class;

	public String ErrorMessage() default "%fieldname% is required";
	public String ErrorMessagRes() default "";
	
	public class RequiredValidator extends ValidatorBase<Required> {

		@Override
		public Class<Required> getAcceptedAnnotation() {
			return Required.class;
		}

		@Override
		protected String doFormatErrorMessage(Context context,
				Required parameters, String fieldName, String errorMessageFormat) {
			return errorMessageFormat.replace("%fieldname%", fieldName);
		}

		@Override
		protected boolean doValidate(Context context, Object value,
				Required parameters, Object model) {
			if (value==null) return false;
			if (Boolean.FALSE.equals(value)) return false;
			if (value instanceof CharSequence){
				if (((CharSequence) value).length() == 0) return false;
			}
			return true;
		}
	}
}
```

We _MUST_ declare the interface's retention as `@Retention(RetentionPolicy.RUNTIME)` or else the attribute will not be available in Runtime.

The field `public Class<?> Validator()` is a must, the ModelValidator only works with annotation with this field. It need to return a class, by default, and should not change, that will be the Validator class.

Everything else in the interface is optional, they will then passed to the validator as "parameters" (or options), but ErrorMessage() and ErrorMessageRes() is recommended to include, it helps the ModelValidator to construct the error message for you.

`public class RequiredValidator extends ValidatorBase<Required>`
declares the validator, which must be a subclass of ValidatorBase<?>, where the <?> you should put the @interface you are accepting.

Three methods needed to be implemented.
  * getAcceptedAnnotation() returns the type of @interface this Validator is accepting.
  * doFormatErrorMessage() returns the formatted error message, most probably you need to do the string replacement there
  * doValidate() actually validates the input value. Here, notice the first parameter **Object value** is the value, but not the observable object. the Whole model is also passed in as parameter.